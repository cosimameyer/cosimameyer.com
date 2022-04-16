---
title: How to set up a working HTTPS encryption with Netlify, blogdown, and rbind.io
author: Cosima Meyer
date: '2021-06-18'
slug: how-to-set-up-a-working-https-encryption
categories: [R-post, R, blogdown, Hugo, Netlify]
tags:
subtitle: ''
summary: ''
authors: []
featured: yes
projects: []
---

If you host a website, it is worth considering using the secure `HTTPS` encryption instead of `HTTP` (also in light of the GDPR).[^1] As someone who is less experienced in setting this up, this can be challenging, particularly if you use a domain that you do not personally own. This post is meant to give a (really) short step-by-step guide for those in a similar situation.  

### The situation

I build my homepage using the wonderful [`blogdown` package](https://bookdown.org/yihui/blogdown/) with a [customized version of the Hugo Academic theme](https://cosimameyer.rbind.io/post/how-to-integrate-a-typing-effect-in-hugo-academic/). To deploy the website, I rely on [Netlify](https://www.netlify.com/), which works like a charm! Since I did not want the default `netlify.app` domain (suffix) to be my domain, I [oppened an issue to request the `rbind.io` suffix](https://github.com/rbind/support/issues/674). Setting this up is extremely straightforward and well explained [here](https://support.rbind.io/about/). Well, now that this worked, I wanted to be able to use `HTTPS` instead of `HTTP`. And here, the challenge began.

### The solution 💡

In retrospect, it was all nicely explained in the answer to my issue, which I was not aware of back then. I hope this post will pop up for future users and point them quickly to the solution.

The [excellent description by Yihui Xie](https://yihui.org/en/2017/11/301-redirect/) describes everything what you need to do (and to know). Here is what I did using my own words:

1. Open your website `.Rproj` in RStudio
2. Open a new plain `Text File`
3. Copy the following line and replace the word `domain` so that it matches your website name (for me, it would be `cosimameyer`). 

```bash
http://domain.rbind.io/*   https://domain.rbind.io/:splat  301!
```

4. Save the file as `_redirects` (no suffix needed) in the `static` folder of your website. This is how the (simplified) root overview of your folder should look like:

```
├── R
├── README.md
├── assets
├── config
├── config.toml
├── content
│   ├── about
│   ├── authors
│   ├── courses
│   ├── home
│   ├── post
│   ├── project
│   ├── publication
│   ├── slides
│   └── talk
├── index.Rmd
├── libs
├── Your-website.Rproj
├── netlify.toml
├── public
├── resources
├── static
│   └── _redirects
└── themes
```

You'll see that `_redirects` is located directly in the `static` folder. 
<!-- Here's my [`_redirects`](https://github.com/cosimameyer/my-website/blob/master/static/_redirects) file.  -->

Well, and that's it -- this little trick did the magic for me! 🪄

[^1]: For a short overview of the difference between `HTTPS` and `HTTP`, see [here](https://www.cloudflare.com/en-gb/learning/ssl/why-is-http-not-secure/).
