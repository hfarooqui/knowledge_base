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
