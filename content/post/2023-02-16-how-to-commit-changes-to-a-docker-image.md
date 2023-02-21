---
title: How to commit changes to a docker image
author: Ming Tang
date: '2023-02-16'
slug: how-to-commit-changes-to-a-docker-image
categories:
  - bioinformatics
  - docker
tags:
  - docker
header:
  caption: ''
  image: ''
---

[Docker](https://www.docker.com/) is a great tool to ensure reproducibility of your computing work. I was
using the bioconductor image on google cloud, but the image does have the `gsutil` command.


You can install once in the container, but once you exit the container, the gsutil command
will be gone. You will need to modify the docker image if you want to keep using it.


### Step 1

```bash
$ sudo docker images -a

REPOSITORY                         TAG            IMAGE ID       CREATED         SIZE
rocker/tidyverse                   latest         d4d41e410fb7   2 months ago    2.16GB
r-base                             latest         3de1ef2039fb   3 months ago    838MB
bioconductor/bioconductor_docker   RELEASE_3_15   75cc0e27e8ea   6 months ago    4.23GB
hello-world                        latest         feb5d9fea6a5   17 months ago   13.3kB
```

### Step 2

run the `bioconductor/bioconductor_docker` docker container interactively in bash:
 
```bash
$ sudo docker run -it 75cc0e27e8ea bin/bash
```

### Step 3

Modify the container by installing `gsutils`

```bash
$ gstuil
bash: gstuil: command not found

$ curl -sSL https://sdk.cloud.google.com | bash

Performing post processing steps...done.

Update done!


Modify profile to update your $PATH and enable shell command completion?

Do you want to continue (Y/n)?
```

It will add the gsutil to your `PATH`. Exit the container:

```bash
$ exit
```

### Step 4 

Find the container ID:

```bash
$ sudo docker ps -a | head
CONTAINER ID   IMAGE                                           COMMAND      CREATED             STATUS                          PORTS                                       NAMES
ab9c4b13c67c   bioconductor/bioconductor_docker:RELEASE_3_15   "/init"      36 seconds ago      Up 35 seconds                   0.0.0.0:8787->8787/tcp, :::8787->8787/tcp   keen_khayyam
d3786b7b53a5   75cc0e27e8ea                                    "bin/bash"   10 minutes ago      Exited (127) 23 seconds ago                                                 quirky_cori
d6a94d025154   bioconductor/bioconductor_docker:RELEASE_3_15   "/init"      About an hour ago   Exited (0) About a minute ago                                               kind_goldwasser
ab7e03d1d665   bioconductor/bioconductor_docker:RELEASE_3_15   "/init"      About an hour ago   Exited (0) About an hour ago                                                wonderful_bouman
```

`sudo docker commit [CONTAINER_ID] [new_image_name]` to commit the changes:

```bash
$ sudo docker commit d3786b7b53a5 bioconductor_gstuils
```

### Step 5

Hoary! The `bioconductor_gstuils` image is created!

```bash
$ sudo docker images
REPOSITORY                         TAG            IMAGE ID       CREATED         SIZE
bioconductor_gstuils               latest         375c7ec8354f   2 minutes ago   5.49GB
rocker/tidyverse                   latest         d4d41e410fb7   2 months ago    2.16GB
r-base                             latest         3de1ef2039fb   3 months ago    838MB
bioconductor/bioconductor_docker   RELEASE_3_15   75cc0e27e8ea   6 months ago    4.23GB
hello-world                        latest         feb5d9fea6a5   17 months ago   13.3kB
```

The `gsutil` command is now avaiable in the new docker container.

```bash
$ sudo docker run -it bioconductor_gstuils bin/bash

$ gsutil
Usage: gsutil [-D] [-DD] [-h header]... [-i service_account] [-m] [-o section:flag=value]... [-q] [-u user_project] [command [opts...] args...]
Available commands:
  acl              Get, set, or change bucket and/or object ACLs
  autoclass        Configure Autoclass feature
  bucketpolicyonly Configure uniform bucket-level access
  cat              Concatenate object content to stdout
  compose          Concatenate a sequence of objects into a new composite object.
  config           Obtain credentials and create configuration file
  cors             Get or set a CORS JSON document for one or more buckets
  cp               Copy files and objects
  defacl           Get, set, or change default ACL on buckets
  defstorageclass  Get or set the default storage class on buckets
  du               Display object size usage
  hash             Calculate file hashes
  help             Get help about commands and topics
  hmac             CRUD operations on service account HMAC keys.
  iam              Get, set, or change bucket and/or object IAM permissions.
  kms              Configure Cloud KMS encryption
  label            Get, set, or change the label configuration of a bucket.
  lifecycle        Get or set lifecycle configuration for a bucket
  logging          Configure or retrieve logging on buckets
  ls               List providers, buckets, or objects
  mb               Make buckets
  mv               Move/rename objects
  notification     Configure object change notification
  pap              Configure public access prevention
  perfdiag         Run performance diagnostic
  rb               Remove buckets
  requesterpays    Enable or disable requester pays for one or more buckets
  retention        Provides utilities to interact with Retention Policy feature.
  rewrite          Rewrite objects
  rm               Remove objects
  rpo              Configure replication
  rsync            Synchronize content of two buckets/directories
  setmeta          Set metadata on already uploaded objects
  signurl          Create a signed URL
  stat             Display object status
  test             Run gsutil unit/integration tests (for developers)
  ubla             Configure Uniform bucket-level access
  update           Update to the latest gsutil release
  version          Print version info about gsutil
  versioning       Enable or suspend versioning for one or more buckets
  web              Set a website configuration for a bucket

...

Use gsutil help <command or topic> for detailed help.
```

Hmm.. but now the below command does not bring up the Rstudio.

```bash
sudo docker run \
    -v /home/tommytang:/home/rstudio/tommytang \
    -v /home/tommytang/R/host-site-library:/usr/local/lib/R/host-site-library \
    -e R_LIBS_USER=/home/tommytang/R/host-site-library \
  	-p 8787:8787 \
  	bioconductor_gstuils
```

What's wrong?

I know I can also add `RUN curl -sSL https://sdk.cloud.google.com | bash` to the bioconductor [docker file](https://github.com/Bioconductor/bioconductor_docker/blob/master/Dockerfile) and 
build a new docker image. That probably will work.

