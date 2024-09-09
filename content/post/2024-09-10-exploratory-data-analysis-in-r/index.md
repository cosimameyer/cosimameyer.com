---
title: Exploratory Data Analysis in R
author: Cosima Meyer
date: '2024-09-08'
slug: exploratory-data-analysis-in-r
categories:
  - R
  - R-post
  - EDA
tags:
  - R
  - R-post
  - EDA
postImage: images/single-blog/eda_cover.png
featureImage: images/single-blog/eda_cover.png
---


Exploratory Data Analysis, or EDA for short, is one of the most important parts of any data science workflow. It's often time-consuming, but its importance should not be underestimated: Understanding your data and identifying potential biases is extremely important for all subsequent steps. Fortunately, there are a number of packages that can help and simplify some steps in the workflow. 

In this two-part series, I will cover both packages in R and in Python. We'll start with the R-based packages. 

In the paper ["overviewR - Easily Explore Your Data in R" (published in JOSS)](https://joss.theoj.org/papers/10.21105/joss.04740), my co-author and I compare the key features of other available EDA packages in R with our package [overviewR](https://cosimameyer.github.io/overviewR/). While overviewR was developed with a specific focus on time series data, its functionality can be applied to a broader range of use cases. I'll use this comparison as a basis here to show the key features of each package:

![small_image](/images/single-blog/overview_eda_r.png)
{{< detail-tag "Alternative text" >}}Table showing the comparison of different packages in R with respect to their functionality in exploratory data analysis.{{< /detail-tag >}}

(The table is taken from the paper ([Meyer & Hammerschmidt (2022), p. 2](https://joss.theoj.org/papers/10.21105/joss.04740)))

For this post, we'll explore the packages in alphabetical order and use the [{palmerpenguins}](https://allisonhorst.github.io/palmerpenguins/) dataset. 

All packages have a depth of functionality that goes beyond this blog post. I will only cover the features that I use frequently, but the interested reader is of course more than encouraged to dive deeper into the richness of the packages ☺️

-----

Before we get started, let's install all the relevant packages. This article will cover the following packages:

{{< toc >}}

```r
# Data
install.packages("palmerpenguins")

# EDA libraries
install.packages("DataExplorer")
install.packages("dlookr")
install.packages("gtsummary")
install.packages("Hmisc")
install.packages("naniar")
install.packages("overviewR")
install.packages("skimr")
install.packages("SmartEDA")
install.packages("summarytools")
```

And now we load the data:

```r
library(palmerpenguins)
data(package = 'palmerpenguins')
```

The penguins dataset has information about - guess what - penguins 🐧 It's a great resource to illustrate EDA, data viz, and more. It was created as an alternative to the iris dataset.

-----

### R Essentials

Let's start with what R offers out of the box. 

```r
dim(penguins)
```

```
[1] 344   8
```

This gives you the dimensions (rows and columns) of your data frame

To print the head or tail of the data, R has the `head()` and `tail` functions.

```r
head(penguins)
```

```
# A tibble: 6 × 8
  species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g sex     year
  <fct>   <fct>              <dbl>         <dbl>             <int>       <int> <fct>  <int>
1 Adelie  Torgersen           39.1          18.7               181        3750 male    2007
2 Adelie  Torgersen           39.5          17.4               186        3800 female  2007
3 Adelie  Torgersen           40.3          18                 195        3250 female  2007
4 Adelie  Torgersen           NA            NA                  NA          NA NA      2007
5 Adelie  Torgersen           36.7          19.3               193        3450 female  2007
6 Adelie  Torgersen           39.3          20.6               190        3650 male    2007
```

```r
tail(penguins)
```

```
# A tibble: 6 × 8
  species   island bill_length_mm bill_depth_mm flipper_length_mm body_mass_g sex     year
  <fct>     <fct>           <dbl>         <dbl>             <int>       <int> <fct>  <int>
1 Chinstrap Dream            45.7          17                 195        3650 female  2009
2 Chinstrap Dream            55.8          19.8               207        4000 male    2009
3 Chinstrap Dream            43.5          18.1               202        3400 female  2009
4 Chinstrap Dream            49.6          18.2               193        3775 male    2009
5 Chinstrap Dream            50.8          19                 210        4100 male    2009
6 Chinstrap Dream            50.2          18.7               198        3775 female  2009
```

Last but not least, I also like to use `str()` and `summary()` which help us to understand the general structure of our data.

```r
str(penguins)
```

```
tibble [344 × 8] (S3: tbl_df/tbl/data.frame)
 $ species          : Factor w/ 3 levels "Adelie","Chinstrap",..: 1 1 1 1 1 1 1 1 1 1 ...
 $ island           : Factor w/ 3 levels "Biscoe","Dream",..: 3 3 3 3 3 3 3 3 3 3 ...
 $ bill_length_mm   : num [1:344] 39.1 39.5 40.3 NA 36.7 39.3 38.9 39.2 34.1 42 ...
 $ bill_depth_mm    : num [1:344] 18.7 17.4 18 NA 19.3 20.6 17.8 19.6 18.1 20.2 ...
 $ flipper_length_mm: int [1:344] 181 186 195 NA 193 190 181 195 193 190 ...
 $ body_mass_g      : int [1:344] 3750 3800 3250 NA 3450 3650 3625 4675 3475 4250 ...
 $ sex              : Factor w/ 2 levels "female","male": 2 1 1 NA 1 2 1 2 NA NA ...
 $ year             : int [1:344] 2007 2007 2007 2007 2007 2007 2007 2007 2007 2007 ...
```

```r
summary(penguins)
```

```
     species          island    bill_length_mm  bill_depth_mm   flipper_length_mm  body_mass_g       sex           year     
 Adelie   :152   Biscoe   :168   Min.   :32.10   Min.   :13.10   Min.   :172.0     Min.   :2700   female:165   Min.   :2007  
 Chinstrap: 68   Dream    :124   1st Qu.:39.23   1st Qu.:15.60   1st Qu.:190.0     1st Qu.:3550   male  :168   1st Qu.:2007  
 Gentoo   :124   Torgersen: 52   Median :44.45   Median :17.30   Median :197.0     Median :4050   NA's  : 11   Median :2008  
                                 Mean   :43.92   Mean   :17.15   Mean   :200.9     Mean   :4202                Mean   :2008  
                                 3rd Qu.:48.50   3rd Qu.:18.70   3rd Qu.:213.0     3rd Qu.:4750                3rd Qu.:2009  
                                 Max.   :59.60   Max.   :21.50   Max.   :231.0     Max.   :6300                Max.   :2009  
                                 NA's   :2       NA's   :2       NA's   :2         NA's   :2  
```

And now let's go into the world of extensions and see what they have for us ✨

-----

### DataExplorer

The first extension on the list is [{DataExplorer}](http://boxuancui.github.io/DataExplorer/). What I like most about it is how much you can get out of it (and your data) with just a single line of code. 

```r
library(DataExplorer)
create_report(penguins)
```

This will give you a really nice HTML report. For the `penguins` data it looks like this (it is scrollable):

{{< slideshow "/media/report.html" >}}

To access the report on a new page, [click here](/media/report.html). 

As you can see, `DataExplorer` covers not only standard descriptive statistics such as distributions of correlation plots, but also pays attention to the memory usage of your data. While this may not be relevant for standard use cases, it can certainly be an important asset when you're dealing with efficient structuring of your data and thinking about data storage.

-----

### dlookr

[{dlookr}](https://choonghyunryu.github.io/dlookr/) builds on its `diagnose` function family. You can simply run `diagnose` on the entire data set (or select specific columns):

```r
library(dlookr)
diagnose(penguins)
```

```
# A tibble: 8 × 6
  variables         types   missing_count missing_percent unique_count unique_rate
  <chr>             <chr>           <int>           <dbl>        <int>       <dbl>
1 species           factor              0           0                3     0.00872
2 island            factor              0           0                3     0.00872
3 bill_length_mm    numeric             2           0.581          165     0.480  
4 bill_depth_mm     numeric             2           0.581           81     0.235  
5 flipper_length_mm integer             2           0.581           56     0.163  
6 body_mass_g       integer             2           0.581           95     0.276  
7 sex               factor             11           3.20             3     0.00872
8 year              integer             0           0                3     0.00872
```

This gives you a variable based overview where you can get a quick look at data types, missing values and unique values, but you can also look at specific data types using `dlookr::diagnose_numeric()` or `dlookr::diagnose_category()`. 

```r
diagnose_numeric(penguins)
```

```
# A tibble: 5 × 10
  variables            min     Q1   mean median     Q3    max  zero minus outlier
  <chr>              <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl> <int> <int>   <int>
1 bill_length_mm      32.1   39.2   43.9   44.4   48.5   59.6     0     0       0
2 bill_depth_mm       13.1   15.6   17.2   17.3   18.7   21.5     0     0       0
3 flipper_length_mm  172    190    201.   197    213    231       0     0       0
4 body_mass_g       2700   3550   4202.  4050   4750   6300       0     0       0
5 year              2007   2007   2008.  2008   2009   2009       0     0       0
```

Here we get standard descriptive statistics for the numerical values, including information on the number of outliers. 

For the categorical variables, this looks like this:

```r
diagnose_category(penguins)
```

```
# A tibble: 9 × 6
  variables levels        N  freq ratio  rank
  <chr>     <chr>     <int> <int> <dbl> <int>
1 species   Adelie      344   152 44.2      1
2 species   Gentoo      344   124 36.0      2
3 species   Chinstrap   344    68 19.8      3
4 island    Biscoe      344   168 48.8      1
5 island    Dream       344   124 36.0      2
6 island    Torgersen   344    52 15.1      3
7 sex       male        344   168 48.8      1
8 sex       female      344   165 48.0      2
9 sex       NA          344    11  3.20     3
```

If outliers are present, two really handy functions are `dlookr::diagnose_outlier()` (for a tabular output) and `dlookr::plot_outlier()` (for a visual view on outliers in numeric variables).

-----

### gtsummary

The functionality of [{gtsummary}](https://www.danieldsjoberg.com/gtsummary/) goes beyond just EDA. We'll focus on the EDA parts of the package, but if you're looking for a way to present your regression results in a visually appealing way, `gtsummary::tbl_regression()` may also become your new best friend.

To generate summary tables of your data, use `gtsummary::tbl_summary()`: 

```r
library(gtsummary)
tbl_summary(penguins)
```

![very_small_image](/images/single-blog/gtsummary_table.png)
{{< detail-tag "Alternative text" >}}Screenshot of the output of tbl_summary. It shows a nicely formatted table with key descriptive statistics of the variables.{{< /detail-tag >}}


You can also subset your dataset first to include only certain variables (or tweak the data a bit to get better labels) - but this function produces a very nice and publication-ready table! It also comes with [examples of how to further customize the appearance of your tables](https://www.danieldsjoberg.com/gtsummary/articles/rmarkdown.html).

-----

### Hmisc

[{Hmisc}](https://hbiostat.org/r/hmisc/) is originally from the field of biostatistics and was one of the very first packages I discovered when looking for support in doing my EDA. I have to admit that I'm only scratching the surface of its functionality here. So if you're looking for a more comprehensive overview, [here](https://hbiostat.org/r/hmisc/#package-usage-and-examples) are more in-depth examples that cover a complete R workflow. 

My go-to function that I have used extensively is `Hmisc::describe()`:

```r
library(Hmisc)
describe(penguins)
```

```
 8  Variables      344  Observations
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
species 
       n  missing distinct 
     344        0        3 
                                        
Value         Adelie Chinstrap    Gentoo
Frequency        152        68       124
Proportion     0.442     0.198     0.360
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
island 
       n  missing distinct 
     344        0        3 
                                        
Value         Biscoe     Dream Torgersen
Frequency        168       124        52
Proportion     0.488     0.360     0.151
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
bill_length_mm 
       n  missing distinct     Info     Mean      Gmd      .05      .10      .25      .50      .75      .90      .95 
     342        2      164        1    43.92    6.274    35.70    36.60    39.23    44.45    48.50    50.80    51.99 

lowest : 32.1 33.1 33.5 34   34.1, highest: 55.1 55.8 55.9 58   59.6
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
bill_depth_mm 
       n  missing distinct     Info     Mean      Gmd      .05      .10      .25      .50      .75      .90      .95 
     342        2       80        1    17.15    2.267     13.9     14.3     15.6     17.3     18.7     19.5     20.0 

lowest : 13.1 13.2 13.3 13.4 13.5, highest: 20.7 20.8 21.1 21.2 21.5
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
flipper_length_mm 
       n  missing distinct     Info     Mean      Gmd      .05      .10      .25      .50      .75      .90      .95 
     342        2       55    0.999    200.9    16.03    181.0    185.0    190.0    197.0    213.0    220.9    225.0 

lowest : 172 174 176 178 179, highest: 226 228 229 230 231
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
body_mass_g 
       n  missing distinct     Info     Mean      Gmd      .05      .10      .25      .50      .75      .90      .95 
     342        2       94        1     4202    911.8     3150     3300     3550     4050     4750     5400     5650 

lowest : 2700 2850 2900 2925 2975, highest: 5850 5950 6000 6050 6300
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
sex 
       n  missing distinct 
     333       11        2 
                        
Value      female   male
Frequency     165    168
Proportion  0.495  0.505
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
year 
       n  missing distinct     Info     Mean      Gmd 
     344        0        3    0.888     2008   0.8919 
                            
Value       2007  2008  2009
Frequency    110   114   120
Proportion 0.320 0.331 0.349

For the frequency table, variable is rounded to the nearest 0
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

As you can see, with just one command you get a nice tabular overview of the standard descriptive statistics for each variable. 

-----

### naniar

[{naniar}](https://naniar.njtierney.com) brings light into your missing data 💡  

The package is all about highlighting and dealing with missing data. One very nice feature is `naniar::miss_var_summary()` which displays your missing data on a variable-basis.

```r
library(naniar)
penguins %>% 
  miss_var_summary()
```

```
# A tibble: 8 × 3
  variable          n_miss pct_miss
  <chr>              <int>    <num>
1 sex                   11    3.20 
2 bill_length_mm         2    0.581
3 bill_depth_mm          2    0.581
4 flipper_length_mm      2    0.581
5 body_mass_g            2    0.581
6 species                0    0    
7 island                 0    0    
8 year                   0    0    
```

By adding `dplyr::group_by()` we can further disaggregate the missingness:

```r
penguins %>%
 dplyr::group_by(year) %>%
 miss_var_summary()
```

```
# A tibble: 21 × 4
# Groups:   year [3]
    year variable          n_miss pct_miss
   <int> <chr>              <int>    <num>
 1  2007 sex                    7    6.36 
 2  2007 bill_length_mm         1    0.909
 3  2007 bill_depth_mm          1    0.909
 4  2007 flipper_length_mm      1    0.909
 5  2007 body_mass_g            1    0.909
 6  2007 species                0    0    
 7  2007 island                 0    0    
 8  2008 sex                    1    0.877
 9  2008 species                0    0    
10  2008 island                 0    0    
# ℹ 11 more rows
# ℹ Use `print(n = ...)` to see more rows
```

In this way, we can see if there are certain years where missingness is disproportionately present (and then derive subsequent steps from this insight).

-----

### overviewR

[{overviewR}](https://cosimameyer.github.io/overviewR/) was originally developed to provide the user with a detailed view of cross-sectional time series data. In addition to providing insights for initial exploratory data analysis, it can be helpful when merging your data (and identifying time series gaps in your data).

Most commands are available to create a tabular or a visual view. 

The command `overviewR::overview_tab()` creates a tabular view based on two predefined variables (id and time). 

```r
library(overviewR)
overview_tab(dat = penguins, id = species, time = year)
```

```
# A tibble: 3 × 2
# Groups:   species [3]
  species   time_frame 
  <fct>     <chr>      
1 Adelie    2007 - 2009
2 Chinstrap 2007 - 2009
3 Gentoo    2007 - 2009
```

As you can see, it works not only for countries (as shown in the documentation of the package), but also for individuals like penguins. This output can also be stored in an object and then converted to a printable LaTeX table to be added as publication-ready information, and for more visual people, we can also display the same information in a plot:

```r
overview_plot(dat = penguins, id = species, time = year)
```

![small_image](/images/single-blog/plot_penguins_overviewr.png)
{{< detail-tag "Alternative text" >}}Plot showing the distribution of the time variable (year) across the id variable (species).{{< /detail-tag >}}

As we can clearly see, all species are present for the entire time frame. 

Both the `overview_plot' and the `overview_tab' functions also have `overview_crossplot' and `overview_crosstab' functions to reflect the cross-tabular nature of the data.

In addition to the time series features, we can also access more general information from the dataset - for example, the proportion of missing values across variables:

```r
overview_na(penguins)
```

![small_image](/images/single-blog/overview_na_penguins.png)
{{< detail-tag "Alternative text" >}}Screenshot showing the missing values in a dataset with a bar plot.{{< /detail-tag >}}

As we can see, the time, species, and island are completely covered, but other variables have missing values. It's up to you to determine how serious these missing values are and what to do with them.

-----

### skimr

[{skimr}](https://docs.ropensci.org/skimr/) is an excellent package if you want a concise overview of the most important descriptive statistics of your data. My go-to call is:

```r
library(skimr)

skim(penguins)
```

```
── Data Summary ────────────────────────
                           Values  
Name                       penguins
Number of rows             344     
Number of columns          8       
_______________________            
Column type frequency:             
  factor                   3       
  numeric                  5       
________________________           
Group variables            None    
> skim(penguins)
── Data Summary ────────────────────────
                           Values  
Name                       penguins
Number of rows             344     
Number of columns          8       
_______________________            
Column type frequency:             
  factor                   3       
  numeric                  5       
________________________           
Group variables            None    

── Variable type: factor ──────────────────────────────────────────────────────────────────
  skim_variable n_missing complete_rate ordered n_unique top_counts                 
1 species               0         1     FALSE          3 Ade: 152, Gen: 124, Chi: 68
2 island                0         1     FALSE          3 Bis: 168, Dre: 124, Tor: 52
3 sex                  11         0.968 FALSE          2 mal: 168, fem: 165         

── Variable type: numeric ─────────────────────────────────────────────────────────────────
  skim_variable     n_missing complete_rate   mean      sd     p0    p25    p50    p75
1 bill_length_mm            2         0.994   43.9   5.46    32.1   39.2   44.4   48.5
2 bill_depth_mm             2         0.994   17.2   1.97    13.1   15.6   17.3   18.7
3 flipper_length_mm         2         0.994  201.   14.1    172    190    197    213  
4 body_mass_g               2         0.994 4202.  802.    2700   3550   4050   4750  
5 year                      0         1     2008.    0.818 2007   2007   2008   2009  
    p100 hist 
1   59.6 ▃▇▇▆▁
2   21.5 ▅▅▇▇▂
3  231   ▂▇▃▅▂
4 6300   ▃▇▆▃▂
5 2009   ▇▁▇▁▇
```

As you can see, it gives you a wealth of descriptive statistics - all readily accessible in your console.

-----

### smartEDA

The goal of [{smartEDA}](https://daya6489.github.io/SmartEDA/) is to make EDA easy by running just one line of code (instead of several long lines of R code). What I use most often is a variation of the following command:

```r
library("SmartEDA")
ExpData(data=penguins,type=1)
```

```
                                          Descriptions     Value
1                                   Sample size (nrow)       344
2                              No. of variables (ncol)         8
3                    No. of numeric/interger variables         5
4                              No. of factor variables         3
5                                No. of text variables         0
6                             No. of logical variables         0
7                          No. of identifier variables         0
8                                No. of date variables         0
9             No. of zero variance variables (uniform)         0
10               %. of variables having complete cases 37.5% (3)
11   %. of variables having >0% and <50% missing cases 62.5% (5)
12 %. of variables having >=50% and <90% missing cases    0% (0)
13          %. of variables having >=90% missing cases    0% (0)
```

`type=1` returns an overview of the data in general. If we choose `type=2`, we get more detailed information on a variable level:

```
  Index     Variable_Name Variable_Type Sample_n Missing_Count Per_of_Missing No_of_distinct_values
1     1           species        factor      344             0          0.000                     3
2     2            island        factor      344             0          0.000                     3
3     3    bill_length_mm       numeric      342             2          0.006                   164
4     4     bill_depth_mm       numeric      342             2          0.006                    80
5     5 flipper_length_mm       integer      342             2          0.006                    55
6     6       body_mass_g       integer      342             2          0.006                    94
7     7               sex        factor      333            11          0.032                     2
8     8              year       integer      344             0          0.000                     3
```

But that's not all - smartEDA also allows you to filter, reshape or group your data before generating the EDA.

-----

### summarytools

[{summarytools}](https://github.com/dcomtois/summarytools) is the last package introduced in this blog post. Similar to the previous packages, it allows you to create descriptive statistics of your data. I think the command I use most from this package is `dfSummary`.

```r
library(summarytools)
view(dfSummary(penguins))
```

This gives you a nice visual, descriptive overview in your viewer window in RStudio with all the essential information you need to get started: 

![small_image](/images/single-blog/summarytools_dfsummary.png)
{{< detail-tag "Alternative text" >}}Screenshot of the output of dfSummary showing distributions visually with histograms and (bar) plots.{{< /detail-tag >}}

-----

Finally, as you have seen, R already provides a good starting set of functions for exploring and understanding your data. If that's not enough, there are several package developers out there to help you out. Which package you choose will ultimately depend on your personal preferences and your use case. 

So let's start exploring your data!

[![](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExaDI4cXRwa2I3NTJzYXN6eHc3cjV5OHRjM3h2ZDFkZGYxZmNzc29wOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/UAkLHNaScd6Zjlx5eZ/giphy.gif)](https://giphy.com/gifs/disneyprincess-UAkLHNaScd6Zjlx5eZ)
{{< detail-tag "Alternative text" >}}A gif showing a person on a simple sailing boat sailing away (to new shores) and exploring the world.{{< /detail-tag >}}
