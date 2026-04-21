# Bash Guide

A complete reference for working in the Bash shell and writing shell scripts.

---

## Table of Contents

1. [What is Bash?](#what-is-bash)
2. [Navigation](#navigation)
3. [Files and Directories](#files-and-directories)
4. [Viewing and Editing Files](#viewing-and-editing-files)
5. [Permissions](#permissions)
6. [Searching](#searching)
7. [Pipes and Redirection](#pipes-and-redirection)
8. [Variables](#variables)
9. [Special Variables](#special-variables)
10. [Conditionals](#conditionals)
11. [Loops](#loops)
12. [Functions](#functions)
13. [Script Basics](#script-basics)
14. [Process Management](#process-management)
15. [Useful Shortcuts](#useful-shortcuts)
16. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## What is Bash?

Bash (Bourne Again SHell) is the default command-line shell on most Linux systems and macOS. It lets you interact with the operating system by typing commands, and can also run scripts — text files containing a sequence of commands.

---

## Navigation

```bash
pwd                  # Print working directory (where you are)
ls                   # List files in current directory
ls -l                # Long format (permissions, size, date)
ls -a                # Include hidden files (starting with .)
ls -lh               # Long format with human-readable sizes
ls -lt               # Sort by modification time

cd /path/to/dir      # Change to an absolute path
cd ..                # Go up one directory
cd ~                 # Go to your home directory
cd -                 # Go back to the previous directory
```

---

## Files and Directories

### Creating

```bash
touch file.txt              # Create an empty file (or update timestamp)
mkdir my-folder             # Create a directory
mkdir -p a/b/c              # Create nested directories in one step
```

### Copying

```bash
cp file.txt copy.txt        # Copy a file
cp -r folder/ backup/       # Copy a directory recursively
```

### Moving and Renaming

```bash
mv file.txt new-name.txt    # Rename a file
mv file.txt ~/Documents/    # Move a file to another directory
mv folder/ ~/Documents/     # Move a directory
```

### Deleting

```bash
rm file.txt                 # Delete a file
rm -r folder/               # Delete a directory and its contents
rm -rf folder/              # Force delete without prompting (use with care)
rmdir folder/               # Delete an empty directory
```

> **Warning:** There is no recycle bin in the terminal. Deleted files are gone immediately.

### Linking

```bash
ln -s /path/to/original shortcut   # Create a symbolic link (shortcut)
```

---

## Viewing and Editing Files

```bash
cat file.txt                # Print entire file to the terminal
less file.txt               # Scroll through a file (q to quit)
head file.txt               # First 10 lines
head -n 20 file.txt         # First 20 lines
tail file.txt               # Last 10 lines
tail -n 20 file.txt         # Last 20 lines
tail -f log.txt             # Follow a file in real time (useful for logs)
wc -l file.txt              # Count lines
wc -w file.txt              # Count words
```

### Editing

```bash
nano file.txt               # Simple terminal editor (Ctrl+O save, Ctrl+X exit)
vim file.txt                # Vim editor (i to insert, Esc then :wq to save and quit)
```

---

## Permissions

Every file has three permission sets: **owner**, **group**, **others**.

```bash
ls -l file.txt
# -rwxr-xr-- 1 alice staff 1024 Apr 8 10:00 file.txt
#  ↑ owner  ↑ group  ↑ others
```

Each set contains read (`r`=4), write (`w`=2), execute (`x`=1).

```bash
chmod 755 script.sh     # rwxr-xr-x (owner: all, group/others: read+execute)
chmod 644 file.txt      # rw-r--r-- (owner: read+write, group/others: read only)
chmod +x script.sh      # Add execute permission for all
chmod -w file.txt       # Remove write permission for all

chown alice file.txt    # Change file owner
chown alice:staff file.txt  # Change owner and group
```

---

## Searching

### Find Files

```bash
find . -name "*.txt"                  # Find all .txt files from current directory
find /home -name "config.json"        # Search in a specific path
find . -type d -name "node_modules"   # Find directories by name
find . -mtime -7                      # Modified in the last 7 days
find . -size +10M                     # Files larger than 10MB
```

### Search Inside Files

```bash
grep "error" log.txt                  # Lines containing "error"
grep -i "error" log.txt               # Case-insensitive
grep -r "TODO" ./src                  # Recursive search in a directory
grep -n "error" log.txt               # Show line numbers
grep -l "error" *.log                 # Only print filenames that match
grep -v "debug" log.txt               # Lines that do NOT match
grep -c "error" log.txt               # Count matching lines
```

---

## Pipes and Redirection

### Pipes

Send the output of one command as input to another with `|`:

```bash
ls -l | grep ".txt"          # List files, then filter for .txt
cat log.txt | wc -l          # Count lines in a file
cat log.txt | sort | uniq    # Sort lines and remove duplicates
ps aux | grep python         # Find running python processes
```

### Redirection

```bash
echo "hello" > file.txt      # Write to file (overwrite)
echo "world" >> file.txt     # Append to file
cat < file.txt               # Read from file as input
command 2> errors.txt        # Redirect stderr to a file
command > out.txt 2>&1       # Redirect both stdout and stderr
command > /dev/null          # Discard output entirely
```

---

## Variables

```bash
name="Alice"                 # Assign (no spaces around =)
echo $name                   # Use a variable
echo "Hello, $name"          # In double quotes, variables expand
echo 'Hello, $name'          # In single quotes, no expansion

readonly PI=3.14             # Constant — cannot be reassigned
unset name                   # Delete a variable
```

### Command Substitution

Capture the output of a command into a variable:

```bash
today=$(date +%Y-%m-%d)
echo "Today is $today"

file_count=$(ls | wc -l)
echo "There are $file_count files"
```

### Arithmetic

```bash
result=$((5 + 3))
echo $result            # 8

x=10
((x += 5))
echo $x                 # 15
```

### String Operations

```bash
str="Hello, World"
echo ${#str}            # Length: 12
echo ${str:7}           # Substring from index 7: "World"
echo ${str:7:5}         # Substring from 7, length 5: "World"
echo ${str/World/Bash}  # Replace first match: "Hello, Bash"
echo ${str,,}           # Lowercase: "hello, world"
echo ${str^^}           # Uppercase: "HELLO, WORLD"
```

---

## Special Variables

| Variable | Meaning |
|----------|---------|
| `$0` | Name of the script |
| `$1`, `$2`, … | Positional arguments passed to the script |
| `$#` | Number of arguments |
| `$@` | All arguments as separate words |
| `$*` | All arguments as one word |
| `$?` | Exit code of the last command (0 = success) |
| `$$` | PID of the current shell |
| `$!` | PID of the last background process |
| `$HOME` | Home directory |
| `$PATH` | Directories searched for commands |
| `$PWD` | Current working directory |

---

## Conditionals

### If / Elif / Else

```bash
if [ condition ]; then
  echo "true"
elif [ other_condition ]; then
  echo "other"
else
  echo "false"
fi
```

### Common Test Conditions

```bash
# Strings
[ "$a" = "$b" ]     # Equal
[ "$a" != "$b" ]    # Not equal
[ -z "$a" ]         # Empty string
[ -n "$a" ]         # Non-empty string

# Numbers
[ "$a" -eq "$b" ]   # Equal
[ "$a" -ne "$b" ]   # Not equal
[ "$a" -lt "$b" ]   # Less than
[ "$a" -gt "$b" ]   # Greater than
[ "$a" -le "$b" ]   # Less than or equal
[ "$a" -ge "$b" ]   # Greater than or equal

# Files
[ -f "$file" ]      # Exists and is a regular file
[ -d "$dir" ]       # Exists and is a directory
[ -e "$path" ]      # Exists (file or directory)
[ -r "$file" ]      # Readable
[ -w "$file" ]      # Writable
[ -x "$file" ]      # Executable
```

> Use `[[ ]]` (double brackets) for safer, more powerful comparisons in Bash — supports `&&`, `||`, pattern matching, and doesn't require quoting variables.

```bash
if [[ "$name" == "Alice" && "$age" -ge 18 ]]; then
  echo "Adult named Alice"
fi
```

### Case Statement

```bash
case "$day" in
  Monday|Tuesday|Wednesday|Thursday|Friday)
    echo "Weekday"
    ;;
  Saturday|Sunday)
    echo "Weekend"
    ;;
  *)
    echo "Unknown"
    ;;
esac
```

---

## Loops

### For Loop

```bash
for i in 1 2 3 4 5; do
  echo "Number $i"
done

# Range
for i in {1..5}; do
  echo $i
done

# C-style
for ((i=0; i<5; i++)); do
  echo $i
done

# Over files
for file in *.txt; do
  echo "Processing $file"
done
```

### While Loop

```bash
count=1
while [ $count -le 5 ]; do
  echo $count
  ((count++))
done
```

### Until Loop

Runs until the condition becomes true:

```bash
count=1
until [ $count -gt 5 ]; do
  echo $count
  ((count++))
done
```

### Loop Control

```bash
break       # Exit the loop
continue    # Skip to the next iteration
```

---

## Functions

```bash
greet() {
  echo "Hello, $1"
}

greet "Alice"      # Hello, Alice
greet "Bob"        # Hello, Bob
```

### Return Values

Functions return an exit code (0–255). To return a string value, use `echo` and capture it:

```bash
add() {
  echo $(( $1 + $2 ))
}

result=$(add 3 4)
echo $result    # 7
```

### Local Variables

```bash
my_function() {
  local temp="I'm local"
  echo $temp
}
```

---

## Script Basics

### Structure

```bash
#!/usr/bin/env bash
# Description: What this script does

set -e    # Exit immediately if any command fails
set -u    # Treat unset variables as errors
set -o pipefail  # Catch errors in pipelines

# Script body
echo "Starting..."
```

The first line (`#!/usr/bin/env bash`) is called a **shebang**. It tells the OS which interpreter to use.

### Making a Script Executable

```bash
chmod +x script.sh
./script.sh
```

### Arguments

```bash
#!/usr/bin/env bash
echo "Script name: $0"
echo "First arg:   $1"
echo "Second arg:  $2"
echo "All args:    $@"
echo "Arg count:   $#"
```

```bash
./script.sh hello world
# Script name: ./script.sh
# First arg:   hello
# Second arg:  world
```

---

## Process Management

```bash
ps aux                    # List all running processes
ps aux | grep myapp       # Find a specific process
kill 1234                 # Send SIGTERM (graceful stop) to PID 1234
kill -9 1234              # Send SIGKILL (force stop) — use as a last resort
pkill myapp               # Kill processes by name

command &                 # Run a command in the background
jobs                      # List background jobs
fg                        # Bring the most recent background job to the foreground
fg %1                     # Bring job #1 to the foreground
bg %1                     # Resume job #1 in the background

Ctrl + C                  # Interrupt (kill) the foreground process
Ctrl + Z                  # Suspend the foreground process
```

---

## Useful Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl + C` | Kill the running command |
| `Ctrl + Z` | Suspend the running command |
| `Ctrl + L` | Clear the terminal |
| `Ctrl + A` | Move cursor to start of line |
| `Ctrl + E` | Move cursor to end of line |
| `Ctrl + U` | Delete from cursor to start of line |
| `Ctrl + K` | Delete from cursor to end of line |
| `Ctrl + R` | Search command history |
| `Tab` | Autocomplete file/command names |
| `↑` / `↓` | Scroll through command history |
| `!!` | Repeat the last command |
| `!$` | Last argument of the previous command |
| `history` | Show command history |

---

## Quick Reference Cheat Sheet

| Task | Command |
|------|---------|
| Print directory | `pwd` |
| List files | `ls -lh` |
| Change directory | `cd path` |
| Create file | `touch file.txt` |
| Create directory | `mkdir dir` |
| Copy file | `cp src dst` |
| Move/rename | `mv src dst` |
| Delete file | `rm file.txt` |
| Delete directory | `rm -r dir/` |
| Print file | `cat file.txt` |
| Scroll file | `less file.txt` |
| Follow file | `tail -f file` |
| Search in file | `grep "term" file` |
| Find file | `find . -name "*.txt"` |
| Pipe output | `cmd1 \| cmd2` |
| Write to file | `cmd > file.txt` |
| Append to file | `cmd >> file.txt` |
| Count lines | `wc -l file.txt` |
| Set variable | `name="value"` |
| Use variable | `echo $name` |
| Arithmetic | `$(( 2 + 2 ))` |
| Command output | `$(command)` |
| Exit code | `$?` |
| Make executable | `chmod +x script.sh` |
| Run script | `./script.sh` |
| Background job | `command &` |
| Kill process | `kill PID` |
| Clear screen | `Ctrl + L` |

---

*Part of the learning-resources collection.*
