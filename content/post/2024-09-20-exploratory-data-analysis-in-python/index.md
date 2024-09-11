---
title: Exploratory Data Analysis in Python
author: Cosima Meyer
date: '2024-09-10'
slug: exploratory-data-analysis-in-python
categories:
  - Python
  - Python-post
tags:
  - Python
  - Python-post
postImage: images/single-blog/eda_cover_python.png
featureImage: images/single-blog/eda_cover_python.png
---

Following up on the [Explanatory Data Analysis in R](https://cosimameyer.com/post/exploratory-data-analysis-in-r/), here's the Python edition 🎉

Never heard of EDA before? Here's the short summary: 

> Exploratory Data Analysis, or EDA for short, is one of the most important parts of any data science workflow. It's often time-consuming, but its importance should not be underestimated: Understanding your data and identifying potential biases is extremely important for all subsequent steps. 

As in R, there are also a few libraries in Python that can help and simplify some steps in the workflow. 

-----

Before we get started, let's install all the relevant libraries. This article will cover the following libraries:

{{< toc >}}

```python
# Data
from palmerpenguins import load_penguins

# EDA libraries
import dtale
import pandas as pd
import sweetviz as sv
from autoviz import AutoViz_Class
from overviewpy.overviewpy import overview_tab, overview_na
from summarytools import dfSummary
from ydata_profiling import ProfileReport
```

And now we load the data:

```python
df = load_penguins()
```

The [penguins dataset](https://allisonhorst.github.io/palmerpenguins/) has information about - guess what - penguins 🐧 It's a great resource to illustrate EDA, data viz, and more. It was created as an alternative to the iris dataset.

-----

### pandas

While the remainder of the post follows an alphabetical order, let's start with `pandas` since the other libraries built upon pandas data frame structure. The [`pandas`](https://pandas.pydata.org) library is a prominent Python library for handling data frames. 

In `pandas`, we can use `shape`. This way, we access the dimensions of the data frame. This gives us a good understanding of the number of rows (344) and columns (8).

```python
df.shape
```

```
(344, 8)
```

In your own notebook, a next step would be to "print" the head of the data - that means to look at the first lines of the data frame:

```python
df.head()
```

```
          species   island      bill_length_mm   bill_depth_mm   flipper_length_mm   body_mass_g   sex      year 
  0       Adelie    Torgersen   39.1             18.7            181.0               3750.0        male     2007 
  1       Adelie    Torgersen   39.5             17.4            186.0               3800.0        female   2007 
  2       Adelie    Torgersen   40.3             18.0            195.0               3250.0        female   2007 
  3       Adelie    Torgersen   NaN              NaN             NaN                 NaN           NaN      2007 
  4       Adelie    Torgersen   36.7             19.3            193.0               3450.0        female   2007  
```

In a similar way, we can also look at the tail:

```python
df.tail()
```

```
          species     island    bill_length_mm   bill_depth_mm   flipper_length_mm   body_mass_g   sex      year 
  339     Chinstrap   Dream     55.8             19.8            207.0               4000.0        male     2009 
  340     Chinstrap   Dream     43.5             18.1            202.0               3400.0        female   2009 
  341     Chinstrap   Dream     49.6             18.2            193.0               3775.0        male     2009 
  342     Chinstrap   Dream     50.8             19.0            210.0               4100.0        male     2009 
  343     Chinstrap   Dream     50.2             18.7            198.0               3775.0        female   2009 
```

In a next step, we use `df.info()` to get a general overview of the dataset:

```python
df.info()
```

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 344 entries, 0 to 343
Data columns (total 8 columns):
 #   Column             Non-Null Count  Dtype  
---  ------             --------------  -----  
 0   species            344 non-null    object 
 1   island             344 non-null    object 
 2   bill_length_mm     342 non-null    float64
 3   bill_depth_mm      342 non-null    float64
 4   flipper_length_mm  342 non-null    float64
 5   body_mass_g        342 non-null    float64
 6   sex                333 non-null    object 
 7   year               344 non-null    int64  
dtypes: float64(4), int64(1), object(3)
memory usage: 21.6+ KB
```

This tells us a lot about the data. We see the column names (= our features/variables), whether there are NAs (`Non-Null Count`) and we also learn more about the data type (`Dtype`).

While `pandas` can be used for any other tasks in the field of data wrangling, it also offers functions for descriptive statistics.

```python
df.describe()
```

```
           bill_length_mm    bill_depth_mm   flipper_length_mm   body_mass_g   year       
  count    342.000000        342.000000      342.000000          342.000000    344.000000 
  mean     43.921930         17.151170       200.915205          4201.754386   2008.029070
  std      5.459584          1.974793        14.061714           801.954536    0.818356   
  min      32.100000         13.100000       172.000000          2700.000000   2007.000000
  25%      39.225000         15.600000       190.000000          3550.000000   2007.000000
  50%      44.450000         17.300000       197.000000          4050.000000   2008.000000
  75%      48.500000         18.700000       213.000000          4750.000000   2009.000000 
  max      59.600000         21.500000       231.000000          6300.000000   2009.000000 
```

As you can see, this provides descriptive statistics for each column in your data frame.

<!--### SciPy-->

### AutoViz

[`AutoViz`](https://pypi.org/project/autoviz/) is a few-line tool that gives you an overview of your data.

To get started, add the following code to your Jupyter notebook:

```python
AV = AutoViz_Class()

dft = AV.AutoViz(
    "",
    sep=",",
    depVar="",
    dfte=df,
    header=0,
    verbose=1,
    lowess=False,
    chart_format="server",
    max_rows_analyzed=150000,
    max_cols_analyzed=30,
    save_plot_dir=None
)
```
[![small_image](/images/single-blog/autoviz_penguins.gif)](/images/single-blog/autoviz_penguins.gif)
{{< detail-tag "Alternative text" >}}Jupyter output showing an interactive AutoViz dashboard.{{< /detail-tag >}}

Running this in your notebook will open a couple of browser tabs that show various HTML-based descriptive statistics output. You can play around and change the settings to explore your data. When applying it to your dataframe, make sure that it is named `df` (otherwise the call will throw an error).

### DTale

[DTale](https://github.com/man-group/dtale) also provides a low-code approach to exploring your data. With the following line you'll get an output in your notebook (if you add `open_browser=True` as argument, it will open in your browser).

```python
dtale.show(df)
```

[![small_image](/images/single-blog/dtale_penguins.gif)](/images/single-blog/dtale_penguins.gif)
{{< detail-tag "Alternative text" >}}Jupyter output showing an interactive DTale dashboard.{{< /detail-tag >}}

Unlike the other one-line workflows in Python's EDA, DTale takes a different approach: it shows you the tabular data as it is and allows you to gain an understanding of different aspects (such as missing data, data types, or even outliers) based on the tabular view. It also gives you the ability to visualize correlations and even manipulate some data. 

### overviewpy

[`overviewpy`](https://cosimameyer.github.io/overviewpy/)  is a work-in-progress version of R's [`overviewR`](). It doesn't have all the features yet, but you can already use some of the main functions.

Like `overviewR`, `overviewpy` was designed with time-series cross-sectional data in mind, but it can also be helpful for other use cases.

```python
overview_tab(df=df, id="species", time="year").reset_index(drop=True)
```

```
# There are some duplicates. We aggregate the data before proceeding.
         species     time_frame 
 0       Adelie      2007-2009  
 1       Chinstrap   2007-2009  
 2       Gentoo      2007-2009  
```

This method provides the time coverage based on two pre-defined variables (id and time). The id can be anything: countries, islands, or even penguins.

To get an understanding of the missing data in your data, you can make use of `overviewpy_na`. 

```python
overview_na(df)
```

![small_image](/images/single-blog/overviewpy_na.png)
{{< detail-tag "Alternative text" >}}Screenshot of the output of overview_na showing the distribution of missing values.{{< /detail-tag >}}

This will illustrate the distribution of missing values across your data frame. For further details on the library roadmap, follow the [link here](https://cosimameyer.github.io/overviewpy/#roadmap).

### summarytools

The [`summarytools`](https://pypi.org/project/summarytools/) is the Python equivalent of R's [`summarytools`](https://github.com/dcomtois/summarytools), and it has my go-to feature implemented:

```python
dfSummary(df)
```

This gives you a nice visual, descriptive overview with all the essential information you need to get started: 

[![small_image](/images/single-blog/summarytools_dfsummary_python.png)](/images/single-blog/summarytools_dfsummary_python.png)
{{< detail-tag "Alternative text" >}}Screenshot of the output of dfSummary showing distributions visually with histograms and (bar) plots.{{< /detail-tag >}}

### SweetViz

[`SweetViz`](https://pypi.org/project/sweetviz/) is a neat library that helps to generate nice EDA reports with only two lines of code! 

```python
report = sv.analyze(df)
report.show_notebook(layout="vertical", w=800, h=700, scale=0.8)
```

[![small_image](/images/single-blog/sweet_viz_penguins.gif)](/images/single-blog/sweet_viz_penguins.gif)
{{< detail-tag "Alternative text" >}}Jupyter output showing an interactive SweetViz dashboard.{{< /detail-tag >}}

The report provides variable-level insights, including descriptive statistics, but also shows a correlation matrix to better understand the statistical relationship between variables.

### YData Profiling

Some may know [`YData Profiling`](https://docs.profiling.ydata.ai/latest/) under its former name "Pandas Profiling". Similar to `SweetViz` and `AutoViz`, it provides a one-line code approach to a comprehensive overview of your data.

```python
ProfileReport(df, title="Penguins' Profiling Report")
```

[![small_image](/images/single-blog/ydata_penguins.gif)](/images/single-blog/ydata_penguins.gif)
{{< detail-tag "Alternative text" >}}Jupyter output showing an interactive YData Profiling dashboard.{{< /detail-tag >}}

This report also provides variable-level insights, including descriptive statistics, but also shows a bivariate scatter plot of different variables, alerts (if there are too many misses), and information about the configurations that generated the report.

-----

And to reiterate what I concluded in the other post: Python provides a very good starting point with functions for exploring and understanding your data. If you need more, there are developers out there who can help you with their libraries. These may be suitable for your use case, but you may need more - so why not build some features and contribute to one of the existing packages? 

Let's start wrangling and developing!

[![](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExbHV0dTJrNnY1bjF3OTcwcjJ6Mnc5YjI2Z3R2bGV2ZzVzZ25wbzF5cSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/2IudUHdI075HL02Pkk/giphy.gif)](https://giphy.com/gifs/pudgypenguins-data-code-coding-2IudUHdI075HL02Pkk)
{{< detail-tag "Alternative text" >}}A gif showing a penguin in front of a screen with the word "Coding" in the upper part of the image.{{< /detail-tag >}}
