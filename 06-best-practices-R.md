---
sandpaper-digest: 276205a2aa30ce125210bcfcc1203ee7
sandpaper-source: /Users/runner/work/new-programming-with-r/new-programming-with-r/episodes/06-best-practices-R.Rmd

title: Best Practices for Writing R Code
teaching: 10
exercises: 0
source: Rmd
---



::::::::::::::::::::::::::::::::::::::: objectives

## Objectives

- Describe changes made to code for future reference.
- Understand the importance about requirements and dependencies for your code.
- Know when to use setwd().
- To identify and segregate distinct components in your code using # or #-.
- Additional best practice recommendations.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

## Questions

- How can I write R code that other people can understand and use?

::::::::::::::::::::::::::::::::::::::::::::::::::

# Keep track of who wrote your code and its intended purpose

Starting your code with an annotated description of what the code does when it is run will help you when you have to look at or change it in the future. Just one or two lines at the beginning of the file can save you or someone else a lot of time and effort when trying to understand what a particular script does.


```r
# This is code to replicate the analyses and figures from my 2014 Science
# paper. Code developed by Sarah Supp, Tracy Teal, and Jon Borelli
```

# Be explicit about the requirements and dependencies of your code

Loading all of the packages that will be necessary to run your code (using `library`) is a nice way of indicating which packages are necessary to run your code. It can be frustrating to make it two-thirds of the way through a long-running script only to find out that a dependency hasn't been installed.


```r
library(ggplot2)
library(reshape)
library(vegan)
```

Another way you can be explicit about the requirements of your code and improve it's reproducibility is to limit the "hard-coding" of the input and output files for your script. If your code will read in data from a file, define a variable early in your code that stores the path to that file. For example


```r
input_file <- "data/data.csv" 
output_file <- "data/results.csv"

# read input
input_data <- read.csv(input_file)
# get number of samples in data
sample_number <- nrow(input_data)
# generate results
results <- some_other_function(input_file, sample_number)
# write results
write.table(results, output_file)
```

is preferable to


```r
# check
input_data <- read.csv("data/data.csv")
# get number of samples in data
sample_number <- nrow(input_data)
# generate results
results <- some_other_function("data/data.csv", sample_number)
# write results
write.table("data/results.csv", output_file)
```

It is also worth considering what the working directory is. If the working directory must change, it is best to do that at the beginning of the script.

:::::::::::::::::::::::::::::::::::::::::  callout

## Be careful when using `setwd()`

One should exercise caution when using `setwd()`. Changing directories in a script file can limit reproducibility:

- `setwd()` will return an error if the directory to which you're trying to change doesn't exist or if the user doesn't have the correct permissions to access that directory. This becomes a problem when sharing scripts between users who have organized their directories differently.
- If/when your script terminates with an error, you might leave the user in a different directory than the one they started in, and if they then call the script again, this will cause further problems. If you must use `setwd()`, it is best to put it at the top of the script to avoid these problems.
  The following error message indicates that R has failed to set the working directory you specified:

```
Error in setwd("~/path/to/working/directory") : cannot change working directory
```

It is best practice to have the user running the script begin in a consistent directory on their machine and then use relative file paths from that directory to access files (see below).


::::::::::::::::::::::::::::::::::::::::::::::::::

# Identify and segregate distinct components in your code

It's easy to annotate and mark your code using `#` or `#-` to set off sections of your code and to make finding specific parts of your code easier. For example, it's often helpful when writing code to separate the function definitions. If you create only one or a few custom functions in your script, put them toward the top of your code. If you have written many functions, put them all in their own .R file and then `source` those files. `source` will define all of these functions so that your code can make use of them as needed.


```r
source("my_genius_fxns.R")
```

# Other ideas

1. Use a consistent style within your code. For example, name all matrices something ending in `_mat`. Consistency makes code easier to read and problems easier to spot.

2. Keep your code in bite-sized chunks. If a single function or loop gets too long, consider looking for ways to break it into smaller pieces.

3. Don't repeat yourself--automate! If you are repeating the same code over and over, use a loop or a function to repeat that code for you. Needless repetition doesn't just waste time--it also increases the likelihood you'll make a costly mistake!

4. Keep all of your source files for a project in the same directory, then use relative paths as necessary to access them. For example, use


```r
dat <- read.csv(file = "files/dataset-2013-01.csv", header = TRUE)
```

rather than:


```r
dat <- read.csv(file = "/Users/Karthik/Documents/sannic-project/files/dataset-2013-01.csv", header = TRUE)
```

{:start="5"}
5\. R can run into memory issues. It is a common problem to run out of memory after running R scripts for a long time. To inspect the objects in your current R environment, you can list the objects, search current packages, and remove objects that are currently not in use. A good practice when running long lines of computationally intensive code is to remove temporary objects after they have served their purpose. However, sometimes, R will not clean up unused memory for a while after you delete objects. You can force R to tidy up its memory by using `gc()`.


```r
# Sample dataset of 1000 rows
interim_object <- data.frame(rep(1:100, 10),
                             rep(101:200, 10),
                             rep(201:300, 10))
object.size(interim_object) # Reports the memory size allocated to the object
rm("interim_object") # Removes only the object itself and not necessarily the memory allotted to it
gc() # Force R to release memory it is no longer using
ls() # Lists all the objects in your current workspace
rm(list = ls()) # If you want to delete all the objects in the workspace and start with a clean slate
```

{:start="6"}
6\. Don't save a session history (the default option in R, when it asks if you want an `RData` file). Instead, start in a clean environment so that older objects don't remain in your environment any longer than they need to. If that happens, it can lead to unexpected results.

7. Wherever possible, keep track of `sessionInfo()` somewhere in your project folder. Session information is invaluable because it captures all of the packages used in the current project. If a newer version of a package changes the way a function behaves, you can always go back and reinstall the version that worked (Note: At least on CRAN, all older versions of packages are permanently archived).

8. Collaborate. Grab a buddy and practice "code review". Review is used for preparing experiments and manuscripts; why not use it for code as well? Our code is also a major scientific achievement and the product of lots of hard work!

9. Develop your code using version control and frequent updates! You can find lessons about version control on [software-carpentry.org/lessons][swc-lessons].

:::::::::::::::::::::::::::::::::::::::  challenge

## Best Practice

1. What other suggestions do you have for coding best practices?
2. What are some specific ways we could restructure the code we worked on today to make it easier for a new user to read? Discuss with your neighbor.
3. Make two new R scripts called `inflammation.R` and `inflammation_fxns.R`.
   Copy and paste code into each script so that `inflammation.R` "does stuff" and `inflammation_fxns.R` holds all of your functions.
   **Hint**: you will need to add `source` to one of the files.

:::::::::::::::  solution

## Solution


```bash
cat inflammation.R
```

```{.output}
# This code runs the inflammation data analysis.

source("inflammation_fxns.R")
analyze_all("inflammation.*csv")
```


```bash
cat inflammation_fxns.R
```

```{.output}
# This is code for functions used in our inflammation data analysis.

analyze <- function(filename, output = NULL) {
  # Plots the average, min, and max inflammation over time.
  # Input:
  #    filename: character string of a csv file
  #    output: character string of pdf file for saving
  if (!is.null(output)) {
    pdf(output)
  }
  dat <- read.csv(file = filename, header = FALSE)
  avg_day_inflammation <- apply(dat, 2, mean)
  plot(avg_day_inflammation)
  max_day_inflammation <- apply(dat, 2, max)
  plot(max_day_inflammation)
  min_day_inflammation <- apply(dat, 2, min)
  plot(min_day_inflammation)
  if (!is.null(output)) {
    dev.off()
  }
}

analyze_all <- function(pattern) {
  # Directory name containing the data
  data_dir <- "data"
  # Directory name for results
  results_dir <- "results"
  # Runs the function analyze for each file in the current working directory
  # that contains the given pattern.
  filenames <- list.files(path = data_dir, pattern = pattern)
  for (f in filenames) {
    pdf_name <- file.path(results_dir, sub("csv", "pdf", f))
    analyze(file.path(data_dir, f), output = pdf_name)
  }
}
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

{% include links.md %}

:::::::::::::::::::::::::::::::::::::::: keypoints

## Keypoints

- Start each program with a description of what it does.
- Then load all required packages.
- Consider what working directory you are in when sourcing a script.
- Use comments to mark off sections of code.
- Put function definitions at the top of your file, or in a separate file if there are many.
- Name and style code consistently.
- Break code into small, discrete pieces.
- Factor out common operations rather than repeating them.
- Keep all of the source files for a project in one directory and use relative paths to access them.
- Keep track of the memory used by your program.
- Always start with a clean environment instead of saving the workspace.
- Keep track of session information in your project folder.
- Have someone else review your code.
- Use version control.

::::::::::::::::::::::::::::::::::::::::::::::::::


