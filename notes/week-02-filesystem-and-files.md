[← Week 2 - Filesystem and files](../weeks/week-02-filesystem-and-files.md)

# Week 2 - Filesystem and files: notes

**Status:** Not started

## Exercise 1

> Study the layout: /etc, /var, /usr, /home, /tmp, /proc. Explore each with ls and find.

_Commands, results, what surprised me._

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

## Exercise 2

> Practice find, less, head, tail -f, du, df, ln (hard and symbolic links), tree.

_Commands, results, what surprised me._

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

## Exercise 4

> Bandit levels 6-12.

_Commands, results, what surprised me._

## Deliverable

> A personal cheat sheet (Markdown) in a git repo; add to it all 6 months.

_Write-up._
