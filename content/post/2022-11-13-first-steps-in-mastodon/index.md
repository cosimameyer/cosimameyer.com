---
title: First steps in Mastodon
author: Cosima Meyer
date: '2022-11-13'
slug: [first-steps-in-mastodon]
categories: [Mastodon, Social Media]
postImage: images/single-blog/mastodon_hello.png
featureImage: images/single-blog/mastodon_hello.png

---

With the events in the past [days/weeks/months](https://www.theguardian.com/technology/2022/nov/12/elon-musk-twitter-chaos-enleashed), more and more people decided to give [Mastodon](https://www.theguardian.com/technology/2022/nov/12/joining-the-herd-whats-it-like-moving-from-twitter-to-mastodon) a try. 

This post is meant by no means to be a complete guide -- I'm still a very beginner with this (for me) new platform. I see it more as a constant WIP where I collect tips and tricks and complement my understanding along the way of discovering "the other side".

For all those of you who are thinking of moving to Mastodon, the best words to describe it over there: friendly and calm 🐘 
My experience has been only positive so far (it may of course change, the longer I'm there - but let's hope not 😉). The only glitch I experienced: The phone app is a bit laggy and my instance had some capacity/performance issues but I'm optimistic it will improve. 
One thing one has to always keep in mind: it's voluntary-based and it’s impressive what people have set up and keep running 👏

With Mastodon, social media is thought differently and it feels much more like a quiet haven compared to what a busy Facebook or Twitter timeline looks like. I enjoy being over here and my feeling is that conversations are more important than writing posts that generate likes. When you start using Mastodon, it really feels like some kind of a Pied Piper vision coming true - and the best part (I think) is actually that it doesn’t have an algorithm that feeds your "news feed" or your "timeline" but it’s really the people you follow and the topics you’re interested in.

I hope we’ll all be able to maintain this environment (although some changes may be inevitable (and maybe also positive) with so many people coming over and forming the network). I'm curious about what the future holds for us 🤗

But how do you get started if you also want to join the party? 

![very_small_image](/images/single-blog/mastodon_hello.png)
{{< detail-tag "Alternative text" >}}Image an elephant/mastodon in blue holding a "hello" sign.{{< /detail-tag >}}

### Choosing an instance

It's easy to choose between instances (the first question you'll encounter when thinking about signing up). There's a [questionnaire/filter function that suggests possible ones](https://t.co/9wEHJSh5ZR). While the choice is important (Mastodon is decentralized and an influx of users on "your chosen server instance" can affect your experience), I didn’t overthink it. Things change so quickly these days and, as far as I understand, [migration within Mastodon is also generally possible](https://t.co/PSImD4lQph). And choosing one server instance over another doesn’t mean that you’re limited to your specific instance and cannot interact with others. You can interact with (and follow - unless they restrict it) everyone on Mastodon across servers! 

### Housekeeping at Mastodon

If you have your Mastodon account now up and running, add a bio 😊 This helps people to get to know you and it feels more like a social gathering than an anonymous network.
You can also add your Mastodon handle to your Twitter profile so that people can find you.

### Migrate your contacts 

To find your contacts on Mastodon, there are great tools such as [debirdify](https://pruvisto.org/debirdify/). The steps are simple: 

1. Authorize access to your Twitter account
2. Click "Search followed accounts" (you can also go for blocked or muted accounts)
3. Download the CSV file.
4. Import them into your profile on Mastodon within Settings > Import and export > Import. Select the respective category to which you want to upload the CSV file. I went with the default option ("Merge")

![smaller_image](/images/single-blog/screenshot_mastodon.jpg)
{{< detail-tag "Alternative text" >}}Screenshot showing the import function of Mastodon where you can select to upload people you are following from Twitter{{< /detail-tag >}}

And now you'll have to wait a bit (it took a few hours for me but it worked 😊). With more and more people moving, it might make sense to repeat this step now and then. If you're still wondering which server instance you should use, the app also shows you where your people are currently at Mastodon. So this might be also a good starting point. It can look like this:

![small_image](/images/single-blog/screenshot_debirdify.png)
{{< detail-tag "Alternative text" >}}Image showing a donut chart with the distribution of users across server instances.{{< /detail-tag >}}

### Tooting at Mastodon

While it's a decentralized system where servers have their own rules and manners, here’s a [nice summary of how tooting works at Mastodon](https://axbom.com/mastodon-tips/). 

One important thing, I think, is to keep the server capacities in mind when sharing large files such as videos (I’m also guilty of it - funded social media platforms easily spoil you 😉). When sharing these files, it might be best to upload them outside of the fediverse - but I haven’t figured out a good place for explanatory GIFs yet (for instance the GIF below):

![small_image](/images/single-blog/git_flow.gif)
{{< detail-tag "Alternative text" >}}GIF showing the commands git add, git commit, git push, git pull in a sequential order{{< /detail-tag >}}

I believe [GIPHY](https://giphy.com) might not be the right place, so uploading them to the static folder of your website might be an option. But I’m more than open to suggestions!

#### Netiquette: Introduce yourself

And now, and that's definitely one thing I really like about Mastodon: there's a great netiquette of introductions. So just introduce yourself by adding the hashtag `#introduction` 🤗 (and pin it to your profile)

#### Cross-posting at Twitter and Mastodon

If you want to cross-post on both services, there are also helper tools for this such as [crossposter](https://crossposter.masto.donte.com.br) or [Moa](https://t.co/0yF6DbEpKy).

#### The search function

The search function does not work as you expect it to. It doesn't have a full text search. So it's best to bookmark a toot once you see (and like) it. 

<!--If you want to find someone on another Mastodon instance, you have to add the instance to the person's handle - but there's a [Firefox extension](https://addons.mozilla.org/en-US/firefox/addon/mastodon-simplified-federation/) that helps you skip this step. -->

### Verification at Mastodon

If you want to have a verification sign (it's not a blue check mark but green and for free ✅), you can add 
`<a rel="me" href="instance/your_mastodon-handle"></a>` on your website and link the website to your profile. I use Hugo Portio and added it to the `head.html` file. [Here's more to it](https://cedricbatailler.me/blog/how-verification-works-on-mastodon/) and [Alison Hill]() also Hugo also updated the Apéro theme.

{{< statictweet "1591166271353225221" >}}

It also works for your GitHub page if your Mastadon page is the only website you're referring to in your GitHub profile (adding the reference to your README doesn't work - unfortunately).

### Writing DMs

Writing direct messages is tricky (I didn’t fully figure it out yet, to be honest). 
Here’s [a good guide on how it works (and why you should rethink the use of private messages on social media platforms)](https://t.co/NIQKSuFUiy). 

But with all the contact info that people provide in their bios, it’s also easy to reach out to them in other ways - which is a nice feature 😊

### And more

For all the academics (and more) who are looking into Twitter data and will likely miss [`{rtweet}`](https://cran.r-project.org/web/packages/rtweet/index.html), [`{rtoot}`](https://cran.r-project.org/web/packages/rtoot/index.html)[^1] is on CRAN 🎉 

{{< statictweet "1591103455699296256" >}}

And, if you're curious about what your Twitter data looks like and want to get some insights out of them, [Observable](https://twitter.com/observablehq) has a [nice framework (and guide) for you](https://observablehq.com/@observablehq/save-and-analyze-your-twitter-archive).




[^1]: Tooting is tweeting at Mastodon.
