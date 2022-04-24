---
title: 'Using Python to scrape and visualize networks in European football'
subitle: 'Visualizing team compositions with plotly in Python'
author: Cosima Meyer
date: '2021-06-19'
slug: ''
category: [Python-post, Python, football, data visualization]
postImage: images/single-blog/python-football-network.png
featureImage: images/single-blog/python-football-network.png
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

<!--<img style="float: left;" src="/images/single-blog/python-football-network.png" alt="A football player" width="200"/>-->

Although I am not an avid European football fan, I sometimes have the chance to watch a match -- and even more so during these days! ⚽️

Now that I've been following the German Bundesliga for some seasons (more irregularly than regularly), I was curious to see which players are part of the national teams. To get an idea of the network, I relied on data by [The Athletic](https://theathletic.com/2594478/2021/06/15/euro-2020-squads-teams-list/) and followed the steps described below. I get an interactive visualization that allows me to compare across national teams.

![small_image](/images/single-blog/teams.png)

<!--<img style="float: center;" src="/images/single-blog/teams.png" alt="A sankey diagram" width="700"/>-->

The complete code can also be accessed in my [GitHub](https://github.com/cosimameyer/uefa-networks-2020/).

## How do I get there?

### Import relevant libraries

```python
import pandas as pd
from pandas import DataFrame
from bs4 import BeautifulSoup
import requests
import re
import ipywidgets as widgets
from ipywidgets import interactive, interact
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
import plotly.graph_objects as go
import plotly
import chart_studio.plotly as py
```

If these libraries are not installed yet, [here](https://packaging.python.org/tutorials/installing-packages/) is a detailed guide how to do it.
<!-- <details><summary>If these libraries are not installed yet, copy-paste these lines to your terminal.</summary> -->
<!-- ```{bash, eval=FALSE} -->

<!-- ``` -->
<!-- </details> -->

### Scrape the data
To scrape the data, I followed this [guide](https://medium.com/@devkosal/scraping-data-with-beautifulsoup-and-selectorgadget-in-python-3-decf798e1a1e).
It is a combination of [SelectorGadget](https://selectorgadget.com) and [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/). BeautifulSoup pulls data from HTML and XML files. SelectorGadget helps to define the CSS selectors to extract specific parts of BeautifulSoup's output. 
You will get an HTML output that I restrict to the desired text (using SelectorGadget to define which text I need). However, since the resulting object still contains output that I did not want to have included, I further had to do some data wrangling until I got the desired data frame.

```python
# Load URL in beautiful soup
url = 'https://theathletic.com/2594478/2021/06/15/euro-2020-squads-teams-list/'

# Get the data
res = requests.get(url)

# Use beautiful soup to parse it
soup = BeautifulSoup(res.text, "html.parser")

# This is how the output looks like -- not really beautiful, right?
soup.head()
```

Now that this does not look too nice, I first rely on SelectorGadget and determine the parts that interest me most. Then, I select them and subset the output to this part.

```python
# SelectorGadget to the rescue :-) 
# I extract the required content (I determined the selection criteria "p , h3" using the SelectorGadget)
plain_text = soup.select("p , h3")

# Extract the plain text (it turned out to work best for me)
text = soup.get_text()

# It's now way better:
text
```

```
\n\n\nEuro 2020: Every national team’s squad list for this summer’s European Championship – The Athletic\n(window.NREUM||(NREUM={})).loader_config={xpid:"VQUEVVZVDRABU1FRAQkAVQ==",licenseKey:"e5d6a2e943",applicationID:"17036873"};window.NREUM||(NREUM={}),__nr_require=function(t,e,n){function r(n){if(!e[n]){var o=e[n]={exports:{}};t[n][0].call(o.exports,function(e){var o=t[n][1][e];return r(o||e)},o,o.exports)}return e[n].exports}if("function"==typeof __nr_require)return __nr_require;for(var o=0;o<n.length;o++)r(n[o]);return r}({1:[function(t,e,n){function r(t){try{c.console&&console.log(t)}catch(e){}}var o,i=t("ee"),a=t(29),c={};try{o=localStorage.getItem("__nr_flags").split(","),
console&&"function"==typeof console.log&&(c.console=!0,o.indexOf("dev")!==-1&&(c.dev=!0),o.indexOf("nr_dev")!==-1&&(c.nrDev=!0))}catch(s){}c.nrDev&&i.on("internal-error",function(t){r(t.stack)}),c.dev&&i.on("fn-err",function(t,e,n){r(n.stack)}),c.dev&&(r("NR AGENT IN DEVELOPMENT MODE"),r("flags: "+a(c,function(t,e){return t}).join(", ")))},{}],2:[function(t,e,n){function r(t,e,n,r,c){try{p?p-=1:o(c||new UncaughtException(t,e,n),!0)}catch(f){try{i("ierr",[f,s.now(),!0])}catch(d){}}return"function"==typeof u&&u.apply(this,a(arguments))}function UncaughtException(t,e,n){this.message=t||"Uncaught error with no additional information",this.sourceURL=e,this.line=n}function o(t,e)
{var n=e?null:s.now();i("err",[t,n])}var i=t("handle"),a=t(30),c=t("ee"),s=t("loader"),f=t("gos")
```

This looks already way better than before! But what you see, I have many `"\n\n\n\n\n"`. I will make use of it and use it to split the string.

```python
# Split the text string (this will lead to separate string on which we can filter)
text_split = text.split("\n\n\n\n\n")

text_split
```

```
['\n\n\nEuro 2020: Every national team’s squad list for this summer’s European Championship – The Athletic\n(window.NREUM||(NREUM={})).loader_config={xpid:"VQUEVVZVDRABU1FRAQkAVQ==",licenseKey:"e5d6a2e943",applicationID:"17036873"};window.NREUM||(NREUM={}),__nr_require=function(t,e,n){function r(n){if(!e[n]){var o=e[n]={exports:{}};t[n][0].call(o.exports,function(e){var o=t[n][1][e];return r(o||e)},o,o.exports)}return e[n].exports}if("function"==typeof __nr_require)return __nr_require;for(var o=0;o<n.length;o++)r(n[o]);return r}({1:[function(t,e,n){function r(t){try{c.console&&console.log(t)}catch(e){}}var o,i=t("ee"),a=t(29),c={};try{o=localStorage.getItem("__nr_flags").split(","),
console&&"function"==typeof console.log&&(c.console=!0,o.indexOf("dev")!==-1&&(c.dev=!0),o.indexOf("nr_dev")!==-1&&(c.nrDev=!0))}catch(s){}c.nrDev&&i.on("internal-error",function(t){r(t.stack)}),c.dev&&i.on("fn-err",function(t,e,n){r(n.stack)}),c.dev&&(r("NR AGENT IN DEVELOPMENT MODE"),r("flags: "+a(c,function(t,e){return t}).join(", ")))},{}],2:[function(t,e,n){function r(t,e,n,r,c){try{p?p-=1:o(c||new UncaughtException(t,e,n),!0)}catch(f){try{i("ierr",[f,s.now(),!0])}catch(d){}}return"function"==typeof u&&u.apply(this,a(arguments))}function UncaughtException(t,e,n){this.message=t||"Uncaught error with no additional information",
this.sourceURL=e,this.line=n}function o(t,e){var n=e?null:s.now();i("err",[t,n])}var i=t("handle"),a=t(30),c=t("ee"),s=t("loader"),f=t("gos")
```

While the American cities at the top are great places, they do not have anything to do with the European Championship. I thus restrict the data further to the required part. To do this, I rely on the index:

```python
# Using the index, I select the part that is relevant for me:
text_split[119]
```

```
'Group A\nTurkey\nGoalkeepers: Altay Bayindir (Fenerbahce), Ugurcan Cakır (Trabzonspor), Mert Gunok (Istanbul Basaksehir)\nDefenders: Kaan Ayhan (Sassuolo), Zeki Celik (LOSC Lille), Merih Demiral (Juventus), Ozan Kabak (Liverpool), Umut Meras (Le Havre), Mert Muldur (Sassuolo), Caglar Soyuncu (Leicester), Ridvan Yilmaz (Besiktas)\nMidfielders: Taylan Antalyali (Galatasaray), Hakan Calhanoglu (AC Milan), Halil Dervisoglu (Brentford), Irfan Can Kahveci (Fenerbahce), Orkun Kokcu (Feyenoord), Abdulkadir Omur (Trabzonspor), Dorukhan Tokoz (Besiktas), Ozan Tufan (Fenerbahce), Okay Yokuslu (West Brom)\nForwards: Kerem Akturkoglu (Galatasaray), Kenan Karaman (Fortuna Düsseldorf), Enes Unal (Getafe), Cengiz Under (Leicester), Yusuf Yazici (LOSC Lille), Burak Yilmaz (LOSC Lille)\nItaly\nGoalkeepers: Gianluigi Donnarumma (Milan), Alex Meret (Napoli), Salvatore Sirigu (Torino)\nDefenders: Francesco Acerbi (Lazio), Alessandro Bastoni (Inter), Leonardo Bonucci (Juventus),
```

This is precisely what I want!

```python
# And I restrict my data to this part only
player = text_split[119]
```

```
['Group A',
 'Turkey',
 'Goalkeepers: Altay Bayindir (Fenerbahce), Ugurcan Cakır (Trabzonspor), Mert Gunok (Istanbul Basaksehir)',
 'Defenders: Kaan Ayhan (Sassuolo), Zeki Celik (LOSC Lille), Merih Demiral (Juventus), Ozan Kabak (Liverpool), Umut Meras (Le Havre), Mert Muldur (Sassuolo), Caglar Soyuncu (Leicester), Ridvan Yilmaz (Besiktas)',
 'Midfielders: Taylan Antalyali (Galatasaray), Hakan Calhanoglu (AC Milan), Halil Dervisoglu (Brentford), Irfan Can Kahveci (Fenerbahce), Orkun Kokcu (Feyenoord), Abdulkadir Omur (Trabzonspor), Dorukhan Tokoz (Besiktas), Ozan Tufan (Fenerbahce), Okay Yokuslu (West Brom)',
 'Forwards: Kerem Akturkoglu (Galatasaray), Kenan Karaman (Fortuna Düsseldorf), Enes Unal (Getafe), Cengiz Under (Leicester), Yusuf Yazici (LOSC Lille), Burak Yilmaz (LOSC Lille)',
 'Italy,'
 'Goalkeepers: Gianluigi Donnarumma (Milan), Alex Meret (Napoli), Salvatore Sirigu (Torino)',
 'Defenders: Francesco Acerbi (Lazio), Alessandro Bastoni (Inter), Leonardo Bonucci (Juventus),
```

### Do some data wrangling

As always, some data wrangling is needed. 

```python
# Remove \xao (better, otherwise we have do deal with it later)
player = player.replace(u'\xa0', u' ')

# And I again split the data -- but this time based on \n
player_row = player.split("\n")

# And that's how it looks like now: 
player_row
```

```
['Turkey',
 'Goalkeepers: Altay Bayindir (Fenerbahce), Ugurcan Cakır (Trabzonspor), Mert Gunok (Istanbul Basaksehir)',
 'Defenders: Kaan Ayhan (Sassuolo), Zeki Celik (LOSC Lille), Merih Demiral (Juventus), Ozan Kabak (Liverpool), Umut Meras (Le Havre), Mert Muldur (Sassuolo), Caglar Soyuncu (Leicester), Ridvan Yilmaz (Besiktas)',
 'Midfielders: Taylan Antalyali (Galatasaray), Hakan Calhanoglu (AC Milan), Halil Dervisoglu (Brentford), Irfan Can Kahveci (Fenerbahce), Orkun Kokcu (Feyenoord), Abdulkadir Omur (Trabzonspor), Dorukhan Tokoz (Besiktas), Ozan Tufan (Fenerbahce), Okay Yokuslu (West Brom)',
 'Forwards: Kerem Akturkoglu (Galatasaray), Kenan Karaman (Fortuna Düsseldorf), Enes Unal (Getafe), Cengiz Under (Leicester), Yusuf Yazici (LOSC Lille), Burak Yilmaz (LOSC Lille)',
 'Italy',
 'Goalkeepers: Gianluigi Donnarumma (Milan), Alex Meret (Napoli), Salvatore Sirigu (Torino)',
 'Defenders: Francesco Acerbi (Lazio), Alessandro Bastoni (Inter), Leonardo Bonucci (Juventus),
```


As you'll see, there are still some empty rows and rows that we do not necessarily need (for instance "Group A" etc.). I'll deal with this now:

```python
# Replace "Group A" etc. with an empty string ("")
player_row = [s.replace("Group A", "") for s in player_row]
player_row = [s.replace("Group B", "") for s in player_row]
player_row = [s.replace("Group C", "") for s in player_row]
player_row = [s.replace("Group D", "") for s in player_row]
player_row = [s.replace("Group E", "") for s in player_row]
player_row = [s.replace("Group F", "") for s in player_row]
player_row = [s.replace("(Photo: Getty Images)", "") for s in player_row]
```

My initial idea was to automatize these steps with a loop, but it did not work -- so I went back to a few copy-pasted lines of code.

Now that these rows turned into empty strings, we can remove them:

```python
# Filter for non-empty strings
player_row = list(filter(None, player_row))

player_row
```

![smaller_image](/images/single-blog/uefa-table1.png)

<!--<img style="float: center;" src="/images/single-blog/uefa-table1.png" alt="A table" width="700"/>-->

The next step is a bit tricky but straightforward. I define an empty dictionary `teams` and loop over the list `player_row` to extract the national team identity (defined by `teams[player_row[i]]`) as well as all the information on the national teams (who the players are, what their home clubs are, and which position they play). The last part is extracted using `player_row[start:end]`. 
If you look at the data above, you'll see that all the information is located in 5 rows (each comma shows a new row). I thus use the `range()` function and start counting in a row `0`, over the length of the list `len(player_row)` and jump in `5` row steps. 

```python
# Define a dictionary
teams = {}
# Extract sub sets for each national team
# I use the "start, stop, step" arguments from range
for i in range(0, len(player_row), 5):
    start = i + 1
    end = i + 5
    teams[player_row[i]] = player_row[start:end]
```

In the next step, I turn it into a data frame.

```python
# Turn it into a DF and do some wranglin
teams_df = DataFrame(teams)

teams_df.head()
```

![smaller_image](/images/single-blog/uefa-table1.png)

<!--<img style="float: center;" src="/images/single-blog/uefa-table1.png" alt="A table" width="700"/>-->

While this already looks promising, it is not exactly what I was looking for. 
The easiest way to deal with the data here was to transpose it.

```python
# Transpose DF
teams_t = teams_df.T

teams_t.head()
```

![smaller_image](/images/single-blog/uefa-table2.png)

<!--<img style="float: center;" src="/images/single-blog/uefa-table2.png" alt="A table" width="700"/>-->

The following steps are about moving the index column, renaming columns, and removing unnecessary strings.

```python
# Move index to column
teams_t = teams_t.reset_index()
# Rename columns
teams_t.rename(columns={ teams_t.columns[0]: "national_team" }, inplace = True)
teams_t.rename(columns={ teams_t.columns[1]: "goalkeeper" }, inplace = True)
teams_t.rename(columns={ teams_t.columns[2]: "defender" }, inplace = True)
teams_t.rename(columns={ teams_t.columns[3]: "midfield" }, inplace = True)
teams_t.rename(columns={ teams_t.columns[4]: "forward" }, inplace = True)

# Remove strings from columns
teams_t["goalkeeper"] = [s.replace("Goalkeepers: ", "") for s in teams_t["goalkeeper"]]
teams_t["defender"] = [s.replace("Defenders: ", "") for s in teams_t["defender"]]
teams_t["midfield"] = [s.replace("Midfielders: ", "") for s in teams_t["midfield"]]
teams_t["forward"] = [s.replace("Forwards: ", "") for s in teams_t["forward"]]

teams_t.head()
```

![smaller_image](/images/single-blog/uefa-table3.png)

<!--<img style="float: center;" src="/images/single-blog/uefa-table3.png" alt="A table" width="700"/>-->

I still have all the names of the player in each position in one column. I know R has a great package called [`splitstackshape`](https://www.r-project.org/nosvn/pandoc/splitstackshape.html) that automatically splits strings, based on a separation criterion in X columns. Unfortunately, I couldn't find a Python equivalent, so I did it in a bit more tedious way.

```python
# And now we can split each column based on ","
teams_t[['goalkeeper1', 'goalkeeper2', 'goalkeeper3']] = teams_t['goalkeeper'].str.split(pat= ',', expand=True)
teams_t[['defender1', 'defender2', 'defender3', 'defender4', 'defender5', 'defender6', 'defender7', 'defender8', 'defender9', 'defender10']] = teams_t['defender'].str.split(pat= ',', expand=True)
teams_t[['midfield1', 'midfield2', 'midfield3', 'midfield4', 'midfield5', 'midfield6', 'midfield7', 'midfield8', 'midfield9', 'midfield10', 'midfield11', 'midfield12']] = teams_t['midfield'].str.split(pat= ',', expand=True)
teams_t[['forward1', 'forward2', 'forward3', 'forward4', 'forward5', 'forward6', 'forward7', 'forward8']] = teams_t['forward'].str.split(pat= ',', expand=True)

teams_t.head()
```

![smaller_image](/images/single-blog/uefa-table4.png)

<!--<img style="float: center;" src="/images/single-blog/uefa-table4.png" alt="A table" width="700"/>-->


Not exactly what I want, so I still have to do some cleaning :-)
To get the format I am looking for, I use `pd.melt`.

```python
# Now transform columns into rows and keep the content
teams_m = pd.melt(teams_t, id_vars=['national_team'],
        value_vars=['goalkeeper1', 'goalkeeper2', 'goalkeeper3', 'defender1', 'defender2', 'defender3', 'defender4', 'defender5', 'defender6', 'defender7', 'defender8', 'defender9', 'defender10',
                    'midfield1', 'midfield2', 'midfield3', 'midfield4', 'midfield5', 'midfield6', 'midfield7', 'midfield8', 'midfield9', 'midfield10', 'midfield11', 'midfield12',
                    'forward1', 'forward2', 'forward3', 'forward4', 'forward5', 'forward6', 'forward7', 'forward8'], var_name = "position", value_name = "player")

teams_m.head()
```

![smaller_image](/images/single-blog/uefa-table5.png)

<!--<img style="float: center;" src="/images/single-blog/uefa-table5.png" alt="A table" width="700"/>-->

That looks almost like I want it to look! I want to remove the numbers in the position and empty rows (with no player). 

```python
# Remove numbers from "position" column
teams_m['position'] = teams_m['position'].str.replace('\d+', '')

# Remove also empty rows with no value in "player"
teams_m = teams_m[teams_m['player'].notnull()]

teams_m.head()
```

![smaller_image](/images/single-blog/uefa-table6.png)

<!--<img style="float: center;" src="/images/single-blog/uefa-table6.png" alt="A table" width="700"/>-->


Well, almost there :-) Now I need to split the last string ("player") into "player_name" and "home_club."

```python
teams_m['home_club'] = teams_m.player.str.extract(r"\((.*?)\)", expand=True)
teams_m['player_name'] = teams_m.player.str.replace(r"\(.*\)","")

# And, for the visualization, I also add a "count"
teams_m["count"] = 1

teams_m.head()
```

![smaller_image](/images/single-blog/uefa-table7.png)

<!--<img style="float: center;" src="/images/single-blog/uefa-table7.png" alt="A table" width="700"/>-->

Et voilà, that's my data frame!

### Getting the visualization

Now the fun part of visualization starts :-) I follow [this approach](https://medium.com/kenlok/how-to-create-sankey-diagrams-from-dataframes-in-python-e221c1b4d6b0), which also provides the great function `genSankey` below.

{{< detail-tag "`getSankey` function provided in [this post](https://medium.com/kenlok/how-to-create-sankey-diagrams-from-dataframes-in-python-e221c1b4d6b0)" >}}

```python
def genSankey(df, cat_cols=[], value_cols='', title='Sankey Diagram'):
    # maximum of 6 value cols -> 6 colors
    colorPalette = ['#4B8BBE', '#306998', '#FFE873', '#FFD43B', '#646464']
    labelList = []
    colorNumList = []
    for catCol in cat_cols:
        labelListTemp = list(set(df[catCol].values))
        colorNumList.append(len(labelListTemp))
        labelList = labelList + labelListTemp

    # remove duplicates from labelList
    labelList = list(dict.fromkeys(labelList))

    # define colors based on number of levels
    colorList = []
    for idx, colorNum in enumerate(colorNumList):
        colorList = colorList + [colorPalette[idx]] * colorNum

    # transform df into a source-target pair
    for i in range(len(cat_cols) - 1):
        if i == 0:
            sourceTargetDf = df[[cat_cols[i], cat_cols[i + 1], value_cols]]
            sourceTargetDf.columns = ['source', 'target', 'count']
        else:
            tempDf = df[[cat_cols[i], cat_cols[i + 1], value_cols]]
            tempDf.columns = ['source', 'target', 'count']
            sourceTargetDf = pd.concat([sourceTargetDf, tempDf])
        sourceTargetDf = sourceTargetDf.groupby(['source', 'target']).agg({'count': 'sum'}).reset_index()

    # add index for source-target pair
    sourceTargetDf['sourceID'] = sourceTargetDf['source'].apply(lambda x: labelList.index(x))
    sourceTargetDf['targetID'] = sourceTargetDf['target'].apply(lambda x: labelList.index(x))

    # creating the sankey diagram
    data = dict(
        type='sankey',
        node=dict(
            pad=15,
            thickness=20,
            line=dict(
                color="black",
                width=0.5
            ),
            label=labelList,
            color=colorList
        ),
        link=dict(
            source=sourceTargetDf['sourceID'],
            target=sourceTargetDf['targetID'],
            value=sourceTargetDf['count']
        )
    )

    layout = dict(
        title=title,
        font=dict(
            size=10
        )
    )

    fig = dict(data=[data], layout=layout)
    return fig
```

{{< /detail-tag >}}


The function takes four arguments:

- `df` - the data frame
- `cat_cols` - the columns that you want to visualize in your Sankey diagram (for me it's `'player_name','home_club', 'national_team'`)
- `value_cols` - to define how thick the Sankeys are
- `title` - the title of your Sankey 

I could plot the Sankey as it is using

```python
# Generate figure
fig = genSankey(teams_m, cat_cols=['player_name','home_club', 'national_team'], value_cols='count', title='Composition of national teams')

# Visualize it
ploty.offline.plot(sankey, validate = False)
```


But this would lead to an overload of Sankeys -- and it might be hard to see anything. Python also has a way to make your results interactively accessible using [`ipywidgets`](https://ipywidgets.readthedocs.io/en/latest/index.html). I mainly follow [this guide](https://blog.ouseful.info/2016/12/29/simple-view-controls-for-pandas-dataframes-using-ipython-widgets/) to visualize my plot.

The code below produces an interactive plot to pick a national team and see which players are part of which club and how the clubs themselves contribute to the national team.

```python
# Select the items that I want to have as a selection
items = sorted(teams_m['national_team'].unique().tolist())

# Define the `view` function
# This function basically takes care of the visualization for me
def view(Team=''):
    df = teams_m[teams_m['national_team'] == Team]
    fig = genSankey(df,
                    cat_cols=['player_name','home_club', 'national_team'],
                    value_cols='count',
                    title='Composition of national teams')
    iplot(fig, validate=False)

# Define the selection widget
w = widgets.Select(options=items)

# And make it interactive
interactive(view, Team=w)
```

And now I can get to the most interesting part - and the reason why I initially started this little project: from which club do most players in the German national team come from? I more or less frequently have the pleasure to watch football on Saturdays, and watching the match by the German team felt as if I was watching a Bayern Munich match. And in fact - I was not off! Eight players (and that's almost 1/3 of the entire national team) come from this club! 

![small_image](/images/single-blog/viz.gif)

<!--<img style="float: center;" src="/images/single-blog/viz.gif" alt="Final and interactive sankey diagram" width="700"/>-->

If you want to visualize the networks yourself, just run the [Juypter notebook](https://github.com/cosimameyer/uefa-networks-2020/).