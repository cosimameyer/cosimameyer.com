---
title: New features in {overviewR}
author: ''
date: '2022-04-24'
slug: new-features-in-overviewr
category: [R-post, R]
postImage: 
featureImage: images/single-blog/overviewr-2022-release.png
featured: yes
image:
  caption: ''
  focal_point: ''
  preview_only: yes
projects: []
---

[`overviewR`](https://cosimameyer.github.io/overviewR/) (v 0.0.10) is on CRAN and comes with new features 🚀 

The package is meant to serve as a Swiss army knife for exploratory data analysis. The basic functions allow you to investigate sample coverage across different time points, missing values across your variables, and also the overlap among two data sets. 

Here are the changes in a nutshell:

{{< toc >}}

---

First we start by installing the newest version and other packages that might be helpful.

```r
# Load the newest CRAN version
install.packages("overviewR", force = TRUE)
library(overviewR) # Easily Extracting Information About Your Data
library(dplyr)
library(magrittr) # A Forward-Pipe Operator for R
```

---

## Multiple time arguments 

`overview_tab` allows you to use multiple time arguments. Here are some examples how to use the function:

Time can be a character vector containing **one** time variable (it can come in a `YYYY` or `YYYY-MM-DD` format and can either come as an integer or in the `POSIXt` format)

```r
overview_tab(dat = toydata, id = ccode, time = year)
```

```
# A tibble: 5 × 2
# Groups:   ccode [5]
  ccode time_frame                  
  <chr> <chr>                       
1 AGO   1990 - 1992                 
2 BEN   1995 - 1999                 
3 FRA   1993, 1996, 1999            
4 GBR   1991, 1993, 1995, 1997, 1999
5 RWA   1990 - 1995     
```

It can also be a list containing multiple time variables (`time = list(year = NULL, month = NULL, day = NULL)`).

```r
overview_tab(dat = toydata, 
             id = ccode, 
             time = list(year = toydata$year, 
                         month = toydata$month, 
                         day = toydata$day),
             complex_date = TRUE)
```

```
# A tibble: 5 × 2
# Groups:   ccode [5]
  ccode time_frame                                                                                                   
  <chr> <chr>                                                                                                       
1 AGO   1990-01-01, 1990-02-02,  …
2 BEN   1995-01-01, 1995-02-02,  …
3 FRA   1993-01-01, 1993-02-02,  …
4 GBR   1991-01-01, 1991-02-02,  …
5 RWA   1990-01-01 - 1990-01-12, …
```

---

## Colors in `overview_plot`

You can use colors in `overview_plot` to identify time periods. Here, we introduce a dummy variable that indicates whether the year was before 1995 or not. We use this dummy to color the time lines using the `color` argument. 

```r
# Code whether a year was before 1995
toydata %<>%
  dplyr::mutate(before = ifelse(year < 1995, 1, 0))

# Plot using the `color` argument
overview_plot(dat = toydata, id = ccode, time = year, color = before)
```

![small_image](/images/single-blog/colors.png)

---

## Change dot size in `overview_plot`

You can also change the dot size in `overview_plot`.

```r
# Plot using the `color` argument
overview_plot(dat = toydata, id = ccode, time = year, dot_size = 5)
```

![small_image](/images/single-blog/large_dots.png)

---

## Visuale cross plots with `overview_crossplot`

`overview_crosstab` has now its visualizing counter-part with `overview_crossplot`!

```r
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

![small_image](/images/single-blog/crossplot_color.png)

---

## Compare two datasets directly

Using `overview_overlap`, you can now compare the overlap in time and id variables across two data sets visually. The visualization comes with two features - a bar plot and a Venn diagram.

```r
# Subset one data set for comparison
toydata2 <- toydata %>% dplyr::filter(year > 1992)

overview_overlap(
  dat1 = toydata,
  dat2 = toydata2,
  dat1_id = ccode,
  dat2_id = ccode,
  plot_type = "bar" # This is the default
)
```

![small_image](/images/single-blog/bar_overlap.png)

```r
overview_overlap(
  dat1 = toydata,
  dat2 = toydata2,
  dat1_id = ccode,
  dat2_id = ccode,
  plot_type = "venn"
)
```

![small_image](/images/single-blog/venn_overlap.png)

---

## Use `data.table` under the hood

And, last but not least, `overview_tab` and `overview_na` now also work if you're using `data.table` objects 🥳 (Thanks to my old team @ Kienbaum for being patient enough to explain and let me learn the (not so intuitive) syntax 👩🏼‍💻)

Here's a more detailed overview of what each function can do:

|                       | Works with `data.frame` objects | Works with `data.table` | Multiple time arguments |
|-----------------------|---------------------------------|-------------------------|-----------------------------------------------------|
|     |                                |                         |                                                     |
| `overview_tab`        | &check;                               | &check;                       | &check;                                                   |
| `overview_na`         | &check;                               | &check;                       |                                                     |
| `overview_plot`       | &check;                               |                         |                                                     |
| `overview_crossplot`  | &check;                               |                         |                                                     |
| `overview_crosstab`   | &check;                               |                         |                                                     |
| `overview_heat`       | &check;                               |                         |                                                     |
| `overview_overlap`    | &check;                               |                         |                                                     |
| `overview_print`    | &check;                               |                         |                                                     |

---

## New website

And, as a bonus, we also updated our [package website](https://cosimameyer.github.io/overviewR/) using the [{preferably} theme](https://preferably.amirmasoudabdol.name/) ✨

[![small_image](/images/single-blog/overviewr-preferably.png)](https://cosimameyer.github.io/overviewR/)
