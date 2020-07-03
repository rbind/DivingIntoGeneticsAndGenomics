---
title: build your own singularity image
author: Ming Tang
date: '2020-07-02'
slug: build-your-own-singularity-image
categories:
  - bioinformatics
  - singularity
tags:
  - singularity
header:
  caption: ''
  image: ''
  preview: yes
---

I was using the [tidyverse rocker image](https://hub.docker.com/r/rocker/tidyverse/) on HPC by `singularity pull`. see my previous [post](https://divingintogeneticsandgenomics.rbind.io/post/run-rstudio-server-with-singularity-on-hpc/).

Everything was OK until I encountered problems installing `jpeg` and `Cairo` R packages. Later, I also had an error installing [scRepertoire](https://github.com/ncborcherding/scRepertoire) dependency `gsl`.

It turns out I have to install debian packages inside the container:

```bash
$ apt update && apt install -y --no-install-recommends libjpeg62-turbo-dev zlib1g-dev libpng-dev \
	&& apt install -y --no-install-recommends libx11-dev libcairo2-dev libxt-dev \
	&& apt install -y libgsl-dev 
```

However, singularity file system is read-only.

You **CAN NOT** do:

```bash
singularity shell rstudio.simg
## and then inside the container
apt install -y --no-install-recommends libjpeg62-turbo-dev zlib1g-dev libpng-dev
```

Nathan Weeks, our previous docker expert at Harvard FAS informatics told me:

> Easiest would be to extend the Docker image, then create a new Singularity image from that.

The followings are the steps I took.

### prepare a dockerfile

On my local mac which has docker running prepare a [Dockerfile](https://github.com/crazyhottommy/rocker_tidyvese_jpeg_cairo) and put it into a folder `rocker-jpeg`:

```
FROM rocker/tidyverse:3.6.3
RUN apt update && apt install -y --no-install-recommends libjpeg62-turbo-dev zlib1g-dev libpng-dev \
	&& apt install -y --no-install-recommends libx11-dev libcairo2-dev libxt-dev \
	&& apt install -y libgsl-dev \
 	&& rm -rf /var/lib/apt/lists/*
```
### build the docker image

```bash
$ cd rocker-jpeg
## build the extended docker image
$ docker build -t rocker-with-jpeg:3.6.3 .

## push to docker hub
$ docker tag rocker-with-jpeg:3.6.3 crazyhottommy/rocker-with-jpeg:3.6.3

$ docker push crazyhottommy/rocker-with-jpeg:3.6.3
```

Now, I can install those packages successfully:

```bash
$ docker run -it rocker-with-jpeg:3.6.3 bash
$ R
```
inside R console:

```r
> install.package(c("jpeg", "Cario"))
```

### use the image 
I can now either directly `singularity pull docker://crazyhottommy/rocker-with-jpeg:3.6.3`
to get the singularity image or convert the docker image to singularity image at my local mac:

```bash
$ docker run -v /var/run/docker.sock:/var/run/docker.sock -v /tmp/singularity:/output --privileged -t --rm quay.io/singularity/docker2singularity:v3.6.3 my-rocker-with-jpeg:3.6.3
```

The resulting singularity image will be in `/tmp/singularity`.

**NOTE**: My previous colleague Seth Sahil mentioned that I may want to use the [geospacial rocker image](https://hub.docker.com/r/rocker/geospatial) since it includes most of the libraries.
