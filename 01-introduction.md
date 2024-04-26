---
title: Introducing the Shell
teaching: 20
exercises: 10
---

## Welcome

<p>

### Announcements (Logistics)

<p>

### Pilot and workshop info

- Lesson site has details and extra info (our pilot is the SparkNotes version.)
- Pilot ~1/2 of material source material - recording.
- Workshop June 21
- (we'll be "hitting the high points" of 5/9 material)

<p>

### Quick show of hands

How many students have used UNIX?<p>
How many students use Terminal on Mac? Powershell or "Windows Subsystem for Linux" on Windows?

### What is the Unix "shell"?

The *shell* is a program that allows you to control your computer by typing instructions on the CLI with a keyboard.
Laptops - GUI/mouse

### Why are we starting with Unix? (CLI)

- Most bioinformatics tools do not have graphical interfaces

- Save time
  - repetitive tasks. The command line allows you to simplify or automate without learning a whole programmming language.
  - less error-prone
  - reproducible - code/commands to re-do your work
  - communicate unambiguously

- Most "big data" work (cloud or "on-prem") requires the command line.

### This recording/section

- move around using CLI
- see files and directories without Mac Finder or Windows file explorer

## How to access the shell

"Terminal"

All Broadies have access to Broad [login servers](https://intranet.broadinstitute.org/bits/service-catalog/scientific-computing/login-servers).
These are 'remote server's, a computer that is not the one you are working on right now.

We will learn the basics of the shell by manipulating some data files on a remote Unix server.

## How to access the remote server

1. Connect to the **Broad-Internal** wireless network.
1. Launch your preferred SSH client, such as Terminal (Mac or Unix) or Windows Subsystem for Linux.
1. Log in to a Broad login server using the [instructions on the Broad Intranet(https://intranet.broadinstitute.org/bits/service-catalog/scientific-computing/login-servers).

After logging in, you will see a screen showing something like this:

```output
Last login: Tue Apr 23 08:33:43 2024 from 10.75.224.147
--------------------------------------------------------------
Welcome to the host named login01
RedHat 7.9 x86_64
--------------------------------------------------------------
Puppet:        7.29.1
Facter:        4.6.1
Environment:   production
FQDN:          login01.broadinstitute.org
VLAN:          32
IP:            69.173.65.17
Born On:       2019-07-28
Uptime:        18 days
Model:         VMware Virtual Platform
CPUs:          2
Memory:        7.62 GiB
--------------------------------------------------------------
 IMPORTANT: This login host is a SHARED resource. Please limit
 your usage to editing, simple scripts, and small data transfer
 tasks. To encourage mindful use, limits have been put in place
 including memory limitations.

 To read more about this service, see https://broad.io/login.

 ################## Monthly Reboot Schedule ##################
 Since there is no convenient time to reboot login hosts we
 are establishing a monthly rotation.

 login01   - 1st Sunday of each month at 6 PM
 login02   - 2nd Sunday of each month at 6 PM
 login03   - 3rd Sunday of each month at 6 PM
 login04   - 4th Sunday of each month at 6 PM

 If starting a long session, choose the host rebooted last
--------------------------------------------------------------
This computer system is the property of the Broad Institute.

It is for authorized use only. By using this system all users
acknowledge notice of, and agree to comply with, Broad's
Acceptable Use (broad.io/AcceptableUse).

Unauthorized or improper use of this system may result in
administrative disciplinary action and/or other sanctions.

By continuing to use this system you indicate
your awareness of and consent to Broad's Acceptable Use Policy.
(broad.io/AcceptableUse).

Log off immediately if you do not agree to the conditions
stated in this warning.
--------------------------------------------------------------
-bash:login01:~ 50393 $
```

This provides a lot of information about the login servers. We're not going to use this information now, so you can clear your screen using the `clear` command.

Type the word `clear` into the terminal and press the `Enter` key.

```bash
clear
```

If you scroll up, you can still see everything that has been outputted to your screen.

## The filesystem - files and directories

directories (aka "folders"),

Several commands are frequently used to create, inspect, rename, and delete files and directories.

```bash
$
```

The dollar sign is a **prompt**, which shows us that the shell is waiting for input;
your shell may use a different character.
do not type the prompt, only the commands that follow it.

`pwd`
("print working directory").
At any moment, our **current working directory**
is the directory that the computer assumes we want to run commands in,
unless we explicitly specify something else.
Here, the computer's response is `/home/unix/<username>`, also known as your home directory.
Angle brackets give you a hint to substitute an appropriate value (without the brackets).

```bash
pwd
```

```output
/home/unix/jlchang

```

Let's look at our file system. We can see what files and subdirectories are in this directory by running `ls`,
which stands for "listing":

```bash
ls
```

```output

```

`ls` prints the names of the files and directories in the current directory in
alphabetical order,

Your output may look different. (Confession, I cleaned up because I knew you were coming over. But now my place is pretty boring ;). Let's get something to look at.

On most Unix systems, you can grab a file over the internet using a tool called `wget`.

```bash
wget https://data.broadinstitute.org/bits_demo/user_education_sessions/Unix101/unix101demo.tgz
```

```output
--2024-04-25 23:25:06--  https://data.broadinstitute.org/bits_demo/user_education_sessions/Unix101/unix101demo.tgz
Resolving data.broadinstitute.org (data.broadinstitute.org)... 69.173.68.137
Connecting to data.broadinstitute.org (data.broadinstitute.org)|69.173.68.137|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2367992 (2.3M) [application/x-gzip]
Saving to: 'unix101demo.tgz'

100%[=====================================================================================>] 2,367,992   --.-K/s   in 0.02s

2024-04-25 23:25:06 (134 MB/s) - 'unix101demo.tgz' saved [2367992/2367992]

```

Now if we `ls`

```bash
ls
```

```output
unix101demo.tar.gz
```

We downloaded a "tarball". It's a compressed file that can be unpacked. Let's unpack it!

```bash
tar -xzf unix101demo.tar.gz
```

Now if we `ls` again

```bash
ls
```

```output
unix101demo  unix101demo.tar.gz
```

Let's explore the `unix101demo` subdirectory.

The command to change locations in our file system is `cd`, followed by a
directory name to change our working directory.
`cd` stands for "change directory".

```bash
cd unix101demo
```

Let's look at what is in this directory:

```bash
ls
```

```output
Dahl  Seuss  authors.txt  bigfile.txt  data  prodinfo454
```

We can make the `ls` output more comprehensible by using the **flag** `-F`,
which tells `ls` to add a trailing `/` to the names of directories:

```bash
ls -F
```

```output
Dahl/  Seuss/  authors.txt  bigfile.txt  data  prodinfo454/
```

Anything with a "/" after it is a directory. Things with a "\*" after them are programs. If
there are no decorations, it's a file. Broad servers already color code by default (directories are blue, programs are green) but `ls -F` can be useful on servers that don't color code.

`ls` has lots of other options. To find out what they are, we can type:

```bash
man ls
```

`man` (short for manual) displays detailed documentation
Some manual files are very long. You can scroll through the
file using your keyboard's down arrow or use the <kbd>Space</kbd> key to go forward one page
and the <kbd>b</kbd> key to go backwards one page. When you are done reading, hit <kbd>q</kbd>
to quit.

No one can possibly learn all of these arguments. man

Let's go into the `Seuss` directory and see what is in there.

```bash
cd Seuss
ls -F
```

```output
Charlie_and_the_Chocolate_Factory  James_and_the_Giant_Peach
```

This directory contains two subdirectories

### Shortcut: Tab Completion

Typing out file or directory names can waste a
lot of time and it's easy to make typing mistakes. Instead we can use tab complete
as a shortcut. When you start typing out the name of a directory or file, then
hit the <kbd>Tab</kbd> key, the shell will try to fill in the rest of the
directory or file name.

From the `Dahl` directory:

```bash
cd J<tab>
```

The shell will fill in the rest of the directory name for
`James_and_the_Giant_Peach`.

Now change directories to `James_and_the_Giant_Peach` in `Dahl`

```bash
cd James_and_the_Giant_Peach
```

Using tab complete can be very helpful. However, it will only autocomplete
a file or directory name if you've typed enough characters to provide
a unique identifier for the file or directory you are trying to access.

For example, if we now try to list the files which names start with `Th`
by using tab complete:

```bash
ls Au<tab>
```

The shell auto-completes your command to `Aunt_Sp`, because there are two files in
the directory that begin with `Aunt_Sp`. When you hit
<kbd>Tab</kbd> again, the shell will list the possible choices.

```bash
ls Aunt_Sp<tab><tab>
```

```output
Aunt_Spiker  Aunt_Sponge
```

Tab completion can also fill in the names of programs, which can be useful if you
remember the beginning of a program name. (Like pwd)

```bash
pw<tab><tab>
```

```output
pwck              pwd               pwhistory_helper  pwmconfig         pwunconv
pwconv            pwdx              pwmake            pwscore
```

Displays the name of every program that starts with `pw`.

## Summary

Congratulations! You've navigated a Unix file system using the command line!
Soon you'll be able to work on a remote server and use
bioinformatic software that is only available in command line versions.
