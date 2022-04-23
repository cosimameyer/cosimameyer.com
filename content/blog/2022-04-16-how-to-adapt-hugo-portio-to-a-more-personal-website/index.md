---
title: Tweaking Hugo Portio theme
author: Cosima Meyer
date: '2022-04-16'
slug: [tweaking-hugo-portio-theme]
categories: []
tags: [blog, blogdown, CSS, HTML]
postImage: images/single-blog/hero_section.png
featureImage: images/single-blog/hero_section.png
Code: True
---

New year, new website? Well, not really. But when starting to set up and move [RAM's website](), I decided that it was probably also time for a fresh look on my own. As always, coming up with *the* perfect theme and moving everything to your new and fully polished website takes longer than expected. The main reason is mainly that it is hard to settle on *the* perfect theme. Spoiler alert, there is no such thing. At least not for me. It *always* involves tweaking and learning new things in JS, CSS, HTML, and all these things I have never been trained in before. But isn't learning new things part of the fun? Well, I collected all the essential tweaks that I made to the [Hugo Portio theme](https://github.com/StaticMania/portio-hugo) to make it a good fit for me. I hope my future self (and hopefully also someone else) will find it useful when looking for an answer or some ideas on how to tweak the theme.

---

### Adjust "about" section

To center my image on the landing page, I replaced the first `col-lg-6` class with `container centered` in the `layouts/partials/hero.html` file. To keep the image right-aligned but to increase the image size, I replaced the second `col-lg-6` with `col-lg-9`.

I also removed everything in the `hero.html` that calls the button or a video (so for instance `<a type="button" class="btn btn-lg btn-primary btn-zoom" href="{{ .buttonURL | absURL }}">{{ .buttonName }}</a>` or `{{ if .videoURL }} ... {{ end }}`). 

Adding now a section like the following allows me to add social media icons next to my image.

```html
<div class="table">
    <ul id="horizontal-list">
      <li><a href="{{ .mail | absURL }}" target="_blank" rel="noopener"><p style="color:#282f49"><i class="fa fa-envelope" style="font-size: 2em;"></i></p></a></li>
      <li><a href="{{ .github | relURL }}" target="_blank" rel="noopener"><p style="color:#282f49"><i class="fa fa-github" style="font-size: 2em;"></i></p></a></li>
      <li><a href="{{ .linkedin | relURL }}" target="_blank" rel="noopener"><p style="color:#282f49"><i class="fa fa-linkedin" style="font-size: 2em;"></i></p></a></li>
      <li><a href="{{ .twitter | relURL }}" target="_blank" rel="noopener"><p style="color:#282f49"><i class="fa fa-twitter" style="font-size: 2em;"></i></p></a></li>
      <li><a href="{{ .orcid | relURL }}" target="_blank" rel="noopener"><p style="color:#282f49"><i class="fa fa-book" style="font-size: 2em;"></i></p></a></li>
    </ul>
</div> 
```

And you would also add the following parts to your `.css`/`.scss`:

```css
.table {
	display: table;
	margin: 0 auto;
	text-align: center;
}

i {
  display: inline-block;
  margin-right: 1em;
}

ul#horizontal-list {
	min-width: 696px;
	list-style: none;
	padding-top: 20px;
	}
	ul#horizontal-list li {
		display: inline-block;
	}
```

My `hero.yml` looks like this:

```yml
---
enable: true
topTitle: 
content: >
  #  Hello, I am Cosima
  
  I love to work with data

github: https://github.com/cosimameyer
linkedin: https://www.linkedin.com/in/cosimameyer/
twitter: https://twitter.com/cosima_meyer
image: images/hero/avatar.jpg
```

And the website is like this:

![small_image](/images/single-blog/hero_section.png)

---

### Typing effect

Adding my beloved typing effect couldn't be easier. I already explained how I did it for [Hugo Academic]() and just did the very same thing for this theme.

1. Add `<script src="https://cdn.jsdelivr.net/npm/typeit@7.0.4/dist/typeit.min.js"></script>` to the top part of the `hero.html`
2. Add `<p class="multipleStrings"></p>` to the `hero.html` where you want the typing effect. It's a placeholder and will be filled in a second.
3. Add the following snippet to the bottom of the `hero.html`:

```html
<script>
  new TypeIt(".multipleStrings", {
    strings: ["This is a great string.", "But here is a better one."],
    breakLines: false,
    loop: true,
    speed: 50
  }).go();
</script>
```

4. Replace "This is a great string." and "But here is a better one." with the text you want to see typed. For me, it was "I love to work with data."
5. (Possibly) Remove the sentence you just included in the snippet from your `hero.yml` (otherwise it will show up twice).

And that's it, here's your typing effect 🥳

![small_image](/images/single-blog/typing-new.gif)

---

### Talk section
The Portio theme does not come with a talk section by default. I thus adjusted the `testimonialSection.html` so that it is a good fit.

The main part of the file now looks like this:

```html
<p class="testimonial__slider_item-content"> <b>{{ .title }}</b> </p>
          <img src="{{ .thumbnail }}" class="portfolio-item-thumb" alt="" />
          
          {{ if .github }}
          <a href="{{ .github | safeURL }}" target="_blank" rel="noopener"> <i class="fa fa-github"></i>{{"GitHub"}}</a>
          {{ end }}
          
          {{ if .slides}}
          <a href="{{ .slides | safeURL }}" target="_blank" rel="noopener"><i class="fa fa-clipboard"></i> {{"Slides"}}</a>
          {{ end }}
          
          {{ if .recording}}
          <a href="{{ .recording | safeURL }}" target="_blank" rel="noopener"><i class="fa fa-youtube"></i> {{"Recording"}}</a>
          {{ end }}
                      	
	        <p class="testimonial__slider_item-content"> {{ .comment | markdownify }} </p>
          
          <p class="testimonial__slider_item-author"><a href="{{ .event_url | safeURL }}" target="_blank" rel="noopener"><span>{{ .event_name }}</span></a> | {{ .time }}</p>
```


And this is what the `testimonialSection.yml` input file can look like:

```yml
  - event_name:       R-Ladies Bergen
    thumbnail:        images/talks/nlp-rladies-bergen.png
    title:            Taking text data to the next level - Using supervised and unsupervised approaches in NLP
    comment:          Add some Description
    time:             December 20, 2020
    github:           https://github.com/cosimameyer/nlp-rladies-bergen
    slides:           https://cosimameyer.rbind.io/slides/nlp-rladies/talk#1
    recording:        https://youtu.be/bvqur70ZmyM
    event_url:        https://www.meetup.com/rladies-bergen/events/274392883/
```

The snippet allows the user to add an `event_name` (usually the conference name or the organizer), a `thumbnail` reference to include an image, the `title` of the talk, a `comment` section to be able to include some more description (in markdown), the `time` of the event, a `github` link to show the code, `slides` for a link to the slide deck, `recording` to add the recording link and `event_url` to link the event page.

In the following case, I didn't add a description but the rest looks like this:

![smaller_image](/images/single-blog/testimonial.png)

---

### Add a link section to the portfolio

The portfolio section links to a short description where you can showcase your service, your client, the challenge, and your solution. I think this is a fantastic approach to also presenting a data science portfolio. My projects are mainly open-source, so there is usually no "real" client involved and instead of services, I usually rely on languages. So this needed some tweaking. After adjusting the `portfolio/single.html` for the main part like this:

```html
  <div class="container">
    <div class="row mb-5">
      <div class="col-lg-10 offset-lg-1 text-center">
        <div class="case-details-title">
          <h1>{{ .Title }}</h1>
        </div>
        <p>
          {{ .Params.shortDescription }}
        </p>
        <div class="case-details-info">
          <div class="case-details-info-item">
            <h5 class="text-light">Programming<br>language</h5>
            <p>{{ .Params.language }}</p>
          </div>
          <div class="case-details-info-item">
            <h5>Field</h5>
            <p>{{ .Params.field }}</p>
          </div>
          {{ if .Params.links }}
          <div class="case-details-info-item">
            <h5>Links</h5>
            {{ if .Params.website}}
            <a href="{{ .Params.website | safeURL }}" target="_blank" rel="noopener"><p style="color:#7e7e8a"><i class="fa fa-globe"></i> {{"Website"}}</p></a>
            {{ end }}
            {{ if .Params.publication}}
            <a href="{{ .Params.publication | safeURL }}" target="_blank" rel="noopener"><p style="color:#7e7e8a"><i class="fa fa-book"></i> {{"Publication"}}</p></a>
            {{ end }}
            {{ if .Params.github }}
            <a href="{{ .Params.github | safeURL }}" target="_blank" rel="noopener"><p style="color:#7e7e8a"><i class="fa fa-github"></i> {{"GitHub"}}</p></a>
            {{ end }}
            {{ if .Params.slides}}
            <a href="{{ .Params.slides | safeURL }}" target="_blank" rel="noopener"><p style="color:#7e7e8a"><i class="fa fa-clipboard"></i> {{"Slides"}}</p></a>
            {{ end }}
            {{ if .Params.recording}}
            <a href="{{ .Params.recording | safeURL }}" target="_blank" rel="noopener"><p style="color:#7e7e8a"><i class="fa fa-youtube"></i> {{"Recording"}}</p></a>
            {{ end }}
            </div>
            {{ end }}
```

I had the following options for each `portfolio.md` file (here's an example of the Telegram bot):

```md
---
date: "2020-07-20T00:00:00Z"
thumbnail: images/portfolio/telegram-bot.png
title: Telegram Bot
challenge: Learning complex concepts can be demanding and it might be hard to remember everything. Research has shown that repetition can help.
solution: A bot that sends you flashcards every morning and evening in Telegram using Python and AWS Lambda.
language: Python, AWS
field: Software development
github: https://github.com/dennis-hammerschmidt/telegram-bot
links: true
---
```

The `date` option gives a date, with `thumbnail` I can link an image, `challenge` describes a challenge that I was facing, and `solution` the respective solution. These were also the default parameters that Hugo Portio comes with. My tweak addressed the options `language`, `field`, `github`, `links` (and, if needed, also `slides`, `recording`, `publication`, and `website`). `language` allows you to add the programming languages and in `field` you can specify which areas the project covers (like software development, ML, ...). If the toggle `links` is set to `true`, the theme will now automatically add all the content that you presented in `github`, `slides`, `recording`, `publication`, and `website`. The output including links will look like this:

![small_image](/images/single-blog/output-with-links.png)

And without links like this:

![small_image](/images/single-blog/output-without-links.png)

If you further add a `| markdownify` after challenge and solution (for instance `{{ .Params.solution | markdownify }}`), you can pass markdown-based text and the theme automatically converts markdown links. 

---

### Remove pattern

The default theme has a few dynamic patterns (bubbles and patterns of crosses) moving up and down. I like the dynamic effect the bubbles generate because they are more transparent and clean (you can see both patterns in the right upper corner of the image below). 

![small_image](/images/single-blog/pattern.png)

If you are like me and don't enjoy the cross patterns so much, just look out for 

```html
  <div class="animate-pattern">
    <img src={{"images/service/background-pattern.svg" | absURL }}
    alt="background-shape">
  </div>
```

and either delete it or use `<!-- Pattern -->` to remove it.


---

### GDPR and cookie consent

[Hugo](https://gohugo.io/about/hugo-and-gdpr/) has a great guide on how to disable all services which I followed. 

If you further want to add a cookie consent banner, I followed [Felipe's guideline](https://littlebigtech.net/posts/hugo-gdpr-cookie-consent-banner/), added the `cookie-consent.html` file, and the `{{- template "partials/templates/cookie-consent.html" . }}` line to `footer.html`.

---

### Details tag

To add the option to unfold code with the common `<details> <summary>Summary text</summary> HERE COMES SOME TEXT </details>` logic, I added a `detail-tag.html` file to `layouts/shortcodes/` which contains:

```html
<details>
  <summary>{{ (.Get 0) | markdownify }}</summary>
  {{ .Inner | markdownify }}
</details>
```

Calling now 

`{{< detail-tag "Summary text" >}} HERE COMES SOME TEXT {{< /detail-tag >}}` 

generates this:

{{< detail-tag "Summary text" >}} 
HERE COMES SOME TEXT 
{{< /detail-tag >}}

---

### Adjust image size
Images are, by default, quite large in the Portio theme. I added the following code to `_common.scss`:

```css
img[alt=smaller_image] { 
    display: block;
    float: none;
    margin-left: auto;
    margin-right: auto;
    max-width: 100%;
    height: auto;
    width: 500px; }
    
img[alt=small_image] { 
    display: block;
    float: none;
    margin-left: auto;
    margin-right: auto;
    max-width: 100%;
    height: auto;
    width: 700px; }

img[alt=very_small_image] { 
    display: block;
    float: none;
    margin-left: auto;
    margin-right: auto;
    max-width: 100%;
    height: auto;
    width: 30%;}
```

It defines three different sizes of the image that I can use by calling on the `alt` parameter in the image. Here are examples (from top to bottom):

![small_image](/images/single-blog/pythonistr.png)

![smaller_image](/images/single-blog/pythonistr.png)

![very_small_image](/images/single-blog/pythonistr.png)

I prefer using HTML code to adjust the image size and leave the `alt` parameter to what it is designated to -- the alternative text. But I couldn't find a way to make it work in this case. 

---

### Add Clipboard Buttons to the code

I like the integration that GitHub offers - to follow this example, I used [Justin's tutorial](https://digitaldrummerj.me/hugo-add-copy-code-snippet-button/) and implemented a "Copy" button that shows up next to code lines.

```python
print('This is a great example how to copy your code')
```

---

Well, that's what I have done so far to make it a better fit for me - but there are for sure plenty more options. I did, for instance, not yet include a section on publications (not sure if I will do it in the future because there are always services like [Google Scholar](https://scholar.google.de) or [ORCID](https://orcid.org) that have a good overview of your publications). If you have any other ideas or suggestions, feel free to reach out. I'm always curious to learn more cool things ☺️