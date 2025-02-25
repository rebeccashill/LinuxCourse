# Linux Command Syntax
Command options and arguments
  - Commands typically have the syntax:
      command option(s) arguments(s)
Options:
  Modify the way that a command works
  Usually consists of a hyphen or dash followed by a single letter
  Some commands accept multiple options which can usually be grouped together after a single hyphen

ls -lt = order files based on last modified time
rm -f = remove file
ls -a, --all = do not ignore entries starting with .
ls -A, --almost-all = do not list implied . and ..
ls -l --author = print the author of each file
ls -b, --escape = print C-style escapes for nongraphic characters
ls --block-size=SIZE = scale sizes by SIZE before printing them

To see these options and more, use the command "man ls"


# Files and Directory Permissions (chmod)
There are 3 types of permissions:
  - r = read
  - w = write
  - x = execute - running a program
Each permission (rwx) can be controlled at 3 levels
  - u - user = yourself
  - g - group = can be people in the same project
  - o - other = everyone on the system
File or directory permission can be displayed by running ls -l command
chmod = command to change permission
  - man chmod to find out more

chmod g-w filename = get rid of write permission

chmod a-r filename = gets rid of all read permissions

chmod u-w filename = remove write permissions from user

chmod u+rw filename = adds read & write permissions to user

chmod o+rw filename = adds read & write permissions to others


# File Permissions Using Numeric Mode
Permission to a file and directory can also be assigned numerically
  - chmod ugo+r FILE    OR     - chmod 444 FILE
Owners assign permission on every file and directory
REMINDER: ls -ltr to see permissions
Assigning numbers to permission types
    0    No Permission       ---
    1    Execute             --x
    2    Write               -w-
    3    Execute+Write       -wx
    4    Read                r--
    5    Read+Execute        r-x
    6    Read+Write          rw-
    7    Read+Write+Execute  rwx

ex:chmod 764 FILE
    -rwxrw-r--
    
chmod 000 filename: remove permissions

chmod 604 -> -rw----r--

There are also online calculators for Linux permissions


# File Ownership Commands (chown, chgrp)
Command to change file ownership
  -chown = changes the ownership of a file
  -chgrp = changes the group ownership of a file
Recursive ownership change option (Cascade)
  - -R


# Access Control List (ACL)
What is ACL?
  - Access Control List (ACL)
  - Provides an additional, more flexible permission mechanism for file systems
  - Designed to assist with UNIX file permissions
  - Allows you to give permissions for any user or group to any disc resource
List of commands for setting up ACL
  -setfac1 -m u:user:rwx /path/to/file: to add permission for user
  -setfac1 -m g:group:rw: to add permissions for a group
  -setfac1 -dm "entry" /path/to/dir: to allow all files or directories to inherit ACL entries from the directory it is within
  -setfac1 -x u:user /path/to/file: to remove a specific entry
  -setfac1 -b path/to/file: to remove all entries (for all users)
Note:
  As you assign the ACL permission to a file/directory, it adds + sign at the end of the permission
  Setting w permission with ACL does not allow to remove a file
  Also, you should be using setfac1 and then getfac1 together

touch tx <------------------------> create file

ls -l tx <------------------------> -rw-r--r--

getfac1 tx <----------------------> #file: tx        user:: rw-

                                   #owner: root     group::r--
                                   
                                   #group: root     other::r--
                                   
setfac -m u:user:rwx /tmp/tx <----> sets permissions to the user

getfac1 /tmp/tx <-----------------> #file tmp/tx    user:: rw-

                                #owner: root    user:rebeccca:rw-
                                
                                #group: root    group::r--
                                
                                                mask::rw-
                                                
                                                other::r--
                                                
setfac -x u:rebeccca /tmp/tx <----> sets permissions to the user

getfac1 tmp/tx <------------------> #file tmp/tx    user:: rw-

                                    #owner: root    user:rebeccca:rw-
                                    
                                    #group: root    group::r--
                                                    mask::rw-
                                                    other::r--
                                                    
rm tx <---------------------------> action not permitted after the last setfac


# Help Commands
There are 3 types of help commands
  -whatis command
  -command --help
  -man command
whatis ls = ls(l), ls(lp) -> list directory contents
whatis pwd = pwd(l), pwd(lp) -> print name of current/working directory, return working directory name


# TAB Completion and Up Arrow Keys
Hitting TAB key completes the available commands, files, or directories
  - chm TAB
  - ls j<TAB>
  - cd Des<TAB>
Hitting up arrow key on the keyboard returns the last command ran
ch -> tab twice -> all the commands that start with ch
ls l -> tab twice -> all the files that start with 'l'

# Adding Text to Files
3 simple ways to add text to a file
  - vi
  - redirect command output > or >>
  - echo > or >>

echo "Jerry Seinfeld" > jerry
cat jerry -> "Jerry Seinfeld"
echo "Jerry show" >> jerry <--> overwrites last file
cat jerry --> "Jerry show"

date >> jerry (put date in jerry file)
cat jerry --> Fri Feb 21 7:30 EST 2025

touch listingofdir
ls -ltr > listingofdir (print output of this into the file)


# Input and Output Redirects (>, >>, <, stdin, stdout, and stderr)
There are 3 redirects in Linux
  -stdin = standard input, file descriptor # as 0
  -stdout = standard output, file descriptor # as 1
  -stderr = standard error, file descriptor # as 2
Output (stdout) - 1
  - By default when running a command its output goes to the terminal
  - The output of a command can be routed to a file using the > symbol
      1. ls -l > listings
      2. pwd > findpath
  - If using the same file for additional output or to append to the same file, then use >>
      1. ls -la >> listings
      2. echo "Hello World" >> findpath

pwd -> /home/user
pwd > findpath
cat findpath -> /home/user

Input (stdin) - 0
  - Input is used when feeding file contents to a file
  - Examples
      1. cat < listings
          mail -s "Office memo" allusers@abc.com < memoletter
  - Error (stderr) - 2
      - stdin -0: When a command is executed, we use a keyboard
      - stdin -1: The command output goes on the monitor
      - stderr -2: If the command produced any error on the screen, then it's considered to be this error
          1. We can use redirects to route errors from the screen
          2. ls -l / root 2 > errorfile
          3. telnet localhost 2 > errorfile

telnet localhost -> Trying::1..
                    Trying 127.0.0.1...
                    Connection refused
telnet localhost2 > errorfile
               ---> Trying::1..
                    Trying 127.0.0.1...
cat errorfile ----> Connection refused


# Standard Output to a File (tee command)
tee = command used to store & view (both at the same time) the output of any command
the command is named after the T-splitter used in plumbing
  - breaks the output of a program so that it can be displayed & saved in a file
  - does both tasks simultaneously, copies the result into the specified files or variables & also display the result
  - output goes into file and should show up on screen
                command -> tee -> stdout
                            |
                            -
                           file
Project:
echo "David Puddy" > eliane-david
  David Puddy
echo "David Puddy" | tee eliane
  David Puddy
cat eliane
  David Puddy
echo "David and Eliane" | tee eliane                        (Erases contents of the old file)
  David and Eliane
cat eliane
  David and Eliane
echo "David Puddy" | tee -a eliane
  David Puddy
cat eliane
  David and Eliane
  David Puddy

wc -c eliane                                                (how many characters do we have in a file?)
29 eliane

ls -l | tee listdir                                         (lists all directories)
cat listdir                                                 (lists all directories)
ls -l | tee file1 file2 file3                               (lists all directories in all 3 files)

tee --help
  - help w/ tee command


# Pipes (|)
A pipe is used by the shell to connect the output of one command directly to the input of another command
symbol for pipe is a vertical bar (|)
      command1 [arguments] | command2 [arguments]


# File Maintenance Commands (cat, less, more, head, tail)
cp = copy
rm = remove
mv = move
mkdir = make directory
rmdir or rm -r = remove directory
chgrp = change group
chown = change owner
rm -Rf = will forcefully remove sub-directories & its contents as well


**File Display Commands**
cat = display contents of a file
more = views content of a file one page at a time, used to navigate through text files
less = same as more, one screen at a time
head = view first lines in a file
tail = lines at the bottom of a file

cp /var/log/messages = copy messages to my location
less messages = display first page of messages file
cat messages = display the entire messages file
head -2 messages = display first 2 lines in a file
tail -1 jerry = display last line in jerry


**Filters/Text Processors Commands**
cut = allows you to cut the output
awk = list by the columns
grep and egrep = search by keywords
sort = sort command sorts output in alphabetical order
uniq = will not show any duplicates
wc = word count command (how many words, letters, & lines in a file)

**cut - Text Processors Commands**
cut filename = does not work
cut --version = check version
cut -c1 filename = list one character
cut -c1, 2, 4 = pick and choose character
cut -c1-3, 6-8 filename = list specific range of characters
cut -b1-3 filename = list by byte size
cut -d: -f 6 /etc/passwd = list first 6th column separated by :
cut -d: -f 6-7 /etc/passwd = list first 6 and 7th column separated by :
ls -l | cut -c2-4 = only print user permissions of files/dir
