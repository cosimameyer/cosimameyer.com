---
title: 'Update of overviewR with new functions!'
summary: ''
tags: []
categories: [R-post, R, package, overviewR]
date: "2020-08-24"
featured: false
draft: false
postImage: images/single-blog/overviewR-new-release.png
featureImage: images/single-blog/overviewR-new-release.png
projects: []
---

We have updated (and extended) [overviewR](https://cosimameyer.github.io/overviewR/) with three new functions:

- `overview_plot`
- `overview_heat`
- `overview_na`

You can also access a detailed overview of all functions in the CheatSheet: 
[![small_image](/images/single-blog/cheatsheet.png)](https://github.com/cosimameyer/overviewR/blob/master/man/figures/CheatSheet_overviewR.pdf)

### ```overview_plot```

`overview_plot` illustrates the information that is generated in `overview_table` in a ggplot graphic. All scope objects (e.g., countries) are listed on the y-axis where horizontal lines indicate the coverage across the entire time frame of the data (x-axis). This helps to spot gaps in the data for specific scope objects and outlines at what time point they occur. 

```r
data(toydata)
overview_plot(dat = toydata, id = ccode, time = year)
```

![smaller_image](/images/single-blog/unnamed-chunk-23-1.png)

<!--<p align="center">
<img src='/images/single-blog/unnamed-chunk-23-1.png' height="300"/>
</p>-->

### ```overview_heat```

`overview_heat` takes a closer look at the time and scope conditions by visualizing the data coverage for each time and scope combination in a ggplot heat map. This function is best explained using an example. Suppose you have a dataset with monthly data for different countries and want to know if data is available for each country in every month. `overview_heat` intuitively does this by plotting a heat map where each cell indicates the coverage for that specific combination of time and scope (e,g., country-year). As illustrated below, the darker the cell is, the more coverage it has. The plot also indicates the relative or absolute coverage of each cell. For instance, Angola ("AGO") in 1991 shows the coverage of 75%. This means that of all potential 12 months of coverage (12 months for one year), only 9 are covered.

```r
toydata_red <- toydata[-sample(seq_len(nrow(toydata)), 64), ]
```

```r
overview_heat(toydata_red,
                ccode,
                year,
                perc = TRUE,
                exp_total = 12)
```

![smaller_image](/images/single-blog/unnamed-chunk-25-1.png)

<!--<p align="center">
<img src='images/single-blog/unnamed-chunk-25-1.png' height="300"/>
</p>-->

### ```overview_na```

`overview_na` is a simple function that provides information about the content of all variables in your data, not only the time and scope conditions. It returns a horizontal ggplot bar plot that indicates the amount of missing data (NAs) for each variable (on the y-axis). You can choose whether to display the relative amount of NAs for each variable in percentage (the default) or the total number of NAs. 

```r
toydata_with_na <- toydata %>%
  dplyr::mutate(year = ifelse(year < 1992, NA, year),
                month = ifelse(month %in% c("Jan", "Jun", "Aug"), NA, month),
                gdp = ifelse(gdp < 20000, NA, gdp))
```

```r
overview_na(toydata_with_na)
```

![smaller_image](/images/single-blog/unnamed-chunk-27-1.png)

<!--<p align="center">
<img src='/images/single-blog/unnamed-chunk-27-1.png' height="300"/>
</p>-->

```r
overview_na(toydata_with_na, perc = FALSE)
```

![smaller_image](/images/single-blog/unnamed-chunk-28-1.png)

<!--<p align="center">
<img src='/images/single-blog/unnamed-chunk-28-1.png' height="300"/>
</p>-->


