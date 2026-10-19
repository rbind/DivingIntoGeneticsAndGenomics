---
title: csvkit to manipulate csv at command line, Rio to interact with R at command line
author: Ming Tang
date: '2014-11-10T12:18:00-08:00'
lastmod: '2014-11-10T12:18:00-08:00'
slug: csvkit-to-manipulate-csv-at-command-line-rio-to-interact-with-r-at-command-line
tags:
- csvkit
- data
- ggplot
- unix
- R
- Rio
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/11/csvkit-to-manipulate-csv-at-command.html
description: I came across csvkit long time ago, but I just began to use it and found it is very handy. It is a python utility to manipulate csv files at command line.…
summary: I came across csvkit long time ago, but I just began to use it and found it is very handy. It is a python utility to manipulate csv files at command line.…
header:
  caption: ''
  image: ''
---

I came across [csvkit](https://csvkit.readthedocs.org/en/0.9.0/) long time ago, but I just began to use it and found it is very handy. It is a python utility to manipulate csv files at command line. you can install it by pip:

pip install csvkit

I played around with the iris data.\
csvlook gives you a database view of the data by adding grids.\
$ cat iris.csv \| head \| csvlook\
\|---------------+-------------+--------------+-------------+--------------\|\
\|  sepal_length \| sepal_width \| petal_length \| petal_width \| species      \|\
\|---------------+-------------+--------------+-------------+--------------\|\
\|  5.1          \| 3.5         \| 1.4          \| 0.2         \| Iris-setosa  \|\
\|  4.9          \| 3.0         \| 1.4          \| 0.2         \| Iris-setosa  \|\
\|  4.7          \| 3.2         \| 1.3          \| 0.2         \| Iris-setosa  \|\
\|  4.6          \| 3.1         \| 1.5          \| 0.2         \| Iris-setosa  \|\
\|  5.0          \| 3.6         \| 1.4          \| 0.2         \| Iris-setosa  \|\
\|  5.4          \| 3.9         \| 1.7          \| 0.4         \| Iris-setosa  \|\
\|  4.6          \| 3.4         \| 1.4          \| 0.3         \| Iris-setosa  \|\
\|  5.0          \| 3.4         \| 1.5          \| 0.2         \| Iris-setosa  \|\
\|  4.4          \| 2.9         \| 1.4          \| 0.2         \| Iris-setosa  \|\
\|---------------+-------------+--------------+-------------+--------------\|

csvstat can output summary statistics for specific columns with -c flag\
-c can accept both the column names and the column numbers as arguments

$ cat iris.csv \| csvstat -c sepal_length,sepal_width

  1. sepal_length\
\<type 'float'\>\
Nulls: False\
Min: 4.3\
Max: 7.9\
Sum: 876.5\
Mean: 5.84333333333\
Median: 5.8\
Standard Deviation: 0.825301291785\
Unique values: 35\
5 most frequent values:\
5.0: 10\
6.3: 9\
5.1: 9\
6.7: 8\
5.7: 8\
  2. sepal_width\
\<type 'float'\>\
Nulls: False\
Min: 2.0\
Max: 4.4\
Sum: 458.6\
Mean: 3.05733333333\
Median: 3.0\
Standard Deviation: 0.434410967735\
Unique values: 23\
5 most frequent values:\
3.0: 26\
2.8: 14\
3.2: 13\
3.4: 12\
3.1: 11

Row count: 150

you can also output only the max, min, mean by adding --max, --min etc

`$ cat iris.csv | csvstat -c sepal_length,sepal_width --max`\
` 1. sepal_length: 7.9`\
` 2. sepal_width: 4.4`

csvcut can cut out specific columns and reorder the column while the unix cut command can\
not reorder the column, one needs to use awk to achieve that.

`$ cat iris.csv | csvcut -c5,4,2 | csvlook | head`\
`|------------------+-------------+--------------|`\
`| species | petal_width | sepal_width |`\
`|------------------+-------------+--------------|`\
`| Iris-setosa | 0.2 | 3.5 |`\
`| Iris-setosa | 0.2 | 3.0 |`\
`| Iris-setosa | 0.2 | 3.2 |`\
`| Iris-setosa | 0.2 | 3.1 |`\
`| Iris-setosa | 0.2 | 3.6 |`\
`| Iris-setosa | 0.4 | 3.9 |`\
`| Iris-setosa | 0.3 | 3.4 |`

csvgrep can grep based on a specific column:

`$ cat iris.csv | csvgrep -c species -m setosa | wc -l`\
`51`

other commands like csvsort, csvjoin, csvsql are also useful, please refer to the link above to see more examples.

I want to plot some nice figure at command line, but I am too lazy to fire Rstudio. by using [Rio](https://github.com/jeroenjanssens/data-science-at-the-command-line/blob/master/tools/Rio), you can leverage the power of R at the command line. you can then visualize it by using the display command from [imagemagic](http://www.imagemagick.org/)\
-g to load the ggplot2 package\
-e to execute the command

update on 11/19/2014, Rio has changed the flag to load dplry and tidyr\
<https://github.com/jeroenjanssens/data-science-at-the-command-line/commit/66fc1fda604f2d420de9490934259093a76fb105>

boxplot of sepal_length for different species :

![](/img/archive/csvkit-to-manipulate-csv-at-command-line-rio-to-interact-with-r-at-command-line/01-selection_025.png)

histogram for sepal_length for all species:

![](/img/archive/csvkit-to-manipulate-csv-at-command-line-rio-to-interact-with-r-at-command-line/02-selection_026.png)

 you can change the color and fill of the bars:

![](/img/archive/csvkit-to-manipulate-csv-at-command-line-rio-to-interact-with-r-at-command-line/03-selection_027.png)

you can also plot histogram for different species with a facet:

![](/img/archive/csvkit-to-manipulate-csv-at-command-line-rio-to-interact-with-r-at-command-line/04-selection_028.png)

scatter plot of sepal_width for different species:

![](/img/archive/csvkit-to-manipulate-csv-at-command-line-rio-to-interact-with-r-at-command-line/05-selection_029.png)

command line is awesome! I love it! It brings me more power!
