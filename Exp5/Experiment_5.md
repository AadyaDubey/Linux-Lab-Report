# Experiment 5: Shell Programming
## Theory Concepts
### 1.  Command Line Arguments 
* Run with arguments: 
```bash
     ./script.sh 10
```  

* Inside script:
``` bash
     echo "First argument is $1"
     echo "Total arguments: $#"
     echo "All arguments: $@"
```  

### 2. Arrays in Bash 
```bash
   arr=(10 20 30 40)
   echo "First: ${arr[0]}"
   echo "All: ${arr[@]}"
```

### 3. Conditional Statements 

* Already seen with `if`
*  We also have `elif` and `case`.

***

## Lab Tasks
### i. Prime Number Check
```bash
#!/bin/bash
echo "Enter a number: "
read num
flag=0

for ((i=2; i<=num/2; i++))
do
    if [ $((num % i)) -eq 0 ]
    then
        flag=1
        break
    fi
done

if [ $flag -eq 0 ]
then
    echo "$num is a prime number."
else
    echo "$num is not a prime number."
fi
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_18_09_2025_19_29_15" src="https://github.com/user-attachments/assets/6b301b07-fd2d-4c3d-80a4-d377b85e21e4" />

### ii. Sum of Digits
```bash
#!/bin/bash
echo "Enter a number: "
read num
sum=0

while [ $num -gt 0 ]
do
    digit=$((num % 10))
    sum=$((sum + digit))
    num=$((num / 10))
done

echo "Sum of digits: $sum"
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_18_09_2025_19_30_10" src="https://github.com/user-attachments/assets/2d15ffba-a121-4109-81c8-41c3227288fa" />

### iii. Armstrong Number

(An Armstrong number of n digits is a number equal to the sum of its digits raised to the power n. Example: 153 = 1³ + 5³ + 3³)
```bash
#!/bin/bash
echo "Enter a number: "
read num
temp=$num
n=${#num}   # number of digits
sum=0

while [ $temp -gt 0 ]
do
    digit=$((temp % 10))
    sum=$((sum + digit**n))
    temp=$((temp / 10))
done

if [ $sum -eq $num ]
then
    echo "$num is an Armstrong number."
else
    echo "$num is not an Armstrong number."
fi
```
<img width="2940" height="1912" alt="VirtualBox_Ubuntu_18_09_2025_19_31_08" src="https://github.com/user-attachments/assets/b38776eb-13bb-44dd-b357-ae0adfeafc7d" />

***
