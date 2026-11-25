# Week 02 - Exercises

## Exercise 1: layout tour (`/etc`)

```bash
# List the first 10 entries in the /etc directory.
ls /etc | head -10

# head /etc/passwd displays the first 10 lines of /etc/passwd, a system file containing basic account information, such as usernames and assigned user IDs.
head /etc/passwd

# `head -n 20 /etc/passwd` would instead show the first 20 lines.
head -n 20 /etc/passwd

# Count all .conf files under /etc, ignoring permission errors
find /etc -name "*.conf" 2>/dev/null | wc -l

# Count all .conf files under /etc, ignoring permission errors, and show the first 10 matches
find /etc -name "*.conf" 2>/dev/null | head -n 10
```

## Exercise 2: tricky file names

```bash
# Read the content of the file called -
cat < -

# Read the content of the file called --spaces in this filename--
cat < --spaces in this filename--
# => -bash: --spaces: No such file or directory
# Without quotes the shell splits the name at the spaces, so it redirects from "--spaces" only.

cat < '--spaces in this filename--'

# Read the content of a hidden file called ...hidden-file
cat < ...hidden-file
```

## Exercise 2: `find` in depth

```bash
# Find all files in the current directory & subdirectories exactly 1033 bytes and not executable
find . -type f -size 1033c ! -executable

# Same search, and show the file type of each match (to check which one is human-readable text)
find . -type f -size 1033c ! -executable -exec file {} +

# Same search, and display the contents of each match
find . -type f -size 1033c ! -executable -exec cat {} +

# Same search, display the contents with all spaces and newlines removed, then end with one newline
find . -type f -size 1033c ! -executable -exec cat {} + | tr -d ' \n';  echo
```

## Exercise 2: `ls` in depth

```bash
# List all files in the current directory, including hidden files, and show their inode numbers
ls -lai

# List all files in the current directory, including hidden files, and show their inode numbers, sorted by inode number
ls -lai | sort -n

# List both the user's home directory (symbolized by the “~” character) and the /usr directory:
ls ~ /usr

# List entries in the current directory as a detailed, time-sorted list, from oldest to newest.
ls -lt --reverse

# Counts the number of entries displayed in /usr/bin.
ls /usr/bin | wc -l

# Display details in long format for entries in three system directories
ls -l /bin /lib /sbin

# List all files in the user's home directory, including hidden files, in long format
ls -la ~

# List the details of the /tmp directory itself, not its contents
ls -ld /tmp

```

## Exercise 2: `du` (disk usage) in depth

```bash
# du stands for disk usage. It reports how much disk space files and directories take up, and it walks the directory tree recursively.

# Show the disk usage of ~/src directory inside the user's home directory, in human-readable format (e.g., KB, MB, GB):
du -sh ~/src

# sizes of everything in /var, hiding permission errors
du -sh /var/* 2>/dev/null

# 10 biggest files and directories under here
du -ah | sort -rh | head

```
