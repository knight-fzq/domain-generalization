# start first shell
#! /bin/bash
echo "Hello World !"
1. #! is a kind of agreed signal to inform system of using which interpreter to perform this scripts, that is, using which shell.  
2. extention of a file do not influence the excution of a script. .sh means shell, .php means php  
3. echo:a command to print text  

# perform shell
1. served as excutable program  
  chmod +x [dir]/[name].sh(let script have the privilege to perform)  
  [dir]/[name].sh(perform the script)  
  
2. perform interpreter directly  
  /bin/sh test.sh  
  /bin/php test.php  

# variable
### your_name="fzq"  
1. only "x","7","_", and the initials can not be number  
2. no space, '_' can be used  
3. no punctuation  
4. no keyword in bash(can use help to check)  
### for file in 'ls /etc' or for file in $(ls /etc)

# using defined variable
### adding $
your_name="fzq"  
echo $your_name  
your_name="xns"  
echo $your_name  
### adding {} to variables(help define the boundary)
for skill in Ada; do  
  echo "i like {$skill} and you"  
done  

# readonly
#!/bin/bash  
myUrl="google.com"  
readonly myUrl  
myUrl="baidu.com" X  
/bin/sh: NAME: This variable is read only.  

# delete variable
### unset
unset variable_name  

### unset variable can not be used again and unset command can not delete readonly variable
myUrl="google.com"  
unset myUrl  
echo $myUrl(no output)  

# type of variable
1. local variable  
2. environment variable  
3. shell variable  

# string
### single qutation mark
1. can not use variable inside single qutation mark  
2. can not use escape character  
3. example: str='this is a string'  

### double qutation mark
1. can use variable inside single qutation mark  
2. can use escape character  
3. example: str="this is %{stri} \n"  

### concat string
your_name="fzq"  
greet="hello,$your_name!"  

### get length of a string
your_name="fzq"  
echo ${#your_name}  

### get sub-string
your_name="fzqfzqfzq"  
echo ${your_name:2:5}  

# array
### define an array
array_name={value1 value2 value3}  
or  
array_name=(  
value1  
value2  
value3  
)  
### read element in the array
${array_name[index]}  
value=${array[9]}  

### get the length of an array
echo ${#array[0]}  
or  
echo ${#array[*]}  

# notes
# note  
or
:<<EOF  
EOF  

