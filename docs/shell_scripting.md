### Unix Architecture
Here is a basic block diagram of a Unix system −

### Unix Architecture
The main concept that unites all the versions of Unix is the following four basics −

#### Kernel
The kernel is the heart of the operating system.
It interacts with the hardware and most of the tasks like **memory management**,** task scheduling **and** file management**

**Shell** − The shell is the **utility that processes your requests**.
When you type in a command at your terminal, the shell interprets the command and calls the program that you want.
The shell uses standard syntax for all commands.
**C Shell, Bourne Shell (bash) and Korn Shell** are the most famous shells which are available with most of the Unix variants.

**Commands and Utilities (Program)** − There are various commands and utilities (Programs) which you can make use of in your day to day activities.
**cp, mv, cat and grep, etc.** are few examples of commands and utilities.
There are over 250 standard commands plus numerous others provided through 3rd party software.
All the commands come along with various options.

**Files and Directories** − All the **data** of Unix is organized into **files**.
All files are then organized into **directories**.
These directories are further organized into a **tree-like structure** called the **filesystem**.

### Standard Unix Streams
Under normal circumstances, every Unix program has three streams (files) opened for it when it starts up −

#### stdin
This is referred to as the **standard input** and the associated** file descriptor is 0**. This is also represented as STDIN. The Unix program will **read** the default input from STDIN.

#### stdout
This is referred to as the **standard output** and the associated **file descriptor is 1**. This is also represented as STDOUT. The Unix program will **write** the default output at STDOUT

#### stderr
This is referred to as the **standard error** and the associated** file descriptor is 2**. This is also represented as STDERR. The Unix program will **write** all the **error** messages at STDERR.

### Absolute/Relative path names
A pathname is **absolute**, if it is described in relation to **root**, thus absolute pathnames **always begin with a /.**
Following are some examples of absolute filenames.
*/etc/passwd
/users/sjones/chem/notes
/dev/rdsk/Os3*

A pathname can also be **relative** to your **current working** directory. Relative pathnames **never begin with /**
*chem/notes
personal/res*

### File permissions
Every file in Unix has the following attributes:
**Owner permissions** − The owner's permissions determine what actions the owner of the file can perform on the file.
**Group permissions** − The group's permissions determine what actions a user, who is a member of the group that a file belongs to, can perform on the file.
**Other (world) permissions** − The permissions for others indicate what action all other users can perform on the file.

#### Changing Permissions
To change the file or the directory permissions, you use the chmod (change mode) command. There are two ways to use chmod — the symbolic mode and the absolute mode.

**Using chmod in Symbolic ModeUsing chmod in Symbolic Mode**

Sr.No. Chmod operator & Description
1 + Adds the designated permission(s) to a file or directory.

2 - Removes the designated permission(s) from a file or directory.

3 = Sets the designated permission(s).

g Group
u User
o Other

E.g.

*$ls -l testfile
-rwxrwxr--  1 amrood   users 1024  Nov 2 00:10  testfile
$chmod o+wx testfile
$ls -l testfile
-rwxrwxrwx  1 amrood   users 1024  Nov 2 00:10  testfile
$chmod u-x testfile
$ls -l testfile
-rw-rwxrwx  1 amrood   users 1024  Nov 2 00:10  testfile
$chmod g = rx testfile
$ls -l testfile
-rw-r-xrwx  1 amrood   users 1024  Nov 2 00:10  testfile*

Here's how you can combine these commands on a single line −
*$chmod o+wx,u-x,g = rx testfile
$ls -l testfile
-rw-r-xrwx  1 amrood   users 1024  Nov 2 00:10  testfile*

**Using chmod with Absolute Permissions**

Number	Octal Permission Representation	 Ref
0	         No permission	                                                                   ---
1	         Execute permission	                    									    --x
2	         Write permission	                                                              -w-
3	         Execute and write permission: 1 (execute) + 2 (write) = 3	    -wx
4	         Read permission	                                                              r--
5	         Read and execute permission: 4 (read) + 1 (execute) = 5	  r-x
6	         Read and write permission: 4 (read) + 2 (write) = 6	           rw-
7	         All permissions: 4 (read) + 2 (write) + 1 (execute) = 7	       rwx

E.g.
*$ chmod 755 testfile
$ls -l testfile
-rwxr-xr-x  1 amrood   users 1024  Nov 2 00:10  testfile
$chmod 743 testfile
$ls -l testfile
-rwxr---wx  1 amrood   users 1024  Nov 2 00:10  testfile
$chmod 043 testfile
$ls -l testfile
----r---wx  1 amrood   users 1024  Nov 2 00:10  testfile*

#### Changing Owners and Groups
**chown** − The chown command stands for "change owner" and is used to change the owner of a file.
*$ chown user filelist*
The value of the user can be either the name of a user on the system or the user id (uid) of a user on the system

**chgrp** − The chgrp command stands for "change group" and is used to change the group of a file.
*$ chgrp group filelist*
The value of group can be the name of a group on the system or the group ID (GID) of a group on the system.


#### SUID and SGID File Permission

As a **regular user, you do not have read or write access to this file** for security reasons, but when you change your password, you need to have the write permission to this file. This means that the passwd program has to give you additional permissions so that you can write to the file /etc/shadow.

Additional permissions are given to programs via a mechanism known as the **Set User ID (SUID) and Set Group ID (SGID) bits.**

When you execute a program that has the SUID bit enabled, you inherit the permissions of that program's owner. Programs that do not have the SUID bit set are run with the permissions of the user who started the program.

This is the case with SGID as well. **Normally, programs execute with your group permissions, but instead your group will be changed just for this program to the group owner of the program.**

The SUID and SGID bits will appear as the letter **"s" **if the permission is available. The SUID "s" bit will be located in the permission bits where the owners’ **execute** permission normally resides.

For example, the command −

*$ ls -l /usr/bin/passwd
-r-**s**r-xr-x  1   root   bin  19031 Feb 7 13:47  /usr/bin/passwd**

Shows that the SUID bit is set and that the command is owned by the root. A capital letter **S** in the execute position instead of a lowercase s indicates that the** execute bit is not set**.

If the sticky bit is enabled on the directory, files can only be removed if you are one of the following users:
The owner of the sticky directory
The owner of the file being removed
The super user, root

To set the SUID and SGID bits for any directory try the following command:
*$ chmod **ug+s **dirname
$ ls -l
d**rwsr-s**r-x 2 root root  4096 Jun 19 06:45 dirname*

### Environement
When you log in to the system, the shell undergoes a phase called **initialization** to **set up the environment**. This is usually a two-step process that involves the shell reading the following files in the order:
- /etc/profile
- ~/.profile
**Note** − The shell initialization process detailed here applies to all Bourne type shells, but some additional files are used by **bash** and **ksh**.

The file **/etc/profile** is maintained by the **system administrator** of your Unix machine and contains shell initialization information required by **all users** on a system.

#### PS1 and PS2 Variables
The file** .profile **is maintaned by **you**. You can add as much shell **customization** information as you want to this file. The minimum set of information that you need to configure includes −
The type of terminal you are using.
A list of directories in which to locate the commands.
A list of variables affecting the look and feel of your terminal.

The characters that the shell displays as your **command (Primary) prompt** are stored in the variable **PS1**.
*$PS1='=>'
=>
=>
=>*
Your prompt will become =>. To set the value of PS1 so that it shows the working directory, issue the command −
*=>PS1="[\u@\h \w]\$"
[root@ip-72-167-112-17 /var/www/tutorialspoint/unix]$
[root@ip-72-167-112-17 /var/www/tutorialspoint/unix]$*
The result of this command is that the prompt displays the user's username, the machine's name (hostname), and the working directory.

The default** secondary prompt** is > (the greater than sign), but can be changed by re-defining the** PS2** shell variable
Following is the example which uses the default secondary prompt:
*$ echo "this is a
>  test"
this is a
test
$

The example given below re-defines PS2 with a customized prompt:
*$ PS2="secondary prompt->"
$ echo "this is a
secondary prompt->test"
this is a
test*
$

#### Escape Sequence & Description
**\t**		Current time, expressed HH:MM :SS

**\d**		Current date, expressed as Weekday Month Date

**\n**		Newline

**\s**		Current shell environment

**\W**		Working directory

**\w**		Full path of the working directory

**\u**		Current user’s username

**\h**		Hostname of the current machine

**\#**		Command number of the current command. Increases when a new command is entered

**\$**		If the effective UID is 0 (that is, if you are logged in as root), end the prompt with the # character; otherwise, use the $ sign

#### Environment Variables
Following is the partial list of important environment variables. These variables are set and accessed as mentioned below −

**DISPLAY**			Contains the identifier for the display that X11 programs should use by default.

**HOME**		Indicates the home directory of the current user: the default argument for the cd built-in command.

**IFS**			Indicates the Internal Field Separator that is used by the parser for word splitting after expansion.

**LANG**		LANG expands to the default system locale; LC_ALL can be used to override this. For example, if its value is pt_BR, then the language is set to (Brazilian) Portuguese and the locale to Brazil.

**LD_LIBRARY_PATH**			A Unix system with a dynamic linker, contains a colonseparated list of directories that the dynamic linker should search for shared objects when building a process image after exec, before searching in any other directories.

**PATH**		Indicates the search path for commands. It is a colon-separated list of directories in which the shell looks for commands.

**PWD**			Indicates the current working directory as set by the cd command.

**RANDOM**			Generates a random integer between 0 and 32,767 each time it is referenced.

**SHLVL**		Increments by one each time an instance of bash is started. This variable is useful for determining whether the built-in exit command ends the current session.

**TERM**		Refers to the display type.

**TZ**			Refers to Time zone. It can take values like GMT, AST, etc.


**UID**			Expands to the numeric user ID of the current user, initialized at the shell startup.


#### The grep Command
There are various options which you can use along with the grep command −

**-v**			Prints all lines that do not match pattern.

**-n**			Prints the matched line and its line number.

**-l**			Prints only the names of files with matching lines (letter "l")

**-c**			Prints only the count of matching lines.

**-i**			Matches either upper or lowercase.

#### The sort Command

**-n**			Sorts numerically (example: 10 will sort after 2), ignores blanks and tabs.

**-r**			Reverses the order of sort.

**-f**			Sorts upper and lowercase together.

**+x**			Ignores first x fields when sorting.

**Listing Running Processes**
**ps axu** 	  Display all processes in BSD format.
 ps -A			Display every active process on a Linux system in generic (Unix/Linux) format.
ps -ef			full-format listing
ps -x			select all processes owned by you
ps -fG apache			list all processes owned by a certain group
ps -fp 2226,1154,1146			Make selection using PID list
ps -e --forest			Print Process Tree
 ps -T -p <pid>			show threads for a process

#### Stopping Processes
**Process is running in the foreground mode:**
Ctrl + C is used to kill a process with signal SIGINT , in other words it is a polite kill
Ctrl + Z is used to suspend a process by sending it the signal SIGTSTP , which is like a sleep signal, that can be undone and the process can be resumed again.
**Process is running in the background**
kill -9 <PID>
9 **SIGKILL**

#### Orphan Process
Normally, when a child process is killed, the parent process is updated via a **SIGCHLD** signal. Then the parent can do some other task or restart a new child as needed. However, sometimes the **parent process is killed before its child is killed**. In this case, the "parent of all processes," the** init** process, becomes the new PPID (parent process ID). In some cases, these processes are called orphan processes.Orphan process are still **running** even if the parent process dies

#### Zombie Process
When a process is **killed**, a **ps **listing may still **show** the process with a **Z state**. This is a zombie or defunct process. The process is **dead** and not being used. These processes are different from the orphan processes. They have **completed** execution but still find an **entry** in the **process table**.

#### Background process 
Runs without being connected to your keyboard. It has standard streams (input, output, error) **connected to that parent** (Shell). The most common type is when you run a shell program with a trailing** &**

#### Daemon Processes
Daemons are **system-related background processes** that often run with the permissions of **root** and services requests from other processes.
A daemon has **no controlling terminal.**  If you do a "**ps -ef**" and look at the tty field, all daemons will have a **?** for the tty. **No Parent**

Daemon is a process that runs in the background, usually **waiting** for something to happen that it is capable of working with. For example, a printer daemon waiting for print commands.

#### The top Command
Diagnostic tool
The top command is a very useful tool for quickly showing processes sorted by various criteria.

#### Job ID Versus Process IDJob ID Versus Process ID
**Background and suspended processes** are usually manipulated via **job number** (job ID). This number is different from the process ID and is used because it is shorter.
In addition, a** job can consist of multiple processes** running in a series or at the same time, in parallel. Using the job ID is easier than tracking individual processes.

#### SIGNALS
```
➜  argus_host git:(argus-poc) ✗ kill -l
HUP INT QUIT ILL TRAP ABRT EMT FPE KILL BUS SEGV SYS PIPE ALRM TERM URG STOP TSTP CONT CHLD TTIN TTOU IO XCPU XFSZ VTALRM PROF WINCH INFO USR1 USR2
```

### Shell script

```
echo "What is your name?"
read PERSON
echo "Hello, $PERSON"
```
```
NAME="Haris Farooqui"
readonly NAME
```
```
NAME="Haris Farooqui"
unset NAME
echo $NAME
```
```
LST=(haris rashid farooqui)
echo ${LST[@]}
```
There must be **spaces between operators and expressions**. </br>
For example, 2+2 is not correct; it should be written as 2 + 2.</br>
val=````expr 2 + 2````

The complete expression should be enclosed between ‘ ‘, called the backtick.
echo "Total value : $val"


### Shell Types
two major types of shells:

<b>Bourne shell</b> −$ character is the default prompt.

<b>C shell</b> − % character is the default prompt.

The Bourne Shell has the following subcategories −
- Bourne shell (sh)
- Korn shell (ksh)
- Bourne Again shell (bash)
- POSIX shell (sh)

The different C-type shells follow −
- C shell (csh)
- TENEX/TOPS C shell (tcsh)

### Variable Types
When a shell is running, three main types of variables are present −

<b>Local Variables</b>: Available only to shell instance
NAME="Haris Faroqui"

<b>Environment Variables</b>:Available to any child process of the shell

<b>Shell Variables</b>:Variable that is set by the shell
- <b>$0</b>      The filename of the current script.
- <b>$?</b>      The exit status of the last command executed.
- <b>$!</b>      The process number of the last background command.
- <b>$n</b>      Value for nth argument
- <b>$#</b>      The number of arguments supplied to a script.
- <b>$$</b>      The process ID of the current shell.
- <b>$*</b>      All the arguments are double quoted.
- <b>$@</b>      All the arguments are individually double quoted.</br>
    $* and $@ both will act the same unless they are enclosed in double quotes, "".</br>
    Both the parameters specify the command-line arguments.</br>
    However, the "$*" special parameter takes the entire list as one argument with spaces</br> 
    "$@" special parameter takes the entire list and separates it into separate arguments.</br>

### Exit status
- 0 Successful
- 1 Unsuccessful

### Operators
<b>Arithmetic operators</b>

-     + (Addition)	    `expr $a + $b` will give 30
-     - (Subtraction)	    `expr $a - $b` will give -10
-     * (Multiplication)	`expr $a \* $b` will give 200
-     / (Division)	    `expr $b / $a` will give 2
-     % (Modulus)	        `expr $b % $a` will give 0
-     = (Assignment)	    a = $b would assign value of b into a
-     == (Comparision)	[ $a == $b ] would return false.
-     != (Not Equality)	[ $a != $b ] would return true.

It is very important to understand that all the conditional expressions should be inside <b>square braces with spaces </b> around them, for example [ $a == $b ] is correct whereas, [$a==$b] is incorrect.


<b>Relational Operators</b>
- **-eq**		two operands are equal or not; if yes, then the condition becomes true.
- **-ne		**two operands are equal or not; if not equal, then the condition becomes true.
- **-gt	** left operand is greater than right operand
- **-lt	**left operand is less than the value of right operand
- **-ge**	left operand is **greater than or equal **to the value of right operand; 
- **-le**	left operand is **less than or equal** to the value of right operand;
- It is very important to understand that all the conditional expressions should be placed inside **square braces with spaces around them**. For example, ```[ $a <= $b ]``` is correct whereas, ```[$a <= $b]``` is incorrect.

**Boolean Operators**
- **!**	This is logical negation. [ ! false ] is true.
- **-o**	This is logical OR. If one of the operands is true, then the condition becomes true.	</br>
```[ $a -lt 20 -o $b -gt 100 ]``` is true.
- **-a**	This is logical AND. If both the operands are true, then the condition becomes true otherwise false. 
```[ $a -lt 20 -a $b -gt 100 ]``` is false.

**String Operators**
- **=**	 two operands are equal or not; [ $a = $b ] is not true.
- **!=**	two operands are equal or not;  	[ $a != $b ] is true.
- **-z**	string operand size is zero

```
if [ ! -z "$str3" ]; then
        echo "Str3 is not null"
fi
```

- **-n**	operand size is non-zero
- **str**	Checks if str is not the empty string; if it is empty, then it returns false.[ $a ] is not false.

```
if [ ! "$str" ];then
   echo NULL
fi
```

**File Test Operators**
- **-b** file is a block special file
- **-c** file is a character special file
- **-d** file is a directory
- **-f** file is an ordinary file as opposed to a directory or special file
- **-g** file has its set group ID (SGID) bit set
- **-k** file has its sticky bit set
- **-p** file is a named pipe
- **-t** file descriptor is open and associated with a terminal
- **-u** file has its Set User ID (SUID) bit set
- **-r** file is readable
- **-w** file is writable
- **-x** file is executable
- **-s** file has size greater than 0
- **-e** file exists; is true even if file is a directory but exists   

### Decision making
```
#!/bin/sh

a=10
b=20

if [ $a == $b ]
then
   echo "a is equal to b"
elif [ $a -gt $b ]
then
   echo "a is greater than b"
elif [ $a -lt $b ]
then
   echo "a is less than b"
else
   echo "None of the condition met"
fi
```

```
#!/bin/sh
FRUIT="kiwi"
case "$FRUIT" in
   "apple") 
   echo "Apple pie is quite tasty." 
   ;;
   "banana") 
   echo "I like banana nut bread." 
   ;;
   "kiwi") 
   echo "New Zealand is famous for kiwi." 
   ;;
esac
```

### Loops
- The **break** statement, exits the loop
- The **continue** statement, exits the current iteration
- while...do...done
- for...in...do...done
- until...do...done
```
################################
E.g.1
################################
#!/bin/sh
a=0
while [ "$a" -lt 10 ]    # this is loop1
do
   b="$a"
   while [ "$b" -ge 0 ]  # this is loop2
   do
      echo -n "$b " # echo -n to avoid printing newline character
      b=`expr $b - 1`
   done
   echo
   a=`expr $a + 1`
done
################################
E.g.2
################################
#!/bin/sh
a=0
while [ $a -lt 10 ]
do
   echo $a
   if [ $a -eq 5 ]
   then
      break
   fi
   a=`expr $a + 1`
done
################################
E.g.3
################################
#!/bin/sh
for var1 in 1 2 3
do
   for var2 in 0 5
   do
      if [ $var1 -eq 2 -a $var2 -eq 0 ]
      then
         break 2 ###### Break out of inner and outer loop
      else
         echo "$var1 $var2"
      fi
   done
done
```

### Substitution
```
#!/bin/sh

a=10
echo -e "Value of a is $a \n"
```
You will receive the following result. Here the -e option enables the interpretation of backslash escapes.
```
Value of a is 10
```
Following is the result without -e option −
```
Value of a is 10\n
```

#### Command substitution
```
#!/bin/sh

DATE=`date`
echo "Date is $DATE"

USERS=`who | wc -l`
echo "Logged in user are $USERS"

UP=`date ; uptime`
```
```
Date is Thu Jul  2 03:59:57 MST 2009
Logged in user are 1
Uptime is Thu Jul  2 03:59:57 MST 2009
03:59:57 up 20 days, 14:03,  1 user,  load avg: 0.13, 0.07, 0.15
echo "Uptime is $UP"
```

#### Variable substitution
**${var}**
Substitute the value of var.

**${var:-word}**
If var is null or unset, word is substituted for var. The value of var does not change.
	
**${var:=word}**
If var is null or unset, var is set to the value of word.
	
**${var:?message}**
If var is null or unset, message is printed to standard error. This checks that variables are set correctly.
	
**${var:+word}**
If var is set, word is substituted for var. The value of var does not change.

```
#!/bin/sh

echo ${var:-"Variable is not set"}
echo "1 - Value of var is ${var}"

echo ${var:="Variable is not set"}
echo "2 - Value of var is ${var}"

unset var
echo ${var:+"This is default value"}
echo "3 - Value of var is $var"

var="Prefix"
echo ${var:+"This is default value"}
echo "4 - Value of var is $var"

echo ${var:?"Print this message"}
echo "5 - Value of var is ${var}"
```
```
Variable is not set
1 - Value of var is
Variable is not set
2 - Value of var is Variable is not set

3 - Value of var is
This is default value
4 - Value of var is Prefix
Prefix
5 - Value of var is Prefix
```

**Here Document**
Here the shell interprets the << operator as an instruction to read input until it finds a line containing the specified delimiter. All the input lines up to the line containing the delimiter are then fed into the standard input of the command.
The delimiter tells the shell that the here document has completed. Without it, the shell continues to read the input forever. The delimiter must be a single word that does not contain spaces or tabs.
```
$wc -l << EOF
   This is a simple lookup program 
	for good (and bad) restaurants
	in Cape Town.
EOF
3
$
```
```
#!/bin/sh

cat << EOF
This is a simple lookup program 
for good (and bad) restaurants
in Cape Town.
EOF
```
```
This is a simple lookup program
for good (and bad) restaurants
in Cape Town.
```
**Input Redirection**
```
$ wc -l users
2 users
$
```
```
$ wc -l < users
2
$
```
**Output Redirection**

**>** wipes out the contents
**>>** appends to existing contents
```
$ who > users

$ cat users
oko         tty01   Sep 12 07:30
ai          tty15   Sep 12 13:32
ruth        tty21   Sep 12 10:10
pat         tty24   Sep 12 13:07
steve       tty25   Sep 12 13:03
$

$ echo line 1 > users
$ cat users
line 1
$

$ echo line 2 >> users
$ cat users
line 1
line 2
$
```

**Discard the output**
**
- 0 STDIN
- 1 STDOUT
- 2 STDERR
**

Following retains stderr, but suppresses stdout.
```command > /dev/null```

Same as above. Here it redirect stderr to stdout and stdout to /dev/null in order to get only stderr on your terminal
```command 2>&1 > /dev/null```

To discard both output of a command and its error output, use standard redirection to redirect STDERR to STDOUT −
```$ command > /dev/null 2>&1```

### Shell functions
```
#!/bin/sh

# Calling one function from another
number_one () {
   echo "This is the first function speaking..."
   number_two
}

number_two () {
   echo "This is now the second function speaking..."
}

# Calling function one.
number_one
```
```
#!/bin/sh

# Define your function here
Hello () {
   echo "Hello World $1 $2"
   return 10
}

# Invoke your function
Hello Haris Farooqui

# Capture value returnd by last command
ret=$?

echo "Return value is $ret"
```

### sed

**p**  Prints the line
`sed -n '1,2p' `
`sed -n '/haris/p'`  Prints all matching lines which has haris in it
**d**  Deletes the line
`sed '1d' `

**s** substitutes string
`s/pattern1/pattern2/`  Substitutes the first occurrence of pattern1 with pattern2
`sed '10s/a/b/g'` Replaces 'a' with 'b' on 10th line
`s|pattern1/|pattern2/` . Alternative String Separator ('|' instead of '/')

**g**  Replaces all matches, not just the first match. `s/pattern1/pattern2/`

**sed delete**
```
➜  ~ cat file.txt
Haris Farooqui
Omar Farooqui
Osman Farooqui
Amina Ahmad
Yasir Ahmad
```
```
➜  ~ cat file.txt | sed '1d' |more
Omar Farooqui
Osman Farooqui
Amina Ahmad
Yasir Ahmad
```
**sed range**
```
➜  ~ cat file.txt | sed '2, 4d' |more
Haris Farooqui
Yasir Ahmad
```
```
➜  ~ cat file.txt | sed -n '1,2p' |more
Haris Farooqui
Omar Farooqui
```
```
➜  ~ cat file.txt | sed s/Yasir/Hafsa/g |more
Haris Farooqui
Omar Farooqui
Osman Farooqui
Amina Ahmad
Hafsa Ahmad
```

**'4,10d'**		Lines starting from the 4th till the 10th are deleted
**'10,4d'**		Only 10th line is deleted, because the sed does not work in reverse direction
**'4,+5d'**		This matches line 4 in the file, deletes that line, continues to delete the next five lines, and then ceases its deletion and prints the rest
**'2,5!d'**		This deletes everything except starting from 2nd till 5th line
**'1~3d'**		This deletes the first line, steps over the next three lines, and then deletes the fourth line. Sed continues to apply this pattern until the end of the file.
**'2~2d'**This tells sed to delete the second line, step over the next line, delete the next line, and repeat until the end of the file is reached
**'4,10p'**		Lines starting from 4th till 10th are printed
**'4,d'**		This generates the syntax error
**',10d'**		This would also generate syntax error
`~ Does not work in zsh`

#### Regular Expressions
**^**		Matches the beginning of lines

**$**		Matches the end of lines

**.**		Matches any single character
**/a.c/**		Matches lines that contain strings such as **a+c**, **a-c**, **abc**, **match**, and **a3c**

**\***		Matches zero or more occurrences of the previous character
**/a*c/**		Matches the same strings along with strings such as **ace**, **yacc**, and **arctic**

**[chars]**		Matches any one of the characters given in chars, where chars is a sequence of characters. You can use the - character to indicate a range of characters.
**/[tT]he/**		Matches the string The and the

**/^$/**		Matches blank lines
**/^.*$/**		Matches an entire line whatever it is
**/ */**		Matches one or more spaces


**[a-z]**		Matches a single lowercase letter
**[A-Z]**		Matches a single uppercase letter
**[a-zA-Z]**		Matches a single letter
**[0-9]**		Matches a single number
**[a-zA-Z0-9]**		Matches a single letter or number

**[[:alnum:]]**		Alphanumeric [a-z A-Z 0-9]
**[[:alpha:]]**		Alphabetic [a-z A-Z]
**[[:blank:]]**		Blank characters (spaces or tabs)
**[[:cntrl:]]**		Control characters
**[[:digit:]]**		Numbers [0-9]
**[[:graph:]]**		Any visible characters (excludes whitespace)
**[[:lower:]]**		Lowercase letters [a-z]
**[[:print:]]**		Printable characters (non-control characters)
**[[:punct:]]**		Punctuation characters
**[[:space:]]**		Whitespace
**[[:upper:]]**		Uppercase letters [A-Z]
**[[:xdigit:]]**		Hex digits [0-9 a-f A-F]
```
$ cat phone.txt
5555551212
5555551213

$ sed -e 's/^[[:digit:]][[:digit:]][[:digit:]]/(&)/g' phone.txt
(555)5551212
(555)5551213
```

**Aampersand Referencing**
```
➜  ~ sed -e 's/^.*/(&)/g' file.txt
(Haris Farooqui)
(Omar Farooqui)
(Osman Farooqui)
(Amina Ahmad)
(Yasir Ahmad)
```

**Multiple sed command**
```
$ sed -e 's/^[[:digit:]]\{3\}/(&)/g'  \ 
   -e 's/)[[:digit:]]\{3\}/&-/g' phone.txt 
(555)555-1212 
(555)555-1213 
```

**Back References**
To do back references, you have to first define a **region** and then refer back to that region. To define a region, you insert** backslashed parentheses \\(...\\)** around each region of interest. The first region that you surround with backslashes is then **referenced** by **\1**, the second region by** \2,** and so on.
```
➜  ~ cat phone.txt
(555)555-1212
(555)555-1213

$ cat phone.txt | sed 's/\(.*)\)\(.*-\)\(.*$\)/Area \ 
   code: \1 Second: \2 Third: \3/' 
Area code: (555) Second: 555- Third: 1212 
Area code: (555) Second: 555- Third: 1213 
```

#### Less vs more
Learn Linux 'less' Command. Similar to more, less command allows you to **view the contents of a file and navigate through file.** The main difference between more and less is that** less command is faster because it does not load the entire file at once** and allows navigation though file using page up/down keys.

```
### disk free
$df -k
Filesystem      1K-blocks      Used   Available Use% Mounted on
/dev/vzfs        10485760   7836644     2649116  75% /
/devices                0         0           0   0% /devices
```
```
### disk usage
$du -h /etc
5k    /etc/cron.d
63k   /etc/default
3k    /etc/dfs
```

#### mount
```
mount -t file_system_type device_to_mount directory_to_mount_to
umount device_to_mount
```
