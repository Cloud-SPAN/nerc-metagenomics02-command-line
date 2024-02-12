---
title: "Working with Files and Directories"
teaching: 30
exercises: 15
questions:
- "How can I view and search file contents?"
- "How can I create, copy and delete files and directories?"
- "How can I repeat recently used commands?"
objectives:
- View, search within, copy, move, and rename files. Create new directories.
- Use wildcards (`*`) to perform operations on multiple files.
- Use the `history` command to view and repeat recently used commands.
keypoints:
- "You can view file contents using `less`, `cat`, `head` or `tail`."
- "The commands `cp`, `mv`, and `mkdir` are useful for manipulating existing files and creating new directories."
- "The `history` command and the up arrow on your keyboard can be used to repeat recently used commands."
---

## Working with Files

### Our data set: FASTQ files

Now that we know how to navigate around our directory structure, let's start working with our sequencing files. 
We are looking at the results from a short-read sequencing experiment, which are stored in our `illumina_fastq` directory.

### Wildcards

Navigate to your `illumina_fastq` directory:

~~~
$ cd ~/cs_course/data/illumina_fastq
~~~
{: .bash}

We are interested in looking at the fastq files in this directory. We can list
all files with the .fastq extension using the command:

~~~
$ ls *.fastq
~~~
{: .bash}

~~~
ERR4998593_1.fastq  ERR4998593_2.fastq
~~~
{: .output}

The `*` character is a special type of character called a wildcard, which can be used to represent any number of any type of character.
Thus, `*.fastq` matches every file that ends with `.fastq`.

This command:

~~~
$ ls *_2.fastq
~~~
{: .bash}

~~~
ERR4998593_2.fastq
~~~
{: .output}

lists only the file that ends with `_2.fastq`.

This command:

~~~
$ ls /usr/bin/*.sh
~~~
{: .bash}

~~~
/usr/bin/amuFormat.sh  /usr/bin/gettext.sh  /usr/bin/gvmap.sh
~~~
{: .output}

Lists every file in `/usr/bin` that ends in the characters `.sh`.
Note that the output displays __full__ paths to files, since
each result starts with `/`.

> ## Exercise
> What command would you use for each of the following tasks? Start from your current directory using a
> single `ls` command for each:
>
> 1.  List all of the files in `/usr/bin` that start with the letter 'c'.
> 2.  List all of the files in `/usr/bin` that contain the letter 'a'.
> 3.  List all of the files in `/usr/bin` that end with the letter 'o'.
> 4.  List all of the files in `/usr/bin` that contain the letter 'a' or the letter 'c'.
>
> Bonus: What would the output look like if a wildcard could *not* be matched? Try listing all files that start 
> with 'missing'.
>
> Hint: Question 4 requires a Unix wildcard that we haven't talked about
> yet. Try searching the internet for information about Unix wildcards to find
> what you need to solve the bonus problem.
>
> > ## Solution
> > 1. `ls /usr/bin/c*`
> > 2. `ls /usr/bin/*a*`
> > 3. `ls /usr/bin/*o`  
> > 4. `ls /usr/bin/*[ac]*`
> > 
> > Bonus: `ls: cannot access 'missing*': No such file or directory`
> {: .solution}
{: .challenge}


## Command History

If you want to repeat a command that you've run recently, you can access previous
commands using the up arrow on your keyboard to go back to the most recent
command. Likewise, the down arrow takes you forward in the command history.

A few more useful shortcuts:

- <kbd>Ctrl</kbd>+<kbd>C</kbd> will cancel the command you are writing, and give you a
fresh prompt.
- <kbd>Ctrl</kbd>+<kbd>R</kbd> will do a reverse-search through your command history.  This
is very useful.
- <kbd>Ctrl</kbd>+<kbd>L</kbd> or the `clear` command will clear your screen.

You can also review your recent commands with the `history` command, by entering:

~~~
$ history
~~~
{: .bash}

to see a numbered list of recent commands. You can reuse one of these commands
directly by referring to the number of that command.

For example, if your history looked like this:

~~~
259  ls *
260  ls /usr/bin/*.sh
261  ls *R1*fastq
~~~
{: .output}

then you could repeat command #260 by entering:

~~~
$ !260
~~~
{: .bash}

Type `!` (exclamation point) and then the number of the command from your history.
You will be glad you learned this when you need to re-run very complicated commands.
For more information on advanced usage of `history`, read section 9.3 of
[Bash manual](https://www.gnu.org/software/bash/manual/html_node/index.html).

## Examining Files --- the `less` program

We now know how to switch directories, run programs, and look at the
contents of directories, but how do we look at the contents of files?

One way to examine a file is to open the file in a read-only format and navigate through it using a program called `less`. The commands for navigating `less` are the same as the `man` program:

| key     | action |
| ------- | ---------- |
| <kbd>Space</kbd> | to go forward |
|  <kbd>b</kbd>    | to go backward |
|  <kbd>g</kbd>    | to go to the beginning |
|  <kbd>G</kbd>    | to go to the end |
|  <kbd>q</kbd>    | to quit |

Enter the following command from within the `illumina_fastq` directory:

~~~
$ cd ~/cs_course/data/illumina_fastq
$ less ERR4998593_1.fastq
~~~
{: .bash}

> ## FASTQ format
> The contents might look a bit confusing. That's because they are in FASTQ format, a popular way to store sequencing data in text-based format.
> These files contain both sequences and information about each sequence's read accuracy.
>
> ![](../fig/fasta_file_format_l2.png){:width="600px"}
>
> Each sequence is described in four lines:
>
> |Line|Description|
> |----|-----------|
> |1|Always begins with '@' and gives the sequence identifier and an optional description|
> |2|The actual DNA sequence|
> |3|Always begins with a '+' and sometimes the same info in line 1|
> |4|Has a string of characters which represent the PHRED quality score for each of the bases in line 2; must have same number of characters as line 2|
>
> ## PHRED score
> ~~~
> Quality encoding: !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJ
>                   |         |         |         |         |
> Quality score:    01........11........21........31........41   
> ~~~
> {: .output}
>
> Quality is interpreted as the probability of an incorrect base call. To make it possible to line up each individual nucleotide with its quality score, the numerical score is encoded by a single character. The quality score represents the probability that the corresponding nucleotide call is incorrect. It is a logarithmic scale so a quality score of 10 reflects a base call accuracy of 90%, but a quality score of 20 reflects a base call accuracy of 99%.                 
{: .callout}

> ## Exercise
>
> 1. Open the `~/cs_course/data/illumina_fastq/ERR4998593_1.fastq` file in `less`. What is the last line of the file? (Hint: use the shortcuts above to speed things up)
>
> > ## Solution
> > 1. `:FFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFF:FFFFFFFF,FFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFF:FFF:FFFFFFFFFFFFFF:FFFFFF:FF:FFFFFFFFFFFFFFFFFFFFFF,F,FFF,FFFFFF,FFFF`
> {: .solution}
{: .challenge}

### Some navigation commands in `less`:

`less` also gives you a way of searching through files. Use the "/" key to begin a search. 
Enter the word you would like to search for and press `enter`. 
The screen will jump to the next location where that word is found.

**Shortcut:** If you hit "/" then "enter", `less` will  repeat the previous search.
`less` searches from the current location and works its way forward. Scroll up a couple lines on your terminal to verify you are at the beginning of the file. 
Note, if you are at the end of the file and search for the sequence "CAA", `less` will not find it. 
You either need to go to the beginning of the file (by typing `g`) and search again using `/` or you can use `?` to search backwards in the same way you used `/` previously.

For instance, let's search forward for the sequence `TTTTT` in our file.
You can see that we go right to that sequence, what it looks like, and where it is in the file. If you continue to type `/` and hit return, you will move
forward to the next instance of this sequence motif. If you instead type `?` and hit return, you will search backwards and move up the file to previous examples of this motif.

> ## Exercise
>
> What are the next three nucleotides (characters) after the first instance of the sequence quoted above (`TTTTT`)?
>
> Share your answer to o see if it matches everyone else's!
>
> > ## Solution
> > `CTT`
> {: .solution}
{: .challenge}

Remember, the `man` program actually uses `less` internally and therefore uses the same commands, so you can search documentation using "/" as well!

### Other programs to look into files: `cat`, `more`, `head`, and `tail`  

Another way to look at files is using the command `cat`. This command prints out the entire contents of the file to the console. In large files, like the ones we're working with today, this can take a long time and should generally be avoided. For small files, it can be a useful tool.

The `more` command prints to the console only as much content of a file as it fits in the screen, and waits for you to press the space bar to print the following portion of the file likweise, and so on until either the last portion of the file is printed or you press the `q` key (for quit) to exit `more`.

There's another final way that we can look at files, and in this case, just look at part of them. This can be particularly useful if we just want to see the beginning or end of the file, or see how it's formatted.

The commands are `head` and `tail` and they let you look at the beginning and end of a file, respectively.

~~~
$ head ERR4998593_1.fastq
~~~
{: .bash}

~~~
@ERR4998593.40838091 40838091 length=151
CCACATGCTTTAAGTGCATGTGGTACTGCTCCAGGACCAGCATTGTAGGTCGCCAATGCTTTGGCGTAGGTGCCATCAAACATATTCGTGTAATGAGCCATGAGATGGGCTGCTCCCTTCAATGCATCAACCGGATTCCACGGATCAATGC
+
:FFFFFFF:,FFF:FFFFFFFFFFFFFFFFFFFF:FFFF:FFFFFFFFFFF:FFFFFFFFF:FFFF:FFFFFFFFFFFFFFFFFFF:FFFF,FFFFFFF,FFF:FF,FFFFFFF::FF::FFFF:FFFFF:,:FFFFF,,FFFFFFFF,,F
@ERR4998593.57624042 57624042 length=151
CCTTACCACACCGGGGCTGTGGCGTTCGACCCCATCGGCAAGGCACTCTGGGTTTCCGATAGCTCGCACCATCGGCTGCTGCGCGTCCGCAATCCGGACGGCTGGGAGAGCAAACTGCTCGTGGACACGGTCATCGGTCAGAAGGACAGGT
+
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFF
@ERR4998593.3 3 length=151
GNGGTGCTCGACGGTGGCTCGGCGGATGCGCATGGCGTCGGGCCTGCGGTCCAGCCGCTCCCGCATGGCGTCGATCACCGCCTCATGCTCCCAGCGTTTGATGCGGCGCTCCTTGCCGCTCGTACACCGGCTCTTCAGCGGGCAGCCGGCGta
~~~
{: .output}

~~~
$ tail ERR4998593_1.fastq
~~~
{: .bash}

~~~
+
FFF:F,FF:FFFFFF:F,:FFF,FF:FF,FFF::F:F,FF,FF,FFF,FFFFFF:FFFFFFF,F,,FFF:FFFF:,FFFF:FF::F:FFF,F:FFFF,:FFFFF,F:F:FF,FF:F:FFFF:FFF:FFF::FF:FF:,::FF,FF:,F,FF
@ERR4998593.55595926 55595926 length=151
CAGTACAACGTTCGCTCCCTGAATTTCTGTTTCTCGGCCGGCGAAGCAATTGCTGTGGCTATCCAGGAGCGGTTCAAGCGGATGTTCGGCGTCGAAATTACGGAAGGCTGCGGGATGACCGAACTGCAAATTTACTCCATGAATCCGCCAT
+
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFFF:FFFFFFF:FF:FFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
@ERR4998593.34263610 34263610 length=151
ACGCCCCACAGGGCGGCACCGACGCCGCCGCCCGGGCCCGCCGGCCCGCCCCGGTGGGCACCGGTTGCCACTGCGGCTTGCTCGGCCGTCTCACTCACTTGGACACACTTCCGTTCTTCACCGTCTCCACTGGCCGGCTAGACCGGTCCCG
+
FFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF,FFF,FFFF:FFFFFFFF,FFFFFF::F:F,FFFFFF,FFFFFFFFFFFFF,F:F:FF,FFFF
~~~
{: .output}

The `-n` option to either of these commands can be used to print the first or last `n` lines of a file.

~~~
$ head -n 1 ERR4998593_1.fastq
~~~
{: .bash}

~~~
@ERR4998593.40838091 40838091 length=151
~~~
{: .output}

~~~
$ tail -n 1 ERR4998593_1.fastq
~~~
{: .bash}

~~~
F:FFFFFFFFFFFFFFFFFFFF:FFFFFFFFFFFFFFFF:FFFFFFFF,FFFFFFFFFFFFFFFFFFFFFFFFFF:FFFFF:FFF:FFFFFFFFFFFFFF:FFFFFF:FF:FFFFFFFFFFFFFFFFFFFFFF,F,FFF,FFFFFF,FFFF
~~~
{: .output}

## Creating, moving, copying, and removing files

Now we can move around in the file structure, look at files, and search files. But what if we want to copy files or move
them around or get rid of them? Most of the time, you can do these sorts of file manipulations without the command line,
but there will be some cases (like when you're working with a remote computer like we are for this lesson) where it will be
impossible. You'll also find that you may be working with hundreds of files and want to do similar manipulations to all
of those files. In cases like this, it's much faster to do these operations at the command line.

We'll continue looking at our large Illumina sequencing files for the next part of the lesson.

### Copying Files

When working with computational data, it's important to keep a safe copy of that data that can't be accidentally overwritten or deleted.
For this lesson, our raw data is our FASTQ files.  

First, let's make a copy of one of our FASTQ files using the `cp` command.

Navigate to the `cs_course/data/illumina_fastq` directory and enter:

~~~
$ cp ERR4998593_1.fastq ERR4998593_1_copy.fastq
$ ls -F
~~~
{: .bash}

~~~
ERR4998593_1.fastq  ERR4998593_1_copy.fastq  ERR4998593_2.fastq
~~~
{: .output}
The prompt will disappear for up to two minutes and reappear when the command is completed and the backup is made.

We now have two copies of the `ERR4998593_1.fastq` file, one of them named `ERR4998593_1_copy.fastq`. We'll move this file to a new directory
called `backup` where we'll store our backup data files.

### Creating Directories

The `mkdir` command is used to make a directory. Enter `mkdir`followed by a space, then the directory name you want to create:

~~~
$ mkdir backup
~~~
{: .bash}

### Moving / Renaming files and directories

We can now move our backup file to this directory. We can move files around using the command `mv`:

~~~
$ mv ERR4998593_1_copy.fastq backup
$ ls backup
~~~
{: .bash}

~~~
ERR4998593_1_copy.fastq
~~~
{: .output}

The `mv` command is also how you rename files. Let's rename this file to make it clear that this is a backup:

~~~
$ cd backup
$ mv ERR4998593_1_copy.fastq ERR4998593_1_backup.fastq
$ ls
~~~
{: .bash}

~~~
ERR4998593_1_backup.fastq
~~~
{: .output}

### Removing files and directories

You can delete or remove files with the `rm` command:

~~~
$ rm ERR4998593_1_backup.fastq
~~~
{: .bash}

**Important**: The `rm` command permanently removes the file. Be careful with this command. It doesn't just nicely put the files in the recycling. They're really gone.

By default, `rm` will not delete directories. You can tell `rm` to delete a directory using the `-r` (recursive) option. Let's delete the backup directory
we just made:

~~~
$ cd ..
$ rm -r backup
~~~
{: .bash}

This will delete not only the directory, but all files within the directory.

> ## Exercise
>
> Starting in the `illumina_fastq` directory, do the following:
> 1. Make sure that you have deleted your backup directory and all files it contains.  
> 2. Create a backup of each of your FASTQ files using `cp`. (Note: You'll need to do this individually for each of the two FASTQ files. We haven't
> learned yet how to do this with a wildcard.)  
> 3. Use a wildcard to move all of your backup files to a new backup directory.
> 4. It doesn't make sense to keep our backup directory inside the directory it is backing up. What if we accidentally delete the `illumina_fastq` directory?
> To fix this, move your new backup directory out of `illumina_fastq` and into the parent folder, `data`.
>
> > ## Solution
> >
> > 1. `rm -r backup`  
> > 2. `cp ERR4998593_1.fastq ERR4998593_1_backup.fastq` and `cp ERR4998593_2.fastq ERR4998593_2_backup.fastq`  
> > 3. `mkdir backup` and `mv *_backup.fastq backup`
> > 4. `mv backup ..` or `mv backup ~/cs_course/data/` (note that you do not need to use the -r flag to move directories like you do when deleting them)
> > 
> > It's always a good idea to check your work. Move to the `data` folder with `cd ..` and then list the contents of `backup` with `ls -l backup`. You should see something like:
> > ~~~
> > -rw-rw-r-- 1 csuser csuser 2811886584 Feb 22 11:25 ERR4998593_1_backup.fastq
> > -rw-rw-r-- 1 csuser csuser 2302264784 Feb 22 11:29 ERR4998593_2_backup.fastq
> >
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Here is what your file structure should look like at the end of this episode:
![A file hierarchy tree](../fig/file_tree_02_ep2.png){:width="500px"}
