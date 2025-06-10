---
title: Are AI Models Hallucinating—Or Just Confabulating?
author: Cosima Meyer
date: '2025-06-10'
slug: are-ai-models-hallucinating-or-just-confabulating
categories:
  - AI
  - Machine Learning
  - NLP
  - Python
  - R
  - Data Science
tags:
  - Blog post
  - ML
  - NLP
  - Python
  - R
  - Data Science
postImage: images/single-blog/hallucination_vs_confabulation.png
featureImage: images/single-blog/hallucination_vs_confabulation.png
---

![small_image](/images/single-blog/hallucination_in_the_news.png)
{{< detail-tag "Alternative text" >}}Image showing media snippets highlighting the term "hallucinations". Links to the media are as follows:

- Forbes (2025): [Why AI 'Hallucinations' Are Worse Than Ever](https://www.forbes.com/sites/conormurray/2025/05/06/why-ai-hallucinations-are-worse-than-ever/)
- LA Times (2025): [AI 'hallucinations' are a growing problem for the legal profession](https://www.latimes.com/business/story/2025-05-22/ai-lawyers-hallucinations-a-growing-problem-for-lawyers)
- Reuters (2025): [Trouble with AI 'hallucinations' spreads to big law firms](https://www.reuters.com/legal/government/trouble-with-ai-hallucinations-spreads-big-law-firms-2025-05-23/)
- The Conversation (2025): [Neurosymbolic AI is the answer to large language models' inability to stop hallucinating](https://theconversation.com/neurosymbolic-ai-is-the-answer-to-large-language-models-inability-to-stop-hallucinating-257752)
- The New York Times (2025): [A.I. Is Getting More Powerful, but Its Hallucinations Are Getting Worse](https://www.nytimes.com/2025/05/05/technology/ai-hallucinations-chatgpt-google.html)
- The Washington Post (2024): [Can AI police itself? Experts say chatbots can detect each other’s gaffes.](https://www.washingtonpost.com/technology/2024/06/20/ai-chatbots-hallucinations-study/)

{{< /detail-tag >}}

These days, it feels like the term _hallucinations_ is everywhere---from media headlines to blog posts, technical reports, and peer-reviewed papers. At its core, the term _hallucination_ is often used to describe what happens when a large language model (LLM) generates an answer that’s incorrect, misleading, or just plain nonsensical.

But thinking about the term itself, I started to wonder whether hallucination is really the right word to describe this well-known issue in natural language generation with LLMs. After some research and discussion, I don’t think it is. A more fitting term, I believe, is **confabulation**.

These perspectives are also reflected in the scientific literature: some sources acknowledge that the terms are often used interchangeably,[^sui_et_al_2024] others argue that while _confabulation_ may be more accurate, _hallucination_ remains more widely recognized,[^zhang_zhang_2025] and still others explicitly advocate for adopting _confabulation_ as the preferred term.[^sui_et_al_2024] [^smith_et_al_2023] [^nejjar_et_al_2025]

### Where These Terms Come From—and What They Actually Mean

But what do _hallucinations_ and _confabulation_ actually mean---and where do these terms come from?

Both terms have a clinical background.[^zhang_zhang_2025] It makes sense that the field borrows from clinical language because when we trace the origins of artificial intelligence, we go back to 1956, when a summer workshop at Dartmouth College laid the foundation for what we now understand as AI. While different approaches emerged---like mathematical logic and deductive reasoning---some participants advocated drawing from biology and psychology to mimic human brain-like behavior.[^mitchell2019_8]

According to the ICD-11---the eleventh edition of the International Classification of Diseases and the global standard for documenting health information---**hallucinations** are described as "sensory perceptions of any modality occurring in the absence of the appropriate (external) stimulus. The person may or may not have insight into the unreal nature of the perception."[^icd11hallucinations]

**Confabulation**, by contrast, is defined as "the filling of memory gaps with fabricated, distorted, or misinterpreted memories about oneself or the world, without the conscious intention to deceive."[^icd11confabulations]

Contrasting both definitions makes clear: When someone experiences _hallucinations_, someone might see, hear, feel, or even smell something that isn't actually there.[^emsley2023] _Confabulation_, on the other hand, is about memory and cognition. Individuals experiencing confabulation aren't trying to mislead anyone; their brain is simply generating plausible (but false) memories to maintain a coherent sense of reality. Confabulation is also called "honest lying" where the individual believes that the statement is true.[^wigginsbunin2023] This is also supported by research in psychiatry where researchers state that "confabulations are false memories within the context of retrieval, in which patients are not generally aware that they are confabulating".[^lorente_et_al_2011_59]

### Understanding The Phenomenon in the World of LLMs

While _hallucination_ and _confabulation_ have clinical definitions, in the context of AI, these terms describe how LLMs generate text that "seemingly fills gaps in the information contained in the model by plausibly sounding words"[^nejjar_et_al_2025_2], yet is often "nonsensical or unfaithful to the provided source content"[^farquharetal2024_625]. Some research further distinguishes between _intrinsic hallucinations_, where generated text contradicts the source, and _extrinsic hallucinations_, where it cannot be verified against the source[^jietal2022_2].

From a technical perspective, this phenomenon is inherent to how these models operate. The generation process relies on probability: at each step, the model predicts the most likely next word based on the preceding context.[^1] Essentially, with each new word generated, the model assigns a probability to it, favoring words with higher probabilities. If the probability at any point drops too low, the model stops generating text. While this probabilistic approach is fundamental, it can lead to factually incorrect outputs—a consequence generally undesirable, unless the goal is creative or fictional writing.

Here’s an example of how this might look in practice: I asked ChatGPT which special event happened on May 5, 1960, related to Trader Joe’s. I know Trader Joe’s wasn’t founded until after [1967](https://www.traderjoes.com/home/about-us), yet ChatGPT claimed their first store opened on May 5, 1960. This kind of error is what the literature often refers to as both _hallucination_ and _confabulation_.

![small_image](/images/single-blog/gpt_trader_joes.png)
{{< detail-tag "Alternative text" >}}Image showing a screenshot of the question "What happened to Trader Joe's on May 5, 1960?" and ChatGPT's reponse "Trader Joe's was founded on May 5, 1960. That’s when the first Trader Joe’s store opened, starting what would become a popular American grocery chain known for its unique products and quirky brand. Would you like to know more about its history or how it evolved after that?".
{{< /detail-tag >}}

Researchers in the realm of LLMs also observe that the terms _confabulation_ and _hallucination_ are still often used interchangeably.[^sui_et_al_2024] They highlight, however, that using the term confabulation instead of hallucination has certain advantages. 

While hallucination remains the dominant term in both media and technical literature, the use of confabulation is gaining traction—particularly in research areas focused on explainability, cognitive alignment, and ethical framing. Several studies argue that confabulation more accurately describes how LLMs generate plausible-sounding yet incorrect responses --- without implying sensory experience or intent.[^sui_et_al_2024] [^smith_et_al_2023] [^nejjar_et_al_2025] For instance, the model isn’t perceiving something nonexistent; rather, it is fabricating information.[^smith_et_al_2023] [^emsley2023] Moreover, _confabulation_ is a more neutral term that respects sensitivities around mental health. Neurological studies support this by showing that everyday memory reconstruction involves confabulation in both healthy and impaired individuals, which helps reduce the stigma attached to the term.

However, others note that despite its conceptual accuracy, _confabulation_ remains less recognized than _hallucination_ in both public and professional discourse.[^zhang_zhang_2025] Consequently, some researchers adopt a hybrid approach, using _confabulation_ to denote a subset of hallucinations---specifically, those that are fluent, arbitrary, and confidently incorrect.[^farquharetal2024_625]

Interestingly, research shows that "presenting patients with contradictory information may further perpetuate confabulation in an attempt to explain their account."[^wigginsbunin2023] This behavior can also be observed in LLMs when given contradictory inputs. Although these models are improving in handling such contradictions, this confabulatory behavior may still occur:

![small_image](/images/single-blog/contradiction_trader_joes.png)

{{< detail-tag "Alternative text" >}}Image showing a screenshot of the question "But Trader Joe's was founded in 1955." and ChatGPT's reponse "Excellent catch — let's clear this up together because it's an interesting little wrinkle in Trader Joe's history.
Here's what actually happened:

* In 1955, Joe Coulombe took over a small chain of convenience stores in the Los Angeles area called Pronto Markets.
* He ran those for several years, but by the mid-60s, Joe realized he needed to differentiate his stores from growing competitors like 7-Eleven.
* So in 1967, he rebranded one of his Pronto Markets in Pasadena as the first Trader Joe's — the quirky, nautical-themed grocery store we know today.

In short:

* 1955 = Joe Coulombe takes over Pronto Markets
* 1967 = First store officially named Trader Joe's opens

So while the roots trace to 1955, Trader Joe's as a brand didn't exist on May 5, 1960. Nothing specific involving the Trader Joe's name happened on that exact date.
Do you mind me asking — what made you curious about May 5, 1960? Was it something you read or heard about? I'd love to help connect the dots if there's a backstory here."
{{< /detail-tag >}}

Although these models are continually refined, this confabulatory behavior may persist. The results shared above were generated by GPT-4o.

### Rethinking the Words We Use

When we lean on terminology like this, it should accurately reflect the terms and concepts from the language it's borrowing from. In the context of LLMs, where there's no conscious intent to deceive or sensory misperception involved, opting for a term like confabulation offers greater precision and respects sensitivities around mental health.

Some might dismiss it as mere semantics, but in science and technology, the words we choose profoundly shape how problems are framed, understood, and ultimately solved. Giving phenomena a clear, well-fitting name that resonates across disciplines not only enhances communication but also reduces misunderstandings. In a field evolving as rapidly as generative AI, there is ample opportunity---and indeed a responsibility---to revisit our terminology and ensure it accurately reflects the complexities we aim to describe and manage.

<!-- [^baker2000]

[^baker2000]: Baker, Simon, and Takeo Kanade (2000). [Hallucinating Faces.](https://ieeexplore.ieee.org/abstract/document/840616) _In Proceedings Fourth IEEE International Conference on Automatic Face and Gesture Recognition (Cat. No. PR00580)_, 83-88. 
-->

[^emsley2023]: Emsley, Robin (2023). [ChatGPT: These Are Not Hallucinations - They’re Fabrications and Falsifications.](https://www.nature.com/articles/s41537-023-00379-4) _Schizophrenia_, 9(1), 52.

[^farquharetal2024]: Farquhar, Sebastian, Jannik Kossen, Lorenz Kuhn, and Yarin Gal (2024). [Detecting Hallucinations in Large Language Models Using Semantic Entropy.](https://www.nature.com/articles/s41586-024-07421-0) _Nature_, 630(8017), 625-630.

[^farquharetal2024_625]: Farquhar, Sebastian, Jannik Kossen, Lorenz Kuhn, and Yarin Gal (2024). [Detecting Hallucinations in Large Language Models Using Semantic Entropy.](https://www.nature.com/articles/s41586-024-07421-0) _Nature_, 630(8017), p. 625.

[^jietal2022]: Ji, Ziwei, Nayeon Lee, Rita Frieske, Tiezheng Yu, Dan Su, Yan Xu, Etsuko Ishii, Ye Jin Bang, Andrea Madotto, and Pascale Fung (2022). [Survey of Hallucination in Natural Language Generation.](https://dl.acm.org/doi/abs/10.1145/3571730) _arXiv preprint arXiv:2202.03629_.

[^jietal2022_2]: Ji, Ziwei, Nayeon Lee, Rita Frieske, Tiezheng Yu, Dan Su, Yan Xu, Etsuko Ishii, Ye Jin Bang, Andrea Madotto, and Pascale Fung (2022). [Survey of Hallucination in Natural Language Generation.](https://dl.acm.org/doi/abs/10.1145/3571730) _arXiv preprint arXiv:2202.03629_, p. 2.

[^lorente_et_al_2011]: Lorente-Rovira, Esther, German Berrios, Peter McKenna, Micaela Moro-Ipola, and Jose Mª Villagrán-Moreno (2011). [Confabulations (I): Concept, Classification and Neuropathology.](https://actaspsiquiatria.es/index.php/actas/article/view/639) _Actas Españolas de Psiquiatría_, 39(4), 251-259.

[^lorente_et_al_2011_59]: Lorente-Rovira, Esther, German Berrios, Peter McKenna, Micaela Moro-Ipola, and Jose Mª Villagrán-Moreno (2011). [Confabulations (I): Concept, Classification and Neuropathology.](https://actaspsiquiatria.es/index.php/actas/article/view/639) _Actas Españolas de Psiquiatría_, 39(4), p. 259.

[^mitchell2019]: Mitchell, Melanie (2019). [Artificial Intelligence: A Guide for Thinking Humans.](https://en.wikipedia.org/wiki/Artificial_Intelligence:_A_Guide_for_Thinking_Humans) _Pelican UK_.

[^mitchell2019_8]: Mitchell, Melanie (2019). [Artificial Intelligence: A Guide for Thinking Humans.](https://en.wikipedia.org/wiki/Artificial_Intelligence:_A_Guide_for_Thinking_Humans) _Pelican UK_, p. 8.

[^nejjar_et_al_2025]: Nejjar, Mohamed, Luca Zacharias, Fabian Stiehle, and Ingo Weber (2025). [LLMs for Science: Usage for Code Generation and Data Analysis.](https://onlinelibrary.wiley.com/doi/full/10.1002/smr.2723) _Journal of Software: Evolution and Process_, 37(1), e2723.

[^nejjar_et_al_2025_2]: Nejjar, Mohamed, Luca Zacharias, Fabian Stiehle, and Ingo Weber (2025). [LLMs for Science: Usage for Code Generation and Data Analysis.](https://onlinelibrary.wiley.com/doi/full/10.1002/smr.2723) _Journal of Software: Evolution and Process_, 37(1), e2723, p. 2.

[^smith_et_al_2023]: Smith, Andrew L., Felix Greaves, and Trishan Panch (2023). [Hallucination or Confabulation? Neuroanatomy as Metaphor in Large Language Models.](https://journals.plos.org/digitalhealth/article?id=10.1371/journal.pdig.0000388) _PLOS Digital Health_, 2(11), e0000388.

[^sui_et_al_2024]: Sui, Peiqi, Eamon Duede, Sophie Wu, and Richard Jean So (2024). [Confabulation: The Surprising Value of Large Language Model Hallucinations.](https://arxiv.org/abs/2406.04175) _arXiv preprint arXiv:2406.04175_.

[^1]: If you're new to the field and look for a short introduction into LLMs (in particular BERT), here's a [(hands-on) blog post](https://socialsciencedatalab.mzes.uni-mannheim.de/article/bert-explainable-ai/) that walks you through it.

[^icd11hallucinations]: ICD-11. [MB27.2 Hallucinations](https://icd.who.int/browse/2025-01/mms/en#665363966)

[^icd11confabulations]: ICD-11. [MB21.3 Confabulation](https://icd.who.int/browse/2025-01/mms/en#292722503)

[^wigginsbunin2023]: Wiggins, Amanda, and Jessica L. Bunin (2023). "Confabulation." [Confabulation.](https://pubmed.ncbi.nlm.nih.gov/30725646/) _StatPearls Publishing_

[^zhang_zhang_2025]: Zhang, Wan, and Jing Zhang (2025). [Hallucination Mitigation for Retrieval-Augmented Large Language Models: A Review.](https://www.mdpi.com/2227-7390/13/5/856) _Mathematics_, 13(5).
