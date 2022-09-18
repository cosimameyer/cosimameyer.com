---
title: Mastering debugging in R
author: Cosima Meyer
date: '2022-09-28'
slug: mastering-debugging-in-r
category: [R, R-post]
postImage: images/single-blog/CS_Debugging.png
featureImage: images/single-blog/CS_Debugging.png
---

One major thing that I learned throughout the years is the power of debugging. Irrespective of the programming language I use, debugging is for me key when it comes to understanding the functionality of code (also for code written by someone else). One of the very first steps when digging into a new coding basis is always turning the debugging mode on - it guides you so nicely through the functions that were written (and that show you how they are connected) 😊

As a fun fact, when I wrote the initial version of my first R package, I wasn’t aware of how debugging works in R and I used a lot of `print()` statements to understand my functions. And there's nothing wrong with using `print()` (or other ways) to understand your function (and is also often the go-to solution for many people 😊) 

{{< statictweet "1569565278861774849" >}}

It works in particular well for smaller functions but once your code universe gets larger, unleashing the power of debugging is a game changer!

### 💡 So, how do you debug in R? 

Three basic commands in RStudio let you do the debugging: `debug(function_name)`, `browser()`, and `undebug(function_name)`. 

With `debug(function_name)` you start the debugging of your function - it’s basically like a mole that digs in. When you're in debug mode, you can also call the objects in your function.

![small_image](/images/single-blog/debug_flow_1_zoom.gif)
{{< detail-tag "Alternative text" >}}GIF showing how a debugger goes through each step of a function after activating it with `debug()`{{< /detail-tag >}}

If you want your debugger to stop at a certain point, you can manually set your [breakpoints in the UI](https://support.rstudio.com/hc/en-us/articles/205612627-Debugging-with-the-RStudio-IDE#stopping-on-a-line); but I’ve learned that this does not work reliably in RStudio. If you have experience with [PyCharm](https://www.jetbrains.com/pycharm/) or [VS Code](https://code.visualstudio.com), there it works smoothly!). But for RStudio, I always go for the good old `browser()` call and place it, where I want my function to stop 😊 the downside with browsers is, that you have to make sure to remove them later to have clean and fully working code. And that’s how the browser works in action:

![small_image](/images/single-blog/debug_flow_2_zoom.gif)
{{< detail-tag "Alternative text" >}}GIF showing how a breakpoint is enforced in the debugging mode using a `browser()`{{< /detail-tag >}}

Once we’re done with debugging, it’s important to turn the debug mode off - simply with `undebug(function_name)`. Or, if you’re working in a package environment hit `devtools::load_all()` or the shortcut: `Cmd` + `Shift` + `L` (or on Windows: `Ctrl` + `Shift` + `L`)

![small_image](/images/single-blog/debug_flow_3_zoom.gif)
{{< detail-tag "Alternative text" >}}GIF showing how to end a debugger using undebug(){{< /detail-tag >}}

While there is also `debugonce()`, I hardly use it because I usually go back into a function more than once. My typical workflow would then be: `debug()` > "find error, fix it" in a loop > "Cmd + Shift + L" because I usually work in a package environment.

### 👩🏼‍💻 More helpful tools for debugging

Another great tool that you can use to better understand code structures is [`{flow}`](https://github.com/moodymudskipper/flow) by [Antoine Fabri](https://github.com/moodymudskipper) - it visualizes a chart diagram of the functional architecture. Let’s take a look at this simple function that calculates the sum:

```r
make_sum <- function(a, b) {
  c <- a + b
  return(c)
}
```
<!--![small_image](/images/single-blog/function2.png)-->

But what happens under the hood? We can see it here in code language but wouldn't it be great to see it more visually? Here's `{flow}`'s moment to shine! ✨ Run this line of code and you'll get a flow diagram that shows you how the function works 🥳

```r
install.packages("flow")
library(flow)
flow_run(make_sum(a=2, b=3))
```

<!--![small_image](/images/single-blog/flow_demo1.png)
{{< detail-tag "Alternative text" >}} {{< /detail-tag >}}-->

![very_small_image](/images/single-blog/flow_demo2.png)
{{< detail-tag "Alternative text" >}}Showing the visualization output of flow. An oval container has the name and the arguments of the function, a rectangular container has the inner core of the function{{< /detail-tag >}}


This looks relatively simple, right? Well, that's the case because it IS a simple function - but it becomes more and more complex, the larger the function gets and the more sub-functions it has. 
Just try it out yourself with one of your favorite functions 👩‍💻 

If you look for more example, search on Twitter for the `#debuggingflow` hashtag that we used when I was curating the R-Ladies' Twitter account 🥳

{{< statictweet "1569728252360499201" >}}
{{< statictweet "1569742444488527872" >}}
{{< statictweet "1569809174795792385" >}}

--------------------

If you want more input, [Shannon Pileggi](https://www.pipinghotdata.com) put some [nice slides on debugging and best practices together](https://rstats-wtf.github.io/wtf-debugging-slides/#/title-slide) and [Jenny Bryan](https://twitter.com/JennyBryan) gave a [a keynote about debugging at the rstudio::conf(2020)](https://github.com/jennybc/debugging) 🗒

📝 For a summary of how debugging works and how it helps you to understand code, here’s a quick summary with a cute mole 💕 (also as 📄PDF for you to [download here](/media/CS_Debugging.pdf)): 

![small_image](/images/single-blog/CS_Debugging.png)
{{< detail-tag "Alternative text" >}}Image showing a mole as a comparison for the debugging process (a mole digs in using `debug()`, stops when there is a `browser()`, and leaves the tunnel when calling `undebug()`. It also shows how the flow package works by visualizing a flow chart.{{< /detail-tag >}}
