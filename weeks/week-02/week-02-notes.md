[← Week 2 - Filesystem and files](week-02-filesystem-and-files.md)

# Week 2 - Filesystem and files: notes

**Status:** In progress

The first directory in the filesystem is called the `root directory` and contains files and subdirectories

_what is the difference /$ and ~$ ?_

In the bash prompt, the part after the colon shows your current directory:

- your-username@hostname:/$ — you're in /, the root of the filesystem (the top-level directory that contains everything else). The $ means a normal (non-root) user.
- your-username@hostname:~$ — you're in ~, your home directory, which is /home/your-username.

_Some Useful Shortcuts_

Useful ways to quickly change the current working directory:

| Shortcut | Description |
| --- | --- |
| `cd` | Changes the working directory to your home directory. |
| `cd -` | Changes the working directory to the previous working directory. |
| `cd ~user_name` | Changes the working directory to the home directory of `user_name`. For example, `cd ~bob` changes to the home directory of user `bob`. |

## Exercise 1

> Study the layout: /etc, /var, /usr, /home, /tmp, /proc. Explore each with ls and find.

_Commands, results, what surprised me._

**Checklist** (session 1, about 1.5 h; see the [plan](week-02-plan.md#session-1-layout-tour-exercise-1)):

- [x] Write the directory table (below).
- [x] `/etc`: `ls /etc | head -30`, `cat /etc/os-release`, `cat /etc/hostname`, `head /etc/passwd`; count configs with `find /etc -name "*.conf" 2>/dev/null | wc -l`.
- [ ] `/var`: `ls /var/log`, `du -sh /var/* 2>/dev/null`; find out what grows here and what `/var/lib` holds.
- [ ] `/usr`: `ls /usr/bin | wc -l`, `ls -l /bin /lib /sbin` (confirm the symlinks), `which dotnet git`.
- [ ] `/home` and `/tmp`: `ls -la ~` (dotfiles such as `.bashrc`, `.profile`, `.ssh`), `ls -ld /tmp` (sticky bit).
- [ ] `/proc`: `head /proc/cpuinfo`, `head /proc/meminfo`, `cat /proc/$$/cmdline`, `ls -l /proc/$$/fd`.
- [ ] WSL: `ls /mnt/c`, `findmnt /mnt/c`, `mount | head`; compare with the week 1 filesystem performance notes.
- [ ] Write down three surprises and one useful `/proc` example.

### Directories of Linux Systems

| Directory | Contents |
| --- | --- |
| `/` | Root directory; the top of the filesystem hierarchy. |
| `/usr/bin` | Essential command binaries. |
| `/boot` | Boot loader and kernel files. |
| `/bin` | Essential command binaries; symlink to `/usr/bin` on modern systems. |
| `/dev` | Device nodes (disks, terminals, `/dev/null`). |
| `/etc` | System-wide configuration files. |
| `/home` | Home directories of regular users. |
| `/lib`, `/lib64` | Shared libraries; symlinks to `/usr/lib` and `/usr/lib64`. |
| `/lost+found` | Files recovered after a filesystem check. |
| `/media` | Mount points for removable media. |
| `/mnt` | Temporary mount points; WSL mounts Windows drives here (`/mnt/c`). |
| `/opt` | Optional third-party software. |
| `/proc` | Virtual filesystem exposing processes and kernel info. |
| `/root` | Home directory of the root user. |
| `/run` | Runtime data since last boot (PID files, sockets). |
| `/sbin` | System administration binaries; symlink to `/usr/sbin`. |
| `/snap` | Installed snap packages. |
| `/srv` | Data served by this system (web, FTP). |
| `/sys` | Virtual filesystem exposing devices and drivers. |
| `/tmp` | Temporary files, world-writable (sticky bit, `drwxrwxrwt`). |
| `/usr` | Installed programs, libraries and shared data. |
| `/var` | Variable data: logs, caches, spool files. |

### `/var/log`: system and application logs

| Path | Contents |
| --- | --- |
| `syslog` | General system messages; the first place to look. |
| `auth.log` | Logins, `sudo` use, SSH attempts, authentication failures. |
| `kern.log`, `dmesg` | Kernel messages and hardware/driver output from boot. |
| `boot.log` | Output of services at boot time. |
| `apt/` | `history.log` (what was installed or removed) and `term.log`. |
| `dpkg.log` | Low-level package install, upgrade and remove records. |
| `unattended-upgrades/` | Automatic security update logs. |
| `journal/` | The systemd journal (binary); read with `journalctl`. |
| `wtmp`, `btmp`, `lastlog` | Binary login records; read with `last`, `sudo lastb` and `lastlog`. |
| `apache2/`, `nginx/`, `postgresql/` | Appear only when those packages are installed. |
| `*.1`, `*.2.gz` | Older logs rotated by `logrotate`; read `.gz` files with `zcat` or `zless`. |

```bash
ls -lh /var/log                 # list the files with sizes
sudo tail -f /var/log/syslog    # follow a log live
sudo less /var/log/auth.log     # page through a log
journalctl -b                   # logs from the current boot
journalctl -u ssh               # logs for one service
sudo du -sh /var/log/*          # what is using space
```

- Many logs are readable only by root or the `adm` group, so use `sudo`.
- Modern Ubuntu keeps much of its data in the systemd journal, so `journalctl` often shows more than the text files.
- On WSL2 there is no full systemd boot by default, so some files (such as `auth.log` or `kern.log`) may be missing or sparse.

## Exercise 2

> Practice find, less, head, tail -f, du, df, ln (hard and symbolic links), tree.

_Commands, results, what surprised me._

**Checklist** (sessions 2 and 3, about 2.5 h; see the [plan](week-02-plan.md#session-2-viewing-size-and-links-exercise-2-part-1)):

Viewing files:

- [ ] `less`: `/pattern`, `n`/`N`, `g`/`G`, `F` (follow), `q`.
- [ ] `head -n 5`, `tail -n 5`, `wc -l`, `file`.

Disk usage and `tree`:

- [ ] `df -h` and `df -hT`; note the 9p/drvfs mounts for Windows drives.
- [ ] `du -sh`, `du -h --max-depth=1 | sort -h`, `du -ah | sort -rh | head`.
- [ ] Explain why `df` (per filesystem) and `du` (per directory tree) can disagree.
- [ ] `sudo apt install tree`; try `tree -L 2`, `tree -a -I node_modules`, `tree -d`.

Links lab in `/tmp/links-lab`:

- [ ] Create `a.txt`, `ln a.txt hard.txt`, `ln -s a.txt soft.txt`.
- [ ] `ls -li`: compare inode numbers (hard link shares one, symlink does not).
- [ ] Delete `a.txt`: hard link still works, symlink is dangling; find it with `find . -xtype l`.
- [ ] `stat` each file and watch the link count.
- [ ] Try a hard link to a directory and across filesystems; note both errors and why symlinks have no such limits.
- [ ] Real-world symlinks: `ls -l /etc/alternatives`, `readlink -f $(which dotnet)`.

`find` in depth (do it under `~`, not `/mnt/c`):

- [ ] Name and type: `-name`, `-iname`, `-type f|d|l`.
- [ ] Size and time: `-size +10M`, `-mtime -1`, `-mmin -30`.
- [ ] Owner and permissions: `-user`, `-perm`.
- [ ] Actions: `-exec cmd {} \;` vs `-exec cmd {} +`; run without `-delete` first.
- [ ] Pruning: `find . -name bin -prune -o -name "*.cs" -print`.
- [ ] Hide errors with `2>/dev/null` and note why it goes at the end.
- [ ] Challenge: the 10 largest files under `~/src`.
- [ ] Challenge: `.csproj` files modified this week.
- [ ] Challenge: all empty directories in a repo.
- [ ] Compare `find` with `locate` (`sudo updatedb`) and `fd`.

### Example of a symbolic link

```bash
lrwxrwxrwx 1 root root 11 2025-08-11 07:34 libc.so.6 -> libc-2.6.so
```

A symbolic link (symlink) is a special file that points to another file or directory by path, so opening the link opens the target.

| Part | Meaning |
| --- | --- |
| `l` | File type: symbolic link |
| `rwxrwxrwx` | Permissions of a symlink are always shown as `777`; the target's permissions apply |
| `libc.so.6` | Name of the link |
| `->` | Points to |
| `libc-2.6.so` | Target file the link resolves to |

Programs ask for `libc.so.6`; the system can swap the real library (`libc-2.6.so`) for a newer one by repointing the link.

## Exercise 3

> Run a .NET app and follow its console output with tail -f while sending requests.

_Commands, results, what surprised me._

**Checklist** (session 4, about 1 h; see the [plan](week-02-plan.md#session-4-net-app-with-tail--f-exercise-3)):

- [ ] Create or reuse an app: `dotnet new webapi -o ~/src/tail-demo`.
- [ ] Terminal 1: `dotnet run > /tmp/app.log 2>&1`.
- [ ] Terminal 2: `tail -f /tmp/app.log`.
- [ ] Terminal 3: `curl -i localhost:<port>/weatherforecast`, plus a request to a missing route (404).
- [ ] Try `tail -n 50 -f`, `tail -F`, `grep --line-buffered` on the live stream, and `less +F`.
- [ ] Add `logger.LogInformation(...)` in code and find the line in the output.
- [ ] Note why stdout and stderr both need redirecting.
- [ ] Note what `tail -f` does when the file is deleted or truncated.
- [ ] Bonus: `ls -l /proc/<pid>/fd` for the dotnet process.

## Exercise 4

> Bandit levels 6-12.

_Commands, results, what surprised me._

**Checklist** (session 5; see the [plan](week-02-plan.md#session-5-bandit-6-12-and-wrap-up-exercise-4--deliverable)). Detailed notes go in [bandit.md](../../overthewire/bandit.md): approach and concept only, never passwords.

- [ ] Level 6 → 7
- [ ] Level 7 → 8
- [ ] Level 8 → 9
- [ ] Level 9 → 10
- [ ] Level 10 → 11
- [ ] Level 11 → 12
- [ ] Level 12 → 13
- [ ] Time-box each level to 30 minutes before looking for hints.
- [ ] Update the progress table in [overthewire/README.md](../../overthewire/README.md).

## Deliverable

> A personal cheat sheet (Markdown) in a git repo; add to it all 6 months.

Started: [cheatsheet.md](../../cheatsheet.md). It is seeded with the commands from Week 1 and has a section ready for each later topic.

_Add the Week 2 commands (`head`, `tail -f`, `du`, `df`, `ln`, `tree`) as I practise them._

**Checklist:**

- [ ] Fill the Viewing files, Disk usage, Links, Finding files and Tricky file names sections of the cheatsheet.
- [ ] Add one "what I'd forget" example per command; only commands I have run.

## Wrap-up

- [ ] Answer the [self-check](week-02-plan.md#self-check-before-moving-on) questions.
- [ ] Tick the exercises and the **Done** box in [week-02-filesystem-and-files.md](week-02-filesystem-and-files.md).
- [ ] Set **Status** at the top of this file to Done.
- [ ] Run the markdownlint check (see the README).
- [ ] Commit on a branch like `docs/week-02-...` and open a PR.
