# Linux Basics

## find 

	find  path -name  pattern
	find path -type (d:directory or f:file)
	find path -perm (0777) // to find file with permission 777
	find path ! -perm (0777) // to find file without permission 777
	find /tmp -type d -empty              // FInd all empty directories
	find /home -user tecmint              // To find all files that belong to user Tecmint under /home directory
	find /home -group developer           // To find all files that belong to group developer under /home directory
	find / -mtime 50                      // To find all the files which are modified 50 days back
	find / -mtime +50 –mtime -100         // To find all the files which are modified more than 50 days back and less than 100 days.
    find / -size +50M -size -100M         // To find all the files which are greater than 50MB and less than 100MB
	find / -type f -size +100M -exec rm -f {} \; // To find all 100MB files and delete them using one single command.
	
	
	
## chmod

```
chmod u+x file_name // u, g, o : user, group,  owner

chmod u+rw,g+x file_name // r,w,x : read,write,execute
```

change the owner of a file using ```chown``` and change the group of a file using ```chgrp``` command. 

```
Commands like ls,cat are files.
When it is run, the content of the program is read from file and loaded into the memory and then instructions are executed by operating system
```

# ps

- While a program is running, it is called a process. 
- A process is uniquely identified by PID - process id. 
- To find out the information about all the processes, you can use the following commands: 
	``` pstree, top, ps ```

- ps lists the processes of the system. Without any arguments, it lists only your processes. To see all of the processes by all users, run

``` 
ps aux


where,

option a displays the processes of all users and not just our own,

option u displays information in user-oriented format and

option x displays the processes running without a controlling terminal. When you run the above command, you can see many ? sign in column TTY(Terminal Type). So, x displays these processes. These processes are mainly background processes.

```

# BSD

- The first column mentions the user id who has started the process.

- To continuously monitor all processes, use the top command. 
- This command is usually used to monitor which processes are running, taking up most CPU or Memory.

- We use ps aux to display the processes of all users. But, we notice here that we don't use the - sign with options that we were using till now. So, using options without - is known as BSD syntax to write commands.
- The other one (one with -), which we use regularly, is known as Standard syntax to write commands.
- ps command has been in use since the early versions of Unix. When it was written, BSD syntax was in use to write commands.
- So, when standard syntax came into the scene, we started to use - with options.
*** But people who used BSD syntax didn't like that they had to modify their programs and habits due to the new syntax. ***
- So, the commands were re-defined with the standard syntax without removing their compatibility with the BSD syntax. 

``` So, the commands which are very old (which were written when BSD syntax was in use) like ps command supports both BSD syntax and standard syntax. ```


# sleep

```

sleep 5m // s,m,h,d : seconds,minutes,hour,days

kill -l // lists all the kill code

kill -9 process_id // signals a kill code to process id

```

# jobs

- In Unix, a job is a group of one or more processes. One of the ways of creating job is when we put a process in the background.

- You can see the list of jobs by using the command (even the ones we send to background using &):

    ``` jobs  ```

- The number that you see in square brackets [] is the job id. This is different from the process id. To bring a process to foreground, you can use

    ```
	fg %jobid or simply fg jobid
	```

- If jobs command lists following:

```
        [1]-  Stopped         tail -f mycmd.sh
        [2]+  Stopped         top
        [3]   Running         sleep 1000 &
```

# Process interaction

- To send a running process to background, first press Ctrl+z to suspend it and then type bg to send the suspended process to background.

- When you exit the shell or disconnect, the processes you were running get killed sometimes. 

- To keep a process running in the background even if the shell has been disconnected, use

    ```
	nohup or screen
	```
# Process hierachy

- A process (parent) can execute another process (child). 
- When a parent process is killed all the child processes are ** automatically assigned to the main system process called init **.
- ***The init process is the first process that is started on your computer and is numbered as 1. If this process is killed the system will shutdown***.

- To see the tree of processes, you can use the command

```
    pstree
```
- A user can **kill only the processes that the user has created not the processes of other users**. ***Only root can kill the processes of other users***

# shell script

- The Shebang interpreter directive takes the following form:

```
#!interpreter [arguments]
```

- If you want that **every variable must be declared before it can be used**, then add set -u at the top of the script after the shebang line.

```
#!/bin/bash
set -u
name=linux
echo "hello $name world"

```

*** Don't forget to add executable permission to script file after saving ***

# Program return value

- To check the return value of the previous command you can check the value of a special variable ?

```
echo $?
```

# Sockets and Ports

There are some programs such as a web server that need to keep running all the time. Such programs are called services.

- You can communicate with such programs even from another computer. These programs or processes bind themselves to a port. 
- In other words, such programs listen on a port. No two programs can listen on the same port.
- Every computer is a given an IP address which is unique in the network so that other computers or users on other computers can connect to such systems.

- For example, a web server listens on 80 port and an email server listens on 25 port.

- You can connect and talk to any service using nc command in the following way:

    ```
	nc computer_name(or ip_address) port_number
	```
	
- The way a person refers to himself as "I", a computer refers itself as **localhost or IP Address 127.0.0.1**. it is also called as a **loopback address**.

# linking files and directories

 

- A link in UNIX is a pointer to the file. It is a connection between a file name and the actual data on the disk. It is of two types:

	- Hard links

    - Soft links (Symbolic links)

- Hard link is the mirror copy to the original file. It means that even if the file is moved or deleted, the hard link will still contain the data of the original file. 
- This is because the **hard link is assigned the same inode value as the original file**.

## Inode value 
	- Whenever a file is created in linux, an inode value is assigned to it. It is a unique index number assigned to each file in Linux/Unix system. When we access the file by its filename, internally, it is accessed through its inode value.

- On the other hand, a soft link is just a reference to another file or directory. It just contains the path to the original file and not the content. 
- If the original file is moved or deleted, the link will point to a non-existent file. This is because **soft links are assigned a different inode number.**


- Hard link copies the entire file content and it can be used only for files. 

	```
	ln source link 
	cat link         // it display source file data after deleting the source file
	```
- soft link makes a link or shortcut which does not copy the entire data. It display error when the source file is deleted.

	```
	ln -s orig_text1 myslink
	rm orig_text1
	cat myslink         // it displayed "myslink : no file or directory found"
	```
	

- Also, to reference the original file of a symbolic link, you can use readlink command. A file is a symbolic link that is again symbolic, you can use -f command.

	```
	readlink myslink // Displayed orig_text1 (even after deleting the file)
	readlink -f myslink // Displayed /home/user/orig_text1 (even after deleting the file)
	```
	
# Chaining Commands

- you want to execute a command only if another command is **successful** then you use ***&&***.

- if you want a command to be executed if another command has **failed**, you use ***||***
	
- to redirect code output

```
echo "hello" > hello.out       // for overwriting
echo "world" >> hello.out      // for appending
cat hello.out
//OUTPUT:
//hello
//world
```

# wc command

- wc command prints the number of characters, words, and lines out of whatever you type on standard input. Start wc command, type some text and press Ctrl+d to end the input:

| --- | --- |
| cmd | represent |
| --- | --- |
| wc -l | number of lines |
| wc -c | number of bytes |
| wc -m | number of char  |
| wc -w | number of words |
| wc -L | longest line (char count) |
| --- | --- |
```
wc
hi
how are you
[CTRL + d]

Output-

        2       4      15                       // line words character(\n)
```

# Filters


```
Contents of  Hello.out :  
hello
world
world
hello
```


- wc : for counting the letters, words, and lines in the input

- grep : displays only the lines from the input in which keyword (which is passed as argument) is found.

- sort : sorts/orders the input lines lexically (alphabetically) by default but can be changed

```
If the input file is big, the sort command might use too much memory. So, you can force sort command to use less memory say 100 MB:

    sort -S 100M
```

- more : displays the input in a page-wise manner

- cat : displays the content of the file passed as an argument

```
cat hello.out | sort (sort -n : for  numeric sort , sort -f : for non case sort, sort -nr : for reverse numeric sort)
OUTPUT:
hello
hello
world
world
```

- sed : substitute a word with another word: sed 's/word/another_word/' . ***The /g is an option of sed which makes replace all occurrences of space instead of only one.***

	```
	Replace all whitespace (multiple tabs and spaces):

    sed -E 's/[ \t]+/\n/g'

	Please note that since we are using regular expressions, we need to specify -E
	```
	
	```
	cat hello.out | sed 's/hello/wor/'          // sed 's/hello/wor/g'
	OUTPUT:
	wor
	world
	world
	wor
	```

- tr : translate character ranges. For example to lowercase characters in input you can use:

    ```
	tr 'A-Z' 'a-z'
	cat hello.out | tr 'a-o' '0-9'
	OUTPUT:
	74999
	w9r93
	w9r93
	74999
	```

- uniq : Display the unique input lines. The input lines need to be sorted. If you want to display frequency, use:

    ```
	uniq -c
	cat hello.out | uniq -c
	OUTPUT:
	1 hello
    2 world
    1 hello
	```

# setuid

- In Unix, there is a file /etc/shadow that contains (one-way)encrypted passwords of every user. The user can not see the contents of the file.
- **This is to defend the password cracking programs.**

- To change the password, the user needs to use the command: passwd. 
- This **passwd command** first ***asks you for your old password and encrypts your input and compares it against the value in the file /etc/shadow***. If it matches then it updates the password file /etc/shadow with new content.

- When you are not allowed to view the /etc/shadow file, how can a program (passwd) do the same when run by you?

- This is where the idea of **special permission called setuid** came into picture. ***A program file can be given setuid permission such that the program becomes the user who owns the program file instead of the user who is running it***

```
chmod +sx temp.sh
```

- The "+s" option sets the "setuid" or "setgid" permission on the file. 
- When the "setuid" permission is set on an executable file, it allows the file to be executed with the permissions of the file owner rather than the permissions of the user 
who executed the file. 
- Similarly, when the "setgid" permission is set on a directory, it allows the files created within that directory to inherit the group ownership of the directory 
instead of the group ownership of the user who created the file.

# sudo command

- This command makes a program run as root (the system administrator). This command is only allowed to be used by few users. Such users are called **sudoers**. 
- You can modify the sudoers using

```
sudo visudo
```

# which command

- To find where is your program located, you can use which command.
```
For example,

which java // would print /usr/bin/java which means java is a command in the directory /usr/bin
```

# Environmental variables

- Unix shell provides environment variables. These variables are used to control the functionality of our whole environment (systemwide) and not only to the current shell. The one restricted only to a particular shell are called shell variables.

- To see the entire list of environment variables, use:

		```set```

- These environment variables can be used by the shell, programs or commands.

- You can set the environment variables simply by assignment: MYVAR=VAL

- For example, ***the $PS1 variable is used by the shell to display the prompt***. To change your prompt, you can try:
```
OLDP=$PS1
PS1='MyPrompt: '
```

- Please note that if you change your prompt it might make you uncomfortable but you can execute any commands as usual. Here is the snapshot of me executing pwd and whoami commands from new prompt.
```
bash-4.2$ OLDP=$PS1
bash-4.2$ PS1='MyPrompt: '
MyPrompt: pwd
/home/sandeepgiri9034
MyPrompt: whoami
sandeepgiri9034
MyPrompt:
```
- To restore back to the previous prompt, try:

	```PS1=$OLDP```
	
-  the environment variable $PATH is a list of directories separated by a colon. It is used by the shell to find the file corresponding to a command.



## Note: 

- By default newline is added after '>' and '>>' command.
- wc command also counts new line (\n ) character
- If error in command occurs, then it won't go to next command after '|'
- 'tr' command will replace the extra characters from the first range with the last character of second range.
- You can use $USER and such values when defining filename``` example : nano who_$USER.sh // will be replaced as who_jeswin.sh```.
- The alternative command to shutdown immediately is: halt
- Please note that above commands (shutdown, halt, reboot) can only be run as root
- ```file``` command can give the type of file.
