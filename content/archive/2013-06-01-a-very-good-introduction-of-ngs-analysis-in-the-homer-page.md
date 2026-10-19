---
title: A very good introduction of NGS analysis in the HOMER page
author: Ming Tang
date: '2013-06-01T16:48:00-07:00'
lastmod: '2013-06-01T16:48:00-07:00'
slug: a-very-good-introduction-of-ngs-analysis-in-the-homer-page
tags:
- homer
- NGS
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/06/a-very-good-introduction-of-ngs.html
description: See the link here:\ http://biowhat.ucsd.edu/homer/basicTutorial/index.html
summary: See the link here:\ http://biowhat.ucsd.edu/homer/basicTutorial/index.html
header:
  caption: ''
  image: ''
---

See the link here:\
[http://biowhat.ucsd.edu/homer/basicTutorial/index.html](http://homer.ucsd.edu/homer/basicTutorial/index.html)

## Setting up your computing environment

Next-generation sequencing analysis is a compute intensive process.  Your average laptop is probably not up to the challenge.  Most software is geared toward UNIX style operating systems, with large servers in mind.  Typically, analysis algorithms will be distributed by researchers in one of three ways:

1.  Stand alone program to run in a UNIX environment (most common)
2.  Webserver where you upload data to be analyzed (i.e. Galaxy)
3.  R software package/library for the R computing environment

> ![Software Platforms](http://homer.ucsd.edu/homer/basicTutorial/vennSoftware.jpg)\
>\
> Some tools are available on multiple platforms, but a vast majority of tools are available as stand alone Linux/UNIX tools.

### Types of Software Environments

> #### Linux/UNIX \[this tutorial\]
>
> > Linux, BSD, Solaris, Mac OS X, or really any UNIX-based operating system will work well.  Linux systems tend to be the most compatible with academic software, and I find it is easier to install analysis software on Linux than any other operating system.  Mac is an attractive choice for many users given it's a good blend between usability and a native UNIX-style operating system.  Windows PCs are not ideal given much of the analysis software is not configured to run on them. However, [Cygwin](http://www.cygwin.com/) is available as a Linux emulator that runs on Windows computers.  You can also always consider dual booting your windows system, or using some sort of virtualization software and run Linux at the same time.

> #### The Cloud
>
> > Another option is to run your analysis on a web server.  These days we call this "analyzing your data in the cloud".  By far the most popular "cloud" based analysis tool is [Galaxy](http://galaxy.psu.edu/), which is a compendium of programs (and user/data management tools) for bioinformatics analysis.\
> >\
> > This tutorial assumes you will analyze data on your own computer/server.  Personally, I feel researchers get more out of it if they learn how to do things on their own, including installing software.  Learning things this way also makes you self-reliant.  Lets say a new method or new piece of software becomes available to help address a key question with your data.  That software is unlikely to be available on Galaxy, and if it does end up being incorporated into Galaxy, it may not end up there for another year.  So... you're stuck running it yourself.

> #### R/Bioconductor
>
> > Another popular option for next-gen analysis is the statistical computing environment [R](http://www.r-project.org/)/[Bioconductor](http://www.bioconductor.org/)(similar to Matlab). R/Bioconductor has a strong community, and R has many built in statistical routines and graphics functions that make developing algorithms in R more attractive than writing your own code in python or C++.  Many R programs also run well under Windows, making it a decent option for Windows users

>\
> This tutorial will cover the use of a UNIX-style operating system on a local server or individual computer.  I feel understanding how to perform analysis from the command line provides researchers with the greatest flexibility, which is key in the fast moving world of next-gen sequencing.  For tutorials on how to use [Galaxy](http://galaxy.psu.edu/) or[R/Bioconductor](http://www.bioconductor.org/), visit their respective websites (they are both excellent sources of knowledge).

### Computer Hardware

> In theory, any computer with enough RAM, hard drive space, and CPU power can be used for analysis - and this includes many laptops.  In general, you will need:\
>
> > 4-10 Gb of RAM Minimum (better to have 96+ Gb)\
> > 500 Gb of disk space (better to have 10+ Tb of space)\
> > Fast CPU (better to have at least 8 cores, more the better)
>
> For example, if you have a brand new Mac Book Pro laptop with lots of RAM, you can probably do sequencing analysis on it.  You may want to sit the computer on a table, because it will get HOT while trying to map 100 million reads to the genome, and might burn your lap if you're sitting with it at home on the couch.\
>\
> **Starter Computer: Mac Pro**\
>
> > Optimally, you'll have a dedicated computer/lab server do store data on and perform analysis.  This way, you can log on to the server from your personal computer and run analysis remotely.  For labs starting out, a popular option is to get a [Mac Pro](http://www.apple.com/macpro/).  Not only is it a fast, capable computer, it can double as a shared lab computer that can shared software, like Illustrator.
>
> **Rack-mount servers**\
>
> > Ideal if you are a lab that does a lot of sequencing
>
> **Supercomputer Clusters**\
>
> > Often you university or institution provides access to a computing cluster for pennies on the dollar (5-10 cents per CPU hour).  These can be great resources for doing genomic mapping if you don't want to invest in more computing power.  Usually accessed via UNIX terminal...
>
> **Virtual/Cloud Computing**\
>
> > Another option (similar to supercomputer clusters) for accessing computing power

### Basic Setup of OS/Environment

> **Linux/UNIX** - Not much to do - a basic Linux/UNIX install includes almost all of the basic tools you'll need.\
>\
> **Mac OS** - You need to install [Xcode](https://developer.apple.com/xcode/), which includes GNU compilers and many other standard UNIX development tools.  What sucks is that Xcode is now distributed through the App store, which I hate with a passion.  Even though Xcode is free, they'll still basically force you to give them your credit card number... Remember to install the "**Command Line tools**" package while installing Xcode (you can also install this separately from the Xcode application after installation).  I also recommend downloading and installing [wget](http://osxdaily.com/2012/05/22/install-wget-mac-os-x/) since it's such a useful program after getting Xcode.\
>\
> **Windows** - You'll need to install [Cygwin](http://www.cygwin.com/).  During the Cygwin install, you'll want to install a bunch of the development tools:\
>
> > Run the install program, and when prompted to choose which packages to install, make sure to install the following:
>
> > 1.      GCC compiler, gcc-core, gcc-g++ (in Devel)
> > 2.      make (in Devel)
> > 3.      mingw64 (in Devel, for pthread support)
> > 4.      perl (in perl)
> > 5.      zip/unzip (in Archive)
> > 6.      wget (in Web) – very useful for downloading things…
> > 7.      ghostscript (in Graphics)
>
> > 

> > Cygwin configuration screen in the setup program (if you already installed cygwin and need to modify it, just rerun the setup program):\
> >
> > > ![cygwin setup](http://homer.ucsd.edu/homer/basicTutorial/cygwinSetup.png)

### Using the Linux/UNIX terminal

> #### Secure Shell (ssh) - accessing remote computers
>
> > One of the best features of the UNIX terminal is that it can be used to access remote computer systems.  If you have an account on a remote computer and it is running sshd (Secure shell daemon), then you can log onto that computer using ssh client software.\
> >\
> > To access a remote computer, you can either use the ssh command from a local terminal (use this if you have Mac OS, Linux, or use Cyginw), or on windows, you may want to use a FREE ssh client program such as [PuTTY](http://www.putty.org/) or select one from [here](http://en.wikipedia.org/wiki/Comparison_of_SSH_clients).\
> >\
> > To access a remote computer, you must know the hostname or IP address of the computer, and your username/password.  To run ssh from the terminal, run the command:\
> >
> > > **ssh username@hostname**\
> > >\
> > > i.e.** ssh cbenner@biowhatt.ucsd.edu**
> >
> > It will then prompt me for my password, and give me a fresh terminal session on the remote computer.\
> >\
> > If you are accessing a server that is always "on", I would highly recommend managing your sessions using [screen](http://www.howtoforge.com/linux_screen), which is a persistent terminal session manager that will allow your commands to run even if you log off.
>
> #### Basic commands:
>
> > In UNIX, whenever you type something into the terminal, the first word is considered the "command".  Every command works like this:**  **
> >\
> >
> > > **command \[options\]**
> >
> > Here are some common commands:
>
> > **cd \<directory name\>** : change your current directory to the directory specified\
> > **ls** : list the files and directories in your current directory. Add options  "-al" to list all details about the files.\
> > **pwd** : return the full path of your current directory\
> > **more \<filename\>** : display the contents of a file\
> > **gzip \<filename\>** or **gunzip \<filename\>** : GNU zip or unzip a file\
> > **tar xvf \<filename.tar\>** : expand a tar archive\
> > **tar zxvf \<filename.tar.gz\>** : expand a GNU zipped tar archive.  Sometimes these have the file extension \*.tgz\
> > **top** : Useful utility to monitor programs that are currently running.\
> >\
> > Getting info about how to run various commands:\
> > ***\<command*\> --help** : will usually return information about how to run the command\
> > **man \<*command***\> : will display the manual for the command (may not work for every command)\
> >\
> > Special keys/commands: **  **
> >\
> >
> > > **"ctrl+c" **: If a command it taking too long, or seems out of control, hit control+C at the same time to quit execution of the current program\
> > > **\<tab\>/Autocomplete** : Most UNIX terminal programs support auto-complete for file names and program names.  While typing a file name in the terminal, if you hit "tab", and there is only one logical choice, the file name will be automatically filled in.
>
> #### Capturing program output: stdout and stderr
>
> > Many UNIX programs will send their output to a special I/O stream called *stdout*. Normally, *stdout* is displayed on the screen.  However, if you wish to capture this output into a file, you need to add "** \> filename**" to the end of the command.  This will capture the output content and save it into the text file "filename".
>
> #### Understanding the file system
>
> > You can think of the UNIX file system as a [hierarchical tree](http://www.december.com/unix/tutor/filesystem.html).  UNIX file systems use a "/" between directory names when specifying a file (windows uses "\\).  There are two concepts you must get a handle on when thinking about UNIX filesystems:\
> >
> > 1.  Absolute File Names (also called the absolute path): Each file has an absolute file name, which starts with a "/".  For example, "/home/cbenner/beerRecipe.txt".  The absolute file name provides the exact location of the file in the system tree relative to the root (i.e. "/").
> > 2.  Relative File Names: You have a current location (or present working directory).  You can learn what it is using the command "pwd". Any filename that doesn't start with a "/" is a relative file name, and will be describe relative to your current position.\
> >\
> >     For example, lets say your present working directory is "/home/cbenner".  If you specify a file by calling it "data/reads.txt", the system will think you are referring to an absolute file name that is "/home/cbenner/data/reads.txt".
> >
> > To specify the current directory (in a relative sense), use the "**.**".  If you want to specify the parent directory relative to your current position, use "**..**".  This[link](http://www.december.com/unix/tutor/filesystem.html) does an decent job describing how to use file names.\
> >\
> > **Special directories:**\
> >
> > > Home Directory: This is the directory where your personal configuration files are stored, and is also usually the first directory that you will be "in" once you log onto the server.  There is a special symbol for your home directory: "**~**/".  For example, if your home directory is "/home/chucknorris/", then the file "~/data.txt" is equivalent to "/home/chucknorris/data.txt".
> >
> > **File Permissions:**\
> >
> > > Each file has permissions that allow you to read (e.g. access), write (e.g. modify), and execute (e.g. run the file as a program).  These three types of permissions allow you to restrict access to the file.  These 3 permissions also apply to yourself, your group, and everyone else on the system.  For example if you type "**ls -al**", you might see something like this:\
> > >\
> > >
> > > > ``` text
> > > > Chriss-MacBook-Pro:repeats cbenner$ ls -al
> > > > total 1152288
> > > > drwxr-xr-x  40 cbenner  staff      1360 Sep 27 21:47 .
> > > > drwxr-xr-x   7 cbenner  staff       238 Sep 22 17:28 ..
> > > > -rw-r--r--   1 cbenner  staff  84930997 Sep  5 12:44 chimp.L1.txt
> > > > -rw-r--r--   1 cbenner  staff    342187 Sep  5 12:44 chimpL1PA2.txt
> > > > -rwxr-xr-x   1 cbenner  staff       284 Sep  5 21:43 combine.pl
> > > > ```
> > >
> > >\
> > > The first several symbols (i.e. drwxr-xr-x) give the permissions on each file.  Normally, you don't have to worry much about the permissions until you become comfortable with UNIX, but if you do need to make a program executable, you should run the following command:\
> > >
> > > > **chmod 755 program.pl**
>
> > > This will allow you to run the file as a program (i.e. "$ ./program.pl").
>
> #### Editing text files
>
> > In general, there are really only two types of files.  ASCII text files, and binary files.  ASCII text files are formatted such that each "byte" is represents a character.  A byte is composed of a value between 0 and 255, and the meaning of each value is encoded in the [ASCII Table](http://www.asciitable.com/). Text files like this are very powerful because virtually any program can interpret them.  Most specialized files you may know of are ASCII text files, such as XML, HTML, CSV, and of course TXT.  Binary files, on the other hand, can adhere to any format, and are usually meant to be read and written by a single program.  An example would be a word doc file, which has a special encoding that is only meant to be accessed my Microsoft Word.  For sequencing, a classic example of a text files is a [FASTQ file](http://en.wikipedia.org/wiki/FASTQ_format), while a common binary file is a [BAM file](http://genome.sph.umich.edu/wiki/BAM).\
> >\
> > Text files are heavily used in the UNIX world due to their general accessibility.  Not only are most data files encoded as text files (like FASTQ), most UNIX configuration files are also text files.   There are several common utilities included in UNIX to manipulate text files, such as **more**, **cut**, **wc**, **cat**, etc.\
> >\
> >\
> > **Text Editors:**\
> >
> > > Knowing how to use UNIX Text Editors is important when achieving comfort with UNIX.  Below are three common editors, with links to pages the give some background information about how to use them.  Each will have countless tutorials covering their use (just google "\<editor\> tutorial":\
> > >\
> > > [**vi**](http://www.cs.colostate.edu/helpdocs/vi.html) : the most common UNIX text editor, found on any system.  Very powerful, but unfortunately a little difficult to use for beginners.  Worth learning, but takes some effort.\
> > >\
> > > [pico](http://www.uic.edu/depts/accc/software/pine/pico.html) : one of the easiest UNIX text editors to use, good for beginners, not as popular as vi.\
> > >\
> > > [emacs](http://www2.lib.uchicago.edu/keith/tcl-course/emacs-tutorial.html) : one of the most extensible UNIX editors, common for power-users.

> #### Installing Software
>
> > There are two general approaches to installing software on a Linux/UNIX system.  The first is to use a package manager (like yum or apt-get).  The second is to directly download the software and install it from source (or simply download the executable program).\
> >\
> > **Package Managers:**\
> >
> > > Installing software with package managers is great because it will make sure all "prerequisites" are also installed to make sure your software runs as planned.  This is typically only an option for well known programs (most academic software must be installed directly from source).\
> > >\
> > > If running a debian flavor of Linux (i.e. Ubuntu), then you will use something like this:\
> > >
> > > > **apt-get install \<package name\>**
> > >
> > > If running a redhat flavor of Linux (i.e. CentOS), then you will use a different package manager, such as:\
> > >
> > > > **yum install \<package name\>**
> > >
> > > In many cases, you must be the root (or superuser) to install system software, so you may have to add "sudo" to the beginning of these commands.
> >
> > **Installing software from source:**\
> >
> > > Most academic software must be directly downloaded from the author's website.  Software like this can come in two flavors, it can be source files or it can be pre-compiled/binary files (programs ready to run).  If you are downloading programs that are pre-compiled, make sure you get the files specific to your system (i.e. Mac OSX, Linux x86_64, Solaris, etc.)\
> > >\
> > > It might sound daunting to install software from source, but usually it is not bad at all.  In 90% of cases, you'll simply run the next couple commands:\
> > >\
> > >
> > >

<ol>
<li>Download the software to your current directory</li>
<li>type: "<b>tar zxvf program.tar.gz</b>"</li>
<li>This will likely make a new directory, use "<b>cd programDirectory</b>" to move into the new directory.</li>
<li>Read the "README.txt" and "INSTALL.txt" files, if they exist.  These files should include directions about how to install the software.</li>
<li>Usually, this means running the next three commands:<br/><blockquote>
<b>./configure</b><b><br/></b><b>make</b><b><br/></b><b>sudo make install</b></blockquote>
</li>
</ol>

> #### Changing your executable PATH variable
>
> > Every program in UNIX is actually a file.  Most program files for system commands, like "**cd**" or "**ls**" are found in special directories such as "**/usr/bin**" or "**/usr/local/bin**".  Whenever you type something into the terminal, the first word you type is the command.  When you press enter, the operating system attempt to find the program file corresponding to the command.  For example, if you type "**ls**", it needs to find the file with the execution instructions that will list your files.\
> > The location of program files is managed in UNIX systems using the **PATH** variable.  This is a variable maintained by the shell (e.g. terminal program) that stores the directories that contain program files.  If you run the command "echo $PATH", the contents of the PATH variable will be shown:\
> >
> > > **echo $PATH**\
> > > /usr/lib64/qt-3.3/bin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/bioinf/seqlogo/weblogo:/bioinf/ucsc:/bioinf/homer/bin/:/bioinf/samtools/samtools-0.1.18/
> >
> > What is shown is a list of directories separated by colons ":".  When you type a command, the operating system will go to each of these directories (in order) and look for an executable program file matching the name of the one you typed.  If you want to figure out where the command is, you can use the "**which \<command\>**" command to return the location of the command you ran.  For example:\
> >
> > > **which ls**\
> > > /bin/ls
> >
> > This shows that the **ls** command is actually a program file located in the **/bin/** directory.\
> > Why is this important?  Often, you may download a program and put it in a directory - lets say I download a program called **"roundhouse"** and place it in the directory "**/home/chucknorris/software/**".  I then make sure it is executable by changing it's permissions: "**chmod 755 roundhouse**".  To run the program from anywhere, I must specify the program file location explicitly: "**/home/chucknorris/software/roundhouse**".  However, if I want to run it like most commands (i.e. just type "**roundhouse**"), I need to place it in a directory that the operating system will search for (e.g. a directory that is included with the **PATH** variable).\
> > For that, you have two options.  First, you *could *copy the **roundhouse** program to a directory that is in the **PATH**, such as copying it to the **/bin**/ directory.  However, this is usually not a good idea since the bin directory is normally reserved for system applications.  Another approach is to modify your **PATH**variable to include the **/home/chucknorris/software/** directory so that the operating system will search it when looking for program files.\
> > The best way to do this (assuming you are using the bash shell, which is the most common), is to edit the "**~/.bashrc**" resource file.  Using **vi** or **pico**, edit the file and include a line such as this one:\
> >
> > > PATH=$PATH:/hoem/chucknorris/software
> >
> > The "**~/.bashrc**" file is a special resource file that is executed whenever you login.  This way, whenever you log into your computer, your PATH variable will be updated to include the special directory.\
> >
> > ## Retrieving and storing sequencing files
> >
> > This section will help you figure out where find sequencing data.  Likely places you will find it:\
> >
> > - On your lab server/computer (someone already generated it, or a bioinformatician placed it in a folder for you)
> > - A **scp**, **ftp**, **smb**, or web server run by your sequencing core or collaborator where your files have been posted
> > - Public data from GEO/SRA, a lab or company website
> >
> >\
> > Obviously, if you are generating your own sequencing data, talk with your collaborators - they will help you figure out where to find the data.\
> >\
> >
> > ### Accessing private data from a sequencing core
> >
> > > Often a sequencing core will give you a URL or ftp address where you can download your data.  Usually this will be password protected, although not always ([security though obscurity](http://en.wikipedia.org/wiki/Security_through_obscurity)).  Access data this way is usually pretty easy.\
> > >\
> > > The most important files to download are the FASTQ files.  These constitute the raw data of the sequencing experiment.  More info on [FASTQ files](http://homer.ucsd.edu/homer/basicTutorial/fastqFiles.html) is covered in the next section.  Most sequencing cores will already demultiplex your samples so that each FASTQ files represents and individual experiment.\
> > >\
> > > It is also worth downloading any instrument files or other quality control statistics.  These can be useful if inturment technicians are troubleshooting problematic results.\
> > >\
> > > **IMPORTANT**: It is important to perform an initial analysis of your data **IMMEDIATELY**.  You may learn quickly that the barcodes used to demultiplex your data were not correct and you lost one of your samples.  If you wait too long, the sequencing core may remove your files, so it's best to catch this early so that you can redo the demultiplexing or adjust parameters on the post-sequencing analysis steps that generate the FASTQ files.\
> > >\
> > > After downloading your data, make sure you BACKUP the data on a separate computer in a separate physical location.  This may include making a copy on a removable hard drive.  The FASTQ files are the raw data, and are needed to publish the experiment.  If you loose them you may have trouble publishing your study even if you have downstream analysis files.\
> > >\
> > > Annotation is also important - make sure you label/rename your FASTQ files or place them in directories that makes sense and help you organize your data.
> >
> >\
> >
> > ### Getting Public Data from GEO/SRA
> >
> > > One very important byproduct of the microarray era was that it is expected that you publish the raw data from high-throughput experiments.  This is great because it forces authors to deposit their sequencing data in public repositories.  If you are interested in their study, you can download their data and reanalyze their data for your own purposes.\
> > >\
> > > Most data is deposited in NCBI [Gene Expression Omnibus](http://www.ncbi.nlm.nih.gov/geo/) (GEO) and/or the NCBI [Short Read Archive](http://www.ncbi.nlm.nih.gov/sra) (SRA).  Some data is deposited in the European equivalent [ArrayExpress](http://www.ebi.ac.uk/arrayexpress/) and the [European Nucleotide Archive](http://www.ebi.ac.uk/ena/home).  Japan has their own archive run through the [DDBJ](http://www.ddbj.nig.ac.jp/).\
> > >\
> > > If you are reading a paper that has high-throughput data, the GEO or SRA should be located near the references.  A good strategy is to search for the document for "GSE", which is the first 3 letters of all GEO accession numbers.  Sometimes it's found in the methods.  Occasionally it's hidden in the supplemental data.  If you can't find one, try searching with "SRA" in case it's a SRA accession number.\
> > >\
> > > If you absolutely can't find it, EMAIL the corresponding author, especially if it's a major journal.  If they are trying to get away without sharing the data and it's something you're interested in, they need to share it.  Ask the author politely for the data in an email.  If they are uncooperative, add the journal editor to the email - you'll be surprised how fast that data will appear in GEO if you do that.\
> > >
> > > #### Getting the FASTQ files for a sequencing study
> > >
> > > > GEO is the most common entry point for sequencing data.  Consider the record for [GSE23619](http://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE23619). The top sections describe the overall experiment, followed by links to individual experiments.  Below that are links to the SRA and supplemental files:\
> > > >
> > > > > ![GEO example](http://homer.ucsd.edu/homer/basicTutorial/geoexample.png)
> > > >
> > > > To get the RAW sequencing data, you can click on the SRA link, or go to the "Supplementary File" section and click on the (ftp) link to download the data.  The easiest way to get the data is to follow the ftp link.  This link will lead you to SRA files, such as[SRR065223.sra](ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP%2FSRP003%2FSRP003475/SRR065223/).  These are FASTQ files that are specially compressed by the Short Read Archive and need to be opened using a special tool available in the SRA toolkit.\
> > > >\
> > > > To download and install the SRA toolkit, follow this [link](http://ftp-private.ncbi.nlm.nih.gov/sra/sdk/2.1.16/) and download the appropriate program files.  The main program of interest is in the toolkit is called fastq-dump, which is a program that will extract FASTQ files from the SRA files.  For example:\
> > > >
> > > > > **fastq-dump SRR065223.sra**\
> > > > > will produce a file named: SRR065223.fastq
> > > >
> > > > If it is a large sequencing study, and you have the tool wget installed, you can download ALL of the SRA files for a study by right clicking on the (ftp) link, and then pasting the URL into the wget command like so:\
> > > >
> > > > > **wget -r ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByStudy/sra/SRP%2FSRP003%2FSRP003475/**
> > > >
> > > > Be sure to add the "**-r**" which will recursively download the files, and you may also need to add a "**/**" to the end of the URL to make sure wget knows that you gave it a directory.\
> > > >\
> > > > After converting files to FASTQ, you should be ready to perform your own analysis on them.
> > >
> > > #### Supplementary Files provided by the author
> > >
> > > > Sometimes the author provides other useful files, such as sam/bam files (premapped files), peak positions, bedGraph files for visualization, rpkm gene expression counts, etc.  Be sure to pay attention to which genome version was used to generate those files.

## Checking and manipulating FASTQ files 

Most modern sequencers produce [FASTQ files](http://en.wikipedia.org/wiki/FASTQ_format) as output, which is a modified version of a traditional [FASTA formatted file](http://en.wikipedia.org/wiki/FASTA_format). FASTQ flles are ASCII text files that encode both nucleotide calls as well as 'quality information', which provides information about the confidence of each nucleotide.  FASTQ format uses 4 lines for each read produced by the sequencer.  Fastq files are nomally given the file extension "**.fq**" or "**.fastq**".  A typical files looks something like this:

> ``` text
> @SRR566546.970 HWUSI-EAS1673_11067_FC7070M:4:1:2299:1109 length=50
> TTGCCTGCCTATCATTTTAGTGCCTGTGAGGTGGAGATGTGAGGATCAGT
> +SRR566546.970 HWUSI-EAS1673_11067_FC7070M:4:1:2299:1109 length=50
> hhhhhhhhhhghhghhhhhfhhhhhfffffe`ee[`X]b[d[ed`[Y[^Y
> @SRR566546.971 HWUSI-EAS1673_11067_FC7070M:4:1:2374:1108 length=50
> GATTTGTATGAAAGTATACAACTAAAACTGCAGGTGGATCAGAGTAAGTC
> +SRR566546.971 HWUSI-EAS1673_11067_FC7070M:4:1:2374:1108 length=50
> hhhhgfhhcghghggfcffdhfehhhhcehdchhdhahehffffde`bVd
> @SRR566546.972 HWUSI-EAS1673_11067_FC7070M:4:1:2438:1109 length=50
> TGCATGATCTTCAGTGCCAGGACCTTATCAAGCGGTTTGGTCCCTTTGTT
> +SRR566546.972 HWUSI-EAS1673_11067_FC7070M:4:1:2438:1109 length=50
> dhhhgchhhghhhfhhhhhdhhhhehhghfhhhchfddffcffafhfghe
> ```
>
>\
> ...

The example above encodes 3 reads (each uses 4 lines to report information).  Each read has:

> 1\. Header line - This line must start with "@", followed by the name of the read (should be unique)\
> 2. The nucleotide sequence\
> 3. A second header line - This line must start with "+".  Usually, the information is the same as in the first header line, but it can also be blank (The "+" is still required though)\
> 4. Quality Information - For each nucleotide in the sequence, an ASCII encoded quality score is reported.  The idea is that better quality scores indicate the base is reliably reported, while poor quality scores reflect uncertaintly about the true identity of the base.

These files represent the primary data generated by the sequencer, and will be requested by other researchers after you publish your study.  Do not loose or modify these files!!!  If you are using Life Tech/ABI Solid sequencers, the data may be returned as a color space FASTQ file (usually with a \***.csfastq**" extension)

### Duplicate and archive

> The most important thing you can do once you get primary data off the sequencer is back to it up.  If you loose a mapping file (sam/bam), it's not a big deal as you can always remap your data, but if you lose the raw sequencing file (i.e. FASTQ), you're in trouble, and may have to repeat the experiment.  Therefore, it is critical that you backup your files to a secure server that is \[ideally\] in a separate physical location than your primary copy.  Storing a single copy on a RAID device does NOT count...\
>\
> The other under appreciated task is to annotate your files.  This means giving them proper names, and including information about how the experiment was done, cell type, etc. so that when you find the file later, you know what the experiment was.  I would strongly recommend including a date in the file name, the cell type, the treatment, the type of experiment, the lab/scientist who performed the experiment.  For example, consider naming them like this:\
>
> > Lab-YYMMDD-Cell-Txn-expID.fastq\
> >\
> > ChuckNorrisLab-120927-Bcell-LPS-0123.fastq
>
> Also, it's probably a good idea to zip the files such that they take up less space.  For example:\
>
> > **gzip \*.fq \*.fastq**

### Removing barcodes

> Depending on your sequencing strategy, you may need to remove certain parts of the sequence that is not biologically meaningful.  For example, if you sequence short RNAs that are between 15-40 bp in size, and you sequence them using 50 nucleotide reads, the sequence will start to identify the adapter used in sequencing library construction at the end of the RNA sequence.  

### Quality value encoding

> Different version of the Illumina pipeline (from back in the day) can produce different encoding of quality.  A discussion of these differences can be found [here](http://maq.sourceforge.net/qual.shtml).  In general, recent implementations of the Illumina pipeline output Sanger-style quality encoding, so you should have to worry much about it.  Many programs, such as bowtie for read mapping, have options to specify which style of encoding is used.

### Performing quality controls on FASTQ files

> It's a good idea to perform a general quality control check on your sequence files - this can help indicate if there were any major technical issues with your sequencing.  I nice tool for this is [FASTQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/), developed by Simon Andrews at the Babraham Institute.  Go [here](http://www.bioinformatics.babraham.ac.uk/projects/download.html#fastqc) to download and install the appropriate version of FASTQC.  After unzipping it, add the main FASTQC directory to your executable path for ease of use.\
>\
> Usually, the easiest way to run FASTQC is on the command line:\
>
> > **mkdir OutputDirectory/\
> > fastqc -o OutputDirectory/ inputFile.fastq**
>
> Unfortunately, it will complain if you do not create the output directory ahead of time.  This analysis will produce several interesting analyses that help you understand how your sequencing went:

### Manipulating FASTQ files

> Since most of the applications covered here involve "re-sequencing" of a known genome, the quality information about each base is not terribly important (it's more important when trying to identify SNP or for de novo genome assembly).  However, sometimes, as a read is sequenced, errors start to appear and the reliability of the sequence goes down.  In these cases it's best to remove these sequences from the mapping to improve downstream analysis.\
>\
> An excellent resource for the manipulation of FASTQ files is the [FASTX program suite](http://hannonlab.cshl.edu/fastx_toolkit/).  These programs can be very useful.  In addition, some mapping tools (i.e. bowtie) have options that perform on-the-fly FASTQ manipulation, such as trimming from the 3' end.\
>\
>
> #### Trimming adapter sequences from your fastq files
>
> > If you perform short RNA sequencing or another type of experiment where the functional sequences you are measuring might be smaller than the read length, it is likely that the 3' end of the read will be adapter sequences from the Illumina library preparation, and not relevant biological/genomic sequence.  You *MUST* **MUST** MUST remove this sequence before trying to map or assemble the reads.  The FASTX program **fastx_clipper** can perform adapter clipping, as can the HOMER utility [homerTools](http://homer.ucsd.edu/homer/ngs/homerTools.html).
>
> #### Trimming sequences based on quality scores
>
> > If your reads are very long, you may want to trim sequences where the quality scores took a dive.  This may be necessary for 100 bp reads if the last 20 bp are all random base calls.  In this case the read may be hard to map since the final 20 bp will be largely wrong.  The FASTX tool **fastq_quality_trimmer **is useful for this purpose.

## Mapping reads to the genome

Once you have checked your FASTQ files and have removed all adapter sequences that might be present, you are ready to map them to a reference genome.  While tools like BLAST and BLAT are powerful methods, they are not specialized for the vast amount of data generated by next-generation sequencers.  It is highly recommended that you use a next-gen specific read alignment program.

### Selecting a reference genome

Both the organism and the exact version (i.e. hg18, hg19) are very important when mapping sequencing reads.  Reads mapped to one version are NOT interchangeable with reads mapped to a different version. I would follow this recommendation list when choosing a genome (Obviously try to match species or sub species when selecting a genome):

1.  1.  Do you have a favorite genome in the lab that already has a bunch of experiments mapped to it?  Use that one.
    2.  Do any of your collaborators have a favorite genome/version?
    3.  Use the latest stable release - I would recommend using genomes curated at UCSC so that you can easily visualize your data later using the[UCSC Genome Browser](http://genome.ucsc.edu/).  (i.e. mm9, hg19)

> 

### Mapping to a normal genomic reference

> You want to map your reads directly to the genome if you are performing:\
>
> - ChIP-Seq
> - GRO-Seq
> - DNase-Seq
> - MNase-Seq
> - Hi-C

> #### Popular short read aligners
>
> > [Full List](http://en.wikipedia.org/wiki/List_of_sequence_alignment_software#Short-Read_Sequence_Alignment)\
> > Most Popular:\
> > [bowtie](http://bowtie-bio.sourceforge.net/index.shtml) : fast, works well\
> > [bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml) : fast, can perform local alignments too\
> > [BWA](http://bio-bwa.sourceforge.net/) - Fast, allows indels
>
> ### Example of alignment with bowtie2:
>
> > #### Step 1 - Build Index (takes a while, but only do this once):
> >
> > After installing [bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml), the reference genome must first be "indexed" so that reads may be quickly aligned.  You can download pre-made indices from the bowtie website (check for those [here](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml) first).  Please be aware that bowtie2 indexes are different than "bowtie" indexes.  To perform make your own from FASTA files, do the following:\
> >
> >

<ol>
<li>Download FASTA files for the unmasked genome of interest if you haven't already (i.e. from <a href="http://hgdownload.cse.ucsc.edu/downloads.html">UCSC</a>).  Do NOT use masked sequences.  I also tend to remove the "*_random.fa" chromosomes.  These often contain part of the canonical chromosomes in addition to regions that cannot be placed in the assembly.  The problem with these regions is that the part shared with the canonical chromosome will be present twice, making it difficult to map the reads to a unique location.</li>
<li>Concatenate FASTA files into a single file.  We can do this using the UNIX <b>cat</b> command, which merges files together<br/><blockquote>
<b>cat *.fa &gt; genome.fa</b></blockquote>
</li>
<li>From the directory containing the genome.fa file, run the "<b>bowtie2-build</b>" command.  The default options usually work well for most genomes.  For example, for hg19:<br/><blockquote>
/path-to-bowtie-programs/bowtie2-build genome.fa hg19</blockquote>
This command will create 6 files with a <b>*.bt2</b> file extension.  These will then be used by bowtie2 or newer versions of tophat to map data.</li>
<ol></ol>
<li>Copy the <b>*.bt2</b> files to the bowtie2 indexes directory (or somewhere you can store them) so that bowtie2 knows where to find them later:<br/><blockquote>
<b>cp *.bt2 /path-to-bowtie-programs/indexes/</b></blockquote>
</li>
</ol>

> >
> > #### Step 2 - Align sequences with bowtie (perform for each experiment):
> >
> > The most common output format for high-throughput sequencing is [FASTQ format](http://homer.ucsd.edu/homer/basicTutorial/fastqFiles.html), which contains information about the sequence (A,C,G,Ts) and quality information which describes how certain the sequencer is of the base calls that were made.  In the case of Illumina sequencing, the output is usually a "s_1_sequence.txt" file.  More recently the Illumina pipeline will output a file that is debarcoded with your sample name such as "Experiment1.fastq".  In addition, [much of the data available in the SRA](http://homer.ucsd.edu/homer/basicTutorial/retrieveFiles.html), the primary archive of high-throughput sequencing data, is in this format.\
> >\
> > To use bowtie2 to map this data, run the following command:\
> >\
> >
> > /path-to-bowtie-programs/bowtie2 -p \<# cpu\> -x \<genome index prefix\> \<fastq file\>  \> \<output filename\>\
> >\
> > i.e.\
> > /programs/bowtie2 -p 8 -x hg19 Experiment1.fastq \> Experiment1.sam
>
> Where \<genome index prefix\> is the common prefix for the **\*.bt2** files that were created using the **bowtie2-build** command in step 1, or from a downloaded index.  If the \*.bt2 files are stored int the "/path-to-bowtie2-program/indexes/" directory, you only need to specify the name of the index.  If the index files are located elsewhere, you can specify the full path names of the index files (in the examples above this would be "-x /programs/indexes/hg19").\
>\
> In the example above, we use 8 processors/threads.  The bowtie2 program is very parallel in nature, with near linear speed up with additional processors.\
>\
> The default output is a SAM file.  To learn more about SAM alignment files, go to the [next section](http://homer.ucsd.edu/homer/basicTutorial/samfiles.html) on SAM/BAM files.\
>\
> There are many other options for bowtie2 that may be important for your study, but most ChIP-Seq data can be mapped using the default options.\
>\
> NOTE: Usually, the process of removing duplicate reads or removing non-unique alignments is handled by the downstream analysis program.

### Mapping to a genome while allowing splicing

> Usually, any kind of RNA-seq method will benefit from looking for splicing junctions in addition to genomic mapping:\
>
> - RNA-Seq (polyA+, total)
> - CLIP-Seq/PAR-CLIP
> - RIP-Seq
> - ChIRP-Seq
> - Ribo-Seq
>
> #### Popular splice read aligners
>
> > Most Popular:\
> > [Tophat](http://tophat.cbcb.umd.edu/)\
> >\
> > Many others...
>
> ### Example of Alignment with Tophat
>
> > Tophat is basically a specialized wrapper for bowtie2 - it manipulates your reads and aligns them with bowtie2 in order to identify novel splice junctions.  It can also use given splice junctions/gene models to map your data across known splice junctions.\
> >
> > #### Step 1 - Build Index (takes a while, but only do this once):
> >
> > > This part is [exactly the same as for bowtie2](http://homer.ucsd.edu/homer/basicTutorial/mapping.html#bowtie2-build) - if you already made or downloaded an index for bowtie2, you can skip this step.
> >
> > #### Step 2 - Align your RNA-seq data to the genome using Tophat
> >
> > > To use tophat to map this data, run the following command:\
> > >
> > > > **/path-to-bowtie-programs/tophat -o \<output directory\> -p \<# cpu\> \</path-to-genome-index/prefix\> \<fastq file\>  **\
> > > >\
> > > > For example:\
> > > > **/programs/tophat -o TophatOutput/ -p 8 /programs/indexes/hg19 Experiment1.fastq**\
> > > >\
> > > > Paired-end Example:\
> > > > **/programs/tophat -o TophatOutputPE/ -p 8 /programs/indexes/hg19 Experiment1.r1.fastq Experiment1.r2.fastq**
> > >
> > >\
> > > In the example above, we use 8 processors/threads.  The tophat2 program contains a mix of serial and parallel routines, so more processors doesn't necessarily mean it will be finished much faster.  In particular, if you are performing coverage based searchers, it may take a long time (multiple processors will not help).  In general, if you have multiple samples to map, it's better to map them at the same time with separate commands rather than mapping them one at a time with mapping processors.\
> > >\
> > > Tophat places several output files in an output directory.  The most important is the "**accepted_hits.bam**" file - this is the alignment file that will be used for future analysis (more info [here](http://homer.ucsd.edu/homer/basicTutorial/samfiles.html)).  There are additional files that can be useful, including a "junctions.bed" file with records all of the splice junctions discovered in the data.
>
> > #### Important Tophat Parameters:
> >
> > > **--library-type \<fr-unstranded \| fr-firststrand \| fr-secondstrand\>**\
> > >
> > > > Describes which method was used to generate your RNA-seq library. If you used a method that doesn't produce strand specific signal (i.e. just sequencing a cDNA library), then select the fr-unstranded. If you use a stranded method that sequences the first DNA strand made (like a dUTP method), then use fr-firststrnad. If you use direct ligation methods, then fr-secondstrand.  Correctly specifying the library type will help Tophat identify splice junctions.
> > >
> > > **-G \<GTF file\>**\
> > >
> > > > Use this option to specify a known transcriptome to map the reads against.  By default, tophat will also search for de novo splice events, but this will help it known were the known ones are so that you don't miss them.  A GTF files are called Gene Transfer Files, and a description of the format can be found [here](http://uswest.ensembl.org/info/website/upload/gff.html?redirect=mirror;source=www.ensembl.org).  To get a GTF file for your organism, you can usually get one from UCSC Table Browser:\
> > > > ![UCSC Table browser GTF](http://homer.ucsd.edu/homer/basicTutorial/ucsctablesGTF.png)\
> > > >\
> > > > In the output format, be sure to select GTF file - the file you download from here should work with tophat.
>
> > #### Tophat Mapping Strategy
> >
> > > If your goal is to identify novel transcripts and you have several separate experiments, I would recommend pooling all of your data together into a single expeirment/FASTQ file and mapping your data in one run.  One of the ways Tophat tries to identify novel junctions is by first identifying "exons" by mapping segments of reads to the genome using bowtie2.  The more segments it's able to map, the more confident it is about putative exons and the greater the chance it will identify unannotated splice sites.\
> > > You can then go back with your novel splice sites and remap the original experiments (not pooled) to get reads for each individual experiment using the "**-j \<raw junction file\>**".  This is most useful for short reads.  This general strategy is also useful if running cufflinks... 

### Mapping bisulfite-treated DNA

> MethylC-Seq, BS-Seq, or RBBS-Seq data requires a special mapping strategy.  In these experiments the genomic DNA is bisulfite treated, causing all unmethylated cytosines to be converted to uracil, which will utimately show up as thymine after sequencing.  This is a clever way to figure out which cytosines are methylated in the genome, but requires a clever mapping strategy to avoid bias detection.  I'll try to include an example of mapping this type of data in the near future, for now consider this [list of BS-aligners](http://www.mi.fu-berlin.de/w/ABI/ExistingBisulfiteMappers).

### *De novo* Assembly

> Sometimes it makes more sense to perform de novo assembly instead of mapping reads to a reference genome.  Assembly is well beyond the scope of this tutorial.\
>\
> Genomics assembly from short reads: [Velvet](http://www.ebi.ac.uk/~zerbino/velvet/), [SOAPdenovo](http://soap.genomics.org.cn/soapdenovo.html)

Transcript assembly: [Trinity](http://trinityrnaseq.sourceforge.net/)

## Understanding and manipulating SAM/BAM alignment files

Once you have mapped your FASTQ file to the reference genome you will normally end up with a SAM or BAM alignment file.  SAM stands for Sequence Alignment/Map format, and BAM is the binary version of a SAM file.  These formats have become industry standards for reporting alignment/mapping information, so it's good to understand them to some degree.  We will also discuss [samtools](http://samtools.sourceforge.net/), which is a free software package for manipulating SAM/BAM files.  Understanding how to use samtools is important since BAM files are often the input files needed for many different analysis programs.  Sometimes they need to be sorted, or have an index, and samtools is an easy way to create one.  [BEDtools](http://code.google.com/p/bedtools/) will also be covered at the end to demonstrate some nifty things you can do once you have a BAM file.

### SAM file format

> For the most part, you don't really need to understand the SAM format since most analysis software will handle it for you.  A full description of the SAM format can be found [here\[PDF\]](http://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&ved=0CCIQFjAA&url=http%3A%2F%2Fsamtools.sourceforge.net%2FSAM1.pdf&ei=ZIRzULzKE8iZiQKjzYGwBA&usg=AFQjCNFkMtvEi6LeiKgpxQGtHTlqWKw2yw&sig2=ntz7TuLiZCC8JNNRLLMJzA).  SAM files are tab-delimited text files, and can be opened using a text editor or viewed using the UNIX "**more**" command.  Most files will start with a header section, which has lines that start with "@" and looks like this:\
>
> > @HD     VN:1.0  SO:coordinate\
> > @SQ     SN:chr1 LN:249250621\
> > @SQ     SN:chr10        LN:135534747\
> > @SQ     SN:chr11        LN:135006516\
> > ...
>
> Most alignment programs will supply a header that looks like this, which contains information about each of the chromosomes that were mapped to.  After the header section, the aligned reads will appear, with one line per alignment (below I added a space between lines):\
>
> > HWI-ST1001:137:C12FPACXX:7:1115:14131:66670     0       chr1    12805   1       42M4I5M \*       0       0       TTGGATGCCCCTCCACACCCTCTTGATCTTCCCTGTGATGTCACCAATATG     CCCFFFFFHHGHHJJJJJHJJJJJJJJJJJJJJJJIJJJJJJJJJJJJIJJ     AS:i:-28        XN:i:0  XM:i:2  XO:i:1XG:i:4   NM:i:6  MD:Z:2C41C2     YT:Z:UU NH:i:3  CC:Z:chr15      CP:i:102518319  XS:A:+  HI:i:0\
> >\
> > HWI-ST1001:137:C12FPACXX:7:2313:17391:30032     272     chr1    13494   1       51M     \*       0       0       ACTGCCTGGCGCTGTGCCCTTCCTTTGCTCTGCCCGCTGGAGACAGTGTTT     CFFFFHHJJJJIJJJJIJJJJJJJJJJJJJJJJJJJJJHHHHFA+FFFC@B     AS:i:-3 XN:i:0  XM:i:1  XO:i:0  XG:i:0NM:i:1   MD:Z:44G6       YT:Z:UU XS:A:+  NH:i:3  CC:Z:chr15      CP:i:102517626  HI:i:0\
> >\
> > HWI-ST1001:137:C12FPACXX:7:1109:17518:53305     16      chr1    13528   1       51M     \*       0       0       CGCTGGAGCCGGTGTTTGTCATGGGCCTGGGCTGCAGGGATCCTGCTACAA     \#############AB=?:\*B?;A?\<2+233++;A+A2+\<7==@7,A\<A\<=\>     AS:i:-5 XN:i:0  XM:i:2  XO:i:0  XG:i:0NM:i:2   MD:Z:8A21T20    YT:Z:UU XS:A:+  NH:i:4  CC:Z:chr15      CP:i:102517592  HI:i:0

> The SAM alignment has several columns:\
>
> 1.  Read Name
> 2.  SAM flag
> 3.  chromosome (if read is has no alignment, there will be a "\*" here)
> 4.  position (1-based index, "left end of read")
> 5.  MAPQ (mapping quality - describes the uniqueness of the alignment, 0=non-unique, \>10 probably unique)
> 6.  CIGAR string (describes the position of insertions/deletions/matches in the alignment, encodes splice junctions, for example)
> 7.  Name of mate (mate pair information for paired-end sequencing, often "=")
> 8.  Position of mate (mate pair information)
> 9.  Template length (always zero for me)
> 10. Read Sequence
> 11. Read Quality
> 12. Program specific Flags (i.e. HI:i:0, MD:Z:66G6, etc.)
>
> Obviously, the chromsome and position are important.  The CIGAR string is also important to know where insertions (i.e. introns) might exist in your read.  The other two important values in a SAM file are the MAPQ value (column 5) and the SAM flag (column 2).  The MAPQ value can be used to figure out how unique an alignment is in the genome (large number, \>10 indicate it's likely the alignment is unique).\
>\
> The SAM flag is a little more difficult to decipher - the value of the flag is formulated as a bitwise flag, with each binary bit corresponding to a certain parameter.  See the format specification for more info \[[pdf](http://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&ved=0CCIQFjAA&url=http%3A%2F%2Fsamtools.sourceforge.net%2FSAM1.pdf&ei=ZIRzULzKE8iZiQKjzYGwBA&usg=AFQjCNFkMtvEi6LeiKgpxQGtHTlqWKw2yw&sig2=ntz7TuLiZCC8JNNRLLMJzA)\].  For example, if the 0x10 bit is set, then the read is aligned as the reverse compliment (i.e. maps to the - strand).\
>\
> NOTE: Usually, the process of removing duplicate reads or removing non-unique alignments is handled by the downstream analysis program.

### Manipulating Alignment Files with samtools

> Samtools is a very useful program for manipulating SAM/BAM files, and is worth learning how to use.\
>
> #### Install samtools
>
> > To install samtools, download the software from the samtools [website](http://sourceforge.net/projects/samtools/files/).  Unzip/untar the file and make the executable using the make command:\
> >
> > > **tar zxvf samtools-0.1.18.tar.bz2\
> > > cd samtools-0.1.18/\
> > > make**
> >
> > This should produce the executable "**samtools**".  You may want to add the samtools directory to your executable path so that all you have to do is type "samtools" instead of the full path.  You can also just copy samtools to a different directory that is already in the executable path.

> #### Converting BAM to SAM and vice versa
>
> > A common thing you may want to do is view the contents of a BAM file.  Since BAM files are a binary format, it is hard to read them with a text editor.  To convert a bam file to a sam file:\
> >
> > > **samtools view -h file.bam \> file.sam**
> >
> > To convert back to a bam file:\
> >
> > > **samtools view -b -S file.sam \> file.bam**

> #### Sorting a BAM file
>
> > Many of the downstream analysis programs that use BAM files actually require a sorted BAM file.  This allows access to reads to be done more efficiently.  To sort a BAM file:\
> >
> > > **samtools sort -m 1000000000 file.bam outputPrefix**
> >
> > The number of -m specifies the maximum memory to use, and can be changed to fit your system.  The output will be placed in file named "outputPrefix.bam".
>
> #### Creating a BAM index
>
> > Some tools require a BAM Index file to more efficiently access reads in a BAM file.  An example of this is viewing alignments using the[UCSC Genome Browser](http://genome.ucsc.edu/goldenPath/help/bam.html).  To create a BAM index, you must first sort the BAM file.  After that, run the following command to make a BAM index:\
> >
> > > **samtools index sorted.bam**
> >
> > This will create a file named sorted.bam.bai which contains the index.

### Using BEDtools to perform basic analysis tasks

> [BEDtools](http://code.google.com/p/bedtools/) is a powerful suite of tools that enables you to perform several different types of analysis.  In many ways, it's like HOMER in that it is flexible and allows you to analyze your data in many different ways.\
>\
>
> #### Installing BEDtools
>
> > To install, [download the program](http://code.google.com/p/bedtools/downloads/list) (something like "BEDTools.v2.16.2.tar.gz") and unzip and un-tar the file.  After that, move into the new directory and type "make" to compile the program:\
> >
> > > **tar zxvf BEDTools.v2.16.2.tar.gz\
> > > cd BEDTools-Version-2.16.2\
> > > make**
> >
> > You might want to edit your executable PATH variable and add the BEDtools directory to it

> #### Creating a Genome Coverage BedGraph
>
> > Experiments such as ChIP-Seq and RNA-Seq are means to measure the density of reads in a given location.  To build a density graph of your reads across the genome, use the BEDtools program "**genomeCoverageBed**" to create a bedGraph file.\
> >
> > > **genomeCoverageBed -ibam SRR065240.notx.bam -bg -trackline -trackopts 'name="notx" color=250,0,0' \> notx.bedGraph**\
>
> > The "-trackline" and "-trackopts" options are used to specify parameters for the bedGraph file so that when you upload it to UCSC or the like it looks good.  It's also a good idea to gzip file file after creating it to reduce upload times to UCSC.
>
> ## Visualizing data in a Genome Browser
>
> The ability to visualize your raw sequencing aligned to the genome is enabled through the use of genome browsers.  Visualizing your data is incredibly important - it enables you to investigate your data and get a feel for how it "looks".  This is incredibly important for quality control - programs such as FASTQC, HOMER, etc. can only do so much.  Years of experience and knowledge of biological systems make the human brain a good tool to investigate data quality.  Also, visualization of sequencing data may help you come up with new ideas about how to analyze the data.\
>\
>
> ### Popular Genome Browsers:
>
> > [UCSC Genome Browser](http://genome.ucsc.edu/)\
> >
> > > This is an awesome genome browser that puts lots of different information at your finger tips, including lots of published studies and ENCODE data.  Big pluses: data integration.  Negatives: slower (web based), a little more difficult to upload large custom data sets.
>
> > [IGV (Integrative Genome Viewer)](http://www.broadinstitute.org/igv/)\
> >
> > > This browser runs locally on your own computer (the more memory you have the better).  It is Java based, and is easy to use on almost any computer.  It doesn't have the same degree of shared information available as UCSC, but it is much faster for browsing across the genome.  Also, it is better for looking at individual reads/looking for variants.
> >
> > Others\
> >
> > > There are tons of genome browsers out there that serve many different needs.  For a list, check out this [link](http://en.wikipedia.org/wiki/Genome_browser).
>
>\
>
> ### Types of custom data files
>
> > A general list of common file formats can be found [here](http://genome.ucsc.edu/FAQ/FAQformat.html).  Popular formats are shown below:\
> >
> > - **bed** - (\*.bed) - BED files are very basic as they simply describe a simple region in the genome.  They are usually used to describe ChIP-Seq peaks and things of that nature.  Nearly every genome browser supports visualization of BED files.
> > - **wiggle** - (\*.wig) - Wiggle files are used to display quantitative information across genomic regions.  Usually used to display read depth from ChIP- or RNA-seq experiments.  Wiggle format is compact and displays data at regular intervals.
> > - **bedGraph** - (\*.bedGraph) - Similar to Wiggle files, these are used to display quantitative data across genomic regions.  They use variable length intervals instead of constant intervals found in wiggle files, and are usually a little bigger in size.
> > - **bam** - (\*.bam) - Display individual reads.  Bam files need to be sorted, and they [need to have an index file](http://homer.ucsd.edu/homer/basicTutorial/samfiles.html) along with the bam file to help the genome browser efficiently find reads in the bam file.  It's best to use a local browser like IGV when visualizing bam files.
> > - **GFF/GTF** - (\*.gff \*.gtf) - Extensible file formats for specifying spliced transcripts and genes.  Transcript assembly programs like cufflinks will generate GTF files that you can then upload to a genome browser.
> >
> > #### Server Resident Files:
> >
> > > In the case of web-based genome browsers such as UCSC, it can be difficult to upload large data files.  To get around this issue, UCSC set up protocols to allow you to post your files on a webserver and then create a track that "points" to the location of your files.  This requires a working webserver, but can be a powerful way to visualize bigwig files. ([more info](http://genome.ucsc.edu/goldenPath/help/bigWig.html))
>
> ### Creating genome browser files
>
> > There are a bunch of specialized programs for creating genome browser files.  For example, HOMER has [specialized routines](http://homer.ucsd.edu/homer/ngs/ucsc.html) for creating browser files.  Often, the output of a program is already suited to be loaded into a genome browser.  For example, cufflinks generates a "transcripts.gtf" file.  Macs creates a peak bed file.  Other common examples:\
> >
> > - [Visualize bam alignment files using samtools](http://homer.ucsd.edu/homer/basicTutorial/samfiles.html#Creating_a_BAM_index)
> > - [Visualize read coverage files using BEDtools](http://homer.ucsd.edu/homer/basicTutorial/samfiles.html)
> > - Or, use [HOMER](http://homer.ucsd.edu/homer/ngs/index.html)
>
>\
>
> ### Loading custom data into the UCSC Genome Browser
>
> > To look at your own data using the UCSC Genome Browser, click on the "Genomes" at the top and look for the "Add Custom Tracks" or "Manage Custom Tracks" button:\
> >
> > > ![UCSC custom track](http://homer.ucsd.edu/homer/basicTutorial/customtrack.png)
> >
> > After uploading your track, the data should appear in the genome browser.  At the bottom of the browser image you'll find a variety of track settings.  The section at the top controls the settings for custom tracks:\
> >
> > > ![UCSC settings](http://homer.ucsd.edu/homer/basicTutorial/UCSCsettings.png)
> >
> > You can change how the track is visualized by clicking on the drop down menu, shown above.  You can also click on the 'blue' link name and change other custom settings.\
> >\
>
> ### Visual Quality Control
>
> > There are several things to look out for when viewing your data in the browser.  Below is a checklist to help guide you.\
> >
> > - Look for spikes in the data.  These may be caused by contaminants, and may cause problems with data analysis.
> > - ChIP-Seq
> >   - Are there nice, defined peaks in the data? Or are there regions of continuous coverage (histone marks)?
> >   - Are there reads on all expected chromsomes?
> >   - Does the pattern match the experiment?
> >     - TFs: Spikes of enrichment near the TSS and distal regulatory elements
> >     - H3K4me3 - enriched near TSS
> >     - H3K4me1/2, H3/H4ac, DNase - enriched near TSS and near distal regulatory elements
> >     - H3K36me3 - enriched across gene bodies
> >     - H3K27me3 - enriched near CpG Islands of inactive genes
> >     - H3K9me3 - enriched across broad domains and repeat elements
> >   - Is the background low, or almost as high as the expected peaks?
> > - RNA-Seq
> >   - Are most reads found on exons?  Or is there a lot of reads in introns/other regions?
> >   - Do you have even read coverage across exons, or is it full of strong spikes?
> >   - Is there a 3' or 5' bias in the data?
> >   - If strand specific, is it the correct strand?
> >   -
