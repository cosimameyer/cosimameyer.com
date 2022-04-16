---
title: New features in {overviewR}
author: ''
date: '2022-04-30'
slug: new-features-in-overviewr
categories: [R-post, R]
postImage: 
featureImage: images/single-blog/overviewr-2022-release.png
featured: yes
image:
  caption: ''
  focal_point: ''
  preview_only: yes
projects: []
---

[`overviewR`](https://cosimameyer.github.io/overviewR/) comes with new features 🚀 

The package is meant to serve as a Swiss army knife for exploratory data analysis. The basic functions allow you to investigate sample coverage across different time points, missing values across your variables, and also the overlap among two data sets. 

Here are the changes in a nutshell:

```r
# Load the GitHub version
library(devtools) # Tools to Make Developing R Packages Easier
devtools::install_github("cosimameyer/overviewR")
library(overviewR) # Easily Extracting Information About Your Data
library(dplyr)
library(magrittr) # A Forward-Pipe Operator for R
```

---

1. `overview_tab` allows you to use multiple time arguments. Here are some examples how to use the function:

Time can be a character vector containing **one** time variable (it can come in a `YYYY` or `YYYY-MM-DD` format and can either come as an integer or in the `POSIXt` format)

```r
overview_tab(dat = toydata, id = ccode, time = year)
```

<!-- ```{r, eval=TRUE, echo=FALSE} -->
<!-- # Load the GitHub version -->
<!-- library(devtools) # Tools to Make Developing R Packages Easier -->
<!-- devtools::install_github("cosimameyer/overviewR") -->
<!-- library(overviewR) # Easily Extracting Information About Your Data -->
<!-- library(dplyr) -->
<!-- library(magrittr) # A Forward-Pipe Operator for R -->
<!-- overview_tab(dat = toydata, id = ccode, time = year) -->
<!-- ``` -->

It can also be a list containing multiple time variables (`time = list(year = NULL, month = NULL, day = NULL)`).

```r
overview_tab(dat = toydata, 
             id = ccode, 
             time = list(year = toydata$year, 
                         month = toydata$month, 
                         day = toydata$day))
```

<!-- ```{r, eval=FALSE, echo=FALSE} -->
<!-- # Load the GitHub version -->
<!-- library(devtools) # Tools to Make Developing R Packages Easier -->
<!-- devtools::install_github("cosimameyer/overviewR") -->
<!-- library(overviewR) # Easily Extracting Information About Your Data -->
<!-- library(dplyr) -->
<!-- library(magrittr) # A Forward-Pipe Operator for R -->
<!-- overview_tab(dat = toydata, id = ccode, time = list(year = toydata$year, month = toydata$month, day = toydata$day)) -->
<!-- ``` -->

---

2. You can use colors in `overview_plot` to identify time periods. Here, we introduce a dummy variable that indicates whether the year was before 1995 or not. We use this dummy to color the time lines using the `color` argument. 

```r
# Code whether a year was before 1995
toydata %<>%
  dplyr::mutate(before = ifelse(year < 1995, 1, 0))

# Plot using the `color` argument
overview_plot(dat = toydata, id = ccode, time = year, color = before)
```

<!-- ```{r, eval=TRUE, echo=FALSE} -->
<!-- # Load the GitHub version -->
<!-- library(devtools) # Tools to Make Developing R Packages Easier -->
<!-- devtools::install_github("cosimameyer/overviewR") -->
<!-- library(overviewR) # Easily Extracting Information About Your Data -->
<!-- library(dplyr) -->
<!-- library(magrittr) # A Forward-Pipe Operator for R -->

<!-- # Code whether a year was before 1995 -->
<!-- toydata %<>% -->
<!--   dplyr::mutate(before = ifelse(year < 1995, 1, 0)) -->

<!-- # Plot using the `color` argument -->
<!-- overview_plot(dat = toydata, id = ccode, time = year, color = before) -->
<!-- ``` -->

---

3. You can also change the dot size in `overview_plot`.

```r
# Plot using the `color` argument
overview_plot(dat = toydata, id = ccode, time = year, dot_size = 5)
```

<!-- ```{r, eval=TRUE, echo=FALSE} -->
<!-- # Load the GitHub version -->
<!-- library(devtools) # Tools to Make Developing R Packages Easier -->
<!-- devtools::install_github("cosimameyer/overviewR") -->
<!-- library(overviewR) # Easily Extracting Information About Your Data -->
<!-- library(dplyr) -->
<!-- library(magrittr) # A Forward-Pipe Operator for R -->

<!-- overview_plot(dat = toydata, id = ccode, time = year, dot_size = 5) -->

<!-- ``` -->

---

4. `overview_crosstab` has now its visualizing counter-part with `overview_crossplot`!

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

<!-- ```{r, eval=TRUE, echo=FALSE} -->
<!-- # Load the GitHub version -->
<!-- library(devtools) # Tools to Make Developing R Packages Easier -->
<!-- devtools::install_github("cosimameyer/overviewR") -->
<!-- library(overviewR) # Easily Extracting Information About Your Data -->
<!-- library(dplyr) -->
<!-- library(magrittr) # A Forward-Pipe Operator for R -->

<!-- overview_crossplot( -->
<!--   toydata, -->
<!--   id = ccode, -->
<!--   time = year, -->
<!--   cond1 = gdp, -->
<!--   cond2 = population, -->
<!--   threshold1 = 25000, -->
<!--   threshold2 = 27000, -->
<!--   color = TRUE, -->
<!--   label = TRUE -->
<!-- ) -->
<!-- ``` -->

---

5. Using `overview_overlap`, you can now compare the overlap in time and id variables across two data sets visually. The visualization comes with two features - a bar plot and a Venn diagram.

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

<!-- ```{r, eval=TRUE, echo=FALSE} -->
<!-- # Load the GitHub version -->
<!-- library(devtools) # Tools to Make Developing R Packages Easier -->
<!-- devtools::install_github("cosimameyer/overviewR") -->
<!-- library(overviewR) # Easily Extracting Information About Your Data -->
<!-- library(dplyr) -->
<!-- library(magrittr) # A Forward-Pipe Operator for R -->

<!-- # Subset one data set for comparison -->
<!-- toydata2 <- toydata %>% dplyr::filter(year > 1992) -->

<!-- overview_overlap( -->
<!--   dat1 = toydata, -->
<!--   dat2 = toydata2, -->
<!--   dat1_id = ccode, -->
<!--   dat2_id = ccode, -->
<!--   plot_type = "bar" # This is the default -->
<!-- ) -->
<!-- ``` -->

```r
overview_overlap(
  dat1 = toydata,
  dat2 = toydata2,
  dat1_id = ccode,
  dat2_id = ccode,
  plot_type = "venn"
)
```

<!-- ```{r, eval=TRUE, echo=FALSE} -->
<!-- # Load the GitHub version -->
<!-- library(devtools) # Tools to Make Developing R Packages Easier -->
<!-- devtools::install_github("cosimameyer/overviewR") -->
<!-- library(overviewR) # Easily Extracting Information About Your Data -->
<!-- library(dplyr) -->
<!-- library(magrittr) # A Forward-Pipe Operator for R -->

<!-- overview_overlap( -->
<!--   dat1 = toydata, -->
<!--   dat2 = toydata2, -->
<!--   dat1_id = ccode, -->
<!--   dat2_id = ccode, -->
<!--   plot_type = "venn" -->
<!-- ) -->
<!-- ``` -->

