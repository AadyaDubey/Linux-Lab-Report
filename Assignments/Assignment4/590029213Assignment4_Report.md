# Assignment
## 1. Write a script to find the largest file in a given directory.
### Script
```bash
#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 <directory>"
    exit 1
fi

if [ ! -d "$1" ]; then
    echo "Error: $1 is not a directory"
    exit 1
fi

largest_file=$(find "$1" -type f -exec du -b {} + 2>/dev/null | sort -nr | head -n 1)

if [ -z "$largest_file" ]; then
    echo "No files found in the directory."
    exit 0
fi

size=$(echo "$largest_file" | awk '{print $1}')
file=$(echo "$largest_file" | cut -f2-)

echo "Largest file: $file"
echo "Size: $size bytes"
```
### Output

## 2. Create a script that counts how many `.sh` files exist in `/home/user`.
### Script
```bash
#!/bin/bash

DIR="/home/user"

if [ ! -d "$DIR" ]; then
    echo "Error: $DIR does not exist."
    exit 1
fi

count=$(find "$DIR" -maxdepth 1 -type f -name "*.sh" | wc -l)

echo "Number of .sh files in $DIR: $count"
```
### Output

## 3. Write a script to monitor CPU usage every 10 seconds and log to a file.
### Script
```bash
#!/bin/bash

LOGFILE="/var/log/cpu_usage.log"

touch "$LOGFILE"

echo "Starting CPU usage monitoring..."
echo "Logging CPU usage to $LOGFILE"
echo "Press Ctrl+C to stop."

while true
do
    timestamp=$(date +"%Y-%m-%d %H:%M:%S")

    cpu_usage=$(top -bn1 | grep "Cpu(s)" | awk '{print 100 - $8 "%"}')

    echo "$timestamp - CPU Usage: $cpu_usage" >> "$LOGFILE"

    sleep 10
done
```
### Output

## 4. Create a script that adds a new user and sets default permissions for their home directory.
### Script
```bash
#!/bin/bash

if [ "$(id -u)" -ne 0 ]; then
    echo "Error: This script must be run as root."
    exit 1
fi

if [ -z "$1" ]; then
    echo "Usage: $0 <username>"
    exit 1
fi

USERNAME="$1"

if id "$USERNAME" &>/dev/null; then
    echo "Error: User '$USERNAME' already exists."
    exit 1
fi

useradd -m "$USERNAME"

if [ $? -ne 0 ]; then
    echo "Failed to create user '$USERNAME'."
    exit 1
fi

chmod 700 /home/"$USERNAME"

echo "Set password for $USERNAME:"
passwd "$USERNAME"

echo "User '$USERNAME' created successfully."
echo "Home directory: /home/$USERNAME"
echo "Permissions set to 700 (rwx------)"
```
### Output

***