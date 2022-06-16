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
\
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

