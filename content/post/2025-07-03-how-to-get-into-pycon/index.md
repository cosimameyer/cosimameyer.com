---
title: My Conference Journey - What I've Learned About Submitting and Reviewing Talks
author: Cosima Meyer
date: '2025-07-03'
slug: submitting-reviewing-talks-insights
categories:
  - Python
  - Python-post
tags:
  - Python-post
  - PyLadies
  - Python
postImage: images/single-blog/pycon.png
featureImage: images/single-blog/pycon.png
---

It's conference season, and my social media feed is buzzing with all the excitement! I love attending conferences, connecting with fellow tech enthusiasts, and learning from diverse perspectives.

Over the years, I've had the chance of experiencing conferences from many angles: as an attendee, an organizer of academic events, a reviewer for both academic and non-academic conferences, and most recently, as a volunteer reviewer for EuroPython and PyLadiesCon. 

Through these experiences, I've noticed common patterns that, for me, really distinguish a strong submission from a less impactful one. I've used these insights for my own abstract submissions (with a small but successful sample size!), and now I want to share them with you.

### Submission

For most (or maybe all?) Python-related conferences, [pretalx](https://pretalx.com/p/about/) seems to be the tool of choice for submitting your proposal. 

![small_image](/images/single-blog/pretalx_general.png)

{{< detail-tag "Alternative text" >}}Screenshot from pretalx showing the proposal title, the session type, the presenter and also gives a glimpse on the abstract.
{{< /detail-tag >}}

The pretalx platform is widely used, and knowing its typical structure can really help. Let's break down each section you'll encounter, from top to bottom.

#### Proposal Title

![small_image](/images/single-blog/proposal_title.png)

{{< detail-tag "Alternative text" >}}Screenshot from pretalx showing the proposal title "Code & Community: The Synergy of Community Building and Task Automation".
{{< /detail-tag >}}

Have a meaningful proposal title. I like to play with them and introduce something that may be counterintuitive. But I've also seen short ones that just get to the point. It's often a matter of taste. But what's really important is:

- **Have a meaningful self-speaking title.** The title will be the first thing your reviewers see. This will guide them and their expectations.
- **Do not use jargon.** It could be a repetition of a meaningful title but it's a bit more. If you're an expert in quantum physics, certain terms may be familiar to you. But that's not the case for your reviewer. Do them a favor and don't use them or use more widely used terms instead.

#### Session Type

![small_image](/images/single-blog/session_type.png)

{{< detail-tag "Alternative text" >}}Screenshot from pretalx showing the session type ("Talk (30 minutes)").
{{< /detail-tag >}}

Typically, you can decide whether you want to go for a shorter or a longer talk. Be realistic (we'll get later to this). If you're in-between two options, there is usually an additional field in the submission mask that allows you to add additional notes to the reviewer. When I get the feeling when reviewing proposals that they are either too long or too short, I also make recommendations for a longer/shorter sessions.

#### Abstract

![small_image](/images/single-blog/abstract.png)

{{< detail-tag "Alternative text" >}}Screenshot from pretalx showing the abstract:

"The Python community is built on a culture of support, inclusion, and collaboration. Sustaining this welcoming environment requires intentional community-building efforts, which often involve repetitive or time-consuming tasks. These tasks, however, can be automated without compromising their value—freeing up time for meaningful human engagement.

This talk showcases my project aimed at supporting underrepresented groups in tech, specifically through building Python communities on Mastodon and Bluesky. A key part of this initiative is the "Awesome PyLadies" repository, a curated collection of PyLadies blogs and YouTube channels that celebrates their work. To enhance visibility, I created a PyLadies bot for social media. This bot automates regular posts and reposts tagged content, significantly extending their reach and fostering an engaged community.

In this session, I’ll cover:
- The role of automation in community building
- The technical architecture behind the bot
- A hands-on demo on integrating Google’s Gemini into community tools
- Upcoming features and opportunities for collaboration

By combining Python, automation, and modern AI capabilities, we can create thriving, inclusive communities that scale impact while staying true to the human-centered ethos of open source."
{{< /detail-tag >}}

This is **the important part** of your submission. Your reviewer will likely spend the most time here. Think of it like a scientific abstract (where we often had a 160-200 word limit), but with an added emphasis on being engaging and accessible. What I cherish as a reviewer:

- **Have a concise abstract**. Your reviewer will likely review more than >30 abstracts. So make their lives easy by having a short but meaningful abstract.
- **Deliver one clear message**. What's the problem? Why should I care? How did you solve it (and what's new about it)? And, if possible, how can we use it for a general audience? This is the same pattern I used for academic abstracts and it feels like it also works for tech conferences. Don't overpack your abstract. Keep it short and simple.
- **Tell a story!** Even within the confines of an abstract, you can hint at a narrative. Think about the arc of your talk: a challenge, an exploration, a discovery, and a resolution. Maybe it's a "before and after" scenario, or a "here's a common problem, and here's my novel solution" approach. A good story makes your topic relatable and memorable, sticking with reviewers long after they've moved on to the next submission.
- **Again, don't use jargon**. Same logic as above. The easier it is for the reviewer to understand the content, the more likely they can assess its value.
- **Give a sneak peak into your talk's structure**. As I've done it in the example - it helps to give the reviewer a very high-level overview of your talks outline (this is optional, but I like it. It also shows that you have already made up your mind about the content)

#### Description

![small_image](/images/single-blog/description.png)

{{< detail-tag "Alternative text" >}}Screenshot from pretalx showing the description:

"My planned outline for the talk is as follows:

- **Introduction**: A brief overview of the project and its goals, focusing on community building and inclusivity within the Python ecosystem (3 minutes)
- **The Importance of Visibility**: Explain the background of the project and why visibility is important (3 minutes)
- **Bot Architecture and Setup**: A technical walkthrough of the bot, its architecture, and how it operates to extend the reach of community content on platforms like Mastodon or Bluesky (5 minutes)
- **Hands-On Demo: Task Automation with Google’s Gemini and GitHub Actions**: A step-by-step guide to integrating Google’s Gemini and GitHub Actions for creating low-barrier, automated workflows tailored for community-building tasks (12 minutes)
- **Looking Ahead**: Provide a forward-looking perspective (upcoming features of the project and future developments) (2 minutes)
- **Q&A and Buffer** (5 minutes)

I hope that the talk will inspire more Pythonistas to automate their tasks, and also more PyLadies to share material publicly and make the public perception of experts in the field more diverse."
{{< /detail-tag >}}

This part is something I have never seen at academic conferences, but it's where I see great leverage to showcase the quality of your submission. It's your chance to give reviewers a deeper insight into your proposed talk. I always use it (and have seen others use it effectively) to highlight my outline, ideally including time stamps. While the final structure might shift slightly, this detailed agenda helps reviewers understand exactly what you plan to deliver and whether it fits within the envisioned time slots. Plus, writing it out forces you to assess if you have enough content, or, more likely, if you need to reduce it.

- **Check whether the allocated time slots include time for Q&A** or if this is extra (less likely) and include it in your detailed agenda.
- **Calculate time for buffer**. I always add X% of buffer time to each slot and then try to round it in a good way.
- **Keep it short and simple.** Again, as for the other parts where there is free text: Be mindful with the workload of your reviewers. The more concise you can deliver your message, the better!

#### Notes (For Reviewers)

There is typically also an optional Notes section. This section is not meant for the public but it's where you address the organizers or the reviewers. When I'm in-between two time slots, I address it there.

#### Remaining Parts

The remaining parts may differ, depending on where you submit your proposal. Typically, you'll need to assess the expected audience expertise with respect to the domain and Python, add some more info and, during the last years, there's also been this question: "Did you use an LLM, e.g. ChatGPT, to help you with this proposal?"

![small_image](/images/single-blog/use_of_llms.png)

{{< detail-tag "Alternative text" >}}Screenshot from pretalx showing the question "Did you use an LLM, e.g. ChatGPT, to help you with this proposal?"
{{< /detail-tag >}}

I think this part is important - and it's free for everyone to use whichever tool they prefer. What I want to caution against is to overuse it. I like the support of LLMs when it comes to proof-reading or finding a nice pun-title based on a list of ideas I previously had. This means: I'm bringing in my own creativity, adding my personal style to it and tweak it a bit. So it feels more like brainstorming with a friend about my submission. 

But I've reviewed proposals in the past where I felt like they written with a LLM. They sounded nice but weren't meaningful. Some conferences now also have a field where your reviewer can set a check mark when they sense this happened. 

It's easy to generate a submission this way but for the reviewers it may then be hard to assess the quality of your talk. Plus, depending on which model you use, all submissions will sound so similar without your own personal twist to it.

So again, feel free to use them but don't overdo it!

### Share Your Submission

Similar to co-brainstorming with an LLM, you'll typically find a link in the submission process that allows you to share your unsubmitted proposal with a friend. I feel like this is a really valuable feature to get feedback from someone you trust who sees your proposal in a similar environment as the reviewers will see it later.

### Reviewing Process

Reviews typically happen on a voluntary basis (and conferences are typically short on reviewers). So if you have time and capacity, feel free to sign up when a call is out! They are typically announced on social media or in mailing lists. The organizers will most likely be very thankful because it reduces the burden on the other reviewers shoulders.

Beyond the good karma you'll undoubtedly earn, reviewing also gives you a fantastic opportunity to glimpse what's new and upcoming in the field. Most calls for reviewers ask you to review at least 20-25 proposals, but in reality, you often end up with more (if you have the time and capacity to do so) due to the frequent shortage of volunteers.

If you do the math and think of a reviewer who spends roughly 10 minutes per submission (depending on the complexity, the text length, and other influencing factors), this will make approximately 4 hours for 25 proposals. And, at least that holds for me, you won't do this in one sitting because reviewing proposals is in fact cognitively demanding. That shouldn't distract you from signing up as a reviewer (it really is a great experience!) but it should rather caution you when submitting your proposal to keep it as concise and meaningful as possible.

Each reviewer will then rank your proposal (the scale may differ, depending on the organizers). 

### Post-Reviewing Process

Sometimes organizers also run a post-review voting process. This allows to get the wider community involved where they vote on a subset of pre-selected proposals. I think this is an interesting step in the selection process and, if made well, definitely makes sense since it allows to tailor towards the greater community's interests.

### Accepted?

Amazing, congratulations! 🎉 
This is a big achievement and you should be proud of yourself! 

Once the conference is getting closer, don't panic. Standing in front of a large(r) audience may not be everyone's cup of tea. But that's ok. From my experience, the community is extremely supportive and you're there for a reason. They liked your submission and I'm sure you have a good story to tell!

And speaking of stories, remember the one you hinted at in your abstract? Now's the time to bring it to life! People remember stories far more than dry facts. Think about what made you passionate about this topic in the first place, or a funny anecdote from your development process. Adding humor, relatable struggles, and moments of discovery will not only make your talk more engaging but also help your audience connect with you and your message on a deeper level. Your audience wants to learn, yes, but they also want to be entertained and inspired. So, go out there, share your expertise, and tell _your_ story!
