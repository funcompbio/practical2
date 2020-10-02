---
layout: page
title: Practical 2
permalink: /practical2/
---

# Objectives

The learning objectives for this practical are:

 * Use a text editor to write your Unix scripts.
 * Redirect terminal output to a pipe.
 * Create your own GitHub profile.

# Setup and background

We will download again some COVID19 data. Please follow the next two steps:

1. Go to the Catalan Health Departament COVID data portal at [https://dadescovid.cat](https://dadescovid.cat)
   and switch the language to "ENGLISH" using the pull-down menu on the top-right corner of the page.
2. Follow the downloads link and on the next page click and download the two files corresponding to the
   "7 DAY AGGREGATION" and the "DAILY DATA" for "COUNTIES" ("COMARQUES" in Catalan). Make sure you know exactly where in your
   filesystem these two files have been downloaded. **Tip:** some browsers automatically download files into
   a folder called "Downloads" or under a name corresponding to the translation of "Downloads" to the default
   language of your operating system.
3. Make a directory in your filesystem, for instance at your _home_ directory,
   called `practical2` and copy in it the CSV files contained in the previous two
   ZIP files that you have downloaded, as we analogously did in practical 1.

# Use a text editor to write your Unix scripts.

If you have not done so yet, please download an install a text editor application
in your computer, following the [setup](/setup/) instructions. Once the text editor
application is installed, run it as follows:

  * If you have installed a _classical_ text editor, open the editor in a new terminal window. Classical editors are tipically called from the Unix shell and it's handy to given them the name of the file you want to edit right way as you called them as a first argument. If you do that, give them the filename `practical2.sh`.

  * If you have installed a _modern_ text editor, open it by starting the application from the graphical user interface of your computer.

Once the text editor application is running, write the following two lines on a new
fresh empty text file:

```
## Script for practical 2

```

Save these contents into a file called `practical2.sh` located at the directory
`practical2` that you created before. During the rest of this practical, write
all the Unix commands that you type in the terminal also in the text file. Please
don't type them twice, first type them in the terminal window and once they work,
then using the mouse copy and paste the command-line into the text editor. Each
time you copy a new line, save the file again. To keep a better record of what
you are doing, add above each shell line a shell comment line, which always starts
with one or more hash characters (`#`), for **example**:

```
## list the files
ls
```

# Redirect terminal output to a pipe 

In the previous practical we have seen how to redirect the terminal output
to a file. In this section we are going to see a similar concept, where instead
of redirecting the terminal output created by a Unix command to a file, we
will redirect that terminal output to *another Unix command* by using what is
known as a [Unix pipeline](https://en.wikipedia.org/wiki/Pipeline_%28Unix%29),
or Unix pipe for short.

The concept of Unix pipes was created by
[Douglas McIlroy](https://en.wikipedia.org/wiki/Douglas_McIlroy), which is based on
his widely adopted summary of the [Unix philosophy](https://en.wikipedia.org/wiki/Unix_philosophy):

> Write programs that do one thing and do it well. Write programs to work
> together. Write programs to handle text streams, because that is a universal
> interface.

The Unix pipe consists of placing the vertical bar (`|`), available in Spanish keyboards
by pressing `AltGr+1`, between two Unix commands. Try the following:

```
$ cat comarques_setmanal.csv | head
```

Notice that while the `cat` command should send the output of the **whole** text file
to the terminal window, we only see the first few lines of that output because the pipe
has sent that whole output to the `head` command, which only shows the first few lines
of its input.
