[← Week 1 - WSL2 as your daily driver notes](week-01-wsl2-as-your-daily-driver.md)

# Restore from a private on-prem NuGet feed in WSL (401 Unauthorized)

A .NET project restores fine on Windows but fails in WSL with `401 (Unauthorized)` on a private Azure DevOps Server feed, even though the personal access token (PAT) is correct. This note describes the symptoms, the cause and the fix.

## Problem

`dotnet restore` fails for the private feed:

```text
error NU1301: Unable to load the service index for source
https://<devops-host>:8080/<collection>/_packaging/<feed>/nuget/v3/index.json.
  Response status code does not indicate success: 401 (Unauthorized).
```

Things that did **not** help, because they were not the cause:

- Putting the PAT in `NuGet.Config` with `ClearTextPassword`.
- Passing the credentials in the environment variable `NuGetPackageSourceCredentials_<SourceName>`.
- Switching the username (plain name vs. `DOMAIN\name`).
- `dotnet nuget locals all --clear`.

### The misleading errors on the way

- `dotnet nuget update source ... --configfile <file>` prints `error: Object reference not set to an instance of an object.` when the source does **not exist in that file**. The source was defined in the repo's `nuget.config`, not in the user config. This is a NuGet CLI bug with a bad message: `update` can only change a source that is in the named file.
- `dotnet nuget add source` then says `The name specified has already been added...` because the repo config already defines it.
- A `--username DOMAIN\name` argument without quotes loses the backslash in bash. Always quote it: `'DOMAIN\name'`.

## Cause

Test the PAT outside NuGet:

```bash
curl -s -o /dev/null -w "%{http_code}\n" -u "<user>:<PAT>" \
  https://<devops-host>:8080/<collection>/_packaging/<feed>/nuget/v3/index.json
```

This returns `200`, so the token, the username and the network are fine. Now look at what the server offers when it sends no credentials:

```bash
curl -s -I https://<devops-host>:8080/<collection>/_packaging/<feed>/nuget/v3/index.json \
  | grep -iE "^HTTP|www-authenticate"
```

```text
HTTP/2 401
www-authenticate: Bearer
www-authenticate: Basic realm="https://<devops-host>:8080/"
www-authenticate: Negotiate
www-authenticate: NTLM
```

The server offers `Negotiate` and `NTLM` as well as `Basic`.

- **curl** sends the PAT as Basic right away, so it works.
- **NuGet** sends no credentials first, sees `Negotiate` and `NTLM` in the challenge, and tries one of those with the PAT. A PAT is not a domain password, so the server rejects it. .NET does not fall back to Basic.
- **On Windows** this works because NuGet signs in with your domain account (integrated authentication), so the PAT is never needed.

So no value in a config file can fix this. The built-in credential path of NuGet is the problem.

## Fix: the Azure Artifacts Credential Provider

A credential provider is a plugin. NuGet asks it for credentials when it gets a 401. The Azure Artifacts Credential Provider can read a PAT from an environment variable and hands it to NuGet.

### 1 - Install the plugin

It installs into `~/.nuget/plugins`. Read the script before you run it:

```bash
wget -O /tmp/install-credprovider.sh https://aka.ms/install-artifacts-credprovider.sh
less /tmp/install-credprovider.sh
bash /tmp/install-credprovider.sh
ls ~/.nuget/plugins/netcore
```

You should see `CredentialProvider.Microsoft`.

### 2 - Create a PAT with the smallest scope

On the DevOps server: **User settings** → **Personal access tokens** → **New Token**.

- Use the **same server** that hosts the feed (not `dev.azure.com`).
- Scope: **Packaging → Read** only.
- Set a short expiry (30 to 90 days).

Never paste the PAT into chats, tickets or files in git. If you did, revoke it and create a new one.

### 3 - Store the PAT in a private file

The plugin reads the variable `VSS_NUGET_EXTERNAL_FEED_ENDPOINTS`. It is a JSON list that maps feed URLs to a username and the PAT. Put it in its own file that only you can read, and load it from `~/.bashrc`:

```bash
cat > ~/.nuget-feed-env <<'EOF'
export VSS_NUGET_EXTERNAL_FEED_ENDPOINTS='{"endpointCredentials":[{"endpoint":"https://<devops-host>:8080/<collection>/_packaging/<feed>/nuget/v3/index.json","username":"<user>","password":"<PAT>"}]}'
EOF
chmod 600 ~/.nuget-feed-env
echo '[ -f ~/.nuget-feed-env ] && . ~/.nuget-feed-env' >> ~/.bashrc
source ~/.bashrc
```

- The `endpoint` must be the exact URL of the feed's `index.json`.
- Check that the variable is set without printing the PAT:

  ```bash
  echo "${VSS_NUGET_EXTERNAL_FEED_ENDPOINTS:+set}"
  ```

  It should print `set`.

### 4 - Restore

```bash
dotnet restore --interactive
dotnet build
```

`--interactive` lets the plugin ask for a login if it needs one. With the variable set, later restores work without it.

If you want proof that the plugin ran, add `-v d` and look for `CredentialProvider` lines. A good run contains:

```text
Using .../CredentialProvider.Microsoft.dll as a credential provider plugin.
VstsBuildTaskServiceEndpointCredentialProvider - Found credentials for endpoint https://<devops-host>...
```

### 5 - Clean up

- Remove any `<packageSourceCredentials>` block from `NuGet.Config` or the repo's `nuget.config`. The PAT should live in one place only.
- Make sure `git status` shows no credentials in `nuget.config`. A file in the repo is committed with the next change.
- The editor may add a UTF-8 BOM to `nuget.config`. `git diff nuget.config` shows it as a changed first line. Revert it with `git checkout nuget.config`.

## Check the setup in order

If a restore fails, work through these in order:

1. **`dotnet nuget config paths` and `dotnet nuget list source`**: which config files are read, and does the source exist?
2. **`curl -u "<user>:<PAT>" <index.json>`**: `200` means the PAT is good. `401` means the token is expired, revoked, from the wrong server or collection, or lacks the Packaging (Read) scope.
3. **`curl -sI <index.json> | grep -i www-authenticate`**: if `Negotiate` or `NTLM` is offered, you need the credential provider.
4. **`dotnet restore --interactive -v d`**: does the log say the provider was used and found credentials?

## Troubleshooting

- **`error: Object reference not set to an instance of an object` on `update source`**: the source is not in the config file you named. Run `dotnet nuget list source` and use the file that defines it, or add the source to that file first.
- **Still `401` and the log has no `CredentialProvider` lines**: the plugin is not installed, or `VSS_NUGET_EXTERNAL_FEED_ENDPOINTS` is not set in this shell. Open a new terminal or run `source ~/.bashrc`.
- **Plugin found credentials but still `401`**: the PAT expired or its scope is wrong. Create a new one and update `~/.nuget-feed-env`.
- **SSL or certificate errors instead of a 401**: WSL does not trust the company CA. Copy the CA certificate to `/usr/local/share/ca-certificates/` as a `.crt` file and run `sudo update-ca-certificates`.
- **The restore works in a terminal but not from a tool**: tools started outside your shell do not read `~/.bashrc`. Start them from the terminal.
- **Packages from the private feed are missing even though you have access**: if `nuget.config` has a `packageSourceMapping`, the package ID must match a pattern for that source.

## Other options

- **Local folder feed**: copy the `.nupkg` files of the private packages from the Windows NuGet cache (`%USERPROFILE%\.nuget\packages`) into a folder, and add it as a source. Remember the source mapping for it. This works offline but does not update itself.
- **Kerberos**: install `krb5-user` and get a ticket with `kinit`. Negotiate then works with your domain account. This needs the Kerberos realm and a working connection to the domain controller from WSL.
- **Windows SDK**: run the build with the Windows `dotnet.exe` until the Linux setup is ready.

> Ask whoever runs the server whether the feed can accept Basic authentication only. That fixes the problem for every Linux client and build agent.
