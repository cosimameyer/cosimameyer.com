---
title: Adding your Hugo Academic blog to R-bloggers and Python-bloggers
author: Cosima Meyer
date: '2021-08-10'
slug: adding-your-hugo-academic-blog-to-r-bloggers-and-python-bloggers
category: [R-post, R, Python-post, Python]
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
postImage: images/single-blog/pythonistr.png
featureImage: images/single-blog/pythonistr.png

---

![very_small_image](/images/single-blog/pythonistr.png)

<!--<img style="float: right;" src="featured.png" alt="Drawing of an R (as a pirate) and a snake (Python)" width="250"/>-->

When you have your website up and running and probably already started a little blog about data science related topics, you may want to share it with an even broader community. Here, [R-bloggers](https://www.r-bloggers.com) and [Python-bloggers](https://python-bloggers.com/) are a great venue!

Both websites publish a collection of R and Python-related RSS feeds (amongst other, [Methods Bites](https://www.r-bloggers.com/author/r-on-methods-bites/) and the [cynkra](https://www.r-bloggers.com/author/patrick-schratz-kirill-muller/) blog are also part of it!). To stay up to date, you can sign up on their newsletters ([R-bloggers](https://www.r-bloggers.com/about/), [Python-bloggers](https://python-bloggers.com/about/)) or follow them on Twitter ([R-bloggers](https://twitter.com/rbloggers/), [Python-bloggers](https://twitter.com/pythonbloggers))  and Facebook ([R-bloggers](https://www.facebook.com/rbloggers/), [Python-bloggers](https://www.facebook.com/pythonbloggers/)). In short, these feeds have an incredibly rich collection of R and Python-related knowledge you can also easily contribute to!

But how? [Both websites offer a nice description](https://www.r-bloggers.com/add-your-blog/) of what to do to integrate your own RSS feed, but I was looking for a more specific solution how to integrate my Hugo-(Academic)-based blog.  

When googling, I found various solutions (see for instance [here](https://lmyint.github.io/post/hugo-academic-tips/#full-content-rss) or [here](https://yongfu.name/2018/12/13/hugo_rss.html)). I followed [Leslie Myint's approach](https://lmyint.github.io/post/hugo-academic-tips/#full-content-rss) that she found and added the following lines to my `config.toml`.[^2] 

```
[outputs]
  home = [ "HTML", "RSS", "JSON", "WebAppManifest" ]
  section = [ "HTML", "RSS" ]
  taxonomy = [ "HTML", "RSS" ]
  taxonomyTerm = [ "HTML", "RSS" ]
```

In my case, there was not much more I needed to change (anymore).[^1] 

I categorize all blog posts (as I also have non-R/Python-related posts and want to keep R and Python-bloggers clean and tidy). It's also in their policies where both feeds request to feed in only ["meaty" posts and to avoid submitting short snippets](https://www.r-bloggers.com/add-your-blog/). To access all R and Python-related posts, I use the categories `R-post` and `Python-post`. I sometimes make short (data science) posts that link to new Methods Bites posts, for instance, or also have a [blog post with helpful code snippets](https://cosimameyer.rbind.io/post/helpful-r-commands/), which I do not want to include in the feeds. 

I call the following URL to access these posts: `https://cosimameyer.rbind.io/category/r-post/index.xml` (or `https://cosimameyer.rbind.io/category/python-post/index.xml` for Python posts). If you want to check how your RSS feed with R posts looks like, enter it [here](https://simplepie.org/demo/). The image below shows a screenshot of my results when entering my RSS feed for the category "R-post," including a preview of one post.

![small_image](/images/single-blog/rss-feed.png)

And that's it! If this page looks good and your full blog post is displayed, you're almost good to go. Check if your feed coheres to the requirements and add the required info to [the form](https://www.r-bloggers.com/add-your-blog/) -- now it's just waiting (a bit) until it's checked and accepted 😊

![smaller_image](https://media.giphy.com/media/GiWEowj3nQv9C/giphy.gif)

[^2]: For home, the original first line only included the following elements: `home = ["HTML", "CSS", "RSS"]` but since my default setting also contained `JSON` and `WebAppManifest`, I left it as it was and just added the additional lines for `taxonomy` and `taxonomyTerm`. As a short disclaimer: I am not entirely sure whether I needed to add these two new lines to `[outputs]`. But since including them worked without any problem, I left them in.
[^1]: This [GitHub issue](https://github.com/wowchemy/wowchemy-hugo-modules/issues/144) gave me the solution. You can automatically access your RSS feed because RSS is enabled by default.