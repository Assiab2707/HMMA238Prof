Introduction to bash
====================

# Preambule

As you already know bash prompt is a `$` sign when you are a standard user. When you are an administrator (often called `root` user) the prompt is a `#`.  

# The Unix directory structure

Some aliases:

- `~` is an alias to your home directory. 
- `.` is an alias to the current directory. 
- `..` is an alias to the parent directory.

For instance 

```bash
$ cd ~
$ pwd
$ cd ../../home/../etc/../home/
$ pwd
```

**Exercise:**
   1. What is the difference between `cd ./toto/tata`, `cd toto/tata` `~/toto/tata` and `/toto/tata`

Many details on the Unix directory structure at https://www.howtogeek.com/117435/htg-explains-the-linux-directory-structure-explained/


## Getting help

To get some help for a command, please use the `man` command. You may also use the `--help` option as in 
```bash
$ man ls
$ ls --help
```

## Regexp 101

Some of the most common regula expression are

- `^` start of line
- `.` any single character 
- `$`  end of line 
- `x*` zero or more occurrence of character x
- `x+` one or more occurrences of character x
- `x?` zero or one occurrence of character x
- `x{n}` exactly n occurrence of character x
- `[...]` range of characters (e.g. [a-z], [A-Z], [a-zA-Z], [0-9], etc...)
- `[^...]` plage de caractères interdits 
 ...

Regexp can be used in various places (`ls`, `grep`, `find`, etc...). For instance to display all the files with an extension in `txt`:
```bash
$ ls *.txt
```
**Exercise:**

1. Go to `/usr/lib/R/bin/R` and list every files starting with a letter `R` and containing i
2. Got to `/var/log/` and list every files with extension `.log`
3. Got to `/var/log/` and list every files with a name starting with a `a` and containing a digit

## Listing files

To list the files in a folder use the command `ls`.

**Exercise:** 

1. describe the option `-a` 
2. describe the option `-R`
3. describe the option `-lh` 
4. List all the files in the directory `/usr/lib/` without `cd` in it

The `file` command can be used to display the information of a file (if not given by the extension...)

**Exercise:**

1. List all the files in the directory `/usr/lib/R/bin` and sort them by size
2. display the type information of the files in `/var/log/` one call to `file`
  
  
## Users

To list the groups you belong to 
```bash
$ groups
```
To list the connected user on your machine 
```bash
$ w
$ who
```

## File permissions

Each file has an **owner** (a user) and a **group** (a group of user). There is 3 types of permissions:

1. read  `r`
2. write `w`
3. execute `x`

There is three permission triads

1. first triad 	 what the owner can do
2. second triad 	what the group members can do
3. third triad    	what other users can do

Each triad

1. first character 	    r: readable
2. second character 	    w: writable
3. third character 	    x: executable

Ref: https://en.wikipedia.org/wiki/File_system_permissions

## Paging program

A paging program displays, one windowful at a time, the contents of a file on a terminal. It pauses after each windowful and prints on the window status line the
       screen the file name, current line number, and the percentage of the file so far displayed. This is not an editor (no modification of the file can be done)

`more` (deprecated) `less` (best choice) `most` (default on your machine, more feature than `less`, but bad keybindings).

```bash
$ man less
$ man most
```

**Useful tips:**

- to search for a word type `/`. To go to the next (resp. previous)  occurence type `n` (resp. `p`). 
- [less only] to go down type `j`, to go up type `k` 
- to go to the begining of file type `g`, to the end `G`
- To quit type `q`. 
- to change the default paging program to `less` 
```bash 
$ export MANPAGER=less
```

Some extra informations: https://unix.stackexchange.com/questions/81129/what-are-the-differences-between-most-more-and-less

# Environment variables

An environment variable (env or envs) is a dynamic-named value that can affect the way running processes will behave on a computer. Many option of bash may be change with envs. To print all the defined envs:
```bash
$ printenv
```
To display a single variable, you may use the prefix `$`. For instance, to display the content of `PATH`
```bash
$ echo ${PATH}
```
To set a new variable (in bash)
```bash
$ export ENV_NAME=toto:tata 
```
List are often separated by `:`. To append a new value at the end 
```bash
$ export ENV_NAME=${ENV_NAME}:tutu
$ echo ${ENV_NAME}
```

Some documentation: https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-a-linux-vps

**Exercice**

 1. Display the PATH env
 2. Is the order of the list important?

## Text editor

In bash, many configuration files are in fact **text file**. You may need to choose a text editor to modify them. Very powerful (and thus complicated) text editors exist: emacs, vim, ... We will focus on nano
```bash
$ nano
```
or `joe` (default on your system).

**Exercice**
  
   1. Set `nano` as your default text editor 


## Some useful command

- list files and get informations: `ls`, `file`, `find` 
- display text content: `echo`, `cat`, `head`, `tail`, `grep`, `fgrep`, `rgrep`
- file handling: `touch`, `mv`, `cp`, `rsync`, `rename`
- unix admin: `which`, `who`, `top`, `htop`, `kill`, `pkill`, `killall`

# Display text content

## Get the data

The dataset we are going to use is available at https://www.data.gouv.fr/fr/datasets/accidents-de-velo-en-france/. We will focus on bicycles accident in France from 2005-2017.

**Exercise:**

1. Create a folder `data_bicycle` and `cd` to it.
2. Download the csv file available at the following url: https://www.data.gouv.fr/fr/datasets/r/ab84353b-498b-4ef5-9a02-a6403f2ead96 as `bicycle_db.csv` (option `-o` of  `wget`)

## Tail, head, cat, wc, split

Please read the manual of `tail`, `head`, `cat`, `wc` and `split`

**Exercise:**

1. Use the word count `wc` command to display the number of lines of `bicycle_db.csv`
2. Display the 53 first line with the `head` command. Same with the 30 last lines (see `tail`)
3. Use the `split` command and its options `-d` `-l` and `--additional-suffix` to create files with a maximum number of lines of 10000 (hint you should get only 6 files) with names `bike01.csv`, ..., `bike06.csv`
  
  
## Grep

Grep print lines of a file  matching a pattern (regex).
```bash
$ man grep
```

**Exercise:**

1. Display the line number of the accident occuring a wednesday of octobre 2017
2. Count the number of accident in 2005 using the command `grep` (hint: remark that each line **starts** with the string `"YYYY` where `YYYY` is the year)

# Pipes and redirections

The I/O of any program launch through the bash are organized in three data streams:

-  STDIN (0) - Standard input (input)
-  STDOUT (1) - Standard output (data output by the command and printed in the terminal)
-  STDERR (2) - Standard error (reserved for error messages, also printed in the terminal)


Piping and redirection is the process used to connect these streams between programs and files.

See: https://ryanstutorials.net/linuxtutorial/piping.php

## Pipes 

In bash the pipe operator is denoted `|`. It allows to compose (mathematically) the ouput of a program as an input of another one. For instance to display the 10 largest file given by `du` (disk use)
```bash
$ du | sort -nr | head
```
or display it in a pager
```bash
$ du | sort -nr | less
```

**Exercise:**
  1. Display the last 15 accident occuring with `Vent fort` condition
  2. Display the type of crossing of the accident occuring with `Vent fort` in 2010. It should return
```bash
Intersection en X
Intersection en T
Intersection en X

```

## Redirections

The operator `>` redirect the stdout of a command (LHS) into a file (RHS). Warning! it erases the file content. The operator  `>>` append the output of the LHS to a file. 

```bash
$ ls /etc > toto.txt
$ cat toto.txt
$ wc -l toto.txt >> toto.txt
$ cat toto.txt
```
Finally, the operator `<` read from the file (RHS) and send the content to stdin (LHS)
```bash
$ wc -l < toto.txt
```



**Exercise:**
1. Create a single file `bike2016.csv` containing all the accident that occured in 2016.
2. Append the accidents of year 2017 to the previous file and then rename it `bike2016_17.csv`.