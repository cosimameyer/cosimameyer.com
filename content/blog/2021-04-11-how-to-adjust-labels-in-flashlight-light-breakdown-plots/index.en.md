---
title: How to adjust labels in {flashlight::light_breakdown} plots
author: Cosima Meyer
date: '2021-04-18'
slug: how-to-adjust-labels-in-flashlight-light-breakdown-plots
categories: [R-post, R]
postImage: images/single-blog/breakdown-plots.png
featureImage: images/single-blog/breakdown-plots.png
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
---

<!--<img style="float: right;" src="featured.png" alt="Drawing of a penguin" width="200"/>-->


To get a better (and more substantive) understanding of models' black boxes, [breakdown plots](https://journal.r-project.org/archive/2018/RJ-2018-072/RJ-2018-072.pdf) are extremely helpful and intuitive. The package [{flashlight}](https://cran.r-project.org/web/packages/flashlight/index.html) makes generating these plots straightforward -- but it has one shortcoming: It's not intuitive how to adjust the labels of your features (or at least I couldn't find an easy answer). This post shows a workaround that I used and I hope it helps others to save some time googling for a solution.

I will use the Palmer [`penguins`](https://allisonhorst.github.io/palmerpenguins/articles/intro.html) data set to show my steps.

```r
# Load the packages
library(flashlight)     # Shed Light on Black Box Machine Learning Models
library(palmerpenguins) # Palmer Archipelago (Antarctica) Penguin Data
library(dplyr)          # A Grammar of Data Manipulation
library(magrittr)       # A Forward-Pipe Operator for R
library(ggplot2)        # Create Elegant Data Visualizations Using 
                        # the Grammar of Graphics

# Load the data
data("penguins")
```

A detailed description how {flashlight} and the breakdown plots work, can be read up [here](https://cran.r-project.org/web/packages/flashlight/vignettes/flashlight.html). I will use a similar example as presented in the manual. In the first step, I show you the result of the common breakdown plot.

![small_image](/images/single-blog/breakdown.png)

{{< detail-tag "Code to generate the breakdown plot" >}}

```r
# Fit model
fit <- lm(bill_length_mm ~ ., data = penguins)

# Make flashlight
fl <-
  flashlight(
    model = fit,
    data = penguins,
    y = "bill_length_mm",
    label = "ols",
    metrics = list(rmse = MetricsWeighted::rmse, 
                   `R-squared` = MetricsWeighted::r_squared)
  )

# Plot the breakdown plot
plot(light_breakdown(fl, new_obs = penguins[3, ], digits = 2))
```
{{< /detail-tag >}}

<br>

The plot shows the single variables' (or *feature*) contributions. Put differently, it visualizes how much each single variable contributes to the prediction. It starts with the average value of the dependent variable (or *target*) in the data. The value 44 indicates that the average bill of the penguins in the data is 44 mm long. Red bars show a negative contribution and blue bars a positive contribution (also indicated with the sign before the variable value). This means that if the penguin is from the species Adelie the bill length is shorter whereas a greater bill depth (here 18 mm) is more likely to contribute to a longer bill length. The order of the variables is sorted by contribution size. This means that variables with greater contributions come first and variables with less contribution least. 

While this figure already tells a lot, it is not publication-ready. To make the appearance visually more attractive, we would want to adjust the labels in an easily readable way and probably also change some other aesthetics.

To do this, we deviate from the procedure and prepare the data before we get started. I tried various ways (labelling the data, adding labels later, etc.) and only the way I show you worked for me. From my understanding, this happens because the [`light_breakdown`](https://github.com/cran/flashlight/blob/master/R/light_breakdown.R) function generates the labels in the plot internally before plotting it. To generate the labels, the function uses the variable names as well as their values.

{{< detail-tag "More details on the output produced by `light_breakdown`" >}}

```r
light_breakdown(fl, new_obs = penguins[3, ], digits = 2)
```

```bash
I am an object with class(es) light_breakdown, light, list 

data.frames (maximum 6 rows shown):

 data 
# A tibble: 6 x 6
   step variable          after before label description                   
  <int> <chr>             <dbl>  <dbl> <chr> <chr>                         
1     0 baseline           44.0   44.0 ols   average in data: 44           
2     1 species            39.7   44.0 ols   species = Adelie: -4.3        
3     2 sex                38.6   39.7 ols   sex = female: -1              
4     3 body_mass_g        37.5   38.6 ols   body_mass_g = 3250: -1.1      
5     4 flipper_length_mm  37.2   37.5 ols   flipper_length_mm = 195: -0.34
6     5 bill_depth_mm      37.5   37.2 ols   bill_depth_mm = 18: +0.28     
```
Everything stored in `description` will be later plotted as labels in your plot.

{{< /detail-tag >}}

<br>
To get meaningful labels, I recode the variable values (here for `sex` I want to see "Female" instead of "female") and the variable names (for instance, the variable `bill_length_mm` is not easily readable whereas `Length of bill (in mm)` is).

```r
penguins %<>%
  # First adjust the labels
   dplyr::mutate(
    sex = case_when(sex == "female" ~ "Female",
                    sex == "male" ~ "Male")) %>% 
  dplyr::rename(
    `Penguin species` = species,
    `Island` = island,
    `Length of bill (in mm)` = bill_length_mm,
    `Depth of bill (in mm)` = bill_depth_mm,
    `Length of flipper (in mm)` = flipper_length_mm,
    `Body mass (in g)` = body_mass_g,
    `Sex` = sex,
    `Year` = year)
```

And we're all set. Now we fit the model again and generate a flashlight.

```r
# Fit model
fit <- lm(`Length of bill (in mm)` ~ ., data = penguins)

# Make flashlight
fl <-
  flashlight(
    model = fit,
    data = penguins,
    y = "Length of bill (in mm)",
    label = "ols",
    metrics = list(rmse = MetricsWeighted::rmse, 
                   `R-squared` = MetricsWeighted::r_squared)
  )
```

We can use the flashlight object `fl` and plot our breakdown plot

```r
plot(light_breakdown(fl, new_obs = penguins[3,], digits = 2)) +
  theme_classic() +
  theme(
    axis.text.y = element_blank(),
    axis.ticks.y = element_blank(),
    axis.title.y = element_blank()
  ) +
  scale_fill_grey()
```

![small_image](/images/single-blog/polished-breakdown.png)

`theme()`, `theme_classic()`, and `scale_fill_grey()` are further adjustments to make the figure publishable in a later manuscript -- but there are [endless possibilities and you can pick whichever works best for you](https://ggplot2.tidyverse.org/reference/ggtheme.html).


