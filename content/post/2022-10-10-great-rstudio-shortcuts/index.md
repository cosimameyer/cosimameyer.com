---
title: Great RStudio Shortcuts
author: Cosima
date: '2022-10-10'
slug: [great-rstudio-shortcuts]
categories: [R, R-post]
tags: [R, R-post]
---

There are so many [RStudio shortcuts out there](https://support.rstudio.com/hc/en-us/articles/200711853-Keyboard-Shortcuts) but these are my TOP5 that I regularly use 😊

I love them because they usually make your life easier. The first one allows you to add a new R code chunk in your Rmd/Quarto file using `Option` + `Cmd` + `I` on a Mac (or `Ctrl` + `Alt` + `I`). And this is exactly what the GIF shows:

![small_image](/images/single-blog/shortcut_chunk.gif)

-----

The next one makes writing a type operator so much simpler! At first, it feels a bit like looking for the keys but once you have it inherited, you probably won't want to go back 😊 So instead of typing `%>%` you can now use `Cmd` + `Shift` + `M` on a Mac (or `Ctrl` + `Shift` + `M`)

![small_image](/images/single-blog/shortcut_pipe.gif)

----- 
Well formatted code can be so beautiful! And there are guidelines for this in R - but it's also implemented as a shortcut in RStudio 🤩 If you highlight the code you want to format and then hit `Cmd` + `Shift` + `A` (or `Ctrl` + `Shift` + `A`), the magic will happen 🪄

![small_image](/images/single-blog/shortcut_format.gif)

One short disclaimer: It's powerful and great but it may (in particular if you write SQL statements or heavily comment your code) also destroy the structure of your code. So use it wisely 🤓

----- 

And the next one is for renaming/replacing multiple objects with the same name at once. Highlight one of the variable names (for instance) and hit `Option` + `Cmd` + `Shift` + `M` (or `Alt` + `Ctrl` + `Shift` + `M`). You can now start typing and replacing both objects at the same time.
 
 ![small_image](/images/single-blog/shortcut_rename.gif)

-----

Since I often work in a package environment, the last shortcut allows me to avoid typing `devtools::load_all()` by using `Cmd` + `Shift` + `L` (or `Ctrl` + `Shift` + `L`) to reload the package. This is particularly helpful when you are currently debugging a function and want to avoid typing the `undebug()` call. 

<!--
### How you define your own shortcut

RStudio is really flexible and lets you define your [own shortcuts](https://support.rstudio.com/hc/en-us/articles/206382178-Customizing-Keyboard-Shortcuts-in-the-RStudio-IDE). Go to "Tools" > "Modify Keyboard Shortcuts ..."-->