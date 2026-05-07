---
title: Helpful R Commands
author: Cosima Meyer
date: '2020-09-22'
slug: helpful-r-commands
category: [R]
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2020-12-01T17:45:42+02:00'
featured: yes
image:
  caption: ''
  focal_point: ''
  preview_only: yes
projects: []
postImage: images/single-blog/helpful.png
featureImage: images/single-blog/helpful.png
---

![very_small_image](/images/single-blog/helpful.png)

<!--<img style="float: left;" src="featured.png" alt="Drawing of a light bulb" width="100"/>-->

This blog post will be regularly updated and is a "brain dump" for all the cool commands that I came across. I hope it helps me to remember them and can also serve others as a repository 👩🏼‍💻 

### Frequency Tables

A next step in our `overviewR` package will be the implementation of a good-looking and easy to access frequency tables and plots (something that I use all the time). Until we have a ready-to-print version in `overviewR`, here is a snippet of `tidyverse` code that can easily generate them :)

```
# Generate some artificial data:
data <- data.frame(y = c(rep(1, 250), rep(0, 50)))

# Generate the frequency table:
data %>% 
  count(y) %>% 
  mutate(prop = round(n/sum(n), 2))
````

Which returns:

```
#   y   n prop
# 1 0  50 0.17
# 2 1 250 0.83
```

### Save an R File With an Automated Date

Let's assume you want to have an individual copy of your file that has a time stamp on it. This is how you could do it:

```
# Generate the path and file name
name <- paste0("/path/to/your/data-", Sys.Date(), ".RData")

# Save it
save(data_set, file = name)
```

Just replace the path where you want to store your file and add the object that contains your `data_set`. The `name` object could look something like this: `../data/conflict-2020-12-01.RData`. R automatically replaced `Sys.Date()` with today's date and you get your time stamp 🙂 This way I make sure to never overwrite previously saved data sets.

### Save Your `xaringan` Slides as a PDF!

I love [`xaringan`](https://github.com/yihui/xaringan) slides -- they are beautiful, easy to create and have many more advantages. The only caveat, if it counts as one, is that it's difficult to save them as a good-looking PDF file. That's what I thought at least until [Theresa Gessler](https://www.ipz.uzh.ch/en/institut/mitarbeitende/staff/tgessler.html) gave me the most straightforward solution to do this! 👩‍💻

Simply type the following lines of code in your console, add the path to your `.Rmd` file as well as the name of your `.Rmd` file and knit your PDF. Your PDF file is stored in the same folder where your .Rmd file is located.

```r
# Load the package
remotes::install_github('rstudio/pagedown')

# Add the path and the name of your .Rmd file
pagedown::chrome_print("path/your-talk.Rmd")
```

### Split Strings Into Single Columns by a Pre-Defined Separator

```r
library(splitstackshape) # Stack and Reshape Datasets After 
                         # Splitting Concatenated Values

# Load fake data
data <- data.frame(
  letter = c("A, B, C", "B, D", "D, E, A", "A"),
  year = c(1990, 2000, 2010, 2020)
)
```

Which looks like this:

|letter  | year|
|:-------|----:|
|A, B, C | 1990|
|B, D    | 2000|
|D, E, A | 2010|
|A       | 2020|

If we now want to split the letter column into its separate letters (each letter gets a new column), we can use the `cSplit` command from the `splitstackshape` package.

```r
data <- splitstackshape::cSplit(data, "letter", sep = ",")
```

The separator indicates that we want to split it column-wise by the comma. This leads us to this data set:

| year|letter_1 |letter_2 |letter_3 |
|----:|:--------|:--------|:--------|
| 1990|A        |B        |C        |
| 2000|B        |D        |NA       |
| 2010|D        |E        |A        |
| 2020|A        |NA       |NA       |

If there was no letter present, the package automatically includes `NA`.

An alternative is the `separate` command from the `tidyr` package. It allows you to manually select the names of your splits. Applying it to the example above, it looks like this:

```r
library(tidyr) # Tidy Messy Data

data <- separate(data, letter, into = c("letter","moreletter", "andmore"), sep = ",", extra = "merge")
```
Which yields the following output:

|letter |moreletter |andmore | year|
|:------|:----------|:-------|----:|
|A      |B          |C       | 1990|
|B      |D          |NA      | 2000|
|D      |E          |A       | 2010|
|A      |NA         |NA      | 2020|

Again, if you have no letters present, the `separate` command also returns `NA`.

### Get Root Overview of Your Working Directory

```r
library(fs) # Cross-Platform File System Operations Based on 'libuv'

wd <- getwd()

# Create directory tree
fs::dir_tree(path = wd, recurse = TRUE)
```

Results in something similar such as:

```
├── README.md
├── code
│   ├── 1_data_preparation.Rmd
│   ├── 2_merging.Rmd
│   ├── 3_analysis.Rmd
│   ├── 3_descriptives.Rmd
│   ├── 4_visualization.Rmd
│   └── helper.R
├── data
│   ├── processed
│   └── raw
├── figures
└── give-it-a-name.Rproj
```
