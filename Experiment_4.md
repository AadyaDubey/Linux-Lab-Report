Experiment 4

# Experiment 4: Shell Programming 
## Theory Concepts

### 1. BASH (Bourne Again SHell) 

* It is the default shell in most Linux distributions.
*  It interprets commands entered in the terminal or written in a script file. 

### 2. Basics of Shell Scripting 

* A shell script is just a text file containing commands.
* By convention, scripts start with a **shebang**: 
```bash
     #!/bin/bash
```

→ This tells the system to run the script using bash.

### 3. Types of Shell 

 **Common shells:**

* `sh` (Bourne Shell)
*  `bash` (Bourne Again Shell)
*  `zsh`, `ksh`, `csh`
*  To check your shell: 
```bash
     echo $SHELL
```	 

### 4. Shell Variables 

 **User-defined:**
```bash
     name="Prateek"
     echo "Hello $name"
```

**System variables (predefined by shell, always uppercase):**
```bash
     echo $HOME
     echo $USER
     echo $PWD
```     

### 5. Keywords & Operators 

* **Keywords**: reserved words like if, then, else, fi, for, while.
* **Operators**:
	* Arithmetic: `+ - * / %`
	*  Comparison: ``-eq -ne -lt -le -gt -ge ``

### 6. Positional Parameters 
Used to handle inputs passed while executing a script: 
```bash
     ./script.sh arg1 arg2
```
* `$0` → script name
*  `$1` → first argument
* `$2` → second argument
***

## Lab Tasks
### i. Hello World Script
```bash
#!/bin/bash
echo "Hello, World!"
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_18_09_2025_18_47_39" src="https://github.com/user-attachments/assets/59a4104c-df8c-4b8d-80c6-662bc5a92161" />

### ii. Personalized Greeting
```bash
#!/bin/bash
echo "Enter your name: "
read name     # 'read' takes user input
echo "Hello, $name! Welcome to Shell Scripting."
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_18_09_2025_18_48_41" src="https://github.com/user-attachments/assets/e7c39f1b-3cb1-4f57-8a3f-fb72819c2a5d" />

### iii. Arithmetic Operations
```bash
#!/bin/bash
echo "Enter first number: "
read num1
echo "Enter second number: "
read num2

echo "Addition: $((num1 + num2))"
echo "Subtraction: $((num1 - num2))"
echo "Multiplication: $((num1 * num2))"
echo "Division: $((num1 / num2))"
```
* `$(())` is arithmetic expansion in bash.
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_18_09_2025_18_49_24" src="https://github.com/user-attachments/assets/349de31b-9991-41b0-835f-218a1906fe7b" />

### iv. Voting Eligibility
```bash
#!/bin/bash
echo "Enter your age: "
read age

if [ $age -ge 18 ]
then
    echo "You are eligible to vote."
else
    echo "Sorry, you are not eligible to vote."
fi
```
* `if [ condition ]` → condition enclosed in `[ ]`
*  `-ge` means **greater than or equal**.

<img width="2940" height="1912" alt="VirtualBox_Ubuntu_18_09_2025_18_50_03" src="https://github.com/user-attachments/assets/f6636ff6-e10e-4b5f-9946-e105ac4decf3" />***
