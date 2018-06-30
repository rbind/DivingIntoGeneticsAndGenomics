---
title: hugo academic theme blog down deployment (some details)
author: Ming Tang
date: '2018-06-29'
slug: hugo-academic-theme-blog-down-deployment-some-details
categories:
  - R
tags: []
header:
  caption: ''
  image: ''
---

I have been following this [tutorial](https://alison.rbind.io/post/up-and-running-with-blogdown/) from Alison and tips from [Leslie Myint](https://lmyint.github.io/post/hugo-academic-tips/) for some customization for deploying my blogdown website

It is quites straighforward to have a working site following Alison's guide. However, you always want some customization of your own site.

I took the tips from Leslie.

###  changed the menue bar to black. I like it better than the default white.

in the `config.toml` file, change the theme:

```toml
[params]
  # Color theme.
  #   Choose from `default`, `ocean`, `forest`, `coffee`, `dark`, or `1950s`.
  color_theme = "custom"
```
and make a file called `custom.toml` in `themes/hugo-academic/data/themes/`. I copy the same `custom.toml` file from Leslie:

```toml
# Theme metadata
name = "custom"

# Is theme light or dark?
light = true

# Primary
primary = "#328cc1"
primary_light = "#328cc1"
primary_dark = "#DA2536"

# Menu
menu_primary = "#494949"
menu_text = "#fff"
menu_text_active = "#328cc1"
menu_title = "#fff"

# Backgrounds
background = "#fff"
home_section_odd = "#fff"
home_section_even = "#f7f7f7"
```

### added twitter in the Contact section.

### added a CV section.

in the `config.toml` file, add the following.

```toml
[[menu.main]]
  name = "CV"
  url = "#cv"
  weight = 6
```

Next, I created a directory called `content/cv` and a PDF version of my CV (`cv.pdf`) in the folder.

inside the `content/home` directory, create `cv.md`:

```
+++
# Custom widget.
# An example of using the custom widget to create your own homepage section.
# To create more sections, duplicate this file and edit the values below as desired.
widget = "custom"
active = true
date = 2016-04-20T00:00:00

# Note: a full width section format can be enabled by commenting out the `title` and `subtitle` with a `#`.
title = "CV"
subtitle = ""

# Order that this section will appear in.
weight = 70

+++

Find my CV in PDF [here](cv/cv.pdf).

```
Note that the path is `cv/cv.pdf`.

### Link all the PDF files of your paper in local.

Many of the papers are not open source, a link to the publisher's website does not render a PDF. I downloaded all my papers and put them in a new folder `static/files/papers`. 

open a `md` file inside `content/publication/` and add the link:

```
# Links (optional). figuring out the relative path is the key...
url_pdf = "files/papers/telemere.pdf"
```
similar to the `CV` section, figuring out the relative path is the key.

### change the header img size.

For posts, projects or publications, one can have a image on top of the writings.
I cropped some images from the paper and put them on top of the publication. However, the image will by default expand 100% of the width, and it does not look nice.

I googled and found https://github.com/gcushen/hugo-academic/issues/84

modify the css at `./themes/hugo-academic/layouts/partials/css/academic.css` in line 124 from 

```css
img,
video {
  height: auto;
  max-width: 100%
  display: block;
}
```
to 

```css
img,
video {
  height: auto;
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
  display: block;
}
```

works fine for me, but it is not the best practice as one changed the default academic theme. following https://sourcethemes.com/academic/docs/customization/#customize-style-css

>For advanced customization of the style, you can link custom CSS assets (relative to your root static/css) from your config.toml using `custom_css = ["custom.css"]`.

so, to overide Academic’s default styles. First, define `custom_css = ["override.css"]` in `config.toml`. Then I created the file `static/css/custom.css`, relative to your website root (i.e. not in the themes directory). Add your custom CSS to this file.I copy the `/themes/hugo-academic/layouts/partials/css/academic.css` to `static/css/custom.css` and modified line 124 as shown above.


