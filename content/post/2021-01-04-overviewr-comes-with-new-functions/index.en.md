---
title: overviewR comes with new functions
author: Cosima Meyer
date: '2021-01-04'
slug: overviewr-comes-with-new-functions
category: [R-post, R, package, overviewR]
tags: []
subtitle: ''
summary: ''
authors: []
featured: yes
image:
  caption: ''
  focal_point: ''
  preview_only: yes
projects: []
postImage: images/single-blog/overviewr-new-functions.png
featureImage: images/single-blog/overviewr-new-functions.png

---

New year, better[`overviewR`](https://cosimameyer.github.io/overviewR/)! It comes with some brand new functions and extensions that make visualizing your sample more beautiful (and easy) than ever! 

Most of the functions are currently only available in the development version that can be downloaded from GitHub but we will submit them to CRAN shortly.

So, what are the major changes and how do they work?

### `overview_plot`

Our old (and beloved) function `overview_plot` git better. It allows you to add colors now!

Here's a minimal working example:

```r
# Load the GitHub version
library(devtools) # Tools to Make Developing R Packages Easier
devtools::install_github("cosimameyer/overviewR")
library(overviewR) # Easily Extracting Information About Your Data
library(magrittr) # A Forward-Pipe Operator for R

# Load the data
data(toydata)

# Code whether a year was before 1995
toydata %<>%
  dplyr::mutate(before = ifelse(year < 1995, 1, 0))

# Plot using the `color` argument
overview_plot(dat = toydata, id = ccode, time = year, color = before)
```

![smaller_image](/images/single-blog/colored_plot.png)

### `overview_crossplot`

Remember how your cross table can look like?

![smaller_image](/images/single-blog/ex3.png)

That's already nice but we could do it more visually, right? That's why we added the function `overview_crossplot`. And that's how it looks like:

```r
# Load the GitHub version
library(devtools) # Tools to Make Developing R Packages Easier
devtools::install_github("cosimameyer/overviewR")
library(overviewR) # Easily Extracting Information About Your Data

# Load the data
data(toydata)

overview_crossplot(
  toydata,
  id = ccode,
  time = year,
  cond1 = gdp,
  cond2 = population,
  threshold1 = 25000,
  threshold2 = 27000,
  color = TRUE,
  label = TRUE
  )
```

![smaller_image](/images/single-blog/crossplot.png)

But that's not all! We also added small changes to some functions.

### `overview_print`
It can now take `label` and `fontsize` arguments to further customize your LaTeX output.

```r
# Load the GitHub version
library(devtools) # Tools to Make Developing R Packages Easier
devtools::install_github("cosimameyer/overviewR")
library(overviewR) # Easily Extracting Information About Your Data

# Load the data
data(toydata)

# Generate the table with the data
output_table <- overview_tab(dat = toydata, id = ccode, time = year)

# Now we convert it to a LaTeX output
overview_print(obj = output_table,
               fontsize = "scriptsize",
               label = "tab:overview")
```


Which leads to this output with our pre-set fontsize and label:

```
% Overview table generated in R version 4.0.2 (2020-06-22) using overviewR 
% Table created on 2021-01-04
\begin{table}[ht] 
 \centering 
 \caption{Time and scope of the sample} 
\label{tab:overview} 
\scriptsize
\begin{tabular}{ll} 
 \hline 
Sample & Time frame \\ 
\hline 
 AGO & 1990 - 1992 \\ 
 BEN & 1995 - 1999 \\ 
 FRA & 1993, 1996, 1999 \\ 
 GBR & 1991, 1993, 1995, 1997, 1999 \\ 
 RWA & 1990 - 1995 \\ 
 \hline 
 \end{tabular} 
 \end{table} 
```

And last but not least some not so new news:

Did you know that the beauty of `overviewR` is also that it is based on the `tidyverse` and `ggplot2`? That means you can basically pipe through the commands and also adjust the plots to your needs!

```r
# Load the GitHub version
library(devtools) # Tools to Make Developing R Packages Easier
devtools::install_github("cosimameyer/overviewR")
library(overviewR) # Easily Extracting Information About Your Data
library(magrittr) # A Forward-Pipe Operator for R
library(dplyr) # A Grammar of Data Manipulation

# Load the data
data(toydata)

# Code whether a year was before 1995
toydata %<>%
  dplyr::mutate(before = ifelse(year < 1995, 1, 0))


toydata %>% 
  dplyr::filter(year >= 1991) %>% 
  overview_plot(id = ccode, time = year, color = before) +
  scale_color_grey()
```

![smaller_image](/images/single-blog/colored_plot_grey.png)


These advancements were heavily inspired by great input that we received during the mini-conference of the Zurich Summer School for Women in Political Methodology 2020. 

As everything, also `overviewR` grows best with support. So, if you have something in mind that we did not implement yet or if you are using `overviewR`, please reach out to us and let us know! 