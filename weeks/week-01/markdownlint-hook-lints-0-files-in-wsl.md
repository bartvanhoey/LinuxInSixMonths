[← Week 1 - WSL2 as your daily driver notes](week-01-notes.md)

# The markdownlint pre-push hook lints 0 files in WSL

This repo has a pre-push hook that lints all Markdown files before every push. In WSL the hook passed every time, while the same check on GitHub failed. This note explains why and how it was fixed.

## Problem

`git push` printed this, and the push went through:

```text
'\\wsl.localhost\Ubuntu\home\<user>\src\MyGitHubRepos\LinuxInSixMonths'
CMD.EXE was started with the above path as the current directory.
UNC paths are not supported.  Defaulting to Windows directory.
markdownlint-cli2 v0.24.2 (markdownlint v0.42.2)
Finding: README.md weeks/*.md notes/**/*.md
Linting: 0 files
Summary: 0 issues in 0 files
```

At the same time, the **Docs checks** workflow on GitHub linted 60 files and failed:

```text
notes/herdr-loses-claude-sessions-in-wsl.md:55:30 error MD060/table-column-style Table column style [Table pipe does not align with header for style "aligned"]
```

## How the hook is set up

The hook is a file in the repo, `.githooks/pre-push`. Git uses it because of one setting in `.git/config`:

```bash
git config core.hooksPath .githooks
```

Git runs `pre-push` before every push. If it exits with an error, the push stops. To skip it in an emergency: `git push --no-verify`.

The hook runs the same command as the GitHub workflow (`.github/workflows/docs-checks.yml`):

```bash
npx --yes markdownlint-cli2 "README.md" "weeks/*.md" "notes/**/*.md"
```

## Cause

Node.js was not installed inside WSL, only on Windows.

1. WSL adds the Windows `PATH` to the Linux `PATH`, so `npx` was found on the Windows side:

   ```bash
   command -v npx    # /mnt/c/Program Files/nodejs/npx
   ```

2. Windows `npx` is a batch file, so it runs through `cmd.exe`.
3. `cmd.exe` can't use a WSL folder (`\\wsl.localhost\...` is a UNC path). It falls back to `C:\Windows`.
4. In `C:\Windows` none of the file patterns match: **0 files**, **0 issues**, exit code 0. The hook passed without checking anything.

## Fix

### 1 - Install Node.js and npm inside WSL

```bash
sudo apt install -y nodejs npm
hash -r                 # make bash forget the old Windows npx path
command -v node npx     # /usr/bin/node and /usr/bin/npx
node --version          # v22.x, same as the GitHub workflow
```

- `npx` comes with the **npm** package. Installing only `nodejs` is not enough: `npx` is still the Windows one.
- `sudo` asks for your password, so run it in a normal terminal. In Claude Code, the `!` prefix has no terminal to ask in: `sudo: A terminal is required to authenticate`.
- `/usr/bin` comes before the Windows folders in the `PATH`, so the Linux `npx` wins once it is installed.

### 2 - Make the hook fail loudly

A broken setup should block the push, not pass silently. The hook now checks which `npx` it finds first:

```sh
#!/bin/sh
# Lint Markdown before every push. Same file set as .github/workflows/docs-checks.yml

npx_path=$(command -v npx) || {
  echo "pre-push: npx not found. Install Node.js in WSL: sudo apt install nodejs npm" >&2
  exit 1
}

case "$npx_path" in
  /mnt/*)
    echo "pre-push: npx is the Windows one ($npx_path), which can't lint files in WSL." >&2
    echo "pre-push: install Node.js in WSL: sudo apt install nodejs npm" >&2
    exit 1
    ;;
esac

npx --yes markdownlint-cli2 "README.md" "weeks/**/*.md" "overthewire/*.md" || exit 1
```

- `command -v npx` prints the path of the `npx` that would run, or fails when there is none.
- `case ... /mnt/*)` matches any path on a Windows drive.
- `>&2` writes the message to stderr, where error messages belong.

### 3 - Test it

Run the hook without pushing:

```bash
sh .githooks/pre-push; echo "exit=$?"
```

```text
Linting: 60 files
Summary: 0 issues in 0 files
exit=0
```

The file count must match the one in the GitHub workflow log. `0 files` means the check didn't run.

## Lesson

A check that reports "0 issues" is not the same as a check that passed. Always look at **how much** it checked. The warning sign was there in every push: `Linting: 0 files`.
