
# Four Unix Concepts
 
Unix is a handy thing to know if you work on the web.  So much of the internet was built from it. Even on a Mac you have
Unix (a flavor if it) under the hood. There are three kep concepts

1. Files
2. Programs and Processes
3. Pipes
4. Environment
5. Permissions

## Files

Most resources available on a Unix systems are access through the concept of
a file.  Most files you deal with day to day are documents (e.g. you're 
reading file as you read this document).  Some files are special. Special
files include things like the ability to send or receive information from
your computer keyboard, mouse, video camera, network sockets or put things 
in the screen. There are lots of special files possible (given the 
increasing types of sensors and display on computers). There are three 
special files that you'll use all the time.

1. standard input (aka stdin)
2. standard output (aka stdout)
3. standard error (aka stderr)

Collectively these are referred to as standard io (aka stdio). 

When you are use Terminal App on a Mac you're leveraging standard io.


### paths, directories, basenames and extensions

Files have a name. The name like a human name has parts.  Unlike a human 
name filenames parts' are organized around reflecting a heirarchy
relationship to other files on the computer system(s). A full filename usually contains three parts. Those parts are--

1. path
2. basename (aka filename)
3. extension (optional)

In Unix a path is the part of the full filename to the left of the right 
most slash characeter (i.e. "/"). The part to the right of the right
most slash is the basename. By convention files' basenames often include
what we call an extention. An extention is everything to the right of the
right most period. You see this in web urls (e.g. .html, .js, .css).

Here are some examples of filenames with paths.

+ /helloworld.txt
+ /home/janedoe/helloworld.txt
+ /www/index.html

In this example of three files with there path notice that "helloworld.txt" 
apears twice.  These are two separate files because the path component of 
the name is different (i.e. /helloworld.txt and /home/janedoe/helloworld.txt 
have different paths).


The path is important. It is our means of organizing information so
we can get to it quickly.  The path part expresses a heirarchy.  Each
part of the path separated by a slash called a directory or in Mac parlance
a folder.  Just like physical folders we can nest them. The path represents
the order of the nesting.

The path made up of a single slash is referred to as the *root* of the file
system.  The path you start in when you first login is is called our *home*
directory.  The path where we store content for distribution on the web is
often called either a "web root", "doc root" or "htdoc root".

By convention certain things in a Unix system are expected to have specific
paths. Here's an example of commons paths you'll ear mentioned--

+ / (root)
+ /etc (ecetera, this is traditionally where we store configuration)
+ /usr (the place where we store runnable programs)
+ /usr/local (the place where we store programs specific to this system)
+ /var (a place where we store files that are not restricted by size)
+ /var/log (where system logs are commonly stored)
+ /www (commonly where you serve webpages)
+ /home (where user files are stored)
+ /home/JaneDoe (where Jane Does' personal files are stored)
+ /home/JaneDoe/public\_html (where Jane Does' personal web files are stored)

On a Mac have other paths--

+ /Applications (where Mac apps are stored)
+ /Users (this replaced /home for user files)
+ /Users/JaneDoe/Sites (Jane Doe's personal web accessible files)
+ /System (this is the place where system files and settings are stored)
+ /Library (parts of programs shared across the system)
+ /Library/WebService/Documents (the default web directory shipped on a Mac)

Finally files can be manipulated by programs.


## Programs and Processes

Programs are files that contain instructions for the computer to run. A web 
browser is a program. A text editor is a program. The operating system is 
also a program (albiet a complex one). When a program is running, the
computer is actively executing its instructions it is called a process.  
Running processes are managed by the operating system (a very special
process).  The operating systems enforces certain rules and allows programs 
to access those special files such as standard io.

When you're using a web browser it is a process. When you use a text editor 
it is a process.  Process can work on files and manipulate them. Each 
process (each running program) has a numeric identify that the operating 
systems use to monitor and control it.  This is sometimes called a "pid" 
which or process id. A "pid" analagous to a filename but for actively
running processes.  The heirarchy of processes are determined by something 
called run levels but most of the time we don't worry about that.


One nice thing about processes as that they can cooprate. Usualy they 
cooperate through the use of files.  The phylosophy of Unix is to have
small simple programs that operate on files.  These small simple programs
can be combined through coopration to do more complex tasks. One way 
to orchestrate this cooperation is through something called a "pipe" 
short for pipe line.


## Pipes, and redirection

Let's say you want to displays the contents of a file screen. Unix has a 
command called "cat" which is short for concatenation. It will read
the contents of a file and display them by sending them to the special file 
called standard out.  Now lets say you want to count the words you just read 
on the screen. Unix also has a command for that. It is calleed "wc" which
is an abbreviation of "word count". One way to count the files would be to 
display the file with the "cat" program and send its output to the "wc" 
program to count the words. The "wc" program can then display the word 
count. In other words we "pipe" the output of "cat" into "wc".

Here's how we might do that for a file called "/home/shakespear/much-ado-about-nothing.txt".

```shell
    cat /home/shakespear/much-ado-about-nothing.txt | wc
```

Instead of seeing the content of "/home/shakespear/much-ado-about-
nothing.txt" you'll see the word count.  By using more than one pipe
you can create complex chains of actions.  Now lets say you want
to save that word count for latter.  You could "redirect" the output
from standard out using a greater than character (i.e. ">"). Let's
save the word count in a file called total-words.txt--

```shell
    cat /home/shakespear/much-ado-about-nothing.txt | wc > total-words.txt
```

Now lets say we wanted to also count the words of _Romeo and Juliet_. 

```shell
    cat /home/shakespear/romeo-and-juliet.txt | wc > total-words.txt
```

There is a slight problem here. Peviously stored the count of _Much
ado about nothing_ and now we've replaced it with the new count for
_Romeo and Juliet_. What if we wanted to keep both?  Instead of
create a new file each time we can use two greater than characters
in a row to say "append" to the file. Here's the version where we
store both counts.

```shell
    cat /home/shakespear/much-ado-about-nothing.txt | wc > total-words.txt
    cat /home/shakespear/romeo-and-juliet.txt | wc >> total-words.txt
```

Notice the second line uses ">>". This means append.  We can redirect
more than standard output. We can also redirect input.  This is done
with the less than sign. Let's say I have a file called helloworld.txt.
I could use the less than sign to tell "cat" to read it from a file
using a less than sign. I can also do thiw with the "wc" command.

```shell
    cat < helloworld.txt
    wc < helloworld.txt
```

Here's a more useful example. You have MySQL installed on your system.
You have a SQL program called wp-dev.sql also on your system. wp-dev.sql
sets up a database and tables for your to do Wordpress project with.
Here's how you can use the less than sign to have the MySQL shell read
that wp-dev.sql file.

```shell
    mysql < wp-dev.sql
```

# Permissions

Unix was designed to be a shared computer system. As a result it
also needs to keep users from stepping on each other toes or 
accidentally removing crital parts of Unix. It does so by setting
permissions on files.  Unix provides three levels of permissions

1. owner (usually the person who created the file)
2. group (group using or working with the file)
3. other (everyone else)

Each user of a Unix system is in one or more groups.  Usually they
are in at least a group with there name.  But they might be in 
other groups too (e.g. admin, staff, www, ipc). If you own a
file you have access through the ownership permission, of you
belong to a group the file is associated with (files can only
be associated with one group) then you can access that way too.
Finally a file can have an "other" permision that allows everyone
to have some level of access.

For owner, group and other there are three types of permission

1. read
2. write
3. execute

Read and write are what you'd guess.  If you have read permission
you can look inside of the file. If you have write permission you
can change the file (including overwriting, removing or creating
new ones).  Execute permission is a little more nuanced.  If the 
file has this permission set the shell can try to "run" it as a
process. This is called executing a program. What is nuanced is
when we have folders, this permission controls where or not you
have access to folders it contains.  In this way you can create
a heirarchy where the outer folders are not listable but the
nested ones are. This is how "dropboxes" are implemented in Unix.

By combining ownership (i.e. owners, groups, other) and permission
types (i.e. read, write, execute) we have a simple but flexiby
system for manage access to most things on the computer.

We use the following Unix commands to manage file permissions--

+ [chmod](http://en.wikipedia.org/wiki/Chmod)
+ [chown](http://en.wikipedia.org/wiki/Chmod)
+ [chgrp](http://en.wikipedia.org/wiki/Chgrp)


# A paragraph about Shells

When login the first program that is run (so you can run other programs)
is called a "shell". Different Unix systems default to deferrent shells.
On Linux and Macs the default shell is usually "bash".  On Solaris sytems
it is often "csh".  The important thing to be aware of is that shells
let you automate actions.  You do this by write files containing shell
commands.  Each way of writing this is dependant on which shell you're
using. Fortunately most Unix systems include more than one shell. So if
you're used to write shell programs for the Bash shell you can run them
one a Solaris system by envoking the bash program to run your script.
Again a "Script" is just a sequence of commands. It is the same commands
you can type in when you login in.  By collecting those commands into
a file you can save typing.  Here's an example of a script that
looks for all the backup files left by a text editor called "vi" and removes
them.

```shell
    #!/bin/bash
    find $HOME -type f | grep -E '~$' | while read ITEM; do
        echo "Removing $ITEM";
        rm -i "$ITEM";
    done
```

If you want to learn more about shell programming there are many good
O'Reilly and Associates books on them.

# Common commands

Here is a list of common commands I use. I probably use about
a six frequently but all of them over the course of a month.

+ [ls](http://en.wikipedia.org/wiki/Ls) (List files and directories)
+ [cd](http://en.wikipedia.org/wiki/Cd_(command)) (change directory)
+ [pwd](http://en.wikipedia.org/wiki/Pwd) (path of working directory)
+ [chmod](http://en.wikipedia.org/wiki/Chmod) (change modulo, this is used to change permissions)
+ [chown](http://en.wikipedia.org/wiki/Chown) (change ownership)
+ [chgrp](http://en.wikipedia.org/wiki/Chgrp) (change group)
+ [cp](http://en.wikipedia.org/wiki/Cp_(Unix)) (copy)
+ [rm](http://en.wikipedia.org/wiki/Rm_(Unix)) (remove)
+ [ln](http://en.wikipedia.org/wiki/Ln_(Unix)) (link)
+ [mkdir](http://en.wikipedia.org/wiki/Mkdir) (make directory)
+ [rmdir](http://en.wikipedia.org/wiki/Rmdir) (remove directory)
+ exit (exit the shell, e.g. logout)
+ [find](http://en.wikipedia.org/wiki/Find) (find a file(s) or directories)
+ [grep](http://en.wikipedia.org/wiki/Grep) (look inside a file(s) for something)
+ [wc](http://en.wikipedia.org/wiki/Wc_(Unix)) (word count)
+ [ssh](http://en.wikipedia.org/wiki/Secure_Shell) (secure shell to access remote systems safely)
+ [scp](http://en.wikipedia.org/wiki/Secure_Copy) (secure copy to copy content to/from remote systems safely)
+ [ssh-keygen](http://en.wikipedia.org/wiki/Ssh-keygen) (generate a private/public key pair for make access easier)
+ [sed](http://en.wikipedia.org/wiki/Sed) (stream editor, handy if you need to cleanup files line by line)
+ [bash](http://en.wikipedia.org/wiki/Bash_(Unix_shell) (Bourne Again Shell, nice to script though a little idiosyncratic), also see [Aliens Bash Tutorial](http://subsignal.org/doc/AliensBashTutorial.html)
+ [echo](http://en.wikipedia.org/wiki/Echo_(command)) (display a line of text to standard out)
+ [mysql](http://dev.mysql.com/doc/) and mysqldump (the command line client  and utility for MySQL relational database)
+ [git](http://git-scm.com/book) (a version control system client, lets you clone, checkout, commit, push and pull content to/from github.com)
+ [kill](http://en.wikipedia.org/wiki/Kill_(command)) (shutdown a process, requires the pid and correct privileges)
+ [ps](http://en.wikipedia.org/wiki/Ps_(Unix)) (process status, lists the running processes)
+ [du](http://en.wikipedia.org/wiki/Du_(Unix)) (disc usage, reports on disc use for a path0
+ [passwd](http://en.wikipedia.org/wiki/Passwd) (changes your password)
+ [nano](http://en.wikipedia.org/wiki/Nano_(text_editor)) (a simple text editor found on most Linux systems and some Macs)
+ [vi](http://en.wikipedia.org/wiki/Vi) (vi one of the original Unix text editors, on almost all systems)
+ [emacs](http://en.wikipedia.org/wiki/Emacs) (an early Unix text editors, on almost all Linux systems)
+ [sudo](http://en.wikipedia.org/wiki/Sudo) (lets you become the administrator of the computer to run privilleged commands)

## Additional starting points

[Unix Starter Guide](http://www.ee.surrey.ac.uk/Teaching/Unix/)

[Mac OS X Unix Tutorial for Beginers](http://acad.coloradocollege.edu/dept/pc/SciCompLab/UnixTutorial/)

[Aliens Bash Tutorial](http://subsignal.org/doc/AliensBashTutorial.html) is 
a good place to find starter recipes for Bash scripting and general mucking 
about the shell. It has been floating around the net for decades. It is
where I look when I've forgotten something.



Mac specific:

+ launchctl (load, unload, start, stop services)
+ port (the Mac Ports manager command)
    - sudo port selfupdate;sudo port upgrade outdated




