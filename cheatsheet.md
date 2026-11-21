[Overview](README.md)

# Linux cheat sheet

My own quick reference, built up week by week over the 26-week plan. One line per command, with only the options I actually use. The details and the stories behind them are in the weekly notes.

**How to add to it:** after each week, move the commands I keep looking up into the right section. Short entries; link to the notes for the long explanation.

## Contents

- [Getting help](#getting-help)
- [Navigation](#navigation)
- [Listing and inspecting files](#listing-and-inspecting-files)
- [Wildcards](#wildcards)
- [Files and directories](#files-and-directories)
- [Links](#links)
- [Finding files](#finding-files)
- [Tricky file names](#tricky-file-names)
- [Viewing files](#viewing-files)
- [Disk usage](#disk-usage)
- [Pipes and text tools](#pipes-and-text-tools)
- [Editors, tmux and SSH](#editors-tmux-and-ssh)
- [Users, groups and permissions](#users-groups-and-permissions)
- [Processes and signals](#processes-and-signals)
- [systemd and journald](#systemd-and-journald)
- [Packages and storage](#packages-and-storage)
- [Networking](#networking)
- [Firewalls](#firewalls)
- [Bash scripting](#bash-scripting)
- [Docker](#docker)
- [Ansible](#ansible)
- [WSL and git](#wsl-and-git)

## Getting help

| Command | What it does |
| --- | --- |
| `type cmd` | Show what kind of command `cmd` is: program, builtin, function or alias. |
| `which cmd` | Show which executable runs for `cmd`. |
| `help cmd` | Help for a shell builtin (`help cd`). |
| `man cmd` | Manual page. `/word` searches, `n` next match, `q` quits. |
| `apropos word` | Search manual page descriptions for `word`. |
| `whatis cmd` | One-line description of `cmd`. |
| `info cmd` | Info entry, sometimes longer than the man page. |
| `alias name='cmd'` | Define a shortcut; `alias` alone lists them. |

The four kinds of commands are explained in [Week 1 notes: Working with Commands](weeks/week-01/week-01-notes.md#5-working-with-commands).

## Navigation

| Command | What it does |
| --- | --- |
| `pwd` | Print the working directory. |
| `cd dir` | Change to `dir`. |
| `cd` or `cd ~` | Go to my home directory. |
| `cd ~user` | Go to the home directory of `user`. |
| `cd ..` | Go up one level. |
| `cd -` | Go back to the previous directory. |
| `clear` | Clear the screen (or Ctrl+L). |

## Listing and inspecting files

| Command | What it does |
| --- | --- |
| `ls -l` | Long listing: type, permissions, links, owner, group, size, date, name. |
| `ls -lh` | Same, with human-readable sizes. |
| `ls -a` | Include hidden files (names starting with `.`). |
| `ls -A` | Same, without `.` and `..`. |
| `ls -lt` | Sort by modification time, newest first. |
| `ls -S` | Sort by size, largest first. |
| `ls -d */` | Only directories. |
| `ls -R` | Recursive. |
| `ls -F` | Mark directories with `/` and executables with `*`. |
| `ls -li` | Show inode numbers (see [Links](#links)). |
| `file name` | Show the file type, based on the contents. |

The fields of `ls -l` are explained in [Week 1 notes: Explanation of ls -l output](weeks/week-01/week-01-notes.md#explanation-of-ls--l-output).

## Wildcards

The shell expands these before the command runs.

| Pattern | Matches |
| --- | --- |
| `*` | Any characters, including none. |
| `?` | Exactly one character. |
| `[abc]` | One character from the set. |
| `[!abc]` | One character not in the set. |
| `[a-z]` | One character in the range. |
| `[[:digit:]]`, `[[:upper:]]`, `[[:alpha:]]` | POSIX character classes. |

## Files and directories

| Command | What it does |
| --- | --- |
| `mkdir dir1 dir2` | Create directories. |
| `mkdir -p a/b/c` | Create nested directories, including missing parents. |
| `cp -i src dst` | Copy, asking before overwriting. |
| `cp -r dir1 dir2` | Copy a directory and its contents. |
| `cp -a src dst` | Copy and keep ownership, permissions and timestamps. |
| `cp -u *.html dir` | Copy only files that are new or newer. |
| `mv old new` | Rename, or move into a directory. |
| `mv -i src dst` | Move, asking before replacing. |
| `rm -i file` | Delete, asking first. |
| `rm -r dir` | Delete a directory and everything in it. |

> **Warning:** there is no recycle bin. Check a wildcard with `ls` before using it with `rm`.

## Links

| Command | What it does |
| --- | --- |
| `ln target name` | Hard link: a second name for the same inode (same data). |
| `ln -s target name` | Symbolic link: a small file that points to `target` by path. |
| `ls -li` | Hard links share an inode number; the link count is the second column. |
| `readlink -f name` | Show where a symbolic link finally points. |

## Finding files

| Command | What it does |
| --- | --- |
| `find . -type f` | All regular files below the current directory (`-type d` for directories). |
| `find . -name '*.md'` | Match by name; quote the pattern so the shell doesn't expand it. |
| `find . -size 1033c` | Exact size in bytes (`c`); `+10M` means larger than 10 MB. |
| `find . ! -executable` | `!` negates the next test. |
| `find . ... -exec cmd {} +` | Run `cmd` on all matches. |

## Tricky file names

| Situation | What works |
| --- | --- |
| File named `-` | `cat ./-` or `cat < -` (plain `cat -` reads standard input). |
| Name with spaces | Quote it: `cat 'my file.txt'`. |
| Name starting with `-` | `cat ./-name` or `cat -- -name`. |
| Hidden file | Starts with `.`; see it with `ls -a`. |

## Viewing files

| Command | What it does |
| --- | --- |
| `cat file` | Print the whole file. |
| `less file` | Page through a file. Space next page, `b` back, `/word` search, `q` quit. |

_Week 2: add `head`, `tail` and `tail -f`._

## Disk usage

_Week 2: add `du` and `df`._

## Pipes and text tools

| Command | What it does |
| --- | --- |
| `cmd \| tr -d ' \n'` | Delete all spaces and newlines from the output. |

_Week 3: add redirection, `grep`, `sort`, `uniq`, `cut`, `sed`, `awk`._

## Editors, tmux and SSH

| Command | What it does |
| --- | --- |
| `ssh -p 2220 user@host` | Connect over SSH on a non-default port. |

_Week 4: add vim, tmux and SSH key basics._

## Users, groups and permissions

| Command | What it does |
| --- | --- |
| `sudo chown -R $USER:$USER dir` | Make me the owner of `dir` and everything in it. |

_Week 5: add `chmod`, `id`, groups and `sudo`._

## Processes and signals

_Week 6._

## systemd and journald

_Week 7._

## Packages and storage

_Week 8._

## Networking

_Week 13._

## Firewalls

_Week 14._

## Bash scripting

_Weeks 15-16._

## Docker

_Weeks 17-20._

## Ansible

_Weeks 23-24._

## WSL and git

| Command | What it does |
| --- | --- |
| `cd ~/src` | Keep repos in the Linux filesystem; `/mnt/c` is much slower. |
| `time cmd` | Measure how long `cmd` takes. |
| `code .` | Open the current folder in VS Code (WSL extension). |
| `gh auth setup-git` | Let git use the GitHub CLI token for HTTPS remotes. |
| `ssh-keygen -t ed25519` | Create an SSH key; add the `.pub` file at github.com/settings/keys. |
| `git config core.hooksPath .githooks` | Enable this repo's pre-push markdownlint hook. |
