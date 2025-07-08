---
title: Google I/O Connect 2025
author: Cosima Meyer
date: '2025-07-08'
slug: google-io-connect-2025
categories:
  - Python
  - AI
  - Python-post
tags:
  - Python-post
  - AI
postImage: images/single-blog/welcome_sign_io_connect.png
featureImage: images/single-blog/welcome_sign_io_connect.png
---

![small_image](/images/single-blog/welcome_sign_io_connect.png)

{{< detail-tag "Alternative text" >}}
A sign showing "Google I/O Connect" and a relatively blue sky.
{{< /detail-tag >}}

I had the chance to attend Google's I/O Connect in Berlin, and it was an impressive event! 
For me, the true joy of working in technology comes from the people you connect with. This year, I had the pleasure of experiencing that not only at PyConDe & PyData but also at Google I/O Connect in Berlin!

Queuing at the entrance during cloudy weather (it got better, I promise! Berlin's weather is just unpredictable), I met Nisanur, and we stuck together for the rest of the event. Thanks so much for the lovely company, Nisanur!

![small_image](/images/single-blog/io_connect.png)

{{< detail-tag "Alternative text" >}}
Green background (plants) with the Google I/O Connect sign and a bear (for Berlin) in the front.
{{< /detail-tag >}}

### My Personal Highlights from I/O Connect

So, let's dive into my personal highlights from a day packed with insightful talks, excitement, and curiosity.

#### The Vision: Keynote and Big Announcements

The welcome addresses and keynote truly set the tone. I loved the **diversity of the keynote** – it felt like every part of Google was represented, from AI to web, cloud, and Android. There were engaging live demos, inspiring stories about making an impact, and forward-looking thoughts on what's next.

![small_image](/images/single-blog/model_evolution.png)

{{< detail-tag "Alternative text" >}}
A slide shown during the keynote highlighting the evolution of (ML) models/approaches. 
{{< /detail-tag >}}

The keynote also included a short recap of the evolution of Google's models. Seeing "old companions" like Word2Vec on the list, and then tracing the trajectory these models and approaches have taken, highlighted how relatively new these advancements are—and yet, how powerful those 'older' approaches still remain!

#### Deep Dives and Hands-On Learning: Workshops

What I liked most about I/O Connect was that it wasn't "just" a conference; it offered **hands-on workshops** where you could dive into the latest features of the Gemini model suite or ADK (more on that in a bit). While workshops filled up quickly (first come, first served!), I was lucky enough to attend Ian Ballantyne's workshop on "5 Practical Gemini API Uses For Developers." We explored various applications of the Gemini model suite, and one in particular really stuck with me.

A while ago, I set up a pipeline to extract information from handwritten forms – a classical OCR pipeline with quality checks for dates and employee numbers. Seeing (even in a mini example) how this could now be done using **Gemini 2.5 Flash** was mind-boggling, especially compared to the amount of code I wrote for those quality checks. I know we touched upon just one form during the workshop, and the devil's in the details, but it left an impact on me.


#### Cutting-Edge AI: ADK, Protocols, and Gemma

##### ADK

Working with other agentic frameworks in my day job, learning alternative approaches is so fascinating! Wietse Venema did an amazing job introducing the [Agent Development Kit (ADK)](https://google.github.io/adk-docs/) and walking us through some live-coding (big kudos to everyone who did it at I/O Connect - it's so great for the audience and I know how nerve-wrecking this can be!). I also liked how a different approach of thinking of agentic workflows was brought up: distinguishing between being "procedural" and "dynamic" - those working in sequential orders and those working in a more dynamic way.

I also love how you can "look under the hood" directly in the local web UI without needing to switch to additional frameworks like LangSmith - but perhaps that's a topic for another blog post! 🤫

If you want more, Wietse shares more insights [here](https://wietsevenema.eu/).

##### MCP vs A2A

Protocols such as [**MCP**](https://modelcontextprotocol.io/introduction) and [**A2A**](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/) are the "new kids on the block" - and they were definitely a hot topic at I/O Connect!

In short, MCP (short for Model Context Protocol) is the USB-C port for your AI application. It allows your AI system to interact with external tools, data sources, and resources - you can think of it as a standardized way for an AI model to access the "outside world" and get the context it needs to perform tasks.

A2A (short for Agent2Agent Protocol), on the contrary, focuses on AI agents communication and coordination. So, while MCP offers a vertical uplift by deepening an agent's individual capabilities, A2A provides a horizontal improvement, enabling more collaborative workflows between agents.

All in its early stages but it looks promising! Google announced that [GenAI SDK now support also MCP definitions](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol)

##### Gemma

I'm a big fan of open and light-weight models. That's why I'm really fond of [Gemma](https://deepmind.google/models/gemma/) and the work the team is doing. As part of the keynote, Gus Martins highlighted that [Gemma 3n runs on as little as 2GB RAM](https://developers.googleblog.com/en/introducing-gemma-3n/). 
This is particularly fascinating because one of the significant ecological footprints of large models comes from their usage. 

The reason Gemma 3n can be so lightweight lies in its selective parameter activation, which leverages the [Matryoshka Transformer architecture](https://arxiv.org/pdf/2310.07707) to selectively activate the model's parameters per request. This way, they are able to reduce compute cost and response times. [Here's](https://ai.google.dev/gemma/docs/gemma-3n#matformer) more on it.

I'm not a big photo person, but we had to take a shot when meeting Gus next to Android :)

![small_image](/images/single-blog/io_connect_photo.png)
{{< detail-tag "Alternative text" >}}
Photo showing Android, Gus Martins, Nisanur Ilhan and Cosima in the "forest".
{{< /detail-tag >}}

#### The I/O Connect Experience: Community and Atmosphere

Joining I/O Connect as part of [**Women Techmakers Ambassadors**](https://developers.google.com/womentechmakers/ambassadors) was one of the best perks! Women Techmakers Ambassadors are a global community of open and tech-enthusiastic people who enjoy getting together and promoting diversity in the field. I got to meet many familiar faces I'd only seen online so far – and I truly hope our paths will cross again!

![small_image](/images/single-blog/io_connect_badge.png)
{{< detail-tag "Alternative text" >}}
A Google I/O Connect event badge.
{{< /detail-tag >}}

Beyond the fascinating tech, let's take a moment to appreciate the incredible **atmosphere** of the event. Can we talk about how cool the forest inside the hall looked? And don't even get me started on the food and coffee... it was a bit like being in heaven!

![small_image](/images/single-blog/android_forest.png)
{{< detail-tag "Alternative text" >}}
An indoor forest installation at the event venue in the background with an Android statue in the front.
{{< /detail-tag >}}

![small_image](/images/single-blog/ice_cream.png)
{{< detail-tag  "Alternative text" >}}
Ice cream in the form of a dinosaur served at the event.
{{< /detail-tag >}}

### Want to Explore More?

If you missed Google I/O 2025, [there are many on-demand videos](https://io.google/2025/explore/) for you to watch and to explore!

Overall, Google I/O Connect was an enriching experience that left me inspired and excited about the future of technology and our community 🫶
