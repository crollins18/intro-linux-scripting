## Module 0 - Getting Started

### Glossary of Basic Commands

Links to the full Linux manual pages are available for each basic command. These are formal documents written by the creators of each command that specify all functionality and options that are available for your use. In some of the manual pages there are sample programs that each command is used in.


#### `echo` - [display text or variable to terminal](https://linux.die.net/man/1/echo)
###### Syntax - `echo [options] <text|variable>`
```bash
$ echo "Hello world!!"
Hello world!!
```

#### <code>ls</code> - [lists directory contents](https://linux.die.net/man/1/ls)
###### Syntax - `ls [options] <location>`
```bash
$ ls # no location provided so assume current directory
file.txt document.pdf child_directory
```

#### <code>pwd</code> - [print working directory](https://linux.die.net/man/1/pwd)
###### Syntax - `pwd [options]`
```bash
$ pwd
/home/user
```

#### <code>cd</code> - [change the current working directory](https://man7.org/linux/man-pages/man1/cd.1p.html)
###### Syntax - `cd [options] <directory>`
```bash
$ cd Downloads # directory can be absolute or relative
$ ls 
file.txt download.pdf untitled.xls
```

#### <code>mkdir</code> - [make directory](https://linux.die.net/man/1/mkdir)
###### Syntax - `mkdir [options] <directory>`
```bash
$ mkdir newDirectory # directory can be absolute or relative
$ ls
newDirectory file.txt old_document
```

#### <code>rmdir</code> - [remove directory](https://linux.die.net/man/1/rmdir)
###### Syntax - `rmdir [options] <directory name>`
```bash
$ rmdir newDirectory # directory can be absolute or relative
$ ls 
file.txt old_document
```


#### <code>cat</code> - [used to join multiple files](https://linux.die.net/man/1/cat)
###### Syntax - `cat [options] <filename>`
* Where the `<filename>` can be repeated infinitely many times for concatenation
* Colloquially only one file is specified just so the user can view its contents in the terminal
```bash
$ cat myfile
this is the text contained in the myfile
```

#### <code>touch</code> - [creates a new blank file](https://man7.org/linux/man-pages/man1/touch.1.html)
###### Syntax - `touch [option] <filename>`
```bash
$ touch newdocument # file path can be absolute or relative
$ ls
newdocument file.txt old_document
```

#### <code>rm</code> - [remove file](https://man7.org/linux/man-pages/man1/rm.1.html)
###### Syntax - `rm [option] <filename>`
```bash
$ rm old_document # file path can be absolute or relative
$ ls
newdocument file.txt
```

#### <code>mv</code> - [move file or directory](https://linux.die.net/man/1/mv)
###### Syntax - `mv [option] <source> <destination>`
```bash
$ ls 
directory1 file1 file2 file3
$ mv file1 directory1 
$ ls
directory1 file2 file3
$ cd directory1
$ ls 
file1
```
---

### Input/Output Redirection
When writing a script in Linux it is often nice to change the behavior of the input, output, and error locations (the terminal is the default). For example, if you wanted to save time typing out input you could change the standard input (stdin) for your script from the terminal to a file. You can also redirect output such that instead of getting the standard output (stdout) in your terminal you could redirect the output to a file. This might be preferred when writing a script to reduce the noise and text overload on the screen. With piping we can even make the output of one command an input for another command.

> The examples below are adapted from this [resource](https://www.redhat.com/sysadmin/linux-shell-redirection-pipelining). If you would like to try out more complex types of redirection check out that article. Do not worry about how these commands specifically work as they will be explained later. For now understand the redirections that are happening.

#### Redirection using `>`
`command > file` 
* send standard output to `file`
```bash
$ echo "writinghelloworldtofile" > myfile.txt
$ cat myfile.txt
writinghelloworldtofile
```

`command &> file`
*  sends standard output and error output to a file
```bash
$ find /usr -name ls &> myfile.txt
$ cat myfile.txt
/usr/bin/ls # standard output
find: '/usr/share/polkit-1/rules.d': Permission denied #standard error
```

#### Redirection using `<`
`command < input`
* Feeds a command input from `input` file
```bash
$ sort < names.txt
abigail
nathan
william
```

#### Piping
Piping is a mechanism that makes it possible to allow commands to be combined or executed. Meaning one command’s output is used as an input for a concurrent command. It is referred to as “piping” as its concept is a process flow being channeled through a pipe from source to destination.

The piping operator is a vertical bar (`|`) to pipe one command to another, separating each command using this operator looks like:
```bash
$ command1 | command2 | command3 
```
```bash
$ echo "joe joe bob bob jane" | wc -w | grep [0-9]
```