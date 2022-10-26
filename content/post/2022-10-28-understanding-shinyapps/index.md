---
title: Understanding ShinyApps
author: Cosima Meyer
date: '2022-10-26'
slug: understanding-shinyapps
category: [R, R-post]
postImage: images/single-blog/CS_Shiny.png
featureImage: images/single-blog/CS_Shiny.png
---

Today, we'll discover how you can use the power of R (and RStudio) to create, for instance, an interactive visualization with the ShinyApp framework. 

### 💡 What is a ShinyApp?
Shiny is a framework that allows you to create web applications - ShinyApps 😊
You can use them for multiple purposes - a common example is visualizing data (for instance the [Scottish Household Survey](https://bit.ly/3TqZevY)), building [interactive appendices](https://bit.ly/shiny-appendix), [search engines](https://bit.ly/shiny-se), and so much more ✨

[![small_image](/images/single-blog/scotland.png)](https://bit.ly/3TqZevY)

[![small_image](/images/single-blog/appendix.png)](https://bit.ly/shiny-appendix)

[![small_image](/images/single-blog/coro2vid.png)](https://bit.ly/shiny-se)

### 💡 What does a ShinApp need?

A ShinyApp consists of two central components. The UI and the server -- or, as I like to think about them, the body 👤 and the brain 🧠

### 💡 What is the UI?

The user interface (or short UI) is like the body of your app 👤 It allows you to define how it looks and where the components (such as text, visualizations, or tables) are placed. It defines the outer appearance of your app. The code works as follows:

![small_image](/images/single-blog/ui.png)

### 💡 What is the server?
The server is the brain - here's where all the computing happens 🧠 You can dump (more or less) all your typical functions (such as plotting something with [📦{ggplot2}](https://ggplot2.tidyverse.org)) in here 🤗 
It can look like this:

![small_image](/images/single-blog/server.png)


### 👩🏼‍💻 How do you set up your own ShinyApp?
But how do we set it up? Easy! 

![small_image](/images/single-blog/shiny_workflow.png)


What you now see in code is how you can fill the UI and server with content. I picked checkboxes for the selection in the sidebar of the UI [(but there are multiple other possibilities for more control widgets)](https://shiny.rstudio.com/tutorial/written-tutorial/lesson3/) and added a ggplot2 inside the `renderPlot` function in the server to generate the visualization 👩🏼‍🎨

And here's how it "runs" (just highlight the code and let it run - a new window with your first ShinyApp will open 🙌)

![small_image](/images/single-blog/shiny_workflow.gif)

What I learned from building ShinyApps:

- Sketch your ShinyApp on paper (or tablet) with a pencil. This helps to understand what you want to get (and also what's probably missing). It's a bit like doing a design workshop with yourself.
- Start simple. Trust me, you ShinyApp will get complicated soon enough 😄
- 📦 [{echarts4r}](https://echarts4r.john-coene.com) is a package for making beautiful visualizations 
- If you're up for a production-ready ShinyApp, have a look at 📦 [{golem}](https://engineering-shiny.org/golem.html) (and the [Golemverse](https://golemverse.org))
- If you're not so much up for writing code, have a look at 📦 [{shinyuieditor}](https://rstudio.github.io/shinyuieditor/index.html) - it allows you to create a UI without coding

---------------

### 💡 What is reactivity and what does it have to do with a carrier pigeon?
To better understand how a ShinyApp works, it's good to understand reactivity. To describe it, I love the image of a carrier pigeon 🐦 (I picked up this idea when reading a post by Garett Grolemund - so all credits go to him ✨): 

What reactivity does is ["a magic trick [that] creates the illusion that one thing is happening, when in fact something else is going on"](https://shiny.rstudio.com/articles/understanding-reactivity.html). Your ShinyApp only re-runs those parts where it is necessary. But what does this have to do with a carrier pigeon? Have a look at the GIF:

![small_image](/images/single-blog/shiny_flow.gif)

It shows your ShinyApp (bottom right) and the server (top right). The user (bottom left) asks for something. In the first round, the user asks for "Rwanda" and - after checking, the ShinyApp does not re-run because it already shows Rwanda. In the next round, the user asks for "Rwanda" and "Angola" - the ShinyApp evaluates and initiates a re-run to update the requested selection. Here comes the carrier pigeon 🐦 It starts sending the signal to update to the server. Once it has delivered the message, it comes back to where it started and waits until it gets a new message to deliver (just like a carrier pigeon 😊). 

This was a simple wrap-up of Garrett Grolemund’s post - if you want to know more about reactive values and observers, have a look [here](https://shiny.rstudio.com/articles/understanding-reactivity.html) 👩🏼‍💻


----------
More helpful resources: 

- 🗂  As announced at RStudioConf2022, you can now also build ShinyApps in Python! [Rami Krispin](https://github.com/RamiKrispin) set up a [great repository](https://github.com/RamiKrispin/shinyelive) that shows you how to set up your ShinyApp in Python using shinyelive 💻
- 📖 If you're up for more input on ShinyApps, here's the bible of Shiny: ["Mastering Shiny"](https://mastering-shiny.org)
- 📺 And a [recording](https://www.youtube.com/watch?v=NOzi6GErpsA) of a talk I gave at CorrelAid's CorrelCon 2020 about the topic
- 👩🏼‍🏫 And here are the [slides and the code](https://github.com/cosimameyer/conflict-elections) (including a [📦{ggplot2}](https://ggplot2.tidyverse.org) and [📦{echarts4r}](https://echarts4r.john-coene.com) comparison) (it’s the material for the CorrelCon 2020)

📝 And if you also keep thinking about brains and bodies, here is more of it to summarize the key points 🤓 (also as 📄PDF for you to [download here](/media/CS_Shiny.pdf)): 

![small_image](/images/single-blog/CS_Shiny.png)

