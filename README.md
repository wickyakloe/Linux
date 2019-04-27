# Linux

List items in directory:

```shell
ls
```

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

Standard variables definied at start are known as the <b>environment</b>

Show <b>environment</b> variables:

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
$[VARIABLENAME] = [VALUE]

i.e.
$TODAY = monday
```

## Files

Make a file:

```shell
$touch [filename.ext]
```