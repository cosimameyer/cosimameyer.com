---
title: Building Mastodon Bots and Promoting the Community - Part 1
author: Cosima Meyer
date: '2023-04-25'
slug: [building-mastodon-bots-and-promoting-the-community-part-1]
postImage: images/single-blog/awesome-pyladies.png
featureImage: images/single-blog/awesome-pyladies.png
category: [R, R-post, Python, Python-post, Community, Mastodon]
tags: [R, Python, PyLadies, RLadies, Community, Mastodon]
---

I spent the last weeks building two small Mastodon bots with the goal in mind to make contributions by [PyLadies](https://pyladies.com) and [R-Ladies](https://www.rladies.org) more visible. PyLadies and R-Ladies are global non-profit organizations that seek to empower and promote greater diversity in the coding world.

The project started when I saw a post on LinkedIn that mentioned several data people to follow - all being male. When asked why there wasn’t more diversity on the list, the post author replied that they should get more visible to be recognized - so I decided to do my share here. 

What was initially an idea, became the development of two bots (a [PyLadies bot](https://botsin.space/@pyladies_bot) and an [R-Ladies bot](https://botsin.space/@rladies_bot)), [a curated blog list](https://github.com/cosimameyer/awesome-pyladies-blogs) with awesome PyLadies on it, and me spending hours reaching out to PyLadies and encouraging them to put their blogs on the list. And I can tell you: It's been great weeks - seeing all the beautiful websites, learning about, and "meeting" new people is always fantastic. It also showed me (once more) how multi-faceted the community is and how much there is to learn! 🤓

I'm planning on writing a series of blog posts that will cover the entire process:
 - Setting up the Awesome PyLadies‘ Blogs Repository (and extending the R-Ladies list)
 - Building Mastodon Bots
 - Deploying Mastodon bots using AWS and GitHub Actions
 
When I started thinking about the bots, I had (and still have) many ideas in mind. While reposting posts that mention a specific hashtag is more or less straightforward and doesn't require any additional input -- but sharing R-Ladies' and PyLadies' blog posts does require a (curated) list with RSS feeds. Luckily there was already the [Awesome R-Ladies' Blog Repository](https://github.com/rladies/awesome-rladies-blogs) in place that I could use to extend and collect RSS feeds for the blogs by R-Ladies. I contacted all the R-Ladies that were already on [the list](https://github.com/rladies/awesome-rladies-blogs) and asked them for their consent to add their RSS feeds and Mastodon handles.

### Setting up the Awesome PyLadies' Blogs Repository 

And this is when also the Awesome PyLadies' Blogs Repository started 🚀 

[![small_image](/images/single-blog/awesome-pyladies.png)](https://github.com/cosimameyer/awesome-pyladies-blogs)
{{< detail-tag "Alternative text" >}}Screenshot of the Awesome PyLadies' Blogs repository showing some text (What is this repository about) and images with a list of blogs (arranged as a grid){{< /detail-tag >}}

I am incredibly thankful that I could build upon the Awesome R-Ladies' Blog Repository. So here are some credits due: It is deeply inspired by the [Awesome R-Ladies' Blogs Repository](https://github.com/rladies/awesome-rladies-blogs) (which is the entire underlying basis). Without this repository, I wouldn't have been able to get so quickly up-and-running with the repository - so all credit goes to them!

### What is the repository about?
As mentioned above, the repository relies heavily on the Awesome R-Ladies blogs repository and I can only repeat their words of what the repository is about. The goals include collecting PyLadies' blogs and making PyLadies more visible. This includes those who identify as a minority gender (including but not limited to cis/trans women, trans men, non-binary, genderqueer, & agender). With your submission, you agree that these entries will be used for the PyLadies' Mastodon bot, which will post (new) PyLadies' blog entries to promote the work of PyLadies around the world.

### How did I find all the PyLadies?
That was more challenging than expected and I went with various channels.
First, I posted in different Slack groups as well as on Mastodon/Twitter about the GitHub repository and encouraged people to add their blogs to the list. But this didn't seem to work as well as expected. So I went on and reached out to people individually. 
To find people, 
- I browsed past Python conferences and their speakers' lists, got the names (ideally a website was linked - but often it wasn't), googled the people and figured out whether they had a website (and a blog), reached out to them (by Slack, e-mail, contact forms, or LinkedIn)
- I checked who posted about Python conferences on both Twitter and Mastodon
- I checked Medium, Mastodon, and Twitter and looked for keywords such as "pyladies" 
- I tracked down the GitHub profiles of those who contributed to the PyLadies repositories and looked for blogs
- of course, I also used ChatGPT (but it didn't work as well as expected - it gave me lists full of made-up names or made-up websites. I cannot tell whether some of the websites worked in the past but I'm also blaming it on hallucination 😵‍💫 (and also on my prompt writing))

And while I was already on it, I did the same for R-Ladies and also reached out to them 💜 I also extended the Awesome R-Ladies list by reaching out to those who weren't on the list and encouraged them to join. If you didn't get a message from me, please feel free to reach out (or add the information yourself, if you want ☺️).

I’m truly sorry if I reached out to you more than once - it’s because, I have to admit, keeping track of so many beautiful blogs is challenging and I think yours should be on the list! ☺️

If you have more ideas, please let me know - I'm feeling a bit like Sherlock who is thinking about new ways how to find people 🕵🏼‍♀️

<br><br><br><br>
### How do you submit your blog?
It is (hopefully) easy and all described [here](https://github.com/cosimameyer/awesome-pyladies-blogs/blob/main/CONTRIBUTING.md). In a nutshell, there are two options:
- **Open an issue** with your blog info and I'll create the JSON for you
- **Create a new file and a PR with your blog info** (there's even a [short cut link](https://github.com/cosimameyer/awesome-pyladies-blogs/new/main/?filename=blogs/your-blog-url.com.json&value=%7B%0A%20%20%22title%22%3A%20%22Your%20title%22%2C%20%2F%2Frequired%0A%20%20%22subtitle%22%3A%20%22subtitle%20or%20tagline%22%2C%20%2F%2Foptional%0A%20%20%22type%22%3A%20%22blog%22%2C%20%2F%2Frequired%0A%20%20%22url%22%3A%20%22https%3A%2F%2Fyour_blog.com%22%2C%20%2F%2Frequired%0A%20%20%22photo_url%22%3A%20%22https%3A%2F%2Fyour_blog.com%2Fyour_photo.png%22%2C%20%2F%2Frequired%0A%20%20%22description%22%3A%20%22Short%20description%20of%20what%20you%20blog%20about%22%2C%0A%20%20%22language%22%3A%20%22en%22%2C%20%2F%2Frequired%0A%20%20%22rss_feed%22%3A%20%22%5Burl%5D%2Ffile.xml%22%2C%20%2F%2Frequired%0A%20%20%22authors%22%3A%20%5B%20%2F%2Frequired%0A%20%20%20%20%7B%0A%20%20%20%20%20%20%22name%22%3A%20%22Your%20Name%22%2C%20%2F%2Frequired%0A%20%20%20%20%20%20%22social_media%22%3A%20%5B%7B%0A%20%20%20%20%20%20%20%20%20%22twitter%22%3A%20%22username%22%2C%0A%20%20%20%20%20%20%20%20%20%22mastodon%22%3A%20%22%40username%40server.org%22%2C%0A%20%20%20%20%20%20%20%20%20%22github%22%3A%20%22username%22%2C%0A%20%20%20%20%20%20%20%20%20%22instagram%22%3A%20%22username%22%2C%0A%20%20%20%20%20%20%20%20%20%22youtube%22%3A%20%22username%2Fend-url%22%2C%0A%20%20%20%20%20%20%20%20%20%22tiktok%22%3A%20%22username%22%2C%0A%20%20%20%20%20%20%20%20%20%22periscope%22%3A%20%22username%22%2C%0A%20%20%20%20%20%20%20%20%20%22researchgate%22%3A%20%22username%22%2C%0A%20%20%20%20%20%20%20%20%20%22website%22%3A%20%22url%22%2C%0A%20%20%20%20%20%20%20%20%20%22linkedin%22%3A%20%22username%22%2C%0A%20%20%20%20%20%20%20%20%20%22facebook%22%3A%20%22username%22%2C%0A%20%20%20%20%20%20%20%20%20%22orcid%22%3A%20%22member%20number%22%2C%0A%20%20%20%20%20%20%20%20%20%22meetup%22%3A%20%22end-url%22%0A%20%20%20%20%20%20%7D%5D%0A%20%20%20%20%7D%0A%20%20%5D%0A%7D) that you can use to create the file)

All details of what can be included and which format is best are provided in the [contribution guidelines](https://github.com/cosimameyer/awesome-pyladies-blogs/blob/main/CONTRIBUTING.md). If there are problems with the PR, opening an issue, or something else - please feel free to reach out! We'll make our way through the GitHub adventure together ☺️

If you have a website or blog feed yourself, please feel free to submit it to the repositories! We're happy about every single contribution ☺️
- [Awesome PyLadies' Blogs Repository](https://github.com/cosimameyer/awesome-pyladies-blogs)
- [Awesome R-Ladies' Blogs Repository](https://github.com/rladies/awesome-rladies-blogs)

And as always, I'm more than happy about feedback or suggestions. Please feel free to reach out to me or open an issue! ☺️
