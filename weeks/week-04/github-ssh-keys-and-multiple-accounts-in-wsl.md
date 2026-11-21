[← Week 4 - Editors, tmux and SSH basics notes](week-04-notes.md)

# Clone from GitHub over SSH in WSL (Permission denied (publickey))

A first `git clone` over SSH in a fresh WSL install fails with `Permission denied (publickey)`. This note explains the cause, the fix, and how to use more than one GitHub account on the same machine.

## Problem

```text
$ git clone git@github.com:bartvanhoey/LinuxInSixMonths.git
Cloning into 'LinuxInSixMonths'...
The authenticity of host 'github.com (140.82.121.3)' can't be established.
ED25519 key fingerprint is: SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

The host-key prompt is normal on the first connection. Answer with the full word `yes`, not `y`. Check the fingerprint against [GitHub's published SSH key fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints) before you accept it.

## Cause

WSL has its own home directory, so it does not see the SSH keys you made on Windows. A fresh install has no key at all:

```bash
ls -la ~/.ssh      # only known_hosts
ssh-add -l         # Could not open a connection to your authentication agent.
```

GitHub only lets you in if your client offers a private key whose public half is registered on your account. No key means `Permission denied (publickey)`.

## Fix: create a key and add it to GitHub

### 1 - Generate a key

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

Accept the default path (`~/.ssh/id_ed25519`). A passphrase is optional but recommended.

### 2 - Copy the public key

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the whole line. Only ever share the `.pub` file, never the private key.

### 3 - Add it to GitHub

On GitHub: **Settings** → **SSH and GPG keys** → **New SSH key**. Paste the line and save.

### 4 - Test and clone

```bash
ssh -T git@github.com
# Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.

git clone git@github.com:bartvanhoey/LinuxInSixMonths.git
```

## Other options

- **Reuse the Windows key**: if you already have a key in `C:\Users\<you>\.ssh`, copy it into WSL. SSH refuses a private key that others can read, so fix the mode:

  ```bash
  cp /mnt/c/Users/<you>/.ssh/id_ed25519* ~/.ssh/
  chmod 600 ~/.ssh/id_ed25519
  ```

- **HTTPS instead of SSH**: `git clone https://github.com/bartvanhoey/LinuxInSixMonths.git` works without login for a public repo. To push you need a personal access token or `gh auth login`.

## Multiple GitHub accounts

You can use several accounts (for example personal and work) on one machine. Each account needs its own key, because GitHub rejects a key that is already on another account. A host alias in `~/.ssh/config` then picks the key.

### 1 - One key per account

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_personal -C "personal"
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_work     -C "work"
```

Add each `.pub` file to its own GitHub account.

### 2 - A host alias per account in `~/.ssh/config`

```text
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes

Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
    IdentitiesOnly yes
```

```bash
chmod 600 ~/.ssh/config
```

`IdentitiesOnly yes` matters. Without it SSH may offer another key first, GitHub accepts it, and you are signed in as the wrong account.

### 3 - Use the alias instead of `github.com`

```bash
git clone git@github-personal:bartvanhoey/LinuxInSixMonths.git
git clone git@github-work:SomeOrg/some-repo.git

# an existing clone
git remote set-url origin git@github-work:SomeOrg/some-repo.git
```

Check each alias. Each should greet you with that account's username:

```bash
ssh -T git@github-personal
ssh -T git@github-work
```

### 4 - The right commit author per account

The SSH key decides who may push. The commit author comes from `user.email`, which is a separate setting. Keep each account's repos in its own folder and let git switch the identity by folder. In `~/.gitconfig`:

```text
[includeIf "gitdir:~/src/work/"]
    path = ~/.gitconfig-work
```

And in `~/.gitconfig-work`:

```text
[user]
    name = Your Name
    email = you@work.example
```

Check it from inside a repo with `git config user.email`.

## Troubleshooting

- **Still `Permission denied (publickey)`**: run `ssh -vT git@github.com` and look for `Offering public key` lines. If none appear, SSH cannot find the key. Check the file name and the `IdentityFile` path.
- **`Hi <other-account>!`**: the wrong key was used. Add `IdentitiesOnly yes` to the host alias and clone with the alias.
- **`UNPROTECTED PRIVATE KEY FILE!`**: the key is readable by others, typically after copying from `/mnt/c`. Run `chmod 600` on it.
- **Passphrase prompt on every command**: start an agent and load the key once per session with `eval "$(ssh-agent -s)"` and `ssh-add ~/.ssh/id_ed25519`.
