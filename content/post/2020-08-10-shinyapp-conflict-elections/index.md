---
title: 'ShinyApp: Conflict Elections in Afghanistan 2019'
summary: ''
tags: []
category: [R, ShinyApp]
date: "2020-08-10"
featured: false
draft: false
projects: [ShinyApp - Conflict elections in Afghanistan 2019]
postImage: images/single-blog/shinyapp-conflict-elections.png
featureImage: images/single-blog/shinyapp-conflict-elections.png
---

ShinyApps are an amazing tool to make research results accessible to a broad audience. This is exactly what talk at [CorrelAid’s Open Online Data Meetup](https://correlaid.org/en/events/2020-08/oodm3-interactive-science/) is about. The app on conflict elections in Afghanistan is a spin-off of a co-authored publication with [Dennis Hammerschmidt](https://github.com/dennis-hammerschmidt) that [analyzes patterns of violence during conflict elections in Afghanistan](https://www.ceeol.com/search/article-detail?id=775100) in 2005.

While the geo-spatially disaggregated data for the original publication is based on [UCDP data](https://ucdp.uu.se/), this ShinyApp is based on [ACLED data](https://acleddata.com/#/dashboard) and shows how violence that includes the Taliban develops starting 180 days up to the election in 2019.

[![small_image](/images/single-blog/conflict-elections.png)](conflict-elections.png)

<!--[![](/media/conflict-elections.png)](http://cosima-meyer.shinyapps.io/conflict-elections/)-->

I use a combination of [shiny](https://shiny.rstudio.com/), [shinydashboard](https://rstudio.github.io/shinydashboard/), and [echarts4r](http://echarts4r.john-coene.com/) in R to create the app.

You can access the entire source code and all materials on my [GitHub](https://github.com/cosimameyer/conflict-elections).