# REMEMBER to restart R after you modify and save this file!

# First, execute the global .Rprofile if it exists. You may configure blogdown
# options there, too, so they apply to any blogdown projects. Feel free to
# ignore this part if it sounds too complicated to you.
if (file.exists("~/.Rprofile")) {
  base::sys.source("~/.Rprofile", envir = environment())
}

# Now set options to customize the behavior of blogdown for this project. Below
# are a few sample options; for more options, see
# https://bookdown.org/yihui/blogdown/global-options.html
options(
  # to automatically serve the site on RStudio startup, set this option to TRUE
  blogdown.serve_site.startup = FALSE,
  # to disable knitting Rmd files on save, set this option to FALSE
  blogdown.knit.on_save = TRUE,
  # build .Rmd to .html (via Pandoc); to build to Markdown, set this option to 'markdown'
  blogdown.method = 'html'
)

# fix Hugo version
options(blogdown.hugo.version = "0.42")

# remove.packages("blogdown") only 0.9 works for create a new post add-in
# devtools::install_version("blogdown", version = "0.9", repos = "http://cran.us.r-project.org") 
# remove.packages("blogdown") # only 1.7 works for serving the sites..
# devtools::install_version("blogdown", version = "1.7", repos = "http://cran.us.r-project.org")

Sys.setenv(RETICULATE_PYTHON = "/Users/tommytang/Library/r-miniconda/envs/r-reticulate/bin/python")
