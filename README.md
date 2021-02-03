New Lesson Template Test
========================

This is a test repository of the new Carpentries lesson template based completely in R! It is very much a WORK IN PROGRESS, so mind your step :smile:
To render this lesson on your own computer, do the following:

1. Do these **ONCE**:
  - Install R: https://cloud.r-project.org/
  - Open R and install the required packages with the following commands
     ```r
     install.packages(c("remotes", "usethis"))
     remotes::install_github("carpentries/sandpaper", dependencies = TRUE)
     ```
  - In R, clone the repository to your Desktop with:
     ```r
     usethis::create_from_github("zkamvar/new-programming-with-r")
     ```
2. Inside the repository, run the following command to **build and display a preview of the lesson locally**:
   ```r
   sandpaper::build_lesson()
   ```




![Build and Deploy Website](https://github.com/swcarpentry/r-novice-inflammation/workflows/Build%20and%20Deploy%20Website/badge.svg)
[![Create a Slack Account with us](https://img.shields.io/badge/Create_Slack_Account-The_Carpentries-071159.svg)](https://swc-slack-invite.herokuapp.com/)
 [![Slack Status](https://img.shields.io/badge/Slack_Channel-swc--r--inflammation-E01563.svg)](https://swcarpentry.slack.com/messages/C9WDPCMUG)

r-novice-inflammation
=====================

[The Carpentries](https://carpentries.org/) teach foundational coding, and data science skills to
researchers worldwide. This GitHub repository generates the Software Carpentry lesson website
 "Introduction to R for non-programmers using inflammation data." The [lesson website can be viewed
here][online]. Making changes in this GitHub repository
allows us to change the content of the lesson website.

The following people are maintainers for this lesson, and are responsible for determining which
changes to incorporate into the lesson:

* [Diya Das](https://diyadas.github.io) (@diyadas)
* [Rohit Goswami](https://carpentries.org/instructors/#HaoZeke) (@haozeke)

Alumni:

* [Daniel Chen](https://software-carpentry.org/team/#chen_daniel) (@chendaniely)
* [Katrin Leinweber](https://carpentries.org/instructors/#katrinleinweber) (@katrinleinweber)


The goal of this lesson is to teach novice programmers to write modular code to
perform a data analysis. R is used to teach these skills because it is a
commonly used programming language in many scientific disciplines. However, the
emphasis is not on teaching every aspect of R, but instead on
language agnostic principles like automation with loops and encapsulation with
functions (see [Best Practices for Scientific Computing][best-practices] to
learn more). This lesson is a translation of the [Python version][py],
and is also available in [MATLAB][MATLAB].

The example used in this lesson analyzes a set of 12 data files with
inflammation data collected from a trial for a new treatment for arthritis (the
data was simulated). Learners are shown how it is better to create a function
and apply it to each of the 12 files using a loop instead of using copy-paste
to analyze the 12 files individually.

[best-practices]: http://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001745
[py]: https://github.com/swcarpentry/python-novice-inflammation
[MATLAB]: https://github.com/swcarpentry/matlab-novice-inflammation

## Contributing

We value your contributions. How to contribute to this lesson is outlined in
[CONTRIBUTING.md](https://github.com/swcarpentry/r-novice-inflammation/blob/master/CONTRIBUTING.md).
If you have questions about our contributing guidelines, please create a new issue in the [issues][]
tab and one of the maintainers will respond.

## Getting Help

Please see [https://github.com/carpentries/lesson-example](https://github.com/carpentries/lesson-example)
for instructions on formatting, building, and submitting lessons,
or run `make` in this directory for a list of helpful commands.

If you have questions or proposals, please send them to the [r-discuss][] mailing list.

[design]: https://github.com/carpentries/lesson-example/blob/master/DESIGN.md
[issues]: https://github.com/swcarpentry/r-novice-inflammation/issues
[knitr]: https://cran.r-project.org/package=knitr
[online]: https://swcarpentry.github.io/r-novice-inflammation/
[pro-git]: http://git-scm.com/book/en/v2/GitHub-Contributing-to-a-Project
[r-discuss]: https://carpentries.topicbox.com/groups/discuss