---
title: Migrating from Jekyll to {sandpaper}
---

The migration process is still being written, so here are my notes for this repo:

> BIG NOTE: At some point, {pegboard} started failing its tests in part due to
> it being too specific for its searches. 

Here's what I did in R:

```r
usethis::create_from_github("swcarpentry/r-novice-gapminder", open = FALSE)
sandpaper::create_lesson("new-programming-with-r")
usethis::use_github()
```

I then went to github and manually changed the branch from master to main.

Back in R in the project:

```r
# Read the episodes into R
src <- "../r-novice-gapminder"
rni <- pegboard::Lesson$new(src)

# Script to transform the episodes via pegboard with traces
transform <- function(e, out = here::here()) { 
  outdir <- fs::path(out, "episodes/") 
  message(glue::glue("{e$name}: converting blockquotes to fenced div")) 
  e$unblock() 
  message(glue::glue("{e$name}: removing Jekyll syntax")) 
  e$use_sandpaper() 
  message(glue::glue("{e$name}: moving yaml items to body")) 
  e$move_questions() 
  e$move_objectives() 
  e$move_keypoints() 
  message(glue::glue("{e$name}: writing output")) 
  e$write(outdir, format = "Rmd", edit = FALSE) 
}

# Transform and write to our episodes folder
purrr::walk(rni$episodes, ~try(transform(.x)))

# Transform reference
ref <- Episode$new(fs::path(src, "reference.md"))
ref$use_sandpaper()$write("learners/")

# Transform setup 
setup <- Episode$new(fs::path(src, "setup.md"))
setup$use_sandpaper()$write("learners/")

# Transform index 
index <- Episode$new(fs::path(src, "index.md"))
index$use_sandpaper()$write(here())

# Copy README
fs::file_copy(fs::path(src, "README.md"), "README.md")

# Copy Figures (N.B. this was one of the pain points for the Jekyll lessons: figures lived above the RMarkdown documents)
fs::file_copy(fs::dir_ls(fs::path(src, "fig"), glob = "*svg"), here("episodes", "fig"))

# Copy the data file to the data folder
fs::dir_copy(fs::path(src, "_episodes_rmd/data/", "episodes/"))
rscripts <- fs::dir_ls(fs::path(src, "r-novice-gapminder/_episodes_rmd/"), glob = "*.R") 
fs::file_copy(rscripts, "episodes/", overwrite = TRUE)
fs::file_delete("episodes/01-introduction.Rmd")
fs::file_delete("learners/Setup.md")
```

After this was done, I added the `.github/` configuration files from https://github.com/zkamvar/testme. I then ran `sandpaper::build_lesson()` to test that it worked (there's something going on with the formatting, but nothing need be changed on this end). 

I then used the following steps to update the config file


```r
library("sandpaper")
# set the order of the learner files
learn <- get_learners()
set_learners(order = learn, write = TRUE)

# set the order of the episode files
ep <- get_episodes()
set_episodes(order = ep, write = TRUE)
```

I ran into [a bug with {sandpaper}](https://github.com/carpentries/sandpaper/issues/53) that will be fixed soon, but it luckily does not affect the results.

I committed all the bits and pushed the results to github where everything was able to install (after I made sure the dependencies were taken care of). 

At this moment, I am finishing up the notes for the migration and setting up the configuration file.
