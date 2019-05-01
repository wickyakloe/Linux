# Linux

Virtual consoles aka tty:

In shell:

```shell
chvt 3
```

Keyboard shorcuts:

GUI mostly on (different per distribution):
Ctrl+Alt+F1
or
Ctrl+Alt+F7

Some distributions use higher virtual consoles on:
tty untill tty12

Show users and status and virtual console:

```shell
$who
wickedPy :0           2019-04-24 12:08 (:0)
```

Show in which target system is started:

```shell
$systemctl get-default
graphical.target
```

Show cli no GUI:

```shell
$systemctl isolate multi-user.target
```

Show GUI:

```shell
$systemctl isolate graphical.target
```

Set default interface:

```shell
//For GUI
$systemctl set-default graphical.target

//CLI
$systemctl set-default multi-user.target
```

Open sub/new shell (Parent-child):

```shell
$bash

//set shell behaviour see "help set"
$help set
$set

```

Run script:

```shell
//In current shell
$.[scriptname.ext]
or
$source [scriptname.ext]

//In subshell after execution returns
to Parent shell
$bash [scriptname.ext]

//In specific situation run from memory
$exec [scriptname.ext]
```

Filter:

```shell
$grep

//pause output
$ls | less
```

Directory browsing:

```shell
//Show files in directory
$ls
README.md

//Show directory your in
$pwd
/home/wickedPy/Development/Linux

//make directory/file
$mkdir /foldername

//remove directory/file
$rm -r /foldername
```

Make a file:

```shell
$touch [filename.ext]
```

Show information on computer:

```shell
$uname
Linux

//show all
$uname -a

//show versionno. current kernel
$uname -r
```

Bash-history:

The shell saves typed commands in a file called ***.bash_history***

Access history:

```shell
//Show history
$history

//Clear history
$history -c
```

- Use the arrow keys up and down on the keyboard.

- Use Ctrl+R and type a part of the command
(this searches backwards in history) pressing again will search further on the same input

- Search number in history and use followed by **!**

```shell
$!17
```

- Use **!** followed by begin of command
(dangerous command gets executed right away without confirmation)

```shell
$!unam
uname -r
4.18.0-18-generic
```

! note extension do not play a role in Linux

## User Accounts

There are 2 types of users accounts in Linux

- privileged users ( root )
- unprivileged users ( normal user )

Show wich user u are:

```shell
$whoami
wickedPy
```

Open root-shell:

```shell
$su -
Password:
```

Set password for root:

```shell
$sudo passwd root
```

## Shell Variables

Standard variables definied at start are known as the **environment**

Show **environment** variables:

```shell
$env
```

Read variable:

```shell
$echo $[variable]

i.e.
$echo $PATH
```

Create variable:

```shell
$[VARIABLENAME] = [value]

i.e.
$TODAY=monday
```

Set variable to no value:

```shell
//using unset
$unset TODAY

//using variable
$TODAY=
```

Self set variables are not automatically available
in subshells, to make it available in a subshell use:

```shell
$export [VARIABLENAME] = [value]

i.e.
$export TODAY = monday
```

## Streams, pipes and redirects

- ***STDIN***: standard input - the input delivered to a command, mostly input from the keyboard. Also known as ***file descriptor 0***

- ***STDOUT***: standard output - the destination the command sends his output, mostly the window where the command is run. Also known as ***file descriptor 1***

- ***STDERR***: standard error - the desination where errors are sent, mostly the window where the command is run. Also know as ***file descriptor 2***

U can send the STDIN, STDOUT and STDERR somewhere else aka ***redirect***:

```shell
//redirect STDIN i.e. use contents of file as input of a command
$ <

//redirect STDOUT i.e. send output of command to file.
//The use of a single > creates a new file or overwrites it.
$ >
//Use >> to add to file
$ >>

//i.e. write output and errors to file wickedtest
$grep root /etc/* > ~/wickedtest

//redirect STDERR i.e. send error output to file
$ 2>

//i.e. search for root in /etc/* and send all errors to null device shows only STDOUT
$grep root /etc/* 2> /dev/null

//i.e. search for root in /etc/* and send all errors to a file wickedtest and show only STDOUT
$grep root /etc/* 2> ~/wickedtest

//i.e. search for root in /etc/* and send all errors to null device and send output STDOUT to file
$grep root /etc/* 2> /dev/null > ~/wickedtest

//Use 2>&1 to tell that STDERR must be used just like STDOUT
```

Pipes:

Use pipe to send the ouput of the first command to the input of the second command

```shell
$ls /etc/ | grep hosts

//use row for row from ouput as command for second input
$ls /etc/ | xargs grep hosts

//or split output i.e. write to file
and filter
$ls /etc/ | tee ~/file | grep host
```

## Processes

Show all active processes:

```shell
$ps aux

//filter out specific process
$ps aux | grep bash
```

## Using Help

Using man aka System Programmers Manual opens in less pager:

```shell
$man passwd

//show description of manpages 1-9
$man 3 intro

//show all information
$man -a passwd

//search command in man
$man -k time

//filter on section 1
$man -k time | grep 1
```

Sometime information is in info:

```shell
$info ls
```

## Working with text files

use cat, head, tail to show contents of file streamed:

use command wc - wordcount to count rows, words, chars
use command nl - to show linenumbers
use command cut -d : -f 1 to use delimiter and show field
use command sort to sort on ascii use sort -f to sort alphabetical use sort -n to sort on number.

```shell
//shows all contents of files
$cat filename.txt

//shows the first 10 rows of file
$head filename.txt

//shows the last 10 rows of file
$tail filename.txt

//use tail with option -f to stream, so if new line is added show this immediatly
$tail -f filename.txt
```

vi to open files - standard starts up in command mode:

```shell
$vi
```

Essential commands:
i - open insert mode at cursor position
o - go to insert mode and open a new line under the current cursor position
Esc - go back from insert mode to command mode
:wq! - save all changes and close file
:q! : close vi without saving curent changes.

Cursormovement in vi:

gg - go to begining in document
G - go to last line in document
:3 - go to the line number
/word - go to the first word in the document after the current cursor position
n - repeat last search
?woord - search from current cursor position to top in document for the "woord"
N - repeast last search reversed

If the arrow keys do not work u can use:

h - move cursor left
j - move cursor down
k - move cursor right
l - move cursor up

Copy, cut and paste in vi:

use command v followed by:

d - delete current selection
y - copy current selectoin
p - paste the last text selection from buffer
dd - delete the current line
yy - copy the current line
v - turn block selection on, move arrow keys to select a text block
x - delete the char at cursor location

Advanced vi options:

u - undo last edit
Ctrl+R - redo last edit
:%s/old/new/g - find and replace the text "old" by the text "new"

Split and Join:

split used to split files
join used to join files not icw split

```shell
//use command to create exmample file
of 1Megabyte and 6 blocks total 6 MB
$dd if=/dev/zero of=~/bigfile bs=1M count=6

//use split on file and generate piecies of 100kb
$split -b 100K bigfile peices

//merge files together again
$for i in stukjes*; do cat $i >> merged; done
```

## Regular Expressions

Always use escape by usinging single quotes
i.e.
$grep 'e\{2\}' file

- ^n - only shows rows begining with "n"
- n$ - only shows rows eding with "n"
- n.x - The dot is used as a matched for a - random char i.e. nix, nax will give a match
- the + - tells that the previous char must be found once
- the * - tells that the previous char can be found multiple times
- \b - search end of word i.e. '/lea\b' will match on lea but not on leanne
- ? - used to tell that at the current position a one char may used but not a must.
- n\{3\} - search for the letter n wich is 3 times findable
- [a-Z] or [:alpha:] - find random chars from range a-Z ( all small and upper case )
- !^a-Z - search for patern where no letters are found.
- \ - search for .\ dot
- [:digit:] - search for random number

Regular expression tools:

- grep is General Regular Expression Parser use option -E for extended regex and use option -F for paterns.
- sed
- awk

Expand, unexpand, fmt, od, paste, pr, tr, uniq:

uniq - check for unique values / show differences:

```shell
//shows unique values only
$uniq file

//show duplicate values only
$uniq --repeated filename
```

tr - used to replace chars or change into something else used wit pipe or redirect only

```shell
//change to uppercase
$cat users | tr a-z A-ZS
```

- expand - convert tabs to spaces
- unexpand - convert spaces to tabs
- fmt - show files in the most optimal view on screen
- od - dump the contents in octal form
- paste - shows rows behind each other on screen use option -d to use delimiter for rows
- pr - converts texttfiles so that they can printed in the most optimal way

## Execute filemanagement tasks

using ls to show list dir/folders/files:

```shell
//Using ls to list files and folders
$ls

//also show hidden folders
$ls -a

//show in long list
$ls -l

//show directory info instead of content in dir
$ls -d
i.e.
$ls -ld [dir]

//sort by modifiation time
$ls -t
```

Making/Removing directorys:

Making dir:

```shell
//will not work because dir bestanden does not exist
$mkdir bestanden/2019

//will work with option -p , creates whole directory
$mkdir -p bestanden/2019
```

Removing dir:

```shell
//this will not work if there are still contents
$rmdir bestanden

//this will remove no mather if there is content or not use with option -f for no confirmation
$rm -r bestanden
```

Copy, remove and move:

- cp - is copy
- rm - is remove
- mv - is move and rename

```shell
//using option -R will copy whole dir structure
$cp -R

//using option -f will copy over all properties of files
$cp -p

//using option -u will copy only if source file is newer then dest file will prevent accidental overwrite i..e
$cp -u

//copies everything starting with abc to bestanden
$cp /etc/[abc]* bestanden

//file globbing with * , ? , [at]
$ls bestan[dt]
bestand
bestant

//Shows all except hidden
$ls *

```

Copying whole devices using dd
i.e. clone hd, makge image file

commonly used device names:

- /dev/sda - first hd (SCSI or SATA interface)
- /dev/sdb - second hd (SCSI or SATA interface)
- /dev/hda - IDE hd ( only old pc's)
- /dev/cdrom - the optical drive
- /dev/zero - special internal device used as input device to generate zero's, hany for deleting hd i.e.
- /dev/null - special internal device used as output device where u can send things to u never want to see back.
- /dev/ramdon - special internal device used as input device to copy random chars from

```shell
//input device output device, one should be a device
$dd if=/dev/cdrom of=~/cdrom.iso

//use with option bs=4096 i.e. to specify block size 4096 bytes
$dd if=/dev/sda of =/dev/sdb bs=4096
```

Using file:

- file - the file command shows what type/kind some file is.

```shell
$file [filename]
```

Archive files and compression: tar cpio, gzip, gunzip, bzip2

- tar - Standard for backups, stand for Tape Archiver. Standard tool for extracting an making back-ups ( older utility cpio). Tar does not compress at default

Using tar:

```shell
//making an archive c for create, v for verbose, f for file and /directory for contents of archive file
$tar cvf archivefilename.tar /directory

//look at contents of tar file
$tar tvf archivefilename

//extract tar archive
$tar xvf archivefilename.tar -C /directory

//using compressing using option -z wich is gzip or option -j which is bzip2
$tar czvg /tmp/homes.tar /home /root

//extract to directory using option -C
$tar xvf archivefile.tar -C /tmp
```

Compression:

Note extension do have meaning here

```shell
//compress using gzip
$gzip filename

//compress using bzip
$bzip filename

//uncrompress/extract using gzip or bzip
$gunzip filename.gz
$bunzip2 filname.bz2
```

Links:

Linux nows 2 kind of links to files

- symbolic link ( soft link )
- hard link

Symbolic links: is a reference to the name of the file. All administration is done on the source file. So you cant set permissions on symbolic links and if the original file gets lost the link will break.
Very flexible u can use them to reference files o directorys even if these are on a different device or server.

Using Symbolic:

```shell
//making a symbolic link i.e.
in your home directory link to file /etc/host link is called computers
$ln -s /etc/hosts ~/computers
```

Hard links: every file on linux has his whole administration saved in the inode($ls -i) of that file. In the invode ther is information about permissions, times, owners and alot more. Trough the inode the shell can access blocks in which the file is saved. Only thing not saved in the inode is the name of the file. Filenames are saved in a different table ( the directorytable ). Each name has a inode linked to it, but the inode does not know which names these are, the only thing known in the inode is the count of names that are linked to the inode. Every name in it self is a Hard link. By working with hard links its possible to link various names to a inode. Special is that there is no difference between the first en each following name that is linked to the inode.
If the source file is deleted has no effect on hard links that ar created later.
Restrictions:

- works only for files on the same parition or logical volume

- cant make hard links to a directory

Using/Making hard links:

```shell
//create hard link computer from myfile
$ln myfile computer

//show that they are the same
$ln -il mijnhosts computers

//Note if you do this both will be updated since its the same file to which 2 different names are linked
$echo hallo >> mijnhosts
```

The linux filesystem:

- FHS - is one of the standard for linux aka as Filesystem Hieraarchy Standard, this sets the directorys and for which purpose these should be used. Want to know more use command $man hier

The most importand directorys and their functions as defined in the FHS:

- / - the rootdirectory, the starting point of the whole directory tree.

- /bin - this directory has binaries ( program files  ) which are necessary when linux is started in minimal mode ( single user mode ), so if necessary the system can be repaired. On modern distributions mostly this is a symbolic link to /usr/bin.

- /sbin - the systembinaries, program files a administrator needs to repair a linux system which has problems. On mondern distributions mostly this is a symblock link to /usr/sbin.

- /usr - the directory where all progam files and services are in. Often this is on a different partition and thas why sometimes in single user mode is not accessible. Under this u will find a couple of directorys which also are under / i.e. bin and sbin.

- /lib - contains library-files. These are files that have a shared code which are necessary/used by the binaries in /bin and /sbin.

- /boot - contains the kernal and related files which are necessary to start a linux-system.

- /etc - here you will find configuration files of which programs and services on your computer make use of. Almost al the files are readable text files.

- /dev - contains the device files. These are files that are used as interface to hardwarecomponents in your computer. Most of these files are automatically made by the service udev.

- /home - contains home directorys of users. This directory is ofter on a different partition.

- /media - used as mount point for devices that are automatically mounted i.e. cd's and usb-sticks.

- /run - a relativtly new addition to the FHS, that is used to save files which are dynamically created for a specific users and processes.

- /mnt - often used as temporary mount point. Some administrators use this to make different sub directorys so that different types of mounts can be reaced trough /mnt.

- /opt - directory in which additional program files are place. Often these are what more bigger program suites. Most distributions dont use this directory.

- /proc - the mount point for the proc-filesystem which is used as interface to the kernel.

- /root - the home directory of the user root.

- /srv - used to save data by different services available on the system. Think of files which are presented by aweb-ftp server as shared files. Not all distributions use this

- /tmp - contains temporary files which are dynamically created by different processes.

- /var - contains different files which are dynamiccaly created by the os and services. Because this files can grow uncontrolled, this is often on a different partition or volume.

Working with locate, whereis and related utilities:

Searching files: search for files can be done with the command $find.
To search for files faster a database can created wherein all relavant program files are index. This database is made with the command $updatedb and in the configuration file /etc/updatedb.conf u can provide the directories as admin. If the db exists u can use the commmand $locate to find files.
Down side to this, the db gets automatically updated once per day, so files made couple of hours ago cant be found.

```shell
//locate file indexed by db
$locate myfile

//create db and index
$updatedb

//create/update and index in backgroud
$updatedb &
```

Using which and whereis:

searches the PATH-variables on your system, with which only the searchpath for programfiles are searched with whereis you will get more results because also the searchpath ot the man pages and other sources of the documentation are search.

Using find:

```shell
//find all files on the system where name begins with hosts saved on the hard drive
$find / -name "hosts*"

//combined options i.e find all files of which linda is the owner and are bigger then 100mb
$find / -user linda -size +100M

//also possbile to execute command on resulsts i.e. find all files of which anita is the owner and move these to the directory /root by using -exec.
note exec need always be closed by \;
{} is used to reference the result of find
$find / -user anita -exec mv {} /root \;

//find all files with owner linda and filter these to look if ther alre files in which the text blah exists
$find / -user linda -exec grep -l blah {} \;


//usefull options
-executable: finds al executables files
-group groupname: finds all files where groupname is the owner
-mmin n: shows all files that are changed n minutes ago
-newer file: finds all files that are newer then file
-nogroup, nouser: search files which have no group or user as owner
-perm[+|-] modues: search all files where a specific permissionsmode is set
-size n: searches all files with a specific size, can be used to specify size i.e. +2G searches all files bigger dan 2 gigabytes also possible to use K or M.
-type t: finds files of a specific type i.e. use f for file or d for directorys.
```