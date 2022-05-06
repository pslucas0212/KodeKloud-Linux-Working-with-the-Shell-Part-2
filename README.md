# KodeKloud Linux Basics: Working with the Shell Part 2

[KodeKloud Linux Basics Course Notes Table of Contents](https://github.com/pslucas0212/LinuxBasics)

## File Compression and Archival

To view file sized in linux we can use the "du" utility.  The du utility shows the file size in kilobytes

```
$ du -sk .bash_history 
32	.bash_history
```
Use -h to make the output more human readable
```
$ du -sh .bash_history 
32K	.bash_history
```
```
$ ls -lh .bash_history 
-rw------- 1 pslucas pslucas 26K May  4 17:47 .bash_history
```
  Disk usage command
```

TAR utility is used to group mutliple files and directories into a single smaller file.  TAR stands for Tape Archive.  Files created with TAR are called TAR balls.

Use -c to compress and -f is use to specify name of the tar ball.
```
$ tar -cf test.tar file1 file2 file
```

To see contents of the tar ball use -tf
```
$ tar -tf test.tar
```

To extract contents from the tar ball use -xf
```
$ tar -xf test.tar
```
$ tar -zcf test.tar file1 file2 file
```

Compression is the technique to reduce file size.
 
 The three most popular compression utilities are:
 - bzip2
 - gzip
 - xz
  
 - Uncompress zipped files with bunzip for bzip2, gunzip for gzip, xunzip for xz
  
 - Use zcat,bzcat,xcat to read zip file contents without uncompressing the file
  
  #### Searching for files and directories
  Locating files or directories
  
  Find files with city.txt
  ```
  $ locate city.txt
  ```
Locate will return all files with path anf the file name.  This command depends on the locate db.  This command may not "work" if the database hasn't been update. 
  
 Update the lcoate database
  
``` 
$ sudo updatedb
```
  
You can alternatively use use the find command with -name for name of file.  Find is very powerful
  ```
 $ find /home/michael -name City.txt
 ```
  
 Search for patters within files or out put use GREP. Note that grep is case sensitive
 - search for word second in the file sample.txt   This will print the line(s) where the search pattern is found.
  ```
  $ grep second sample.txt
  ```
  To make the grep search case insensitive use the -i switch with grep\
  ```
  $ grep -i Capital sample.text
  ```
  Search pattern recursively in a directory
  ```
  $ grep -r "third line" /home/michael
  ```
  Search for a string through set of files in a directory.  Note in the example below to search all of /etc we need to be "root"
  ```
  $ sudo grep -ir 172.16.238.197 /etc/
  ```
  Show all the lines in a file that don't have a particular pattern
  ```
  $ grep -v "printed" sample.txt
  ```
  
  Example matching whole words or phrases.  You have the following file
  ```
  $ cat examples.txt 
grep examples
linux exam on 19th
```
 If you grep "exam" you will get the following
```
$ grep exam examples.txt 
grep examples
linux exam on 19th
```
  Find only whole words
  ```
  $ grep -w exam examples.txt 
linux exam on 19th
  ```
  Skip lines with the search word
  ```
  $ grep -wv exam examples.txt 
grep examples
  ```
  
 Consider the following file:
 ```
 $ cat premier-league-table.txt 
1 Arsenal
2 Liverpool
3 Chelsea
4 Manchester City
```

 To see the team that finished after Aresenal, use -A for seeing the number of lines after a search string:
```
$ grep -A1 Arsenal premier-league-table.txt 
1 Arsenal
2 Liverpool
  ```
 
  To see the team that finished after Aresenal, use -B for seeing the number of lines before a search string:
```
$ grep -B1 "Manchester City" premier-league-table.txt 
3 Chelsea
4 Manchester City
```

You can combine -A and -B in a search
```
grep -A1 -B1 Chelsea premier-league-table.txt 
2 Liverpool
3 Chelsea
4 Manchester City
```
  
  #### IO Redirection
  There are three data streams created when you launch a linux command
  - Standard Input - stdin - accepts input - $ cat sample.txt - aka (std0)
  - Standard ouput - stdout - aka (std1)
  - standard Error - stderr - redirect to file - aka (std2)
  
  Redirect stdout.  This will overwrite the contents of the file
  ``` 
  $ echo $SHELL > shell.txt
  ```
  Append stdout to a file
  ```
  $ echo "some text" >> shell.txt
  ```
  
Redirect STDERR.  You must use the number 2.  If the file is missing it will create file
```
 $ cat missing_file 2>> error.txt
 ```
  Dump stderr. Send the output to the bit bucket /dev/null
```
$ cat missing_file 2> /dev/null
 ```
  
#### Command Line Pipes
Use command line pipe to send standard out of one command as the standard in for another command.  Use the vertical line symbol to pipe '|'
```
$ grep hello sample.txt | less
```
Pipes output to less command

You can use as many pipes as needed
 
Tee command - standard output printed to screen before overwriting file
```
  $ echo $SHELL | tee shell.txt
```
Use -a to append to file
```
$ echo $SHELL | tee -a shell.txt
```
  
#### VI editor
Most popular text editor used with Linux

- open a file
```
 $ vi sample.txt
```

- Three operation modes in VI
  - Command - VI starts in the command and only understands commands
  - Instert
  - Last line
  
 - In the command you can Copy, paste, delete or navigate to a specific line
 
- Switch from command line mode to insert mode type i
- Switch from insert mode to command by hitting the Esc key
- Switch to last line mode type :
- You can also enter the Insert mode with O A I o a I
  
Command Mode
- The mouse cursor allows you to highlight and the copy, cut, paste or inster text
- copy, cut, past, insert text
- use arrow keys or hjkl to navigate key
- to copy a line, move the cursor to the line and type yy
- Next move the cursor above the line where you want to paste the text and use p
- ZZ to save file
- to delete charactaer, move cursor to the desired character and type x
- to delete entire line move the cursor anywhere on the line you are deleting and  type dd
- to delete 3 lines type d3d.  Change the 3 to any number of lines you want to delet
- to undo a change, type u
- To redo a change type Ctrl-r
- to find text type /<text>, to move down through the text, type n to move down, to move up N
- move ? to move up the file
- to enter inser moded type i, o, a, or A  Last line wil indidcate that you are in Insert mode
  - i - insert
  - a - append
  - overwrite
- to go to last line mode type :
    - to save file type :w
    - to quite type : then q
    - to write and quite type : then wq
    - to quit with out making changes type :q!
 
VIM is VI improved - sometimes vi now points to vim.

## Security and File Permissions

- Acess Controls -
- PAM - Pluggable Authentication Modle - authenticate users to programs and services
- Network security - secuirty applied to services usingnetworking with iptables and firewalld
- SELinux - security policies to isolate services/processes running on the system
- SSH hardening - security login
  
#### Linux Accounts
- Every user on linux has a linux accoutn with user name, user id, and password to logon to the system.  Ever user has a unique user id.  Information about users is stored in the /etc/passwd file
- Group is a collection of users that have a common need for accessing particular Linux resources.  Information about groups is stored in the /etc/group file.  Each group have a unique id called the gid
  
- Each user has a username, unique id - UID and belongs to a group with a group id - GID.  Run the following information to get user information:
```
$ id <username>
$ grep <username> /etc/passwd
  ```
- we can see see the users home director and shell by grepping on their name in the /etc/passwd file
  
User account type refers to individual users that need access to Linux serves

- Superuser account has a uid = 0 and has complet control over the linux system
- System accounts - UID <100 or between 500 - 1000
- Service account - run nginx, etc.
                            
- Run who command to see who is logged in and the last time the system was rebooted
```
$ who
```
#### Switch users
You can use the su command to switch to root or another user, but this is not recommended as you need the password of the user you are switching to
```
$ su -
$ su -c whoami
```
Sudo is the recommended command for priveleged escalation.  To run commands as root user.  The user is prompted for their password.  Sudo users and privelees are defined in the /etc/sudoers file.    
sudoers file
  
#### User Management
Managing Users:
Commands to Add (create user) user; see user bob's uid and gid, home directory and shell; "see" bob's password setting in the /etc/shadow file, set Bob's password, check how you are logged into the system and change your password...
```
$ useradd bob
$ grep -i bob /etc/passwd
$ grep -i bob /etc/shadow
$ passwd bob
$ whoami
```
  
Common options to use with useradd
* -c custom comments
* -d customer home directory
* -e expirdy date
* -G creat use with mutilple secondary groups
* -s specifiy login shell
* -u specify UID

```
$ useradd -u 1099 -g 1009 -d /home/robert -s /bin/bash -c "Mercury Project Member" bob
$ id bob
$ grep -i bob /etc/passwd
```
Delete user  
```
  $ userdel bob
```
Create group
```
$ groupadd -g 1011 developer
```
Delete group
```
$ groupdel developer
```
#### Access Control Files
These files are found under /etc directory, are only accessible to root, and should never be modified directly with vi or VIM, but with their "special" editor.  

- /etc/passwd - contains information about users including user name, id, groups, home directory and shell
```
$ grep -i bob /etc/passwd
```
- /etc/shadow - containers users password that is hashed
- /etc/groups - contains groups
  
/etc/passwd contains user information  
```
$ grep -i pslucas /etc/passwd
```
USERNAME:PASSWORD:UID:GID:GECOS:HOMEDIR:SHELL  
pslucas:x:1000:1000::/home/pslucas:/bin/bash  
Password is always x as the password is kept in the /etc/shadow file.  The GECOS CSV comma separated other information include full name, phone number and location information is optional  
  
/etc/shadow
```
$ grep -i pslucas /etc/passwd 
```
USERNAME:PASSWORD:LASTCHANGE:MINAGE:MAXAGE:WARN:INACTIVE:EXPDATE
username, hashed password, the rest self explanatory - Note: LASTCHANGE the date since the password last changed is Epic.  Minimum and max days before they need to change password,  number of days to warn the user before the password expiration.  EXPDAT - nunmber of days when the account expires in an epic date format   
  
/etc/group
```
$ grep -i pslucas /etc/group
```
NAME:PASSWORD:GID:MEMBERS
Group name, password set to x saved in the shadow file, Group ID, memeber list comma separated
 
#### Linux File Permissions  
  
Use ls -l command to get information about file type and permissions  
  
File Type | Identifier
--------- | ----------
Directory | d
Regular File | -
Character Devicd | c
Link | l
Socket File | s
Pipe | p
Block device | b  
 
File permission example  -rwxrwxr-x
first three characters is for User - u  
seconde three charactars is for Group - g  
Third three characters is for Other - o  
  
File Permissions  
  
Bit | Purpose | Octal Value
--- | ------- | ------------
 r | Read | 4
 w | Write | 2
 x | Execute | 1  
  
Directory Permissions  
  
Bit | Purpose | Octal Value
--- | ------- | ------------
 r | Read | 4
 w | Write | 2
 x | Execute | 1
'-' | No Permission | 0  
  
Directory permission heirachy  
Permissions first check owner if owner then only owner permissions applied  
Next check group permission if not owner, but member of group then group permissions apply  
Finally check other permission
  
File Permission Example  
  
Example 1 | Example 2 | Example 3 | Example 4
--------- | --------- | --------- | ---------
rwx | rw- | -wx | r-x
4+2+1 | 4+2+0 | 0+2+1 | 4+0+1
7 | 6 | 3 | 5  
  
Changing file permissions  
chmod <permissions> file  
Change numerically or symbollically  
Symbolic mode example
 ```
 $ chmod u+rwx test-file
 $ chmod ugo+r test-file
 $ chmod o-rwx test-file
 $ chmod u+rwx,g+r-x,o-rwx test-file
```
Numeric mode example
 ```
 $ chmod 777 test-file
 $ chmod 555 test-file
 $ chmod 660 test-file
 $ chmod 750 test-file
```
  

Change ownership and group  - chown owner:group file
```
$ chown bob:developer test-file
$ chown bob andoid.pak
$ chgrp androd. test-file
```  
  
#### SSH and SCP  
SSH for logging into and executing commands on a remote computer  
ssh <hostname  or ip adderss>  
ssh <user@hostname>  
ssh -l <user> <hostname>
```
$ ssh devapp01
```
This uses the id that you are logged on locally to acess the remote server  
 
You can enable the passwordlesss login with keys - public and private key  
Setting up password-less SSH example:  
1. Create key-pair
```
$ ssh-keygen -t rsa
```
You can except the defaults or provide custoem information  
 
Notice where the keys are stored:  
Public key - /home/bob/.ssh/id_rsa.pub  
Private key - /home/bob/.ssh/id_rsa

2. Copy remote key to target remote server  
```  
$ ssh-copy-id bob@devapp01
```
The public key is now stored on the remote server here:  
/home/bob/.ssh/authorized_keys  
  
#### SCP  
SCP - secure copy for use with remote servers using TSL/SSL
```
$ scp /home/bob/caleston-code.tar.gz devapp01:/home/bob
```
Copy directory and preserve permissions example
```
$ scp -pr /home/bob/media /devapp01:/home/bob
```
  
