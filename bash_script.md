Putting all the linux commands in one file to run all at once is bash script
.sh can be extension but not mandatory
#!- shabang chracter - this tells path of the interpreter. Which means open /bin/bash and execute all the below commands.
If we write ruby then it will run ruby commands same like python respective paths are written

#!/bin/bash 
echo "Hello"

Now how to run the file
go to the current directory or add the relative path and give the file name as .py 

./firstscript.sh

But permission has to be given to the file to run the commands.
chmod 400 fisrtscript.sh

=> We can tell Chatgpt to write bash script to install something, store somewhere and etc and it will generate the whole file.
=> We can also use variables to store the data but it is temporary as it is stored in RAM

SKILL = "DEVOPS"
echo $SKILL = DEVOPS
=> Now in our shell file starting only we can write all these varibales. 
=> Its like if we give URl = "google.com" then we can only use $URL everytime instead of writing the whole URl


COMMAND LINE ARGUMENTS:

if given x=3 and done echo $x then it prints 3 but
jsut echo $y then prints nothing
But if we give file name 4 and then run then it gives o/p 4

$0- name of bash script
1 - 9 first 9 args of bash
# - how many args passed to bash
@ - all args
? - exit status of most recently run
$ - process id of current script
USER- user name of user running script
HOSTNAME- hostname of machine
SECONDS- no of seconds since script stated
RANDOM- returns a random number
LINENO- retuns current line no inbash

QUOTES: '' and ""
=> Normaly x="3" and x='3' both give same results on echo
but if echo "I have $x books" , means here 3 will come if 'I have $x books' here $x is only getting printed
  Now if i want to print 3 books are 5$ then - echo "$x books are \$5" 

COMMAND SUBSTITUTION:  


