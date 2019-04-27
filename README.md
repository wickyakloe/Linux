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

Directory browsing:

```shell
//Show files in directory
$ls
README.md

//Show directory your in
$pwd
/home/wickedPy/Development/Linux
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

## Files

Make a file:

```shell
$touch [filename.ext]
```