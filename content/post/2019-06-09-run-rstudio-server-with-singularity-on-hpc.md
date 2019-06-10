---
title: Run Rstudio server with singularity on HPC
author: Ming Tang
date: '2019-06-09'
slug: run-rstudio-server-with-singularity-on-hpc
categories:
  - HPC
  - docker
  - singularity
  - genomics
tags:
  - scRNAseq
  - HPC
  - docker
  - singularity
header:
  caption: ''
  image: ''
---

### Background
Please read the following before go ahead:

* what is [docker](https://www.docker.com/)?  
* what is [Rocker](https://www.rocker-project.org/)?  
* what is [singularity](https://www.sylabs.io/docs/)?  


from Harvard Research computing website: [Odyssey has singularity installed](https://www.rc.fas.harvard.edu/resources/documentation/software/singularity-on-odyssey/).

>Why Singularity?
There are some important differences between Docker and Singularity:

* Docker and Singularity have their own container formats.
* Docker containers may be imported to run via Singularity.
* Docker containers need root privileges for full functionality which is not suitable for a shared HPC environment.
* Singularity allows working with containers as a regular user. No sudo is required,

Our [informatics group](https://informatics.fas.harvard.edu/) has several big memory (1TB) computing nodes that allow us to run interactive jobs. I want to have big memory to run Rstudio for my scRNAseq data.

### Run Rstudio server with singularity
I basically followed this tutorial https://www.rocker-project.org/use/singularity/ written by my colleague Nathan Weeks sitting in the same office with me. Thanks!

First, go to https://www.rocker-project.org/images/ choose the image you want. I use `tidyverse` heavily, so I downloaded the `tidyverse` image buit upon `Rstudio` image


```bash
## ssh to remote HPC and pull the docker image by singularity
ssh bio1
mkdir singularity-images; cd !$
singularity pull --name rstudio.simg docker://rocker/tidyverse:latest


# This example bind mounts the /project directory on the host into the Singularity container.
# By default the only host file systems mounted within the container are $HOME, /tmp, /proc, /sys, and /dev.
# type in the password you want to set, make it more complicated than this dummy one
PASSWORD='xyz' singularity exec --bind=/project  rstudio.simg rserver --auth-none=0  --auth-pam-helper-path=pam-helper --www-address=127.0.0.1

```

back to my mac and connect to it via [SSH tunnel](https://www.ssh.com/ssh/tunneling/):

Nathan explained by drawing the following.

![](/img/ssh_tunnel.jpg)
```bash
ssh -Nf -L 8787:localhost:8787 bio1
```


on my local mac, open `localhost:8787` in web browser, type in the Odyssey (HPC) user name and password (xyz in this dummy example). Rstudio server now is ready for me! Magic!!!

Note: if mulitple people using the same node for Rstudio sever, you will need to pick a different
port than `8787`.
### One more note on R packages

create an `.Renviron` file in your home diretory
```bash 
# User-installed R packages go into their home directory
echo 'R_LIBS_USER=~/R/%p-library/%v' >> ${HOME}/.Renviron
```
The platform and version will be replaced by the corresponding R versions

```bash
ls ~/R/x86_64-pc-linux-gnu-library/
3.6
```

install packages inside Rstudio and the packages will be installed to `~/R/x86_64-pc-linux-gnu-library/3.6`. R version in this singularity image is R3.6. Note that if you use R on command line at the remote machine and use the same version of R. the library may not be compatible. e.g. singularity container is based on debian （Ubuntu) and HPC is based on RPM (CentOS). One may need to have mulitiple `.Renviron` file and switch back and forth depending on which R one is using. If you have better options, please let me know!

### Jump to other folders
by default, Rstudio opens the home directory. if you want to go to other folders, you can click `...` in the file pane.
You can then type in the path you want to jump to.

![](/img/change_path.png)

### Submit a slurm job

If you do not have a big computing node that you can run interactive job, you can follow Nathan's [tutorial](https://www.rocker-project.org/use/singularity/) on how to submit slurm job to run Rstudio server with singularity.