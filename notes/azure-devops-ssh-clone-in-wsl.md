# Clone an Azure DevOps repo over SSH in WSL

## 1 - Generate an SSH key (RSA)

Azure DevOps doesn't accept ed25519 on older setups, so RSA is the safe choice:

```bash
ssh-keygen -t rsa-sha2-512 -b 4096 -C "you@example.com"
```

Accept the default path (`~/.ssh/id_rsa`) and optionally set a passphrase.

## 2 - Copy the public key

```bash
cat ~/.ssh/id_rsa.pub
```

## 3 - Add it to Azure DevOps

- Go to **User settings** (top right) → **SSH public keys** → **New Key**.
- Give it a name and paste the key.

## 4 - Add the host to your SSH config (optional)

This avoids algorithm errors:

```bash
nano ~/.ssh/config
```

```bash
Host ssh.dev.azure.com
    HostName ssh.dev.azure.com
    User git
    IdentityFile ~/.ssh/id_rsa
    IdentitiesOnly yes
    PubkeyAcceptedAlgorithms +ssh-rsa
    HostkeyAlgorithms +ssh-rsa
```

```bash
chmod 600 ~/.ssh/config
```

## 5 - Test the connection

```bash
ssh -T git@ssh.dev.azure.com
```

The first time, type `yes` to trust the host. You should see a message saying you authenticated successfully.

## 6 - Clone

In the repo, click **Clone** → **SSH** to get the URL:

```bash
git clone git@ssh.dev.azure.com:v3/<organization>/<project>/<repo>
```

## On-prem Azure DevOps Server: "no matching key exchange method found"

### Problem

Cloning from an on-prem Azure DevOps Server (not `ssh.dev.azure.com`) fails before authentication even starts:

```text
$ git clone ssh://<devops-host>:22/<collection>/<project>/_git/<repo>
Cloning into '<repo>'...
Unable to negotiate with 10.x.x.x port 22: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1,diffie-hellman-group14-sha1
fatal: Could not read from remote repository.
```

The server only offers old SHA-1 key exchange methods. Modern OpenSSH disables them by default, so the client and server share no common method.

### Fix

**1. Add a host block to `~/.ssh/config`** that enables the legacy method for this host only:

```bash
Host <devops-host>
    HostName <devops-host>
    User git
    IdentityFile ~/.ssh/id_rsa
    IdentitiesOnly yes
    KexAlgorithms +diffie-hellman-group14-sha1
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
```

- Use `diffie-hellman-group14-sha1`, not `group1`. The server offers both, and group14 is the stronger one.
- `User git` is what Azure DevOps SSH normally expects. If you get a permission error, check that the user matches your server setup.
- The legacy algorithms apply only to this host. Don't put them under `Host *`.

**2. Accept the host key in a normal WSL terminal.** The first connection asks you to trust the server's host key. Tools that run git without a terminal (for example the `!` shell in Claude Code) fail with:

```text
ssh_askpass: exec(/usr/bin/ssh-askpass): No such file or directory
Host key verification failed.
```

Run this in a regular terminal tab, check the fingerprint against the one your admins give you, and answer `yes`:

```bash
ssh -T <devops-host>
```

It may print a permission or shell-access message afterwards. That is fine: the key is now saved in `~/.ssh/known_hosts`.

**3. Clone again:**

```bash
git clone ssh://<devops-host>:22/<collection>/<project>/_git/<repo>
```

If it then fails with `Permission denied (publickey)`, add `~/.ssh/id_rsa.pub` under **User settings** → **SSH public keys** in DevOps (see step 3 above).

> The server's SHA-1 key exchange is deprecated. Tell whoever runs the server so they can update its SSH configuration.

## Troubleshooting

- **`Permission denied (publickey)`**: the key isn't added to Azure DevOps, or the wrong key is being offered. Check with `ssh -vT git@ssh.dev.azure.com`.
- **Use the WSL key, not the Windows one**: keys in `/mnt/c/...` often fail with "permissions too open". Keep them in `~/.ssh` on the Linux filesystem.
- **`no matching key exchange method found`**: see the on-prem section above.
- **`Host key verification failed` with `ssh_askpass`**: no terminal to confirm the host key. Run `ssh -T <host>` in a normal terminal first.
- **Spaces in project names**: URL-encode them as `%20` in the clone URL.
