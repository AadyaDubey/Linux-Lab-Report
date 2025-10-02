# Experiment 7: Shell Programming
## Theory
### 1. Processes
A process is an instance of a running program. When you execute a command in Linux, the operating system creates a process.
* Each process has a unique PID (Process ID).
*  The parent process (PPID) is the process that started it.

#### Command: `ps`
* Shows currently running processes.
```bash
ps
```
Output example:
```bash
  PID TTY          TIME CMD
 1234 pts/0    00:00:00 bash
 1256 pts/0    00:00:00 ps
```
Here, `PID` is the process ID, and `CMD` is the command being run.

### 2. Process States
Processes can be in different states:

* Running (R): currently executing.
* Sleeping (S): waiting for input/output.
*  Stopped (T): paused.
*  Zombie (Z): finished but still in process table.

#### Command: `top`

* Displays dynamic list of running processes, their CPU and memory usage.
```bash
top
```
Press `q` to quit.

### 3. Process Hierarchy

Processes form a tree structure: parent → child.

#### Command: `pstree`

* Displays process hierarchy.
```bash
pstree
```
Example (simplified):
```bash
systemd─┬─bash───pstree
```

### 4. Killing Processes

#### Command: `kill <PID>`
```bash
Sends a signal to stop a process.
```

Example:
```bash
kill 1234
```
Stops process with PID 1234.

#### Command: `kill -9 <PID>`
* Forcefully kills a process.

### 5. Process Prioritization

Every process has a priority (nice value). Lower values = higher priority. Range: -20 (highest) to +19 (lowest).

#### Command: `nice -n <value> command`
* Start a process with a specific priority.

Example:
```bash
nice -n 10 sleep 60
```
Starts `sleep 60` with low priority.

#### Command: `renice <value> -p <PID>`
* Change priority of a running process.

Example:
```bash
renice 5 -p 1234
```

### 6. Scheduling Processes
#### Command: `at`
* Schedules one-time tasks.

Example:
```bash
echo "ls > output.txt" | at now + 1 minute
```
Runs `ls` after 1 minute.

#### Command: `cron`
* Schedules recurring tasks using a crontab file.

Example:
```bash
crontab -e
```
To run `date` every minute, add:
```bash
* * * * * date >> time.log
```

***
## Lab Exercises
### i. Check if File Exists
```bash
#!/bin/bash
echo "Enter filename: "
read file

if [ -e "$file" ]
then
    echo "File exists. Contents are:"
    cat "$file"
else
    echo "File does not exist."
    echo "Do you want to create it? (y/n)"
    read choice
    if [ "$choice" = "y" ]; then
        touch "$file"
        echo "File $file created."
    fi
fi
```
**New Commands Used:**
* `-e` → checks if file exists.
*  `cat file` → displays file content.
*   `touch file `→ creates an empty file.
<img width="2940" height="1912" alt="lab1" src="https://github.com/user-attachments/assets/7bdbc99e-2259-4f13-97ba-0d15362265fd" />


### ii. Print Numbers from 1 to 10
```bash
#!/bin/bash
for i in {1..10}
do
    echo $i
done
```
New Concept:
* `{1..10} `→ brace expansion to generate sequence.

<img width="2940" height="1912" alt="lab2" src="https://github.com/user-attachments/assets/8f9de8ca-4961-4d69-a070-d783b0c76a89" />

### iii. Count Lines, Words, and Characters
```bash
#!/bin/bash
if [ $# -eq 0 ]
then
    echo "Usage: $0 filename"
    exit 1
fi

file=$1

if [ -e "$file" ]
then
    echo "Lines: $(wc -l < $file)"
    echo "Words: $(wc -w < $file)"
    echo "Characters: $(wc -m < $file)"
else
    echo "File not found!"
fi
```
New Commands:
* `$#` → number of command line arguments.
*   `$1` → first argument.
*    `wc` → word count utility:
        * `wc -l` → count lines.
        * `wc -w` → count words.
        * `wc -m` → count characters.

<img width="2940" height="1912" alt="lab3" src="https://github.com/user-attachments/assets/29787bb2-77a9-4361-ac2d-ad18934c037e" />

### iv. Factorial Using Function
```bash
#!/bin/bash
factorial() {
    num=$1
    fact=1
    while [ $num -gt 1 ]
    do
        fact=$((fact * num))
        num=$((num - 1))
    done
    echo $fact
}

echo "Factorial of 5 is: $(factorial 5)"
echo "Factorial of 7 is: $(factorial 7)"
echo "Factorial of 10 is: $(factorial 10)"
```
New Concepts:
* Function definition in bash:
```bash
  function_name() { commands }
  ```
* `$1` → function argument.
*  `$(command)` → command substitution (returns output).

<img width="2940" height="1912" alt="lab4" src="https://github.com/user-attachments/assets/8b65632a-e8ab-4fd4-bf99-e1d0946bb7fd" />

***
  
## Assignment

1. Write a script that monitors the top 5 processes consuming the most CPU and logs them into a file every 10 seconds.
Hint: Use `ps -eo pid,comm,%cpu --sort=-%cpu | head -6`.
```bash
#!/bin/bash

LOG_FILE="cpu_monitor.log"
INTERVAL=10 

echo "Starting CPU monitoring. Logging to $LOG_FILE every $INTERVAL seconds."
echo "Press Ctrl+C to stop."

while true; do
  TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")
  echo "--- $TIMESTAMP ---" >> "$LOG_FILE"
  ps -eo pid,comm,%cpu --sort=-%cpu | head -n 6 >> "$LOG_FILE"
  echo "" >> "$LOG_FILE"
  sleep "$INTERVAL"
done
```
<img width="2940" height="1912" alt="assignment1" src="https://github.com/user-attachments/assets/f9d47b25-02c5-4311-b232-ad64ed06c461" />

2.  Write a script that accepts a PID from the user and displays its details (state, parent process, memory usage).
Hint: Use `ps -p <PID> -o pid,ppid,state,comm,%mem`.
```bash
#!/bin/bash

read -p "Enter the PID: " pid

if ps -p "$pid" > /dev/null 2>&1; then
    echo "Process details:"
    ps -p "$pid" -o pid,ppid,state,comm,%mem
else
    echo "No process found with PID $pid"
fi
```
<img width="2940" height="1912" alt="assingment2" src="https://github.com/user-attachments/assets/5b1abafb-444f-4b84-bce1-9d1fe6b845b7" />

3. Create a script that schedules a task to append the current date and time to a log file every minute using cron.
```bash
#!/bin/bash
LOG_FILE="/home/aadya/exp7/assignment/logfile.log" 
CURRENT_DATETIME=$(date +"%Y-%m-%d %H:%M:%S")
echo "$CURRENT_DATETIME" >> "$LOG_FILE"
```
Open crontab for editing,
```bash
crontab -e
```
In crontab,
```bash
* * * * * /home/aadya/exp7/assignment/logfile.log
```
<img width="2940" height="1912" alt="assingment3" src="https://github.com/user-attachments/assets/1a8cbe0b-7d87-472d-8573-5bbd8930c1a1" />

4. Modify the factorial function to check if input is negative. If yes, display an error message.
```bash
#!/bin/bash

factorial() {
  local num=$1
  local result=1

  if (( num < 0 )); then
    echo "Error: Factorial is not defined for negative numbers."
    return 1 
  fi

  if (( num == 0 )); then
    echo 1
  else
    for (( i=1; i<=num; i++ )); do
      result=$(( result * i ))
    done
    echo "$result"
  fi
}

read -p "Enter a non-negative integer: " input_num


fact_result=$(factorial "$input_num")

if [ $? -eq 0 ]; then
  echo "The factorial of $input_num is: $fact_result"
fi
```
<img width="2940" height="1912" alt="assingment4" src="https://github.com/user-attachments/assets/b852ef7d-41d5-4088-9f50-6d869b0b2818" />

5. Write a script that accepts a filename as an argument. If the file exists, display the number of lines starting with a vowel.
Hint: Use `grep -i "^[aeiou]" filename | wc -l`.
```bash
#!/bin/bash

if [ -z "$1" ]; then
  echo "Usage: $0 <filename>"
  exit 1
fi

filename="$1"

if [ -f "$filename" ]; then
  vowel_count=$(grep -c '^[AEIOUaeiou]' "$filename")
  echo "Number of lines starting with a vowel in '$filename': $vowel_count"
else
  echo "Error: File '$filename' not found."
  exit 1
fi
```
<img width="2940" height="1912" alt="assingment5" src="https://github.com/user-attachments/assets/19d4f8e8-a09b-43f3-9b18-e5ddddc76179" />

