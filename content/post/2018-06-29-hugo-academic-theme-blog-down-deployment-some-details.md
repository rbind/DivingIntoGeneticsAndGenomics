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

in the `config.toml` file, add my twitter 

```toml
[params]
  # Some other stuff...

  email = "lmyint@macalester.edu"
  address = ""
  office_hours = ""
  phone = ""
  skype = ""
  telegram = ""
  twitter = "tangming2005"
```

I also had to update `themes/hugo-academic/layouts/partials/widgets/contact.html`. I duplicated the section of the HTML that displays the e-mail address parameter 

```html
{{ with $.Site.Params.email }}
<li>
  <i class="fa-li fa fa-envelope fa-2x" aria-hidden="true"></i>
  <span id="person-email" itemprop="email">
  {{- if $autolink }}<a href="mailto:{{ . }}">{{ . }}</a>{{ else }}{{ . }}{{ end -}}
  </span>
</li>
{{ end }}
```

and changed to twitter:

```html
{{ with $.Site.Params.twitter }}
<li>
  <i class="fa-li fa fa-twitter fa-2x" aria-hidden="true"></i>
  <span>
  <a href="https://twitter.com/{{ . }}">{{ . }}</a>
  </span>
</li>
{{ end }}
```

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

**The above three steps followed exactly of Leslie.**

### inactivated the selected publication widget

I found the `selected_publications` widget is a bit clunky, so I inactivated it.
change `content/home/publications_selected.md`

from 
```
+++
# Selected Publications widget.
# This widget displays publications from `content/publication/` which have
# `selected = true` in their `+++` front matter.
widget = "publications_selected"
active = true
```
to

```
+++
# Selected Publications widget.
# This widget displays publications from `content/publication/` which have
# `selected = true` in their `+++` front matter.
widget = "publications_selected"
active = false
```

link the menue bar to `publications`.

in the `config.toml`, change

```toml
[[menu.main]]
  name = "Publications"
  url = "#publications_selected"
  weight = 2
```

to 

```toml
[[menu.main]]
  name = "Publications"
  url = "#publications"
  weight = 2
```
### add the talks & teachings to menue 

I do not like the `teaching` custom widget. one can only list the courses there. I know it is called custom so one can change it, but I do not know how.

The `talks` widget is what I need. I will put both talks and teachings there. I can then click the talks and teachings to get more details.

First, inactivate the `teaching` widget.

modify `content/home/teaching.md`

from 
```
+++
# Custom widget.
# An example of using the custom widget to create your own homepage section.
# To create more sections, duplicate this file and edit the values below as desired.
widget = "custom"
active = true
date = 2016-04-20T00:00:00
```
to

```
+++
# Custom widget.
# An example of using the custom widget to create your own homepage section.
# To create more sections, duplicate this file and edit the values below as desired.
widget = "custom"
active = false
date = 2016-04-20T00:00:00
```
The wideget will be gone in the home page. 

in the `config.toml` file, add the following and comment out the `teaching`

```toml
[[menu.main]]
  name = "Talks & Teachings"
  url = "#talks"
  weight = 6
  
#[[menu.main]]
#  name = "Teaching"
#  url = "#teaching"
#  weight = 6
```

change the `content/home/talks.md` file

from

```
+++
# Recent and Upcoming Talks widget.
widget = "talks"
active = true
date = 2016-04-20T00:00:00

title = "Recent & Upcoming Talks"
subtitle = ""
```
to 

```
+++
# Recent and Upcoming Talks widget.
widget = "talks"
active = true
date = 2016-04-20T00:00:00

title = "Talks & Teachings"
subtitle = ""

```

### added google analytics to track website traffic

follow https://support.google.com/analytics/answer/1042508
log into https://analytics.google.com/analytics/web
click `ADMIN` in the left bottom corner.
create a new property, type in the website name: `divingintogeneticsandgenomics.rbind.io`.

I got a track ID: `UA-84019592-2`

in the `config.toml` file:

```toml
# Enable analytics by entering your Google Analytics tracking ID
googleAnalytics = "UA-84019592-2"
```
Now, you can check the traffic reports in real-time.


### Link all the PDF files of your papers in local.

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

read more on the Hugo academic page for more tutorials https://sourcethemes.com/academic/docs/writing-markdown-latex/#links

