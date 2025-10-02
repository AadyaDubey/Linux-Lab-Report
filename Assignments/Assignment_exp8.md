# Assignment
## 1. Write a script that starts a background job (e.g., sleep 60), lists all jobs, brings the job to the foreground, and then terminates it.
### Input
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
### Output
<img width="2940" height="1912" alt="a1" src="https://github.com/user-attachments/assets/6d406959-8829-4819-bb44-d681c99757b8" />


## 2.  Create a script that compares two files and displays whether their contents are identical or different.  
Hint: Use `cmp` or `diff`.
### Input
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
### Output
<img width="2940" height="1912" alt="a2" src="https://github.com/user-attachments/assets/8ec27946-e4a5-4cda-8eed-b45b5eb29bad" />


## 3. Write a script that counts the number of processes currently being run by your user.  
Hint: Use `ps -u $USER | wc -l`.
### Input
```bash
#!/bin/bash

count=$(ps -u $USER | wc -l)

count=$((count - 1))

echo "User: $USER"
echo "Number of processes currently running: $count"
```
### Output
<img width="2940" height="1912" alt="a3" src="https://github.com/user-attachments/assets/98cd12d0-abb4-41fe-87e7-82e97a35e8c9" />


## 4. Develop a script that monitors memory usage every 5 seconds and logs it into a file.  
Hint: Use `free -m` inside a loop with `sleep`.
### Input
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
### Output
<img width="2940" height="1912" alt="a4" src="https://github.com/user-attachments/assets/a133a98e-2948-4dfd-90fd-9fcadc0af296" />


## 5. Write a script that prompts for a filename and a search pattern, then displays the count of matching lines.  
Hint: Combine `grep -c`.
### Input
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
### Output
<img width="2940" height="1912" alt="a5" src="https://github.com/user-attachments/assets/00f18e44-3ee8-43ef-a1d8-3eaec0861cc8" />


