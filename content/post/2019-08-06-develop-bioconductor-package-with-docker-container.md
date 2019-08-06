---
title: Develop Bioconductor packages with docker container
author: Ming Tang
date: '2019-08-06'
slug: develop-bioconductor-packages-with-docker-container
categories:
  - R
  - bioinformatics
  - package
tags:
  - Bioconductor
header:
  caption: ''
  image: ''
---

### Readings 

links to read:

* [https://www.bioconductor.org/developers/package-guidelines/#rcode](https://www.bioconductor.org/developers/package-guidelines/#rcode)

* [https://github.com/Bioconductor/Contributions](https://github.com/Bioconductor/Contributions)

* use container  [https://github.com/Bioconductor/bioconductor_full](https://github.com/Bioconductor/bioconductor_full)

I am following the last link.

### pull the container

```bash
docker pull bioconductor/bioconductor_full:devel
docker images 

REPOSITORY                       TAG                  IMAGE ID            CREATED             SIZE
bioconductor/bioconductor_full   devel                ae3ec2be7376        3 hours ago         5.7GB
seuratv3                         latest               9b358ab1fd63        2 days ago          2.76GB
```
It is 5.7G in size.

start the Rstuido from the image. I have another Rstudio instance using port 8787, let me use a different one (e.g. 8080).  docker Rstudio default port is `8787`, change the host port to `8080`.

```bash
mkdir -p ~/R/host-site-library

docker run                                      \
  -e PASSWORD="xyz"                   \
  -p 8080:8787                                \
  -v ~/R/host-site-library:/usr/local/lib/R/host-site-library  \
  -v ~/github_repos:/home/rstudio \
  bioconductor/bioconductor_full:devel
```

NOTE: The path `~/R/host-site-library` is mapped to .libPaths() in R. So, when R is started, all the libraries in the host directory `host-site-library` are available to R. It is stored on your machine mounted from the volume you fill in place of host-site-library.

The mounted path must be an **absolute path**.

I also mounted the `github_repo` folder in my host machine to the docker home directory. Because every time you exit a container and start it again, the modification you did to the container will be gone (unless you make an modified image and use that for the next time). I will save the R package in my `~/github_repo` folder in the host machine.

type `localhost:8080`, you should see the Rstudio login page. username is `rstudio`, password is `xyz` in this dummy example.

In Rstudio:

```r 
> .libPaths()
[1] "/usr/local/lib/R/host-site-library" "/usr/local/lib/R/site-library"     
[3] "/usr/local/lib/R/library"
```

you will see `/usr/local/lib/R/host-site-library` is in the `libpath`, that corresponds to the `~/R/host-site-library` in the host machine, if you do package installation, it will be installed in the`~/R/host-site-library` .

### start an R package use usethis

follow these two blog posts and [R packages book](http://r-pkgs.had.co.nz/) by Hadley Wickham:  

* https://blog.methodsconsultants.com/posts/developing-r-packages-using-gitlab-ci-part-i/  

* https://www.hvitfeldt.me/blog/usethis-workflow-for-package-development/

```r 
install.packages(c("devtools", "roxygen2", "usethis", "available", "rmarkdown"))

## check if the package name is avaiable (not used by others)
library(available)
available("myawesomepackage")

library(usethis)
usethis::create_package("~/myawesomepackage")

use_git()
use_github()
use_mit_license("Ming Tang")
usethis::use_pipe()

```

Add a function

```r 
usethis::use_r("myawesomefunc")
```

On mac:  
`command + option + shift + R` for inserting roxygen comment.  
`command + shift + D` for documentation.  
`command + shfit + B` for building package.  

add more functions, repeat.

### Next
Next is to add test and setup some continuous integration. Read this [Automate testing of your R package using Travis CI, Codecov, and testthat](https://jef.works/blog/2019/02/17/automate-testing-of-your-R-package/) by Jean Fan.


