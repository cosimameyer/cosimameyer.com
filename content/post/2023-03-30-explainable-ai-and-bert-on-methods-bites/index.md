---
title: Explainable AI and BERT on Methods Bites
author: Cosima Meyer
date: '2023-03-30'
slug: [explainable-ai-bert]
categories:
  - R-post
  - Python-post
  - NLP
  - ML
  - XAI
tags:
  - NLP
  - ML
  - XAI
postImage: images/single-blog/bert_embed.png
featureImage: images/single-blog/bert_embed.png
---



Together with [Andreas Küpfer](https://andreaskuepfer.github.io) I spent the last months writing and thinking about how to best explain BERT (using [huggingface](https://huggingface.co)), apply it to a typical social science problem, and show the functionality of explainable AI. When working with large language models, there is often the problem of "black boxes" that make it difficult to explain (and understand) the behavior of the model. While working in academia, I was fascinated by the possibilities of understanding what models do and why. There are model agnostic methods like [breakdown plots] (https://cosimameyer.com/post/how-to-adjust-labels-in-flashlight-light-breakdown-plots/), which I used in my dissertation to add local explainability to my models. I think it's an undervalued concept that we will hopefully see a lot more of in research papers (and beyond). 

Throughout the blog post, we dive into the concept of integrated gradients to add some explainability to our BERT model, and provide a hands-on application using [`Transformers Interpret`](https://github.com/cdpierse/transformers-interpret), a Python library built on top of [Captum](https://captum.ai), which provides explainability and interpretability for PyTorch models. Finally, we raise some challenges and opportunities for explainable AI, both in academia and in our everyday lives. 

We tailored the blog post for a diverse audience and provided a reading guide for the different experience levels of the readers. The blog post is available at [Methods Bites](https://www.mzes.uni-mannheim.de/socialsciencedatalab/article/bert-explainable-ai/) and comes with a [Jupyter notebook hosted on Google Colab](https://colab.research.google.com/drive/1fkQG8Px6Ug69lcexKUrEPHpr_tfo_qCd?usp=sharing#offline=true&sandboxMode=true) that allows you to run the code yourself. Here we also take advantage of how nicely R and Python can work together.

And, as the icing on the cake, we use illustrations to visualize more complex concepts and make them (hopefully) easily accessible. Here's a little taste:

![small_image](https://github.com/cosimameyer/illustrations/blob/main/nlp/BERT_Embedding_blue.png?raw=true)

{{<detail-tag "Alternative text">}} The visualization shows how BERT understands the text.
With BERT, you identify the order of the input. You give the model information about different embedding layers (the tokens (BERT uses special tokens ([CLS] and [SEP]) to make sense of the sentence), the positional embedding (where each token is placed in the sentence), and the segment embedding (which gives you more info about the sentences to which the tokens belong).{{</detail-tag>}}</br>

If this made you curious, here's a short wrap-up of all essential links:

- [Blog post](https://www.mzes.uni-mannheim.de/socialsciencedatalab/article/bert-explainable-ai/)
- [Google Colab Jupyter Notebook](https://colab.research.google.com/drive/1fkQG8Px6Ug69lcexKUrEPHpr_tfo_qCd?usp=sharing#offline=true&sandboxMode=true)
- [Illustrations](https://github.com/cosimameyer/illustrations)