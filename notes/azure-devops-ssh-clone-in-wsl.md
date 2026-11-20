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

## Troubleshooting

- **`Permission denied (publickey)`**: the key isn't added to Azure DevOps, or the wrong key is being offered. Check with `ssh -vT git@ssh.dev.azure.com`.
- **Use the WSL key, not the Windows one**: keys in `/mnt/c/...` often fail with "permissions too open". Keep them in `~/.ssh` on the Linux filesystem.
- **Spaces in project names**: URL-encode them as `%20` in the clone URL.
