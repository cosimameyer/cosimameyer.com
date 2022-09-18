---
title: Understanding the Basics of Package Writing in R
author: Cosima Meyer
date: '2022-10-17'
slug: understanding-the-basics-of-package-writing-in-r
category: [R, R-post]
postImage: images/single-blog/CS_Package.png
featureImage: images/single-blog/CS_Package.png
---

Writing a package sounds big - and it can for sure be. But in its simplest form, it’s not that much more than putting a function in a package structure. The R community is great and came up with multiple great helpers that make your life easier!

### 💡 What’s in an R package? 
Simply speaking, an R package allows you to put functions in a box and make them available for others to use. Ideally, your R package also comes with unit tests that make sure that your package works (or if it doesn't throw meaningful errors and let you dive into the functions and explore why it doesn't) and it adheres to the common standards of developing a package. 

### 🕵🏼‍️ Essential components of an R package

A package structure usually consists of 

- a `DESCRIPTION` file that gives the user all core information about your package
- a `NAMESPACE` file that contains information on exported and imported functions
- an `R` folder where multiple R files live that contain your functions (it's usually a good idea to write one R file per function and to also call the file name as you call your function. Trust me, this makes debugging (and also maneuvering through your functions) so much easier 🥳)
- a `man` folder that contains all manuals for your functions (it will be automatically generated once you created functions and called `devtools::document()`)
- a `tests` folder where your unit tests live (this is where the [`{testthat}`](https://testthat.r-lib.org) package is extremely helpful!)


### 👩🏼‍💻 How to build your package?

RStudio is great, just follow these steps: Select "File", "New Project...", "New Directory" and select "R Package". You can now give your R package a meaningful name, select a path and hit "Create project". 

![small_image](/images/single-blog/setup.png)
{{< detail-tag "Alternative text" >}} Showing RStudio wizard when creating a new package {{< /detail-tag >}}

You're now ready to go! Once executed, you have a fully functional package structure in your  Rproject (that we already discussed) 😊 Now it's time to move your function to your `R/` folder and populate it!

![small_image](/images/single-blog/structure.png)
{{< detail-tag "Alternative text" >}} Showing how the RStudio desktop version looks like with a typical package structure (gitignore, Rbuildignore, DESCRIPTION, man, NAMESAPCE, R/) {{< /detail-tag >}}

Let's do this with a simple `make_sum()` function: 

```r
make_sum <- function(a, b) {
  c <- a + b
  return(c)
}
```

Open a new package environemtn, add a new file called `make_sum.R` in the `R/` folder and copy-paste the code from `make_sum`. Now there's already the very first function that lives in the package 🤩 Calling now `devtools::load_all()` allows us to use the function.

![small_image](/images/single-blog/set_up_package.gif)
{{< detail-tag "Alternative text" >}}There is an R file called "make_sum.R" with the function code to calculate the sum. We run "devtools::load_all()" and call "make_sum(1,3)" and get our result ("4") {{< /detail-tag >}}

This is the very beginning of a package! 

👩🏼‍🏫 I collected these steps to develop your package (and more) in my talk at [CorrelAid's CorrelCon 2021](https://bit.ly/pkg-development).


###  Helpful tools

When building your R package, you can luckily rely on the work of others who provide an excellent framework to get you started:

- 📦 [{devtools}](https://devtools.r-lib.org):  A collection of great functions to make developing your package easy
- 📦 [{usethis}](https://usethis.r-lib.org): A library to automate your package and project setup
- 📦 [{roxygen2}](https://roxygen2.r-lib.org): This package does many things in the background and you won't often realize it's there: it allows you to describe your functions and your package and to produce accompanying standard files that are good practice when creating your package
- 📦 [{testthat}](https://testthat.r-lib.org): Helps you set up your unit tests
- 📦 [{xpectr}](https://github.com/LudvigOlsen/xpectr): Extremely helpful package when you need inspiration for standardized unit tests
- 📦 [{covr}](https://covr.r-lib.org): Tells you more about your test coverage (visually)
- 📦 [{goodpractice}](http://mangothecat.github.io/goodpractice/): It checks whether your package is consistent with common practices and - the best part - it gives you really helpful tips and recommendations on where and how to improve 😍
- 📦 [{inteRgrate}](https://jumpingrivers.github.io/inteRgrate/): It's a package by [Jumping Rivers](https://www.jumpingrivers.com) that gives some more tests to check that your package adheres to a good style

If you want to know what else is out there, [Indrajeet Patil](https://sites.google.com/site/indrajeetspatilmorality/) collected [many great packages (and resources) that can help you to build your package](https://bit.ly/awesome-r-package-tools). 🖥

------------

If this inspired you and you want to write your package now, there are plenty of great resources out there! 

📺 Several R-Ladies talked about how to set up your package within no time:

- ["Making and publishing your first R package" (Sarah Popov) @ R-Ladies Bergen](https://www.youtube.com/watch?v=9ZKGLv84nZY)
- ["Introduction to Package Development in R" (Shelmith Kariuki) @ R-Ladies Nairobi](https://www.youtube.com/watch?v=WAzqMAnElmw)
- ["DIY R Package" (Dr Fonti Kar) @ R-Ladies Sydney](https://www.youtube.com/watch?v=4-9jX5F_l8g)
- ["R package development" (Maëlle Salmon) @ R-Ladies East Lansing and R-Ladies Chicago](https://www.youtube.com/watch?v=IlWMkz769B4)

📑 [Dennis Hammerschmidt](http://dennis-hammerschmidt.rbind.io/) and I wrote a [blog post](https://bit.ly/r-package-development) about our experience when building our package [{overviewR}](https://github.com/cosimameyer/overviewR) and (hopefully) an easy how-to guide that also features more resources at the end (and it also comes with a checklist that I always use when I update our package and before sending it to [CRAN](https://cran.r-project.org)).

📝 While it's hard to put everything on one page, here's my attempt to summarize the most important things to know about package development in R (also as 📄PDF for you to [download here](/media/CS_Package.pdf)): 

![small_image](/images/single-blog/CS_Package.png)
{{< detail-tag "Alternative text" >}} A summary reiterating the basic structure in package development (DESCRIPTION, NAMESPACE, R/, man/, and tests/) as well as helpful packages (devtools, use this, roxygen2, testthat, xpectr, cover, goodpractice, inteRgrate) reiterating the previous tweets. {{< /detail-tag >}}
