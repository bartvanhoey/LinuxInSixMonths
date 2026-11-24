[← Week 2 - Filesystem and files](week-02-filesystem-and-files.md) | [My notes](week-02-notes.md)

# Week 2 - Filesystem and files: detailed plan

**Goal:** Move around confidently and know where things live.

**Budget:** 6-8 hours in 5 sessions. The checklists to tick off live in [week-02-notes.md](week-02-notes.md); this file is the reference behind them.

## Overview

| Session | Focus | Time | Exercise |
| --- | --- | --- | --- |
| 1 | Layout tour with `ls` and `find` | 1.5 h | 1 |
| 2 | Viewing files, disk usage, links, `tree` | 1.5 h | 2 |
| 3 | `find` in depth | 1 h | 2 |
| 4 | .NET app plus `tail -f` | 1 h | 3 |
| 5 (weekend) | Bandit 6-12, then cheatsheet and wrap-up | 2-3 h | 4 + deliverable |

## Session 1: Layout tour (Exercise 1)

The directory table lists what each directory is for. The exercise asks you to explore them, so verify each one with evidence.

- **`/etc`:** `ls /etc | head -30`, `cat /etc/os-release`, `cat /etc/hostname`, `head /etc/passwd`. These are plain-text configs. Run `find /etc -name "*.conf" 2>/dev/null | wc -l`.
- **`/var`:** `ls /var/log`, `du -sh /var/* 2>/dev/null`. Find out what grows here (logs, apt cache) and what is in `/var/lib`.
- **`/usr`:** `ls /usr/bin | wc -l`, `ls -l /bin /lib /sbin` (confirm the symlinks), `which dotnet git` and see where each lives.
- **`/home`, `/tmp`:** `ls -la ~` for hidden dotfiles (`.bashrc`, `.profile`, `.ssh`). Run `ls -ld /tmp` to confirm the sticky bit.
- **`/proc`:** `head /proc/cpuinfo`, `head /proc/meminfo`, `cat /proc/$$/cmdline`, `ls -l /proc/$$/fd`. `$$` is the PID of your shell, so this shows a live process as files.
- **WSL-specific:** `ls /mnt/c`, `findmnt /mnt/c`, `mount | head`. Compare with the week 1 notes on Linux vs Windows filesystem performance.

**Write down:** three things that surprised you, plus a `/proc` example you found useful.

## Session 2: Viewing, size and links (Exercise 2, part 1)

- **Viewing:**
  - `less`: `/pattern` to search, `n` and `N` to move between matches, `g` and `G` to jump to start and end, `q` to quit, `F` to follow like `tail -f`.
  - `head -n 5` and `tail -n 5`.
  - `cat`, `wc -l`, `file`.
- **Disk usage:**
  - `df -h` and `df -hT` (note the 9p/drvfs mounts for Windows drives).
  - `du -sh dir`, `du -h --max-depth=1 | sort -h`, and `du -ah | sort -rh | head`.
  - Know the difference: `df` is per filesystem, `du` is per directory tree.
- **`tree`:** `sudo apt install tree`, then `tree -L 2`, `tree -a -I node_modules`, `tree -d`.
- **Links lab (in `/tmp/links-lab`):**
  1. Create `a.txt`, then `ln a.txt hard.txt` and `ln -s a.txt soft.txt`.
  2. Run `ls -li` and compare inode numbers. The hard link shares the inode and the symlink does not.
  3. Delete `a.txt`. The hard link still works, and `soft.txt` is now broken. Find the dangling link with `find . -xtype l`.
  4. Run `stat` on each, and watch the link count change.
  5. Try a hard link to a directory and across filesystems. Note both errors, and why symlinks don't have those limits.
- **Real-world symlink:** `ls -l /etc/alternatives` and `readlink -f $(which dotnet)`.

## Session 3: `find` in depth (Exercise 2, part 2)

Practice these patterns, each on a real directory:

- **By name and type:** `find . -name "*.cs"`, `-iname`, `-type f|d|l`.
- **By size and time:** `-size +10M`, `-mtime -1`, `-mmin -30`.
- **By owner and permissions:** `-user`, `-perm`. This is the groundwork for Bandit.
- **Actions:** `-exec cmd {} \;` vs `-exec cmd {} +`, and `-delete`. Run it without `-delete` first.
- **Pruning:** `find . -name bin -prune -o -name "*.cs" -print`.
- **Errors:** `2>/dev/null` to hide permission-denied noise. Learn why it goes at the end.
- **Mini challenges:**
  - Largest 10 files under `~/src`.
  - All `.csproj` files modified this week.
  - All empty directories in a repo.
  - Compare `find` with `locate` (`sudo updatedb`) and `fd` if installed.

## Session 4: .NET app with `tail -f` (Exercise 3)

1. Create `dotnet new webapi -o ~/src/tail-demo` (or reuse an existing app).
2. In one terminal, write the console output to a file: `dotnet run > /tmp/app.log 2>&1`.
3. In a second terminal, run `tail -f /tmp/app.log`.
4. In a third terminal, send requests with `curl -i localhost:<port>/weatherforecast`. Also send a request to a non-existent route for a 404.
5. Experiment:
   - `tail -n 50 -f`
   - `tail -F` (survives log rotation)
   - `grep --line-buffered` on a live stream
   - `less +F`
6. Add logging in code (`logger.LogInformation(...)`) and find it in the output.
7. Bonus: `ls -l /proc/<pid>/fd` for the dotnet process to see the open log file and sockets.

**Notes to capture:** why stdout and stderr both need redirecting, and what happens to `tail -f` when the file is deleted or truncated.

## Session 5: Bandit 6-12 and wrap-up (Exercise 4 + deliverable)

**Bandit 6-12** are almost all `find`, `grep`, `sort`, `uniq`, `strings`, `base64`, `tr` and `xxd`. Per [bandit.md](../../overthewire/bandit.md):

- Use the level template. Write the approach and the concept learned, but never passwords or solutions.
- Try each level with `man` and your own notes first. Time-box each level to 30 minutes before looking for hints.
- Expect these themes: `find` by size/owner/group (6-7), `grep` and `sort | uniq -u` (8-9), `strings` (9), `base64 -d` (10), `tr` rot13 (11), and repeated decompression with `xxd -r` (12).
- Update the progress table in [overthewire/README.md](../../overthewire/README.md).

**Cheatsheet (deliverable):** add to [cheatsheet.md](../../cheatsheet.md) the Viewing files, Disk usage, Links, Finding files and Tricky file names sections. Include one-line "what I'd forget" examples. Only commands you have actually run.

**Wrap-up:**

- Tick the exercises and the **Done** box in [week-02-filesystem-and-files.md](week-02-filesystem-and-files.md).
- Set the status in [week-02-notes.md](week-02-notes.md) to **Done**.
- Run the markdownlint check before pushing (see the README).
- Commit via a branch like `docs/week-02-...`, matching the week 1 PR flow.

## Self-check before moving on

- [ ] Can you explain the difference between a hard link and a symlink, and what `rm` does to each?
- [ ] Can you tell where config, logs, binaries and user data live without looking?
- [ ] Can you find "files over 100 MB modified in the last week" in one command?
- [ ] Can you tell why `df` and `du` can disagree?
- [ ] Can you follow a live log and filter it?

## Risks and tips

- **WSL-specific:** `/mnt/c` is slow and has odd permissions, so do `find` and `du` exercises under `~`, not there.
- **Safety:** never run `find / -delete` or `-exec rm`. Dry run first, and stay in `/tmp` or `~/src`.
- **Bandit 6+** requires reading man pages carefully, and that is the real skill being trained.
