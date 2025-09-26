# Experiment 6: Shell Programming
## Theory
### 1. Shell Loops
Loops are used to execute a block of code repeatedly.

* For Loop
```bash
for i in 1 2 3 4 5
do
    echo "Number: $i"
done
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_18_09_2025_23_59_11" src="https://github.com/user-attachments/assets/7fe87f33-4818-4175-b03a-30ab0aa4ea48" />

* While Loop
```bash
i=1
while [ $i -le 5 ]
do
    echo "Count: $i"
    i=$((i+1))
done
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_00_07" src="https://github.com/user-attachments/assets/52ce21aa-ad6e-4af9-b1cf-a2efa31e4842" />

* Until Loop
```bash
i=1
until [ $i -gt 5 ]
do
echo "Count: $i"
i=$((i+1))
done
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_18_09_2025_23_59_40" src="https://github.com/user-attachments/assets/3e2144b5-eaa7-42aa-834d-151cfdd0e9f1" />

The for loop iterates over items, while runs until the condition becomes false, and until runs until the condition becomes true. 

### 2. Loop Control
* break: exit from the loop.
* continue: skip current iteration and continue with the next. 

Example:
```bash
for i in {1..5}
do
    if [ $i -eq 3 ]; then
        continue
    fi
    echo "Number: $i"
done
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_12_48" src="https://github.com/user-attachments/assets/9e6f8459-ee6b-4b5d-94a0-b410c4ae8762" />

### 3. I/O Redirection

Redirects output, input, or errors.

* `>` : redirect output (overwrite).
*  `>>`: append output.
*   `<` : redirect input.
*   `2>`: redirect errors. 

Examples:
```bash
echo "Hello" > file.txt
echo "World" >> file.txt
cat < file.txt
ls /notexist 2> error.log
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_05_35" src="https://github.com/user-attachments/assets/8d40871a-f4a0-4a0b-a0cf-7d970c3df20a" />
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_06_42" src="https://github.com/user-attachments/assets/2d9e69f6-e298-46ea-b38b-837fca2a0507" />
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_07_13" src="https://github.com/user-attachments/assets/707f0b6a-88c1-405c-b49b-d8d13aba95f5" />

### 4. Shell Functions
Functions allow code reuse and modularity.
```bash
greet() {
    echo "Hello $1"
}
greet "User"
```
Here`$1` refers to the first argument passed to the function. 
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_17_02" src="https://github.com/user-attachments/assets/696bcc6d-1d0b-439d-ae51-f94a0c597a7a" />


### 5. Regular Expressions
Regular expressions are used for pattern matching. They can be used with tools like grep.

Example:
```bash
echo "hello123" | grep -E "[a-z]+[0-9]+"
```
This matches strings with letters followed by digits. 

### 6. Script Debugging and Troubleshooting

* Run script with debugging: 
```bash
bash -x script.sh
```

* Add manual debug statements: 
```bash
echo "Debug: variable=$var"
```
***
## Lab Exercises
### i. Palindrome Check
```bash
#!/bin/bash
echo "Enter a number: "
read num
rev=0
temp=$num

while [ $temp -gt 0 ]
do
    digit=$((temp % 10))
    rev=$((rev * 10 + digit))
    temp=$((temp / 10))
done

if [ $num -eq $rev ]
then
    echo "$num is a palindrome."
else
    echo "$num is not a palindrome."
fi
```
Explanation: Digits are extracted one by one using `% 10`. They are combined in reverse order. The reversed number is compared with the original.
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_23_18" src="https://github.com/user-attachments/assets/862dc76d-a069-4fe7-afff-fb68e3fc5977" />


### ii. GCD and LCM
```bash
#!/bin/bash
echo "Enter two numbers: "
read a b

x=$a
y=$b
while [ $y -ne 0 ]
do
    temp=$y
    y=$((x % y))
    x=$temp
done
gcd=$x

lcm=$(( (a * b) / gcd ))

echo "GCD: $gcd"
echo "LCM: $lcm"
```
Explanation: The Euclidean algorithm is used for GCD. LCM is calculated using the formula `(a*b)/GCD`.
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_23_47" src="https://github.com/user-attachments/assets/b3f5d8d6-ca9a-4800-92e1-d92ab1b4d74d" />


### iii. Sorting Numbers
```bash
#!/bin/bash
echo "Enter numbers separated by space: "
read -a arr

echo "Ascending Order: "
printf "%s\n" "${arr[@]}" | sort -n

echo "Descending Order: "
printf "%s\n" "${arr[@]}" | sort -nr
```
Explanation: `read -a` stores input in an array. `sort -n` arranges numbers in ascending order, and `sort -nr` in descending order. 
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_24_35" src="https://github.com/user-attachments/assets/367de85b-3fba-46d0-8b5f-5107268f7ab3" />

***
## Assignment
1. Write a function to calculate the factorial of a number using a loop.
```bash
#!/bin/bash
echo "Enter a number: "
read a

factorial=1
i=1
while [ $i -le $a ]
do
	factorial=$((factorial*i))
	i=$((i+1))
done

echo "Factorial of the given number is: $factorial"
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_33_56" src="https://github.com/user-attachments/assets/e902e965-8315-4814-bd06-a3205be713f2" />

2. Write a script that reads a filename and counts how many times a given word appears in it.
```bash
#!/bin/bash

if [ -z "$1" ] || [ -z "$2" ]; then
  echo "Usage: $0 <filename> <word_to_count>"
  exit 1
fi

filename="$1"
word_to_count="$2"

if [ ! -f "$filename" ]; then
  echo "Error: File '$filename' not found."
  exit 1
fi

count=$(grep -o -i -w "$word_to_count" "$filename" | wc -l)

echo "The word '$word_to_count' appears $count times in '$filename'."
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_46_27" src="https://github.com/user-attachments/assets/5356046c-d578-4e83-b46d-4e5944508199" />

3. Write a script that generates the first N Fibonacci numbers using a while loop.
```bash
#!/bin/bash

usage() {
    echo "Usage: $0 <number_of_fibonacci_numbers>"
    echo "Generates the first N Fibonacci numbers."
    exit 1
}

if [ -z "$1" ]; then
    usage
fi

N=$1

if ! [[ "$N" =~ ^[0-9]+$ ]] || [ "$N" -le 0 ]; then
    echo "Error: N must be a positive integer."
    usage
fi

a=0
b=1

count=0

echo "The first $N Fibonacci numbers are:"

if [ "$N" -ge 1 ]; then
    echo -n "$a "
    count=$((count + 1))
fi

if [ "$N" -ge 2 ]; then
    echo -n "$b "
    count=$((count + 1))
fi

while [ "$count" -lt "$N" ]; do
    fn=$((a + b)) 
    echo -n "$fn " 
    a=$b         
    b=$fn         
    count=$((count + 1))
done

echo "" 
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_52_45" src="https://github.com/user-attachments/assets/62b1cf3d-e9e7-41bf-a570-7e8f67423945" />

4. Write a script that validates whether the entered string is a proper email address using a regular expression.
```bash
#!/bin/bash

regex="^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$"

echo "Enter an email address:"
read email_address

if [[ "$email_address" =~ $regex ]]; then
  echo "'$email_address' is a valid email address."
else
  echo "'$email_address' is NOT a valid email address."
fi
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_00_56_23" src="https://github.com/user-attachments/assets/223835e4-b418-4a8b-9933-51a3b6bda9cc" />

5. Write a script with an intentional error, run it with `bash -x`, and explain the debug output.
```bash
#!/bin/bash

echo "Starting script..."

non_existent_command "hello world"

echo "Script finished."
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_19_09_2025_01_41_59" src="https://github.com/user-attachments/assets/1faffcda-2a09-414a-bfad-d748ab73615f" />

**Explanation of the Debug Output:**  
* `+ echo 'Starting script...'`:
    * The `+` prefix indicates that this line is from the bash `-x` debug output.
    * echo `'Starting script...'` shows the command being executed after variable expansion (though no variables are present here). This is the first command in the script.
    
* `Starting script...`:
    * This is the actual output generated by the echo command from the script itself, printed to standard output.

* `+ non_existent_command 'hello world'`:
    * Again, the `+` prefix signifies debug output.
    * `non_existent_command 'hello world'` reveals the next command the shell is attempting to execute, along with its arguments. This is where the intentional error lies. 

* `my_script.sh: line 5: non_existent_command: command not found`:
    * This is the error message generated by Bash.
    * `my_script.sh: line 5:` indicates the script file and line number where the error occurred.
    `non_existent_command: command not found` clearly states the nature of the error: the command `non_existent_command` could not be found in the system's PATH.
* `+ echo 'Script finished.'`:
    * The `+` prefix indicates that this line is from the bash `-x` debug output.
    * echo `+ echo 'Script finished.'` shows the command being executed after variable expansion (though no variables are present here). This is the first command in the script.
* `Script finished.`:
    * This is the actual output generated by the echo command from the script itself, printed to standard output.

***
