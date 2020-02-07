# Working on the shell

Table of Contents:

- [Login into a shell environment](#Login-into-a-shell-environment)
  - [Runlevels and targets](#Runlevels-and-targets)
  - [Root or not](#Root-or-not)
- [Working on the command line](#Working-on-the-command-line)
  - [Execute in subshell or not](#Execute-in-subshell-or-not)
- [Handy shell commands](#Handy-shell-commands)
  - [Bash history](#Bash-history)
- [Using streams, pipes and redirects](#Streams,-pipes-and-redirects)
  - [Work further on output: tee and xargs](#Work-further-on-ouput:tee-and-xargs)
- [Using help](#Using-help)
  - [Man-sections](#Man-sections)
  - [Using man](#Using-man)
  - [Using info](#Using-info)
  - [The option --help](#The-option---help)

## Login into a shell environment

When using a distro with no GUI you will
be presented with the shell after boot i.e. ubuntu server.
When using a GUI you can right click on the desktop and
select 'open in terminal', if this is not available search
trough the menus for 'terminal'.

Working with virtual consoles aka tty:

GUI mostly on (different per distribution):
Ctrl+Alt+F1
or
Ctrl+Alt+F7

In shell:

```shell
#Same as key combination above
chvt 3
```

Some distributions use higher virtual consoles on:
tty8 trough tty12

Show users and virtual console:

```shell
$who
wickedPy :0           2019-04-24 12:08 (:0)
#user :consoleno
```

*[TOC](#Working-on-the-shell)*

### Runlevels and targets

The systemd servcie manager definies multiple targets i.e.:

- multi-user.target (This starts all standard processes)
- graphical.target (This starts multi-user.target and the graphical environment)

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
#For GUI
$systemctl set-default graphical.target

#CLI
$systemctl set-default multi-user.target
```

*[TOC](#Working-on-the-shell)*

### Root or not

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

*[TOC](#Working-on-the-shell)*

## Working on the command line

When logged in you will have access to the bash-shell
trough a terminal or console.

Standard variables definied at start of bash-shell are known as the **environment**

Show **environment** variables:

```shell
$env
```

Read variable:

```shell
$echo $[variable]

#i.e.
$echo $PATH
```

Create variable:

```shell
$[VARIABLENAME] = [value]

#i.e.
$TODAY=monday
```

Call variable:

```shell
#call variable
$ $PATH
```

Set variable to no value:

```shell
#using unset
$unset TODAY
```

Self set variables are not automatically available
in subshells, to make it available in a subshell use:

```shell
$export [VARIABLENAME] = [value]

#i.e.
$export TODAY = monday
```

*[TOC](#Working-on-the-shell)*

### Execute in subshell or not

Open sub/new shell (Parent-child):

```shell
$bash

#set shell behaviour see "help set"
$help set
$set
```

Run script:

```shell
#In current shell
$.[scriptname.ext]
#or
$source [scriptname.ext]

#In subshell note after execution of script returns to Parent shell
$bash [scriptname.ext]

#In specific situation run from memory
$exec [scriptname.ext]
```

*[TOC](#Working-on-the-shell)*

## Handy shell commands

The bash-shell also has internal commands u can use, these
are commands that are always available when the shell is started.

```shell
#Show files in directory
$ls
README.md

#Show directory your in
$pwd
/home/wickedPy/Development/Linux

#make directory
$mkdir /foldername

#remove directory/file
$rm -r /foldername

#make a file
$touch [filename.ext]

#Show computer information
$uname
Linux

#show all
$uname -a

#show versionno. current kernel
$uname -r

#filter search for root in /etc/*
$grep root /etc/*

#pipe output to less
$ls | less

#show contents of file
$cat [filename]

#list all active processes
$ps aux
```

*[TOC](#Working-on-the-shell)*

### Bash history

The shell saves typed commands in a file called ***.bash_history***

Access history:

```shell
#Show history
$history

#Clear history
$history -c
```

- Use the arrow keys up and down on the keyboard.

- Use Ctrl+R and type a part of the command
(this searches backwards in history) pressing again will search further back.

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

*[TOC](#Working-on-the-shell)*

## Streams, pipes and redirects

- ***STDIN***: standard input - the input delivered to a command, mostly input from the keyboard. Also known as ***file descriptor 0***

- ***STDOUT***: standard output - the destination the command sends his output to, mostly the window where the command is run. Also known as ***file descriptor 1***

- ***STDERR***: standard error - the desination where errors are sent to, mostly the window where the command is run. Also know as ***file descriptor 2***

U can send the STDIN, STDOUT and STDERR somewhere else aka ***redirect***:

```shell
#redirect STDIN i.e. use contents of file as input of a command
$ <

#redirect STDOUT i.e. send output of command to file.
#The use of a single > creates a new file or overwrites it.
$ >
#Use >> to adds to a file
$ >>

#i.e. write output and errors to file wickedtest
$grep root /etc/* > ~/wickedtest

#redirect STDERR i.e. send error output to file
$ 2>

#i.e. search for root in /etc/* and send all errors to null device shows only STDOUT
$grep root /etc/* 2> /dev/null

#i.e. search for root in /etc/* and send all errors to a file wickedtest and show only STDOUT
$grep root /etc/* 2> ~/wickedtest

#i.e. search for root in /etc/* and send all errors to null device and send output STDOUT to file
$grep root /etc/* 2> /dev/null > ~/wickedtest

Use 2>&1 to tell that STDERR must be used just like STDOUT
```

Pipes:

Use pipe to send the ouput of the first command to the input of the second command

```shell
$ls /etc/ | grep hosts

#use row for row from ouput as command for second input
$ls /etc/ | xargs grep hosts

#or split output i.e. write to file
#and filter
$ls /etc/ | tee ~/file | grep host
```

*[TOC](#Working-on-the-shell)*

### Work further on output: tee and xargs

There are 2 commands which make it possible to work further on the output:

- tee
  - Splits the output of a command

- xargs
  - Used to further process and edit the result of the first command and use it as input for the second command

Example xargs:

```shell
#U will see only files
#in which the charset hosts are in the filename
#this is because grep searches the charset hosts in the output
$find /etc | grep hosts

#U will now see a different result
#this is because xargs uses each file found by find as argument for grep
$find /etc | xargs grep hosts
```

With a pipe u send the output STDOUT of the first command to the input STDIN of the second command. With xargs u make sure the result of the first command gets passed line by line as argument to the second command.

Example tee:

```shell
#Here there are 2 things being done with the output of ls
#First the output of ls gets redirected to a file called file_1
#Second the lines are filtered with grep host
$ls /etc/ | tee ~/file_1 | grep host
```

*[TOC](#Working-on-the-shell)*

## Using Help

The most important way to ask help on linux commands is the command
man aka System Programmers Manual this opens in a less pager

Layout of man pages(other sections maybe present):

|NAME|Description|
|-|-|
|NAME|The name of the command with a short description of the command|
|Synopsis|Summary of how to command is used. Here u will see all available options and if the option is mandatory(the option is stated within brackets[]) or optional(The option is stated without brackets)|
|Description|Extensive description of the command. Read this to get a complete picture of the possibilities the command has to offer|
|Options|Complete list of all options and per option the explanation|
|Files|Short list of files that are related to the command i.e. config files used by the command|
|See also|List with related commands|
|Author|The person who has written this man page.|

Reading the help:

Example command lvresize --help:

- If a item is between brackets [] it is optional
- If there a multiple items within brackets [] separated by a | sign
the | should be interpreted as or. So u can choose one of the items i.e.
u can choose between -A or --autobackup
- If a item is within curly brackets {} u must choose one.
- Items that are not between anything([]{}) are mandatory

*[TOC](#Working-on-the-shell)*

### Man sections

|Section|Description|
|-|-|
|0 - Header files |Contains information about header files. These files are mostly in the directory /usr/include. These are files containing common code used by your programs. The information in this section is mostly of interest to C-Programmers.|
|1 - Executable programs or shell commands|Contains information about executable programs and shell commands. This is for the end user the most import section, because all commands the end user uses are documented here.|
|2 - System calls|Contains information mostly of importance to programmers who are looking into the interal workings of the kernel. A system call is a function presented by the linux kernel. As administrator u will be confronted with this if u are looking into the optimazation of the performance of linux systems|
|3 - Library calls|Contains information interesting to programmers. A library is a file containing code which can be used by different programs ( same a dll in windows ). This section gives information about the functions provided by the library.|
|4 - Special files|Contains information of the device-files which are in directory /dev/ . This can be usefull to learn more about different devices and the way these devices can be used. These files are needed to access external devices on your computer|
|5 - File formats and conventions eg Configuration files|Contains information how files are to be structered and formated|
|6 - Games|TMost linux distros dont use this and now info is available|
|7 - Miscellaneous|Contains information about different things shipped with your linux distro. Mostly these are basic stuff which do not belong to a specific command|
|8 - System administration commands|Contains information about commands of imporatance to a administrator.|
|9 - Kernel routines|Optional section(mostly it is not even installed) contains information how different kernelroutines work on your computer. Only interesting if you are kerneldeveloper|

### Using man

```shell
$man passwd

#show description of manpages 1-9
$man 3 intro

#show all information
$man -a passwd

#search command in man
$man -k time

#filter on section 1
$man -k time | grep 1

#Recreate mandb run as root
$sudo mandb
```

*[TOC](#Working-on-the-shell)*

### Using info

Sometimes information for a command is in info:

Navigation in info:

u: Go up a level in the hierarchy

n: Go to the next subject
p: Go to the previous subject

*: Takes u trough the menu item to the underlying subject

```shell
$info ls
```

*[TOC](#Working-on-the-shell)*

### The option --help

Where man and info give the user a extensive description of commands, whreas the option --help give the user a summary of how to use the command.

Note: If the information shown is to large for the screen pipe it trough less to show it page by page.

```shell
$useradd --help
Usage: useradd [options] LOGIN
       useradd -D
       useradd -D [options]

Options:
  -b, --base-dir BASE_DIR       base directory for the home directory of the
                                new account
  -c, --comment COMMENT         GECOS field of the new account
  -d, --home-dir HOME_DIR       home directory of the new account
  -D, --defaults                print or change default useradd configuration
  -e, --expiredate EXPIRE_DATE  expiration date of the new account
  -f, --inactive INACTIVE       password inactivity period of the new account
  -g, --gid GROUP               name or ID of the primary group of the new
                                account
  -G, --groups GROUPS           list of supplementary groups of the new
                                account
  -h, --help                    display this help message and exit
  -k, --skel SKEL_DIR           use this alternative skeleton directory
  -K, --key KEY=VALUE           override /etc/login.defs defaults
  -l, --no-log-init             do not add the user to the lastlog and
                                faillog databases
  -m, --create-home             create the user's home directory
  -M, --no-create-home          do not create the user's home directory
  -N, --no-user-group           do not create a group with the same name as
                                the user
  -o, --non-unique              allow to create users with duplicate
                                (non-unique) UID
  -p, --password PASSWORD       encrypted password of the new account
  -r, --system                  create a system account
  -R, --root CHROOT_DIR         directory to chroot into
  -s, --shell SHELL             login shell of the new account
  -u, --uid UID                 user ID of the new account
  -U, --user-group              create a group with the same name as the user
  -Z, --selinux-user SEUSER     use a specific SEUSER for the SELinux user mapping
      --extrausers              Use the extra users database
```

*[TOC](#Working-on-the-shell)*
