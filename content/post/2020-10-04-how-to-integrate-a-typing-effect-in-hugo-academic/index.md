---
title: How to Integrate a Typing Effect in Hugo Academic
author: Cosima Meyer
date: '2020-10-04'
slug: how-to-integrate-a-typing-effect-in-hugo-academic
category: [Hugo, website, R-post, R]
postImage: images/single-blog/typing.png
featureImage: images/single-blog/typing.png
---

Have you ever wondered how to integrate a typing effect in your Hugo Academic theme easily? It shouldn't be too difficult. That's also what I thought when I started with it. One quick disclosure before getting started: I've never been professionally trained in HTML, CSS, or JS. So for someone more proficient in this field, it might seem to be a straightforward task. Here's my approach that eventually worked for me:

1. If you want it as an interactive effect in the "About" section (an HTML document), you will need to adjust it. I heavily relied on the great [step-by-step guide](https://isabella-b.com/blog/hugo-academic-customization/) by [Isabella Benabaye](https://isabella-b.com) to get a centered avatar without the biography.
2. As a careful reader, you will just have realized that the biography section (including my education) is now missing. That's why I added an ["About me"](https://cosimameyer.com/about/) section (again inspired by Isabella's [source code](https://github.com/isabellabenabaye/isabella-b.com)). I tweaked it a bit so that it fits my personal preferences.
3. Now comes the typing magic! ✨ I've tried [typed.js](https://mattboldt.com/demos/typed-js/) first but couldn't get it working on my own site. I then discovered [TypeIt.js](https://typeitjs.com) and basically followed the [guideline](https://typeitjs.com/#installation).

    1. Add `<script src="https://cdn.jsdelivr.net/npm/typeit@7.0.4/dist/typeit.min.js"></script>` to the top part of your `about.html` (stored in `themes/hugo-academic/layouts/partials/widgets`)
    2. Add the following part at the position where you want to have the typing effect in your `about.html` file: `<p class="multipleStrings"></p>`. This will be something like a place holder, and we still need to define what you want to include.
    3. That comes in the next step. To define `multipleStrings`, add the following lines of code to the bottom of the `about.html` file:
    ```
    <script>
      new TypeIt(".multipleStrings", {
        strings: ["This is a great string.", "But here is a better one."],
        breakLines: false,
        loop: true,
        speed: 50
      }).go();
    </script>
    ```
    4. As a last step, save the `about.html` and render the page.
    

And voilà, that's what you get:
![small_image](/images/single-blog/typing.gif)
