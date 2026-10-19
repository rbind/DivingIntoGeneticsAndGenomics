---
title: dangerous rm command
author: Ming Tang
date: '2017-08-04T20:51:00-07:00'
lastmod: '2017-08-04T20:51:00-07:00'
slug: dangerous-rm-command
tags:
- terminal
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/08/dangerous-rm-command.html
description: rm command is very dangerous because after you remove something, you can not recover it. There is no trash bin in the unix system. If you have some raw data…
summary: rm command is very dangerous because after you remove something, you can not recover it. There is no trash bin in the unix system. If you have some raw data…
header:
  caption: ''
  image: ''
---

`rm` command is very dangerous because after you remove something, you can not recover it. There is no trash bin in the unix system. If you have some raw data (e.g fastq files), you'd better make them safe by changing the file permissions. in an empty directory, make a folder `foo`:

``` text
mkdir test
cd test
mkdir foo
cd foo
touch {1..4}.fastqs
ls
1.fastqs  2.fastqs  3.fastqs  4.fastqs
cd ..
```

let's first make the foo directory unwritable

``` text
ls -l
drwxr-x--- 2 krai genomic_med   512 Aug  4 22:27 foo

ls -l foo
total 0
-rw-r----- 1 krai genomic_med 0 Aug  4 22:31 1.fastqs
-rw-r----- 1 krai genomic_med 0 Aug  4 22:31 2.fastqs
-rw-r----- 1 krai genomic_med 0 Aug  4 22:31 3.fastqs
-rw-r----- 1 krai genomic_med 0 Aug  4 22:31 4.fastqs

#remove the write privilege for the foo folder
chmod u-w foo
ls -l
dr-xr-x--- 2 krai genomic_med 512 Aug  4 22:31 foo

# the files inside the foo folder does not change
ls -l foo
-rw-r----- 1 krai genomic_med 0 Aug  4 22:31 1.fastqs
-rw-r----- 1 krai genomic_med 0 Aug  4 22:31 2.fastqs
-rw-r----- 1 krai genomic_med 0 Aug  4 22:31 3.fastqs
-rw-r----- 1 krai genomic_med 0 Aug  4 22:31 4.fastqs

# now you can not remove the foo folder:
rm -rf foo
rm: cannot remove `foo/2.fastqs': Permission denied
rm: cannot remove `foo/1.fastqs': Permission denied
rm: cannot remove `foo/4.fastqs': Permission denied
rm: cannot remove `foo/3.fastqs': Permission denied

# rm -rf foo/*
rm: cannot remove `foo/1.fastqs': Permission denied
rm: cannot remove `foo/2.fastqs': Permission denied
rm: cannot remove `foo/3.fastqs': Permission denied
rm: cannot remove `foo/4.fastqs': Permission denied
```

let's make the fastq files unwritable, but change the foo folder back to default:

``` text
chmod u+w foo
ls -l
drwxr-x--- 2 krai genomic_med 512 Aug  4 22:31 foo

chmod u-w foo/*fastqs
ls -l foo
-r--r----- 1 krai genomic_med 0 Aug  4 22:31 1.fastqs
-r--r----- 1 krai genomic_med 0 Aug  4 22:31 2.fastqs
-r--r----- 1 krai genomic_med 0 Aug  4 22:31 3.fastqs
-r--r----- 1 krai genomic_med 0 Aug  4 22:31 4.fastqs

# let's try to remove the fastqs
rm foo/*fastqs
rm: remove write-protected regular empty file `foo/1.fastqs'?
```

The unix system asks to confirm deletion of the file. let's remove by force:

``` text
rm -rf foo/*fastqs
# the system even did not ask!
ls foo/
# nothing!
```

The files are removed! You can not recover them. see a post here <https://unix.stackexchange.com/questions/48579/why-can-rm-remove-read-only-files>

> Any attempt to access a file's data requires read permission. Any attempt to modify a file's data requires write permission. Any attempt to execute a file (a program or a script) requires execute permission...

> Because directories are not used in the same way as regular files, the permissions work slightly (but only slightly) differently. An attempt to list the files in a directory requires read permission for the directory, but not on the files within. An attempt to add a file to a directory, delete a file from a directory, or to rename a file, all require write permission for the directory, but (perhaps surprisingly) not for the files within. Execute permission doesn't apply to directories (a directory can't also be a program). But that permission bit is reused for directories for other purposes.

> Execute permission is needed on a directory to be able to cd into it (that is, to make some directory your current working directory).

> Execute is needed on a directory to access the "inode" information of the files within. You need this to search a directory to read the inodes of the files within. For this reason the execute permission on a directory is often called search permission instead.

Conclusion: All `rm` needs is write+execute permission on the parent directory. The permissions of the file itself are irrelevant.
