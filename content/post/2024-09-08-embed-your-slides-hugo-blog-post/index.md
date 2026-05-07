---
title: How to Embed Your Slides in a Hugo Blog Post
author: Cosima Meyer
date: '2024-09-08'
slug: embed-your-slides-hugo-blog-post
categories:
  - Hugo
tags:
  - Hugo
postImage: images/single-blog/embed_slides.png
featureImage: images/single-blog/embed_slides.png

---

Who spends way too much time making beautiful slides? 🙋🏼‍♀️ 

When I give a talk, I like to share my slides with others. Typically, I upload them to my website and add them to the "Talk" section, embedding a link that takes interested people to the deck. But what if you want to highlight your slides in a blog post? 

For this, I added a `slideshow.html` file to `layouts/shortcodes/slideshow.html`. 

To do this, first create the file:

```shell
touch slideshow.html
```

Then add the following content to the newly created file: 

```html
<iframe src="{{.Get 0}}" width=100% height="400" frameborder="0" scrolling="auto" allowfullscreen="allowfullscreen"></iframe>
```

For example, calling `{{</* slideshow "LINK_TO_SLIDE" */>}}` (and replacing `LINK_TO_SLIDE` with the location of your slides) will allow your slides to be displayed within your blog. The reader can then browse through the slide deck by clicking on the deck and using the arrow keys to navigate. 

This works for many different formats - here are a few examples and how to embed them:

### Quarto-Based
```markdown
{{</* slideshow "https://cosimameyer.com/slides/pyladiescon-2023/#/title-slide.html#1" */>}}
```

{{< slideshow "https://cosimameyer.com/slides/pyladiescon-2023/#/title-slide.html#1" >}}

These slides are [Quarto-based](https://quarto.org/docs/presentations/) and the link has more explanation if you want to play around with them yourself. 

### R-Markdown Based

But it also works with other slides such as [RMarkdown-based versions](https://github.com/yihui/xaringan).

```markdown
{{</* slideshow "https://cosimameyer.com/slides/correlcon2021/talk.html#1" */>}}
```

{{< slideshow "https://cosimameyer.com/slides/correlcon2021/talk.html#1" >}}

### Google Slides

Other HTML-based slides, such as Google Slides, can also be embedded:

```markdown
{{</* slideshow "https://docs.google.com/presentation/d/1jGU_I6YTkVtlyFvyac83UmjkontzzvaqPcveEVA35dw/embed?start=false&loop=false&delayms=3000" */>}}
```

{{< slideshow "https://docs.google.com/presentation/d/1jGU_I6YTkVtlyFvyac83UmjkontzzvaqPcveEVA35dw/embed?start=false&loop=false&delayms=3000" >}}

A quick tip if you want to embed your Google Slides: 
1. Publish them to the web (File > Share > Publish to web)
2. Copy the link; replace `pub` with `embed` (see example) and use the link

### PDF

We can also use the same shortcode to embed PDFs. Be sure to place your slides in your `static/` folder as well. With PDF, your reader will scroll through the slides:

```markdown
{{</* slideshow "/slides/Career_Talk_2024.pdf" */>}}
```

{{< slideshow "/slides/Career_Talk_2024.pdf" >}}


And that's all you need! ✨




