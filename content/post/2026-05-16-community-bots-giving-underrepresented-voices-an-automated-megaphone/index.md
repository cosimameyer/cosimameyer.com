---
title: "Community Bots: Giving Underrepresented Voices an Automated Megaphone"
author: Cosima Meyer
date: '2026-05-16'
slug: community-bots-giving-underrepresented-voices-an-automated-megaphone
categories:
  - Python
  - Python-post
  - Community
tags:
  - Python
  - PyLadies
  - RLadies
  - Bluesky
  - Community
  - Automation
postImage: images/single-blog/rladies-pyladies-bots.png
featureImage: images/single-blog/rladies-pyladies-bots.png
---

I started the community-bots project because I kept noticing how much great content exists in the PyLadies and RLadies+ communities - blog posts and open-source libraries - and how little of it may travel beyond each community's immediate circle. This feels like a missed opportunity and so I built bots to help.

The [community-bots project](https://github.com/cosimameyer/community-bots) runs two bots on Bluesky - one for [PyLadies](https://bsky.app/profile/did:plc:cyhjdt4mp7h4c2ufw3nwcqqx) and one for [RLadies+](https://bsky.app/profile/rladies-bot.bsky.social) - that automatically share community-created content on a regular schedule.

### What the Bots Do

Each bot does a few different things. It boosts posts tagged with `#pyladies` or `#rladies`, so content from community members gets a wider audience without anyone having to do anything extra. It also draws from two curated lists - [awesome-pyladies-creations](https://github.com/cosimameyer/awesome-pyladies-creations) and [awesome-rladies-creations](https://github.com/rladies/awesome-rladies-creations) - and shares blog posts and open-source libraries from community members on a rolling basis. Beyond that, the bots share portraits of women in tech from the [Amazing Women in Tech gallery](https://gallery.cosimameyer.com/amazing-women-in-tech/) and celebrate community achievements.

### How It All Works

The whole setup lives in a GitHub repository and uses GitHub Actions as a CRON trigger. When the schedule fires, Python scripts pull from the curated lists, figure out what hasn't been posted yet, and compose the post. I also added a Google Gemini integration that generates short summaries of blog posts before sharing them, giving readers a sense of what a post is about before they click through.


![small_image](/images/single-blog/architecture_bots.png)
{{< detail-tag "Alternative text" >}}Diagram showing the bot architecture: GitHub Actions triggers a Python script on a regular schedule. The script gathers RSS feed content, calls the Gemini API to generate a summary, then combines the title, author, hashtags, link, and summary into a post that is published to Bluesky.{{< /detail-tag >}}

If you want to go deeper on the technical side, I wrote about [building the bots](/post/2023-09-17-building-mastodon-bots-and-promoting-the-community-part-2/) and [adding Gemini](/post/how-to-add-google-gemini-to-your-python-project-that-makes-use-of-github-actions/) in earlier posts.

### Where Things Are Now

The bots have grown to 1,900+ followers on Bluesky combined (as of May 2026) and are posting regularly. The Mastodon bots are currently paused while I look for a new instance to host them - if you have ideas or can help, feel free to [open an issue](https://github.com/cosimameyer/community-bots/issues/new/choose) or reach out.

I also had the chance to talk about the project at [PyConDE/PyData 2025](https://drive.google.com/file/d/1vMlaJ3vbV7ONJg24dqBFIUp6sJUjYL_u/view) and [PyLadiesCon 2023](https://bit.ly/pyladiescon2023-slides), which was a lot of fun.

### Want to Get Involved?

The bots depend on the curated lists that feed them, and those lists are open for contributions. If you know of a PyLadies or RLadies+ blog, a library built by a community member, or a woman in tech worth highlighting, I'd love to hear from you - and so would the bots! You can find everything at [cosimameyer.github.io/community-bots](https://cosimameyer.github.io/community-bots/).
