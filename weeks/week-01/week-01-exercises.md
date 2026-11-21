# Week 01 - Exercises

```bash
# Create a new directory called "week-01" and navigate into it
mkdir week-01
cd week-01

# Create a new file called "exercise.txt" and add some text to it
echo "This is my first exercise file." > exercise.txt

# Display the contents of "exercise.txt"
cat exercise.txt

# Create a new file called "notes.md" and add some markdown content to it
echo "# Week 01 Notes" > notes.md
echo "These are my notes for week 01." >> notes.md

# Display the contents of "notes.md"
cat notes.md

# Create a new directory called "subdir" and navigate into it
mkdir subdir
cd subdir

# Create a new file called "subfile.txt" and add some text to it
echo "This is a file inside the subdirectory." > subfile.txt

# Display the contents of "subfile.txt"
cat subfile.txt

# Navigate back to the "week-01" directory
cd ..

# List all files and directories in "week-01"
ls -l

# Read the content of the file called -
cat < -

# Read the content of the file called --spaces in this filename--
cat < --spaces in this filename--
=> -bash: --spaces: No such file or directory

cat < '--spaces in this filename--'

# Read the content of a hidden file called ...hidden-file
cat < ...hidden-file

# Find all files in the current directory & subdirectories exactly 1033 bytes and not executable
find . -type f -size 1033c ! -executable

# Find all files in the current directory & subdirectories exactly 1033 bytes and not executable and human readable file type
find . -type f -size 1033c ! -executable -exec file {} +

# Find all files in the current directory & subdirectories exactly 1033 bytes and not executable and human readable file type and display their contents
find . -type f -size 1033c ! -executable -exec cat {} +

# Find all files in the current directory & subdirectories exactly 1033 bytes and not executable and human readable file type and display their contents without newlines
find . -type f -size 1033c ! -executable -exec cat {} + | tr -d ' \n';  echo





```
