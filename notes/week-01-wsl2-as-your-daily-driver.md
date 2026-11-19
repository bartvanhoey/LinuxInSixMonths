[← Week 1 - WSL2 as your daily driver](../weeks/week-01-wsl2-as-your-daily-driver.md)

# Week 1 - WSL2 as your daily driver: notes

**Status:** Not started

## Exercise 1

> Install WSL2 with Ubuntu LTS and Windows Terminal; install git and the .NET SDK inside WSL.

_Commands, results, what surprised me._

## Exercise 2

> Keep repos in the Linux filesystem (~/src), not under /mnt/c. Notice the speed difference.

_Commands, results, what surprised me._

## Exercise 3

> Do one full working day of git, dotnet build and dotnet test only from the WSL shell.

_Commands, results, what surprised me._

## Exercise 4

> Read The Linux Command Line (Shotts) part 1; start OverTheWire Bandit, levels 0-5.

### 2 - NAVIGATION

```bash
# Navigate the filesystem
pwd # print working directory
ls # list all files in the current directory
cd ~/src # change directory to ~/src
cd /usr/bin # change directory to /usr/bin
cd .. # go up one directory level
cd / # change directory to the root directory
cd - # go back to the previous directory
cd -user_name # change directory to the home directory of user_name
cd ~ # change directory to the home directory of the current user
clear # clear the terminal screen
```

### 3 - EXPLORING THE SYSTEM

```bash
# List files and directories
ls /etc # list all files in the /etc directory
ls ~ /usr # list all files in the /usr directory
ls -l # list all files with detailed information
ls -a # list all files, including hidden files
ls -lh # list all files with detailed information in human-readable format
ls -lt # list all files with detailed information sorted by modification time
ls -A # list all files, including hidden files, but excluding . and ..
ls -d */ # list only directories in the current directory
ls -R # list all files and directories recursively
ls -S # list all files with detailed information sorted by file size
ls -F # list all files with a trailing / for directories and * for executables
file filename # display the description of the file type for filename
less filename # view the contents of filename one page at a time

```

#### Explanation of ls -l output

![ls -l output: long file list with detailed information](../images/week-01/long_file_list_detailed_information.png)

```bash
-rwxrwxrwx   1 root root 2260248 Nov  9  2024 init
```

The line above can be broken down as follows:

| Field | Meaning |
| --- | --- |
| `-` | File type: `-` regular file, `d` directory, `l` symbolic link |
| `rwx` | Owner permissions: read, write, execute |
| `rwx` | Group permissions: read, write, execute |
| `rwx` | Permissions for everyone else: read, write, execute |
| `1` | Number of hard links |
| `root` | Owner of the file |
| `root` | Group of the file |
| `2260248` | Size in bytes (about 2.2 MB) |
| `Nov  9  2024` | Last modification date |
| `init` | File name |

`rwxrwxrwx` is mode `777`: anyone can read, write and execute the file.

_Moved to [Week 2, Exercise 1](week-02-filesystem-and-files.md#exercise-1): Directories of Linux Systems._

_Moved to [Week 2, Exercise 2](week-02-filesystem-and-files.md#exercise-2): Example of a symbolic link._

### 4 Manipulating Files and Directories

```bash

mkdir # Create directories
cp # Copy files and directories
mv # Move or rename files and directories
rm # Remove files and directories
ln # Create hard and symbolic links
mv # Move or rename files and directories

cp -u *.html destination # Copy all .html files to destination if they are newer than the existing ones

```

#### Wildcards

The shell expands wildcards (globs) into matching file names before the command runs.

| Wildcard | Matches |
| --- | --- |
| `*` | Any characters, including none: `*.html`, `report*` |
| `?` | Exactly one character: `file?.txt` matches `file1.txt`, not `file10.txt` |
| `[abc]` | Any one character from the set: `file[123].txt` |
| `[!abc]` | Any one character not in the set: `file[!0-9].txt` |
| `[a-z]` | Any one character in the range: `[A-Z]*` matches names starting with an uppercase letter |
| `[[:alpha:]]` | Any alphabetic character (POSIX class) |
| `[[:digit:]]` | Any digit |
| `[[:upper:]]` | Any uppercase letter |
| `[[:lower:]]` | Any lowercase letter |
| `[[:alnum:]]` | Any letter or digit |

```bash
ls *.txt # all files ending in .txt
ls ???  # all files with exactly three characters
ls [[:upper:]]* # all files starting with an uppercase letter
ls *[![:digit:]] # all files not ending with a digit
```

#### mkdir - Create directories

```bash
    mkdir dir1
    mkdir dir1 dir2 dir3 # Create multiple directories at once
    mkdir dir1/dir2/dir3 # Create nested directories
```

#### cp - Copy files and directories

```bash
    cp input.txt output.txt # Copy input.txt to output.txt
    cp item... directory # Copy multiple items to a directory
```

Common `cp` options:

| Option | Long option | Meaning |
| --- | --- | --- |
| `-a` | `--archive` | Copy files and directories with all attributes, including ownership and permissions. Normally copies take the default attributes of the user doing the copy. |
| `-i` | `--interactive` | Prompt before overwriting an existing file. Without it, `cp` overwrites silently. |
| `-r` | `--recursive` | Copy directories and their contents. Required for directories (or use `-a`). |
| `-u` | `--update` | Copy only files that do not exist in the destination or are newer than the existing ones. Skips files that need no copy. |
| `-v` | `--verbose` | Show informative messages while copying. |

Common `cp` applications:

| Command | Result |
| --- | --- |
| `cp file1 file2` | Copy `file1` to `file2`. If `file2` exists, it is overwritten; otherwise it is created. |
| `cp -i file1 file2` | Same, but prompt before overwriting `file2`. |
| `cp file1 file2 dir1` | Copy `file1` and `file2` into `dir1`. `dir1` must already exist. |
| `cp dir1/* dir2` | Copy all files in `dir1` into `dir2` (wildcard). `dir2` must already exist. |
| `cp -r dir1 dir2` | If `dir2` does not exist, create it with the contents of `dir1`. If `dir2` exists, copy `dir1` (and its contents) into `dir2`. |

#### mv - Move or rename files and directories

```bash
    mv input.txt output.txt # Move input.txt to output.txt
    mv item... directory # Move multiple items to a directory
```

Common `mv` options (same meaning as for `cp`):

| Option | Long option | Meaning |
| --- | --- | --- |
| `-i` | `--interactive` | Prompt before overwriting an existing file. Without it, `mv` overwrites silently. |
| `-u` | `--update` | Move only files that do not exist in the destination or are newer than the existing ones. |
| `-v` | `--verbose` | Show informative messages while moving. |

Common `mv` applications:

| Command | Result |
| --- | --- |
| `mv file1 file2` | Move (rename) `file1` to `file2`. If `file2` exists, it is replaced. Either way, `file1` no longer exists. |
| `mv -i file1 file2` | Same, but prompt before replacing `file2`. |
| `mv file1 file2 dir1` | Move `file1` and `file2` into `dir1`. `dir1` must already exist. |
| `mv dir1 dir2` | If `dir2` does not exist, rename `dir1` to `dir2`. If `dir2` exists, move `dir1` (and its contents) into `dir2`. |

#### rm - Remove files and directories

```bash
    rm file1 file2 # Remove files
    rm -r dir1 # Remove a directory and its contents recursively
```

Common `rm` options:

| Option | Long option | Meaning |
| --- | --- | --- |
| `-i` | `--interactive` | Prompt before deleting a file. Without it, `rm` deletes silently. |
| `-r` | `--recursive` | Delete directories and everything inside them, including subdirectories. Required to delete a directory. |
| `-f` | `--force` | Ignore nonexistent files and never prompt. Overrides `--interactive`. |
| `-v` | `--verbose` | Show informative messages while deleting. |

Common `rm` applications:

| Command | Result |
| --- | --- |
| `rm file1` | Delete `file1` silently. |
| `rm -i file1` | Same, but prompt for confirmation first. |
| `rm -r file1 dir1` | Delete `file1`, and delete `dir1` with all its contents. |

> **Warning:** Linux has no recycle bin. `rm` deletes permanently, and `rm -rf` does so without asking. Check the wildcard or path with `ls` first.

## Deliverable

> A working WSL dev setup and a short note of what felt different from Windows.

_Write-up._
