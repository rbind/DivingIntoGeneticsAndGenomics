---
title: cores, cpus and threads
author: Ming Tang
date: '2017-07-10T09:05:00-07:00'
lastmod: '2017-07-10T09:05:00-07:00'
slug: cores-cpus-and-threads
tags:
- cores
- cpus
- threads
- unix
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2017/07/cores-cpus-and-threads.html
description: cores, cpus and threads :\ http://www.slac.stanford.edu/comp/unix/package/lsf/currdoc/lsfadmin/index.htm?limcore_detection.html~main\ Traditionally, the…
summary: cores, cpus and threads :\ http://www.slac.stanford.edu/comp/unix/package/lsf/currdoc/lsfadmin/index.htm?limcore_detection.html~main\ Traditionally, the…
header:
  caption: ''
  image: ''
---

### Some reading for the basics

cores, cpus and threads :\
[http://www.slac.stanford.edu/comp/unix/package/lsf/currdoc/lsf_admin/index.htm?lim_core_detection.html~main](http://www.slac.stanford.edu/comp/unix/package/lsf/currdoc/lsf_admin/index.htm?lim_core_detection.html%7Emain)\
Traditionally, the value of ncpus has been equal to the number of physical CPUs. However, many CPUs consist of multiple cores and threads, so the traditional 1:1 mapping is no longer useful. A more useful approach is to set ncpus to equal one of the following:

- The number of processors
- Cores—the number of cores (per processor) \* the number of processors (this is the ncpus default setting)
- Threads—the number of threads (per core) \* the number of cores (per processor) \* the number of processors

Hyper-threading:\
<https://www.howtogeek.com/194756/cpu-basics-multiple-cpus-cores-and-hyper-threading-explained/>\
Understanding Linux CPU Load - when should you be worried?\
<http://blog.scoutapp.com/articles/2009/07/31/understanding-load-averages>

### Quote from our HPC Admin

From our HPC admin Sally Boyd:

> On our systems there are actually 2 CPUs with 12 Cores each for a total of 24 ppn (processors per node).\
> We use CPU and Core interchangeably, but we shouldn’t. We do not use hyperthreading on any of our clusters because it breaks the MPI software (message passing interface, used for multi-node processing). You can consider one thread per processor/core. So the most threads you can have is 24. If various parts of your pipeline use multiple threads and they’re running at the same time, you might want to be sure that all of those add up to 24 and no more. The other thing is that there is some relatively new (to us) code out there that calls a multi-threaded R without specifying number of threads, or else it starts up several iterations of itself, such that the scheduler is not aware. This causes lots of issues. I don’t recall if the code you were running previously that used so many resources was one of those or not.

### My problem

I was runnning parallellized freebayes on cluster and needed to specify the number of cores.<https://github.com/ekg/freebayes/blob/master/scripts/freebayes-parallel>\
The command I run:

``` text
./freebayes-parallel regions_to_include_freebayes.bed 4 -f {config[ref_fa]} \
        --genotype-qualities \
        --ploidy 2 \
        --min-repeat-entropy 1 \
        --no-partial-observations \
        --report-genotype-likelihood-max \
        {params.outputdir}/{input[0]} {params.outputdir}/{output} 2> {params.outputdir}/{log}
```

it uses GNU parallel under the hood.

``` text
regionsfile=$1
shift
ncpus=$1
shift

command=("freebayes" "$@")

(
#$command | head -100 | grep "^#" # generate header
# iterate over regions using gnu parallel to dispatch jobs
cat "$regionsfile" | parallel -k -j "$ncpus" "${command[@]}" --region {}
) | ../vcflib/scripts/vcffirstheader \
  | ../vcflib/bin/vcfstreamsort -w 1000 \
  | vcfuniq # remove duplicates at region edges
```

Note that `freebayes-parallel` was hard-coded `../vcflib/..` one can put the `vcflib bin` to PATH, and call `vcffirstheader` and `vcfstreamsort` directly.\
How many threads will be used? In my command, I specified `-j 4`. effectively, the commands is

``` text
(cat regions_to_include_freebayes.bed \
| parallel -k -j 4 "freebayes --region {} -f {config[ref_fa]} \
        --genotype-qualities \
        --ploidy 2 \
        --min-repeat-entropy 1 \
        --no-partial-observations \
        --report-genotype-likelihood-max \
        {params.outputdir}/my.sorted.bam 2> {params.outputdir}/{log})  \
| vcffirstheader \
| vcfstreamsort -w 1000 \
| vcfuniq > {params.outputdir}/{output}
```

At least 1 `cat` + 4(-j) + 3 (pipes) = 8 threads will be used.\
checking how many cores I have in the computing nodes:

``` text
cat /proc/cpuinfo | grep "model name"
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz
model name : Intel(R) Xeon(R) CPU E5-2697 v2 @ 2.70GHz

grep "model name" /proc/cpuinfo | wc -l
24
```

I reserved 12 cores to run the job. checking the job after submitting:

``` text
bjobs -l 220806

## some output
 RUNLIMIT
 1440.0 min of chms025

 MEMLIMIT
     32 G
Mon Jul  3 16:06:49: Started 12 Task(s) on Host(s) <chms025> <chms025> <chms025
                     > <chms025> <chms025> <chms025> <chms025> <chms025> <chms0
                     25> <chms025> <chms025> <chms025>, Allocated 12 Slot(s) on
                      Host(s) <chms025> <chms025> <chms025> <chms025> <chms025>
                     <chms025> <chms025> <chms025> <chms025> <chms025> <chms025
                     > <chms025>, Execution Home </rsrch2/genomic_med/krai>, Ex
                     ecution CWD </rsrch2/genomic_med/krai/scratch/TCGA_CCLE_SK
                     CM/TCGA_SKCM_FINAL_downsample_RUN/SNV_calling>;
Mon Jul  3 21:15:41: Resource usage collected.
                     The CPU time used is 2132 seconds.
                     MEM: 1.1 Gbytes;  SWAP: 2.3 Gbytes;  **NTHREAD: 17**
                     PGID: 26713;  PIDs: 26713 26719 26722 26729 26734 26783
                     26784 26786 26788 1301 1302 1303 1304 26785 26787 26789

 MEMORY USAGE:
 MAX MEM: 1.9 Gbytes;  AVG MEM: 1 Gbytes
```

It says 17 threads are used.\
I went to the computing nodes, and checked PIDs related to my job:

``` text
ssh chms025
uptime
21:19:39 up 410 days,  9:33,  1 user,  load average: **5.94, 5.91, 5.87**

top -u krai -M -n 1 -b | grep krai
32381 krai      20   0  486m 314m 1808 R 100.0  0.1   0:01.37 freebayes
32382 krai      20   0  240m 224m 1808 R 98.4  0.1   0:01.15 freebayes
32360 krai      20   0  195m 179m 1912 R 92.6  0.0   0:02.95 freebayes
32390 krai      20   0  204m 188m 1808 R 54.0  0.0   0:00.28 freebayes
32388 krai      20   0 15568 1648  848 R  1.9  0.0   0:00.02 top
26713 krai      20   0 20388 2684 1460 S  0.0  0.0   0:41.56 res
26719 krai      20   0  103m 1256 1032 S  0.0  0.0   0:00.00 1499116008.2208
26722 krai      20   0  103m  804  556 S  0.0  0.0   0:00.00 1499116008.2208
26729 krai      20   0  258m  22m 4352 S  0.0  0.0   0:02.19 python
26734 krai      20   0  105m 1420 1144 S  0.0  0.0   0:00.00 bash
26783 krai      20   0  103m 1300 1060 S  0.0  0.0   0:00.00 freebayes-paral
26784 krai      20   0  103m  488  244 S  0.0  0.0   0:00.00 freebayes-paral
26785 krai      20   0  115m 4872 1928 S  0.0  0.0   0:05.03 python
26786 krai      20   0  100m 1288  480 S  0.0  0.0   0:00.00 cat
26787 krai      20   0 29152  11m 1344 S  0.0  0.0   1:46.80 vcfstreamsort
26788 krai      20   0  139m 9.9m 2036 S  0.0  0.0   1:11.87 perl
26789 krai      20   0 21156 1580 1308 S  0.0  0.0   1:34.24 vcfuniq
31906 krai      20   0 96072 1768  840 S  0.0  0.0   0:00.00 sshd
31907 krai      20   0  106m 2076 1464 S  0.0  0.0   0:00.07 bash
32389 krai      20   0  100m  836  732 S  0.0  0.0   0:00.00 grep
```

Indeed, there are 4 freebayes (-j 4 from parallel) are running. 1 `cat`, 1 `vcfstreamsort`, 1 `vcfuniq`, not sure where are the 2 `python`, 1 `grep`, 1 `perl`, 2 `bash` from. My guess is that some scripts are wrapped shell scripts.
