---
title: Newest Features in overviewR
author: Cosima Meyer
date: '2026-05-09'
slug: newest-features-in-overviewr
categories:
  - R
  - R-post
tags:
  - R
  - R-post
postImage: images/single-blog/overviewR0014.png
featureImage: images/single-blog/overviewR0014.png
---

### What's New in overviewR 0.0.14? 
#### A Quick Look at the Latest Update

If you work with panel data in R — data that tracks multiple units (like countries, companies, or people) over time — you've probably run into the challenge of simply *understanding* what your data set covers. Which units are present in which time periods? Where are the gaps? That's exactly the problem [overviewR](https://github.com/cosimameyer/overviewR) tries to solve, and where the latest release brings some nice improvements.

Here's a quick run-through of what you can expect from version 0.0.14.

#### Markdown Support: Beyond LaTeX

The biggest addition is a function called `overview_markdown()`. Previously, if you wanted to export your data summaries as nicely formatted tables, you could use `overview_latex()` to generate LaTeX output. A good aproach if you're writing academic papers, but not so helpful if you're writing a README, a blog post, or documentation in Markdown.

`overview_markdown()` converts the output of `overview_tab()` or `overview_crosstab()` directly into a clean Markdown table. You can customize headers, titles, and even export the result to a `.md` file.

```r
toydata |> 
  overview_tab(id = ccode, time = year) |> 
  overview_markdown()
```

```
## Time and scope of the sample
| Sample | Time frame |
|--------|------------|
| AGO | 1990 - 1992 |
| BEN | 1995 - 1999 |
| FRA | 1993, 1996, 1999 |
| GBR | 1991, 1993, 1995, 1997, 1999 |
| RWA | 1990 - 1995 | 
```

Which, if put into a markdown file, gives you this beautiful table:

| Sample | Time frame |
|--------|------------|
| AGO | 1990 - 1992 |
| BEN | 1995 - 1999 |
| FRA | 1993, 1996, 1999 |
| GBR | 1991, 1993, 1995, 1997, 1999 |
| RWA | 1990 - 1995 | 

(The layout changes depending on the attached styles attached to your markdown rendering.)

What works for `overview_tab` should also work for `overview_crosstab`, right? And it does! Just set `crosstab = TRUE`.

```r
overview_crosstab(
  dat = toydata,
  cond1 = gdp,
  cond2 = population,
  threshold1 = 25000,
  threshold2 = 27000,
  id = ccode,
  time = year
) |> 
overview_markdown(
  crosstab = TRUE
)
```

```
## Time and scope of the sample
| | **Condition 1**: Fulfilled | **Condition 1**: Not fulfilled |
|---|---|---|
| **Condition 2**: Fulfilled | AGO (1990, 1992), FRA (1993), GBR (1997) | BEN (1996, 1999), FRA (1999), GBR (1993), RWA (1992, 1994) |
| **Condition 2**: Not fulfilled | BEN (1997), RWA (1990) | AGO (1991), BEN (1995, 1998), FRA (1996), GBR (1991, 1995, 1999), RWA (1991, 1993, 1995) | 
```

And this gives you:

| | **Condition 1**: Fulfilled | **Condition 1**: Not fulfilled |
|---|---|---|
| **Condition 2**: Fulfilled | AGO (1990, 1992), FRA (1993), GBR (1997) | BEN (1996, 1999), FRA (1999), GBR (1993), RWA (1992, 1994) |
| **Condition 2**: Not fulfilled | BEN (1997), RWA (1990) | AGO (1991), BEN (1995, 1998), FRA (1996), GBR (1991, 1995, 1999), RWA (1991, 1993, 1995) | 

#### Smarter Venn Diagrams

The `overview_overlap()` function, which lets you visually compare two datasets, now supports **area-proportional Euler diagrams**. Previously, Venn diagrams in overviewR showed overlapping circles of equal size regardless of how much actual overlap existed. By setting `proportional = TRUE` when using `plot_type = "venn"`, the circles are now drawn so that their sizes reflect the true proportions of overlap. Under the hood, this uses the [`eulerr`](https://cran.r-project.org/web/packages/eulerr/vignettes/introduction.html) package.

```r
overview_overlap(
  dat1 = toydata,
  dat2 = toydata2,
  dat1_id = ccode,
  dat2_id = ccode,
  plot_type = "venn",
  proportional = TRUE
)
```

![small_image](/images/single-blog/overview_overlap_venn.svg)
{{< detail-tag "Alternative text" >}}
Two circles in different blue shades with the sames "Data set 1" and "Data set 2" where data set 2 represents a subset of data set 1.
{{< /detail-tag >}}

So the proportions of the circles reflect now the numbers.

#### More flexible plot colors

`overview_plot()` previously required mapping a column to a color aesthetic. Based on community feedback, you can now pass a simple color string (e.g., `color = "steelblue"`) to style all lines and points uniformly:

```r
overview_plot(
  dat = toydata, 
  id = ccode, 
  time = year,
  color = "steelblue"
)
```

![small_image](/images/single-blog/overview_plot_steelblue.svg)
{{< detail-tag "Alternative text" >}}
Visualizing the time plot from `overview_plot` with steelblue as the color of the dots and lines connecting consecutive time dots.
{{< /detail-tag >}}

If you want, you can also define the colors manually using `ggplot2::scale_color_manual` or pass color palettes using `ggplot2::scale_color_brewer`

```r
overview_plot(
  dat = toydata, 
  id = ccode, 
  time = year,
  color = post_1995
) + ggplot2::scale_color_brewer(palette = "Set1")
```

![small_image](/images/single-blog/overview_plot_color_palette.svg)
{{< detail-tag "Alternative text" >}}
Visualizing the time plot from `overview_plot` with red and blue as the colors of the dots and lines connecting consecutive time dots. The colors are coded based on whether the entry was after 1995 or not.
{{< /detail-tag >}}

#### Better Missing-Data Warnings

Working with time-series data means missing values can silently distort your results. Version 0.0.14 extends `overview_na()` to send warnings for NA values in `month` or `day` columns. This early heads-up can help you catch gaps in your time coverage before they cause surprises later on.

#### Getting Started

At the time when this blog post goes live, the newest version is also available on CRAN and you can install it directly from there:

```r
install.packages("overviewR")
```

Alternatively, you can also get the development version from GitHub:

```r
# install.packages("devtools")
devtools::install_github("cosimameyer/overviewR")
```

For more details, check out the [full documentation](https://cosimameyer.github.io/overviewR/).

Did you get a chance to give it a try? We're very much looking forward to hearing your feedback!