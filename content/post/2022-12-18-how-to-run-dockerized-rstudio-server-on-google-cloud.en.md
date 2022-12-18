---
title: How to run dockerized Rstudio server on google cloud
author: Ming Tang
date: '2022-12-18'
slug: how-to-run-dockerized-rstudio-server-on-google-cloud
categories:
  - bioinformatics
  - docker
tags:
  - bioinformatics
  - docker
header:
  caption: ''
  image: ''
---

### Create a google VM

Follow the process using the console https://cloud.google.com/compute/docs/instances/create-start-instance#console_1

### Install docker

Follow https://docs.docker.com/engine/install/debian/

**Note** this example for the `debian` build. If you created your VM using `ubuntu` as the boot disk, you should follow the 
`ubuntu` section https://docs.docker.com/engine/install/ubuntu/.

In the GCP VM:

```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null  
  
  
sudo apt-get update


sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

verify it works

```bash 
sudo docker run hello-world
```

### Install bioconductor docker container

create a folder for R library outside the containter in your home directory on the VM.

In the GCP VM:

```bash
cd ~ 
mkdir -p R/host-site-library
cd R/host-site-library
pwd 
/home/tommytang/R/host-site-library

# make the dir writable for Rstudio to install custom R packages 
chmod a+w -R /home/tommytang/R/host-site-library
```
Run the docker command

```bash
 sudo docker run \
    -v /home/tommytang:/home/rstudio/tommytang \
    -v /home/tommytang/R/host-site-library:/usr/local/lib/R/host-site-library \
    -e R_LIBS_USER=/home/tommytang/R/host-site-library \
    -e PASSWORD=password \
  	-p 8787:8787 \
  	bioconductor/bioconductor_docker:RELEASE_3_15  
```
`-v /home/tommytang:/home/rstudio/tommytang` mounts `/home/tommytang` in your VM to the docker container directory `/home/rstudio/tommytang` so Rstudio in the container can see your files in your VM.


`-v /home/R/host-site-library:/usr/local/lib/R/host-site-library` mounts `/home/tommytang/R/host-site-library` in your VM to the docker container directory  `/usr/local/lib/R/host-site-library`

Note, you want to make `/home/tommytang` to be writable by Rstudio too.

```bash
chomd a+w -R /home/tommytang
```

If no password is specified, it will generated a random one in the command output.
    
```bash   
 sudo docker run \
    -v /home/tommytang:/home/rstudio/tommytang \
    -v /home/tommytang/R/host-site-library:/usr/local/lib/R/host-site-library \
    -e R_LIBS_USER=/home/tommytang/R/host-site-library \
  	-p 8787:8787 \
  	bioconductor/bioconductor_docker:RELEASE_3_15   
    
```
The first time you run this command, docker will pull the image to the VM.

### ssh tunneling

execute this command in your local mac:

```bash
gcloud compute ssh tommytang@tommy-vm -- -L 8787:localhost:8787
```

### open Rstudio server on your local computer

1. go to `localhost:8787`
2. type in the username: `rstudio`; password: `<YOUR_PASS>`


### references

* details of libpath in the docker container https://github.com/Bioconductor/bioconductor_docker/issues/10

* issue I openned on github https://github.com/Bioconductor/bioconductor_docker/issues/66

* https://www.bioconductor.org/help/docker/

* https://github.com/Bioconductor/bioconductor_docker
