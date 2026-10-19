---
title: What does __name__ == "__main__" mean?
author: Ming Tang
date: '2013-04-04T19:02:00-07:00'
lastmod: '2013-04-04T19:02:00-07:00'
slug: what-does-name-main-mean
tags:
- python
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/what-does-name-main-mean.html
description: 'But you want to make your code more structured, so you come up with this:'
summary: 'But you want to make your code more structured, so you come up with this:'
header:
  caption: ''
  image: ''
---

### 

### <http://love-python.blogspot.com/>

### [What does \_\_name\_\_ == "\_\_main\_\_" mean?](http://love-python.blogspot.com/2012/12/what-does-name-main-mean.html)

When you run a python script directly (example: $ python script.py), you want to set a starting point of the script. Python scripts are interpreted from the first line, then goes to the second line and so on ...

``` text
import module

def my_function():
  # code here

x = my_function()
```

But you want to make your code more structured, so you come up with this:

``` text
import module

def my_function():
  # code here

def main():
  x = my_function()

# the program starts from here
main()
```

This is good, but problem is, if you import the script from another script (from module import \*), the main() function gets executed, but you may not want to do this. You want to call the main() function only when this script is exclusively executed. And you can do this using [\_\_name\_\_ == "\_\_main\_\_"](http://docs.python.org/2/library/__main__.html).

``` text
import module

def my_function():
   # code here

def main():
   x = my_function()

# the program starts from here
if __name__ == "__main__":
   main()
```

Thus you can make your script to make it a reusable script (import from another script) and a standalone script as well.
