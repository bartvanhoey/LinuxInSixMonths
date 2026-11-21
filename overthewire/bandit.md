[← OverTheWire notes](README.md)

# Bandit: notes

Game page: [overthewire.org/wargames/bandit](https://overthewire.org/wargames/bandit/)

Connect with SSH. The level page gives the exact host, port and username. Never write passwords in these notes.

## Level template

Copy this block for each level.

```markdown
### Level N → N+1

**Status:** Not started | In progress | Done

**Goal:** one sentence, in my own words.

**Commands used:** `cmd1`, `cmd2`

**Approach:** what I tried, in order, and what finally worked (no passwords, no pasted answers).

**What I learned:** the Linux concept behind the level.

**Stuck on:** what blocked me, and how I got unstuck (man page, search, hint).
```

## Levels

### Level 0 → 1

**Status:** Done

**Goal:** Log in over SSH and read the password from a file in the home directory.

**Commands used:** `ssh`, `ls`, `cat`

**Approach:** Connected with `ssh -p 2220 bandit0@bandit.labs.overthewire.org`, listed the home directory and read the `readme` file with `cat`.

**What I learned:** SSH on a non-default port (`-p`), and that you land in your home directory after logging in.

**Stuck on:** _…_

### Level 1 → 2

**Status:** Done

**Goal:** Read a file whose name is just `-`.

**Commands used:** `cat`

**Approach:** `cat -` reads from standard input instead of the file, so it just waits. Redirecting the file into `cat` with `cat < -` works, and so does giving a path, `cat ./-`.

**What I learned:** Many commands treat `-` as "standard input". A path prefix like `./` makes the shell and the command see it as a file name.

**Stuck on:** _…_

### Level 2 → 3

**Status:** Done

**Goal:** Read a file whose name contains spaces and starts with `--`.

**Commands used:** `cat`

**Approach:** Unquoted, the shell split the name at the spaces, so it tried to read a file called `--spaces` and failed. Quoting the whole name worked: `cat < '--spaces in this filename--'`.

**What I learned:** The shell splits words at spaces before the command runs. Quotes keep a name together, and a leading `--` would otherwise look like an option (`cat ./--…` or `cat -- '…'` avoids that).

**Stuck on:** _…_

### Level 3 → 4

**Status:** Done

**Goal:** Find and read a hidden file in the `inhere` directory.

**Commands used:** `cd`, `ls -a`, `cat`

**Approach:** A plain `ls` shows nothing. `ls -a` shows files whose names start with a dot, including one starting with `...`, which `cat` can then read.

**What I learned:** Files starting with `.` are hidden by convention, not protected. `ls -a` shows them.

**Stuck on:** _…_

### Level 4 → 5

**Status:** Done

**Goal:** Find the only human-readable file among several files in `inhere`.

**Commands used:** `file`, `cat`

**Approach:** Ran `file` on all files to see their types, then read the one reported as ASCII text.

**What I learned:** `file` looks at the contents, not the extension, to tell text from binary data.

**Stuck on:** _…_

### Level 5 → 6

**Status:** Done

**Goal:** Find the one file in `inhere` that is human-readable, exactly 1033 bytes and not executable.

**Commands used:** `find`, `file`, `cat`, `tr`

**Approach:** `find . -type f -size 1033c ! -executable` narrowed it to one file. Running `file` on the match with `-exec file {} +` confirmed it was text. `cat` printed it, and `tr -d ' \n'` stripped the padding whitespace.

**What I learned:** `find` can filter by type, exact size (`c` = bytes) and permissions, negate a test with `!`, and run a command on each match with `-exec`.

**Stuck on:** _…_
