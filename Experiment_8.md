

# Experiment 8: Shell Programming
## Theory
### 1. Process Control and Signals
Processes can receive signals from the OS or the user to control execution.

* `kill -l` → list all signals.
* Common signals:
	*  `SIGINT (2)` → interrupt (Ctrl+C).
	*  `SIGTERM (15)` → terminate gracefully.
	*  `SIGKILL (9)` → force kill.


Example:
```bash
kill -9 1234
```
Kills process with PID 1234 forcefully.


### 2. Process Monitoring and Resource Usage

Commands:
* `top` → live view of processes, CPU, memory.
*  `htop` (if installed) → user-friendly version of `top`.
*   `ps aux` → snapshot of all processes.
*   `free -h` → shows memory usage.
*   `uptime` → system load averages.

### 3. Process Communication

Processes communicate using files, pipes, or sockets.

`Pipes (|)` → pass output of one command to another. Example:
```bash
ps aux | grep bash
```
Finds running processes with `bash`.

### 4. Process Synchronization

To prevent conflicts, processes can be synchronized:
* `wait` → wait for a background job to finish.
* File locks and semaphores (advanced, beyond this lab).


Example:
```bash
sleep 5 &
wait
echo "Finished after 5 seconds"
```

### 5. Background Processes and Job Control

* Add `&` at the end to run command in background.
```bash
sleep 30 &
```
* `jobs` → shows background jobs.
* `fg %1`→ bring job 1 to foreground.
* `bg %1` → resume job 1 in background.


### 6. System Monitoring and Logging

* `dmesg | less` → kernel/system messages.
* `journalctl` (systemd systems) → system logs.
* `last` → last logged-in users.
*  `who` or `w` → users currently logged in.

***
## Lab Exercises
### i. Check File Permissions
```bash
#!/bin/bash
echo "Enter filename:"
read file

if [ -e "$file" ]; then
    [ -r "$file" ] && echo "File is readable"
    [ -w "$file" ] && echo "File is writable"
    [ -x "$file" ] && echo "File is executable"
else
    echo "File does not exist."
fi
```
New Concepts:
* `-r` → check if readable.
*  `-w` → check if writable.
*   `-x` → check if executable.
<img width="2940" height="1912" alt="le1" src="https://github.com/user-attachments/assets/d1da2763-04e7-40aa-a1d9-1e58c08b70f6" />


### ii. String Operations
```bash
#!/bin/bash
echo "Enter first string:"
read str1
echo "Enter second string:"
read str2

# String length
echo "Length of first string: ${#str1}"
echo "Length of second string: ${#str2}"

# Concatenation
concat="$str1$str2"
echo "Concatenated string: $concat"

# Comparison
if [ "$str1" = "$str2" ]; then
    echo "Strings are equal"
else
    echo "Strings are not equal"
fi
```
New Concepts:
* `${#var}` → length of string.
*  `"$str1$str2"` → string concatenation.
*   `=` operator → string comparison.
<img width="2940" height="1912" alt="le2" src="https://github.com/user-attachments/assets/eb10268a-90b4-4be5-b0d1-118c3b46bb4f" />


### iii. Search for a Pattern in a File
```bash
#!/bin/bash
echo "Enter filename:"
read file
echo "Enter pattern to search:"
read pattern

if [ -e "$file" ]; then
    echo "Matching lines:"
    grep "$pattern" "$file"
else
    echo "File not found!"
fi
```
New Command:
* `grep pattern file` → searches for matching lines.
<img width="2940" height="1912" alt="le3" src="https://github.com/user-attachments/assets/b446b56a-6b6e-4eec-b9b0-c87084284786" />


### iv. Display System Information
```bash
#!/bin/bash
echo "System Information:"
echo "-------------------"
echo "Date and Time: $(date)"
echo "Logged in users: $(who)"
echo "System Uptime: $(uptime -p)"
echo "Memory Usage:"
free -h
echo "Disk Usage:"
df -h
```
New Commands:
* `date` → current date and time.
*  `who` → list logged-in users.
*  `uptime -p` → pretty uptime format.
*  `free -h` → memory usage in human-readable format.
*  `df -h` → disk usage.
<img width="2940" height="1912" alt="le4" src="https://github.com/user-attachments/assets/ddc39199-2921-4cc1-bf13-089cea8095d4" />


## Assignment
1. Write a script that starts a background job (e.g., sleep 60), lists all jobs, brings the job to the foreground, and then terminates it.
```bash
#!/bin/bash

sleep 60 &

echo "Listing all jobs:"
jobs

echo "Bringing the job to the foreground..."
fg %1

echo "Terminating the job..."
kill $!
```
<img width="2940" height="1912" alt="a1" src="https://github.com/user-attachments/assets/6d406959-8829-4819-bb44-d681c99757b8" />


2.  Create a script that compares two files and displays whether their contents are identical or different.  
Hint: Use `cmp` or `diff`.
```bash
#!/bin/bash

if [ $# -ne 2 ]; then
  echo "Usage: $0 <file1> <file2>"
  exit 1
fi

file1=$1
file2=$2

if [ ! -f "$file1" ] || [ ! -f "$file2" ]; then
  echo "Error: One or both files do not exist."
  exit 1
fi

if cmp -s "$file1" "$file2"; then
  echo "The files are identical."
else
  echo "The files are different."
fi
```
<img width="2940" height="1912" alt="a2" src="https://github.com/user-attachments/assets/8ec27946-e4a5-4cda-8eed-b45b5eb29bad" />


3. Write a script that counts the number of processes currently being run by your user.  
Hint: Use `ps -u $USER | wc -l`.
```bash
#!/bin/bash

count=$(ps -u $USER | wc -l)

count=$((count - 1))

echo "User: $USER"
echo "Number of processes currently running: $count"
```
<img width="2940" height="1912" alt="a3" src="https://github.com/user-attachments/assets/98cd12d0-abb4-41fe-87e7-82e97a35e8c9" />


4. Develop a script that monitors memory usage every 5 seconds and logs it into a file.  
Hint: Use `free -m` inside a loop with `sleep`.
```bash
#!/bin/bash

LOG_FILE="memory_log.txt"

echo "Starting memory monitoring... Logs will be saved in $LOG_FILE"
echo "Timestamp               | Total(MB) | Used(MB) | Free(MB)" > "$LOG_FILE"
echo "---------------------------------------------------------" >> "$LOG_FILE"

while true
do
    timestamp=$(date +"%Y-%m-%d %H:%M:%S")
    
    mem_info=$(free -m | grep Mem | awk '{print $2"        | "$3"      | "$4}')
    
    echo "$timestamp | $mem_info" >> "$LOG_FILE"
    sleep 5
done
```
<img width="2940" height="1912" alt="a4" src="https://github.com/user-attachments/assets/a133a98e-2948-4dfd-90fd-9fcadc0af296" />


5. Write a script that prompts for a filename and a search pattern, then displays the count of matching lines.  
Hint: Combine `grep -c`.
```bash
#!/bin/bash

read -p "Enter the filename: " filename

if [ ! -f "$filename" ]; then
  echo "Error: File '$filename' does not exist."
  exit 1
fi

read -p "Enter the search pattern: " pattern

count=$(grep -c "$pattern" "$filename")

echo "The pattern '$pattern' appears in $count line(s) of '$filename'."
```
<img width="2940" height="1912" alt="a5" src="https://github.com/user-attachments/assets/00f18e44-3ee8-43ef-a1d8-3eaec0861cc8" />


