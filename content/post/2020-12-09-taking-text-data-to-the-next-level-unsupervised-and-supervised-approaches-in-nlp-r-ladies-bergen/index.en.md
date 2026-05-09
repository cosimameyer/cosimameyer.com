---
title: Taking Text Data to the Next Level - Unsupervised and Supervised Approaches in NLP @R-Ladies Bergen
author: Cosima Meyer
date: '2020-12-09'
slug: taking-text-data-to-the-next-level-unsupervised-and-supervised-approaches-in-nlp-r-ladies-bergen
category: [R-post, R, NLP, quanteda]
postImage: images/single-blog/rladies-bergen.png
featureImage: images/single-blog/rladies-bergen.png
tags: []
subtitle: ''
summary: ''
authors: []
#toc: true
---

I had the great pleasure to talk about NLP at [R-Ladies Bergen](https://www.meetup.com/rladies-bergen/events/274392883/) yesterday. Thanks to everyone for making this event so much fun! 
The talk covers both unsupervised and supervised approaches and introduces you to [`quanteda`](https://quanteda.io/), an R package that allows you to perform NLP tasks. 

All material can be accessed [**here**](https://github.com/cosimameyer/nlp-rladies-bergen) (including [slides](https://cosimameyer.com/slides/nlp-rladies/talk.html#1), [raw](https://github.com/cosimameyer/nlp-rladies-bergen/tree/main/code) and [deployed](https://nlp-bergen.netlify.app/) code as well as the [recording](https://youtu.be/bvqur70ZmyM)). The talk itself is heavily based on this [blogpost](https://www.mzes.uni-mannheim.de/socialsciencedatalab/article/advancing-text-mining/).

Here are some further insights into the talk:

![small_image](/images/single-blog/terms.png)

<!--<img style="float: center;" src="/images/single-blog/terms.png" alt="A table" width="700"/>-->

{{< detail-tag "Code" >}}
<!--<details>
<summary>Code</summary>-->
  
  ```r
  # Plot a word cloud
  quanteda::textplot_wordcloud(
    # Load the DFM object
    mydfm,
    # Define the minimum number the words have to occur
    min_count = 3,
    # Define the maximum number the words can occur
    max_words = 500,
    # Define a color
    color = wes_palette("Darjeeling1")
  )
  ```
{{< /detail-tag >}}

![small_image](/images/single-blog/wordcloud.png)

<!--<img src="/images/single-blog/wordcloud.png" width="400" />-->
{{< detail-tag "Code" >}}

  
  ```r
  # This code is heavily inspired by Julia Silge's blog post
  # (https://juliasilge.com/blog/sherlock-holmes-stm/)
  ```
  
{{< /detail-tag >}}

![small_image](/images/single-blog/topicmodel.png)
<!--<img src="/images/single-blog/topicmodel.png" width="400" />--> 
  {{< detail-tag "Code" >}}

  
  ```r
  # This code is heavily inspired by this blog post:
  # (https://www.mzes.uni-mannheim.de/socialsciencedatalab/article/advancing-text-mining/)
  ```
  
  {{< /detail-tag >}}
  
  ![small_image](/images/single-blog/share_un.png)
<!--  <img src="/images/single-blog/share_un.png" width="400" />-->

<!--</p>-->

  {{< detail-tag "Code" >}}

  
  ```r
  data %>%
  # Generate the country name for each country using the 
  # `countrycode()` command
    dplyr::mutate(countryname = countrycode(ccode, "iso3c", "country.name")) %>%
  # Filter and only select specific countries that we want to compare
    dplyr::filter(countryname %in% c(
      "Germany",
      "France",
      "United Kingdom",
      "Norway",
      "Spain",
      "Sweden"
  )) %>%
  # Now comes the plotting part :-)
    ggplot() +
  # We do a bar plot that has the years on the x-axis and the level of the 
  # net-sentiment on the y-axis
  # We also color it so that all the net-sentiments greater 0 get a 
  # different color
    geom_col(aes(
      x = year,
      y = net_perc,
      fill = (net_perc > 0)
    )) +
  # Here we define the colors as well as the labels and title of the legend
    scale_fill_manual(
      name = "Sentiment",
      labels = c("Negative", "Positive"),
      values = c("#C93312", "#446455")
    ) +
  # Now we add the axes labels
    xlab("Time") +
    ylab("Net sentiment") +
  # And do a facet_wrap by country to get a more meaningful visualization
    facet_wrap(~ countryname)
  ```
  
  {{< /detail-tag >}}
  
  ![small_image](/images/single-blog/share_un_country.png)
<!--  <img src="/images/single-blog/share_un_country.png" width="400" />-->
  
  {{< detail-tag "Code" >}}

  
```r
# Inspired here: https://bit.ly/37MCEHg

# Get the 30 top features from the DFM
freq_feature <- topfeatures(mydfm, 30)

# Create a data.frame for ggplot
data <- data.frame(list(
  term = names(freq_feature),
  frequency = unname(freq_feature)
))

# Plot the plot
data %>%
    # Call ggplot
    ggplot() +
    # Add geom_segment (this will give us the lines of the lollipops)
    geom_segment(aes(
      x = reorder(term, frequency),
      xend = reorder(term, frequency),
      y = 0,
      yend = frequency
    ), color = "grey") +
  # Call a point plot with the terms on the x-axis 
  # and the frequency on the y-axis
    geom_point(aes(x = reorder(term, frequency), y = frequency)) +
  # Flip the plot
    coord_flip() +
  # Add labels for the axes
    xlab("") +
    ylab("Absolute frequency of the features")
```
  
{{< /detail-tag >}}

![small_image](/images/single-blog/freq_plot.png)
<!--<img src="/images/single-blog/freq_plot.png" width="400" /> -->

{{< detail-tag "Code" >}}

  
  ```r
  data %>%
  # Generate the continent for each country using the `countrycode()` command
    dplyr::mutate(continent = countrycode(ccode, "iso3c", "continent", 
                              custom_match = c("YUG" = "Europe"))) %>%
  # We group by continent and year to generate the average sentiment by 
  # continent and and year  
    group_by(continent, year) %>%
    dplyr::mutate(avg = mean(net_perc)) %>%
  # We now plot it
    ggplot() +
  # Using a line chart with year on the x-axis, the average sentiment 
  # by continent on the y-axis and colored by continent
    geom_line(aes(x = year, y = avg, col = continent)) +
  # Define the colors
    scale_color_manual(name = "", values = wes_palette("Darjeeling1")) +
  # Label the axes
    xlab("Time") +
    ylab("Average net sentiment") 
  ```
{{< /detail-tag >}}

![small_image](/images/single-blog/negative-sentiment.png)

<!--<img src="/images/single-blog/negative-sentiment.png" width="400" />-->

These figures above show the output of more basic supervised and unsupervised models in NLP that you can use and that we covered during the talk. And as you work more and more with textual data, you will see that there is so much more in the field of NLP including [document similarity](https://cosimameyer.com/slides/nlp-rladies/talk.html#56), [text generation](https://arxiv.org/abs/1906.01946) or even [chat bots](https://cosimameyer.com/slides/nlp-rladies/talk.html#57) that you can create using your knowledge and starting with the same simple steps that I presented in the talk 👩🏼‍💻


If you want more resources, you can access them here:

- Quanteda

  - [Kohei Watanabe and Stefan Müller: Quanteda Tutorials](https://tutorials.quanteda.io)
  - [Quanteda Cheat Sheet](https://muellerstefan.net/files/quanteda-cheatsheet.pdf)

- More on text mining and NLP
  - [Cosima Meyer and Cornelius Puschmann: Advancing Text Mining with R and quanteda](https://www.mzes.uni-mannheim.de/socialsciencedatalab/article/advancing-text-mining/)
  - [Justin Grimmer and Brandon Stewart: Text as Data: The Promise and Pitfalls of Automatic Content Analysis Methods for Political Texts](https://www.cambridge.org/core/journals/political-analysis/article/text-as-data-the-promise-and-pitfalls-of-automatic-content-analysis-methods-for-political-texts/F7AAC8B2909441603FEB25C156448F20)
  - [Dan Jurafsky and James H. Martin: Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/)

- Sentiment analysis
  - [sentimentr](https://github.com/trinker/sentimentr)
  - [Hammerschmidt/Meyer 2020: Money Makes the World Go Frowned - Analyzing the Impact of Chinese Foreign Aid on States' Sentiment Using Natural Language Processing](https://www.tectum-shop.de/titel/chinas-rolle-in-einer-neuen-weltordnung-id-97867/) (for an applied example of sentiment analysis)

- Model validation
  - [oolong: Validation of dictionary approaches and topic models](https://cran.r-project.org/web/packages/oolong/index.html)
  - [stminsights](https://github.com/cschwem2er/stminsights)
  
- More general resource
  - [Data Science & Society](https://dssoc.github.io/schedule/)
  - [RegEx Cheat Sheet](https://rstudio.com/wp-content/uploads/2016/09/RegExCheatsheet.pdf)
  - [Stringr Cheat Sheet](https://github.com/rstudio/cheatsheets/blob/master/strings.pdf)