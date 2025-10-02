# Experiment 3: Linux File Manipulation and System Management Tutorial 
**Name:** Aadya Dubey  
**Roll No.:** 590029213  
**Date**:11/09/2025
***
# Aim: 
To understand and perform advanced file and directory operations in Linux using commands for searching, compressing, archiving, and linking files effectively.

# Requirments:
* Operating System: Ubuntu running on Oracle VirtualBox
* Shell: Bash (Bourne-Again Shell)
***
***


## File Searching with `find`
The find command searches for files and directories based on various criteria.

Basic Syntax:
```bash
find [path] [expression] [action]
```
Search by Name:
```bash
# Find files by exact name
find /home -name "document.txt"

# Case-insensitive search
find /home -iname "Document.txt"

# Wildcard patterns
find /home -name "*.txt"
find /home -name "report_*"
find /etc -name "*conf*"
```
Search by Type:
```bash
# Find directories only
find /home -type d -name "backup*"

# Find regular files only
find /home -type f -name "*.log"

# Find symbolic links
find /home -type l
```
Search by Size:
```bash
# Files larger than 100MB
find /home -size +100M

# Files smaller than 1KB
find /home -size -1k

# Files exactly 500 bytes
find /home -size 500c
```
Search by Time:
```bash
# Modified in last 7 days
find /home -mtime -7

# Accessed more than 30 days ago
find /home -atime +30

# Changed in last 24 hours
find /home -ctime -1
```
Search by Permissions:
```bash
# Files with specific permissions
find /home -perm 644

# Files with at least specific permissions
find /home -perm -644

# Executable files
find /home -perm /111
```
Advanced Examples:
```bash
# Find and delete empty files
find /tmp -type f -empty -delete

# Find large files and list them
find /home -size +50M -ls

# Find files and execute command
find /home -name "*.tmp" -exec rm {} \;

# Find with multiple conditions
find /home -name "*.txt" -size +1M -mtime -7
```

## Pattern Searching with `grep`
The grep command searches for specific patterns within files.

Basic Syntax:
```bash
grep [options] pattern [file(s)]
```
Basic Examples:
```bash
# Search for pattern in file
grep "error" /var/log/syslog

# Case-insensitive search
grep -i "Error" logfile.txt

# Search in multiple files
grep "TODO" *.txt

# Recursive search in directories
grep -r "function" /home/user/code/

# Show line numbers
grep -n "pattern" file.txt
```
Advanced Options:
```bash
# Whole word only
grep -w "cat" animals.txt

# Invert match (lines that don't contain pattern)
grep -v "debug" logfile.txt

# Count occurrences
grep -c "error" logfile.txt

# Show only matching part
grep -o "email@[a-zA-Z0-9.-]*" contacts.txt

# Show context lines
grep -A 3 -B 3 "pattern" file.txt  # 3 lines after and before
grep -C 2 "pattern" file.txt       # 2 lines of context
```
Regular Expressions:
```bash
# Beginning of line
grep "^Error" logfile.txt

# End of line
grep "finished$" logfile.txt

# Any character
grep "a.b" file.txt  # matches "aab", "acb", etc.

# Character classes
grep "[0-9]" file.txt        # any digit
grep "[A-Z]" file.txt        # any uppercase letter
grep "[aeiou]" file.txt      # any vowel

# Extended regex
grep -E "(error|warning)" logfile.txt
grep -E "[0-9]{3}-[0-9]{3}-[0-9]{4}" contacts.txt  # phone numbers
```

## File Archiving with `tar`
The tar command creates archives (tarballs) from multiple files and directories.

Basic Syntax:
```bash
tar [options] archive_name files/directories
```
Creating Archives:
```bash
# Create tar archive
tar -cf backup.tar /home/user/documents

# Create compressed tar archive (gzip)
tar -czf backup.tar.gz /home/user/documents

# Create compressed tar archive (bzip2)
tar -cjf backup.tar.bz2 /home/user/documents

# Verbose output
tar -cvf backup.tar /home/user/documents
```
Extracting Archives:
```bash
# Extract tar archive
tar -xf backup.tar

# Extract to specific directory
tar -xf backup.tar -C /restore/location

# Extract compressed archive
tar -xzf backup.tar.gz

# List contents without extracting
tar -tf backup.tar

# Extract specific files
tar -xf backup.tar file1.txt dir1/file2.txt
```
Common Options:

* **c**: create archive
*  **x**: extract archive
*   **t**: list contents
*   **f**: specify filename
*   **v**: verbose output
*   **z**: gzip compression
*   **j**: bzip2 compression
*   **C**: change directory 


## File Compression with `gzip/gunzip`
### The `gzip` Command
Compresses files using the gzip algorithm.

Examples:
```bash
# Compress file (original is replaced)
gzip largefile.txt  # creates largefile.txt.gz

# Keep original file
gzip -k largefile.txt

# Compress with maximum compression
gzip -9 largefile.txt

# Compress multiple files
gzip file1.txt file2.txt file3.txt

# View compression ratio
gzip -v largefile.txt
```

### The `gunzip` Command
Decompresses gzip files.

Examples:
```bash
# Decompress file
gunzip largefile.txt.gz

# Keep compressed file
gunzip -k largefile.txt.gz

# Test integrity
gunzip -t largefile.txt.gz

# View contents without decompressing
zcat largefile.txt.gz
zless largefile.txt.gz
zgrep "pattern" largefile.txt.gz
```

## Creating Links with `ln`  
The ln command creates links between files.

### Hard Links
Multiple directory entries pointing to the same file data.
```bash
# Create hard link
ln original.txt hardlink.txt

# View inode numbers (same for hard links)
ls -li original.txt hardlink.txt
```
Characteristics of Hard Links:

* Share the same inode number
* Cannot link to directories (usually)
* Cannot cross file system boundaries
* Deleting one link doesn't affect others
* No distinction between "original" and "link" 


### Symbolic (Soft) Links
Pointers to other files or directories.
```bash
# Create symbolic link
ln -s /path/to/original.txt symlink.txt

# Link to directory
ln -s /home/user/documents doc_link

# Absolute path link
ln -s /usr/bin/python3 python

# Relative path link
ln -s ../config/settings.conf current_settings
```
Characteristics of Symbolic Links:

* Different inode number from target
* Can link to directories
*  Can cross file system boundaries
*   Becomes broken if target is deleted
*   Shows link relationship in ls -l 


Practical Examples:
```bash
# Create backup script link in PATH
sudo ln -s /home/user/scripts/backup.sh /usr/local/bin/backup

# Version management
ln -s /opt/app/v2.1 /opt/app/current

# Configuration management
ln -s /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/
```
***

# OBERVATIONS
* Successfully searched files using find and grep with different options.
* Created and extracted archives using tar.
* Compressed and decompressed files with gzip/gunzip.
* Created hard and symbolic links using ln.

***

# CONCLUSION
The experiment helped in understanding advanced file and directory operations in Linux, enhancing efficiency in file management through searching, archiving, compression, and linking techniques.

***
