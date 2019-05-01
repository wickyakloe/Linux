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
