---
title: R-Ladies & PyLadies - Bringing the Communities Together
author: Cosima
date: '2022-07-31'
slug: r-ladies-and-pyladies-bringing-the-communities-together
category: [R, Python, Python-post, R-post]
postImage: images/single-blog/rladies-pyladies.jpg
featureImage: images/single-blog/rladies-pyladies.jpg
---

Together with [PyLadies Tunis](https://twitter.com/pyladiestunis) and [PyLadies Munich](https://twitter.com/pyladiesmunich) we kicked off a fantastic event last Wednesday - and I'm still overwhelmed and touched by the impressions.

When I started the [R-Ladies Cologne](https://twitter.com/rladies_cologne) chapter in 2021, I envisioned an event that seeks to bring both R and Python communities together. From my experience, both communities are so generous and helpful and I wanted to overcome the (why the heck this ever started existing) "divide" between both languages. Instead, I wanted to show how great multilingualism can be and how Python and R can complement each other. And I believe that, with the help of our amazing speakers [Rhian Davis](https://twitter.com/StatsRhian) and [Deena Gergis](https://www.linkedin.com/in/deena-gergis/), we made a step towards this direction.[^1] If you didn't have a chance to attend our event, here's a short wrap-up that hopefully inspires you ✨

-----

Rhian showed us how she programs [bilingually at work](https://statsrhian.github.io/presentations/2022-07-27-tale-of-two-languages/2022-07-27-tale-two-languages.pdf) and told us how [Jumping Rivers](http://jumpingrivers.com) overcomes possible hurdles and benefits from working as a bilingual team. And most importantly, as Rhian said, it's not about the language you should think of but of the great people you will be working with 😊

<!--{{< statictweet "1552357448488521728" >}}-->

-----

Deena then walked us through the [MLflow framework and showed us how to use it in action](https://github.com/Deena-Gergis/mlflow_tracking) and how it can be applied to both languages. [MLflow](https://www.mlflow.org/docs/latest/index.html) is a powerful library that allows you to manage end-to-end machine learning cycles. Deena showed us a demo of a typical workflow both theoretically and practically. If you're up for trying it out yourself, have a look at Deena's [GitHub repository](https://github.com/Deena-Gergis/mlflow_tracking) where you'll get everything you need.

<!--{{< statictweet "1552354284662149127" >}}-->

-----

Along with the input talks, we had two quiz rounds showing the audience how similar both languages are and telling some tips and tricks. 

<!--{{< statictweet "1552356844491915266" >}}-->

If you want to revisit them yourself, here are the questions and answers (and a bit more) 🤓

{{<detail-tag "What are (originally) specific IDEs for Python and R?">}} ![smaller_image](/images/single-blog/ide.png) While you can also work in other languages in RStudio, it was originally [developed for R](https://www.rstudio.com/products/rstudio/) (as the name tells, but it will be soon renamed to [Posit](https://www.rstudio.com/blog/rstudio-is-becoming-posit/) - signalling it's inclusiveness 💜) {{</detail-tag>}}</br>

{{<detail-tag "Which packages can you use to call the other language?">}} ![smaller_image](/images/single-blog/call.png) If you want to see `RPy2` in action, check out the question with the Jupyter notebook - we have a demo there!{{</detail-tag>}}</br> 

{{<detail-tag "How do you assign objects in R and Python? (Good coding style 😉)">}} ![smaller_image](/images/single-blog/assigned.png) Teams may have varying preferences and working guidelines, but according to [Hadley Wickham's style guide](http://adv-r.had.co.nz/Style.html), it's preferred to use the `<-` operator for assignments in R (although `=` also works in R). {{</detail-tag>}}</br>

{{<detail-tag "What is the main repository for libraries?">}} ![smaller_image](/images/single-blog/lib.png) If you ever wanted to develop a [R package](https://www.mzes.uni-mannheim.de/socialsciencedatalab/article/r-package/) or [Python library](https://py-pkgs.org) yourself and contribute to the repositories, follow the links to get detailed guides how to do it.  {{</detail-tag>}}</br>

{{<detail-tag "Can you use R in a Jupyter Notebook?">}} ![smaller_image](/images/single-blog/jupyter.png) And, as promised, [here's a short demo](https://www.youtube.com/watch?v=yhY50Akyk3A&list=PL3rWokTqpOz1S_oltCrgD_yS_3cvoX7cp&index=2) showing you how to use the `RPy2` library in action. {{</detail-tag>}}</br>

{{<detail-tag "With which index does an array start?">}} ![smaller_image](/images/single-blog/index.png) All you have to remember (and it can easily become the source of some errors when switching between both languages: the R index is 1-based, Python's index is 0-based. If you want to know more, here's a [short post by Benjamin Smith ](https://www.linkedin.com/pulse/r-python-analysis-index-benjamin-smith).{{</detail-tag>}}</br>

{{<detail-tag "What are the commands to use to know the type of a variable in R and Python?">}} ![smaller_image](/images/single-blog/type.png) Here's more on the [class() function in R](https://stat.ethz.ch/R-manual/R-devel/library/base/html/class.html) and the [type() method in Python](https://www.geeksforgeeks.org/python-type-function/).{{</detail-tag>}}</br>


<!--{{< statictweet "1552347731858948097" >}}-->

We are incredibly thankful to [aiven](https://aiven.io/) and [O'Reilly](https://www.oreilly.com/) for sponsoring the prizes for our quiz.

-----

And last but not least, big shoutout to my co-organizers [Amal Tlili](https://twitter.com/AmalTlili20), [Hédia Tnani](https://twitter.com/TnaniHedia), [Laysa Uchoa](@laysauchoa), and [Mouna Belaid](https://twitter.com/mounaa_belaid) - organizing this event with you showed me once more how fun it is to work with people from the community and how much dedication there is. It was a big pleasure starting this journey with all of you 💜

<!--{{< statictweet "1552510588911341568" >}}-->

I'm very much looking forward to what's coming next!

-----

If you're up for more material, we collected everything [here](https://github.com/rladiescologne/r-and-python-bridging-communities):

📺 [YouTube recording](https://www.youtube.com/watch?v=F1n35M3Uihk&t=1s)

👩‍💻 [Deena's MLflow workflow](https://github.com/Deena-Gergis/mlflow_tracking)

🖥 [Rhian's slides](https://statsrhian.github.io/presentations/2022-07-27-tale-of-two-languages/2022-07-27-tale-two-languages.pdf)

[^1]: This also fits so nicely with the topics discussed at this year's [rstudio::conf2022](https://appsilon.com/posit-rstudio-rebrands/) as well as at [JuliaCon 2022](https://twitter.com/Rami_Krispin/status/1553458177680613377). It shows, how important it is for us to think beyond the "superiority" of one language over another but to embrace different approaches and treasure them. 
