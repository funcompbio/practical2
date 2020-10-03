---
layout: page
title: Practical 2
permalink: /practical2/
---

# Objectives

The learning objectives for this practical are:

 * Use a text editor to write your Unix scripts.
 * Redirect terminal output to a pipe.
 * Extract rows and columns.
 * Sorting lines.
 * Counting occurrences.
 * Pasting columns.

# Setup and background

We will download again some COVID19 data. Please follow the next two steps:

1. Go to the Catalan Health Departament COVID data portal at [https://dadescovid.cat](https://dadescovid.cat)
   and switch the language to "ENGLISH" using the pull-down menu on the top-right corner of the page.
2. Follow the downloads link and on the next page click and download the two
   files corresponding to the "7 DAY AGGREGATION" for "CATALUNYA" and "COUNTIES"
   ("COMARQUES" in Catalan). It is important that in the same day to
   make the data files comparable because the data is updated daily. Make sure you know
   exactly where in your filesystem these two files have been downloaded.
   **Tip:** some browsers automatically download files into a folder called "Downloads"
   or under a name corresponding to the translation of "Downloads" to the default
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
$ cat catalunya_setmanal.csv | head
```

Notice that while the `cat` command should send the output of the **whole** text file
to the terminal window, we only see the first few lines of that output because the pipe
has sent that whole output to the `head` command, which only shows the first few lines
of its input. This can be graphically represented as follows:
<img src="singlepipe.png" style="display: block; margin-left: auto; margin-right: auto; width: 50%; height: auto;">

# Extract rows and columns

Text files such as CSV files have a matrix layout with rows corresponding to lines
and columns to values separated by some delimiter character, which is a semicolon (`;`)
in the case of the previous file `catalunya_setmanal.csv`. Because of its matrix layout,
a CSV file can be always opened by any spreadsheet software, such as Microsoft Excel;
see image below.

<img src="ExcelCSV.png" style="display: block; margin-left: auto; margin-right: auto; width: 80%; height: auto;">

However, there are at least two circumstances in which working with CSV files from the
Unix command-line is preferable to do it from a spreadsheet software such as Microsoft
Excel:

  1. Spreadsheet software will always attempt to load the whole data into main memory.
  This may become prohibitive when having tens of thousands of rows, which is common for
  molecular data.
  2. Every spreadsheet software uses its own format to store the data, which makes it
  prone to [digital obsolescence](https://en.wikipedia.org/wiki/Digital_obsolescence),
  the fact that old software or and old version of a software that opens a file is
  no longer available. CSV files, and text files in general, can never become obsolete
  because their format does not depend on any specific software to be read or written.

Here we will learn to do two common operations on data organized in a matrix layout:
extract rows and extract columns. To extract rows from a text file in Unix we will
use the command `grep`, which requires two bits of information:

```
$ grep pattern filename
```
where `pattern` is the text that we expect to match to the lines we want to extract,
while `filename` is the name of the file from which we want to extract the lines matching
the pattern. Note that `pattern` can be something sophisticated such as a
[regular expression](https://en.wikipedia.org/wiki/Regular_expression) (not covered in
this practical) and `filename` can be ommitted when want `grep` to read input from
a pipe.

For instance, the column `RESIDENCIA` in the COVID19 data
indicates whether the row contains data derived from geriatric-care residences
(value `Si`) or not (value `No`). You can check the
[documentation](https://dadescovid.cat/documentacio?lang=eng) at the Catalan COVID19
data portal to understand why the data is provided separately for these two types
of population.

Let's say we want to extract the rows for the weekly county COVID19 data derived
from the population that does not live in geriatric-care residences into a separate
file called `catalunya_setmanal_general.csv`.

```
$ grep No catalunya_setmanal.csv > catalunya_setmanal_general.csv
```
Now, repeat the command but this time extracting the rows corresponding to the
population that **does live** in geriatric-care residences into a separate file
called `catalunya_setmanal_geriatric.csv`.

**Warning**: when using the terminal output redirection mechanism (`>`) you should
**never** use as output filename, a filename that is being used as input in the same
command line because that would lead to overwriting the input file and end without or
with corrupted output.

Extracting columns can be done using the Unix command `cut`, which in the case of
CSV files also requires specifying the options `-d` and `-f`:

```
$ cut -d 'delimiter' -f field filename
```
The option `-d` allows us to specify a
[delimiter character](https://en.wikipedia.org/wiki/Delimiter), which by default
is the [TAB character](https://en.wikipedia.org/wiki/Tab_key) and should be always
specified between single quotes (e.g., `,`). The option `-f` allows us to specify
the columns, also known as
([fields](https://en.wikipedia.org/wiki/Data_field) in this context. For instance,
let's say we want to extract the last column of the CSV file
`catalunya_setmanal.csv`, corresponding to the number of exitus at each week.
Taking into account that this file uses the semicolon (`;`) as field separator,
we should write:

```
$ cut -d ';' -f 15 catalunya_setmanal.csv | head
```

Now let's say we want to extract this column from the geriatric subset of the data.
We could either run the previous command on the file we created before:

```
$ cut -d ';' -f 15 catalunya_setmanal_geriatric.csv | head
```
or had we not generated that file, we could have done it from the original data
file using two pipes, as follows:

```
$ grep Si catalunya_setmanal.csv | cut -d ';' -f 15 catalunya_setmanal_geriatric.csv | head
```
Note that in both cases the output is identical.

# Sort rows

Unix provides a command called `sort` to order rows of a file in a number of ways.
By default, it orders rows in increasing alphabetical order. Note for instance that
in the `cataluna_setmanal.csv` file, the column `DATA_INI` and `DATA_FI` contain the
initial and end date of the recorded data for each row, that first lines correspond
to more recent data and that date is written in a format that the alphabeic order
matches the time order. Type the following four commands:

```
$ cut -d ';' -f 3,15 catalunya_setmanal.csv | head
$ cut -d ';' -f 3,15 catalunya_setmanal.csv | tail
$ cut -d ';' -f 3,15 catalunya_setmanal.csv | sort | head
$ cut -d ';' -f 3,15 catalunya_setmanal.csv | sort | tail
```
Looking at the output of each them, can you explain their differences?

# Remove consecutive duplicated lines

The Unix command `uniq` removes consecutive duplicated lines. Look for instance
at the beginning and the end of the file `comarques_setmanal.csv`:

```
$ head comarques_setmanal.csv
NOM;CODI;DATA_INI;DATA_FI;RESIDENCIA;IEPG_CONFIRMAT;R0_CONFIRMAT_M;TAXA_CASOS_CONFIRMAT;CASOS_CONFIRMAT;TAXA_PCR;PCR;PERC_PCR_POSITIVES;INGRESSOS_TOTAL;INGRESSOS_CRITIC;EXITUS
ALT CAMP;01;2020-09-22;2020-09-28;Si;;;0.0000;0;1192.8429;6;0.0000;0;0;0
ALT CAMP;01;2020-09-22;2020-09-28;No;20.6327;0.282313;9.1355;4;938.6776;411;0.9901;0;0;0
ALT CAMP;01;2020-09-21;2020-09-27;Si;;;0.0000;0;1192.8429;6;0.0000;0;0;0
ALT CAMP;01;2020-09-21;2020-09-27;No;27.127;0.329932;18.2710;8;899.8515;394;1.7903;0;0;0
ALT CAMP;01;2020-09-20;2020-09-26;No;35.8897;0.436508;22.8388;10;897.5676;393;2.3077;0;0;0
ALT CAMP;01;2020-09-20;2020-09-26;Si;;;0.0000;0;1192.8429;6;0.0000;0;0;0
ALT CAMP;01;2020-09-19;2020-09-25;Si;;;0.0000;0;1192.8429;6;0.0000;0;0;0
ALT CAMP;01;2020-09-19;2020-09-25;No;52.4957;0.604875;25.1227;11;922.6904;404;2.4938;0;0;0
ALT CAMP;01;2020-09-18;2020-09-24;Si;;;0.0000;0;8548.7077;43;0.0000;0;0;0
$ tail comarques_setmanal.csv 
VALLES ORIENTAL;41;2020-02-28;2020-03-05;Si;;;0.0000;0;0.0000;0;0.0000;0;0;0
VALLES ORIENTAL;41;2020-02-28;2020-03-05;No;;;0.0000;0;2.4560;10;0.0000;0;0;0
VALLES ORIENTAL;41;2020-02-27;2020-03-04;Si;;;0.0000;0;0.0000;0;0.0000;0;0;0
VALLES ORIENTAL;41;2020-02-27;2020-03-04;No;;;0.0000;0;1.9648;8;0.0000;0;0;0
VALLES ORIENTAL;41;2020-02-26;2020-03-03;Si;;;0.0000;0;0.0000;0;0.0000;0;0;0
VALLES ORIENTAL;41;2020-02-26;2020-03-03;No;;;0.0000;0;1.7192;7;0.0000;0;0;0
VALLES ORIENTAL;41;2020-02-25;2020-03-02;No;;;0.0000;0;1.2280;5;0.0000;0;0;0
VALLES ORIENTAL;41;2020-02-25;2020-03-02;Si;;;0.0000;0;0.0000;0;0.0000;0;0;0
VALLES ORIENTAL;41;2020-02-24;2020-03-01;No;;;0.0000;0;0.7368;3;0.0000;0;0;0
VALLES ORIENTAL;41;2020-02-24;2020-03-01;Si;;;0.0000;0;0.0000;0;0.0000;0;0;0
```
Lines appear to be grouped by county name, which occurs in the first column.
Let's extract the first column, apply the `uniq` command on its output and
count the number of resulting lines:

```
$ cut -f 1 -d ';' comarques_setmanal.csv | uniq | wc -l
      43
```
Can you figure out to what corresponds this number?

In this case, duplicated lines were occurring in consecutively one after each
other. However, if this were not the case, what do you think we could do before
using the `uniq` command to bring duplicated lines together?

# Count consecutive occurrences

In a previous practical, we have seen the command `wc`, which can be employed to
count the lines of a text file. Here we want to learn the command `uniq` with
its option `-c`, which allows one to count consecutive repeated lines. This is
useful to count occurrences of interest in a file. For instance, let's say we
want to count the number of different exitus occurrences in the file
`catalunya_setmanal.csv`, i.e., how many lines (weeks) reported 0 exitus, how
many reported 1, how many reported 2, etc. We need to extract the exitus column
(15), sort it and apply the `uniq -c` command:

```
$ cut -f 15 -d ';' catalunya_setmanal.csv | sort | uniq -c | head
```
Let's say we want to see the most frequent occurrences first. We would need then
to sort the previous output numerically (option `-n`) and from largest to
smallest (option `-r`), as follows:

```
$ cut -f 15 -d ';' catalunya_setmanal.csv | sort | uniq -c | sort -n -r | head
 153 0
  19 11
  16 9
  12 25
  12 10
  12 1
  11 8
  10 2
   9 27
   9 23
```
So the most frequent reported exitus figure was 0 in 153 lines, the second most
frequent one was 11 in 19 lines, and so on. We can also tell `sort` to order
that output by the second column using the option `-k`, which would give us
the whole ordered frequency distribution of exitus:

```
$ cut -f 15 -d ';' catalunya_setmanal.csv | sort | uniq -c | sort -n -k 2 | head -20
   1 EXITUS
 153 0
  12 1
  10 2
   6 3
   7 4
   3 5
   4 6
   4 7
  11 8
  16 9
  12 10
  19 11
   9 12
   7 13
   6 14
   2 15
   3 16
   9 17
   2 18
```

# Paste columns

The Unix command `paste` allows us to paste in parallel lines of given files using
a `TAB` as delimiter character by default, which can be changed with the option
'-d'. For instance, see what happens when extract two columns from the CSV file
and paste them again:

```
$ cut -d ';' -f 3 catalunya_setmanal.csv > catalunya_setmanal_dataini.csv
$ cut -d ';' -f 15 catalunya_setmanal.csv > catalunya_setmanal_exitus.csv
$ paste catalunya_setmanal_dataini.csv catalunya_setmanal_exitus.csv | head
DATA_INI	EXITUS
2020-09-12	57
2020-09-12	0
2020-09-12	13
2020-09-11	0
2020-09-11	14
2020-09-11	56
2020-09-10	53
2020-09-10	0
2020-09-10	17
```

# Exercise

Compare the number of exitus among general population (i.e., excluding geriatric
residences) in the month of March 2020 between two of your favorite Catalan counties.
For instance, this would be the output for `SEGRIA` (left) and `OSONA` (right):

```
11      53
10      54
11      57
13      53
13      53
14      50
12      51
13      40
13      38
12      34
10      36
10      30
6       23
7       16
4       17
2       14
2       12
2       8
2       6
1       6
0       4
0       1
0       1
0       1
0       1
0       0
0       0
0       0
0       0
0       0
0       0
0       0
0       0
0       0
0       0
0       0
0       0
```
