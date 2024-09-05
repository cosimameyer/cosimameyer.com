---
title: Add a panel feature to show multiple languages in Hugo blog posts
author: Cosima Meyer
date: '2024-09-04'
slug: [panel-feature-hugo-blog-posts]
categories:
  - R
  - Python
  - Python-post
  - R-post
  - Hugo
tags:
  - R-post
  - R
  - Python-post
  - Python
  - Hugo
postImage: images/single-blog/panel_set_example.png
featureImage: images/single-blog/panel_set_example.png

---

I often want to show multiple languages in one post. Instead of posten them sequentially, I enjoy to have them in tabs that allow me to easily switch between them. I didn't find an out-of-the box feature that Hugo (or my theme) provides, so here's a step-by-step guide to implement this feature in your own blog:



{{< tabs id="example1" titles="R 💜, Python 🐍" >}}

Here's an example for some R code that you can show to your readers: 

```r
a <- 7
b <- 5
```


<!-- tab -->

And here's the same in Python:

```python
a = 7
b = 5
```
{{< /tabs >}}

And here are the steps that you need to follow to implement it:

In your `shortcodes` folder, create a new file called `tabs.html`. You can do this manually or by using the following command:

```shell
touch tabs.html
```

Open the file and add the following content:

```html
{{ $id := .Get "id" | default "tabs" }}
{{ $titles := .Get "titles" }}
{{ $contents := .Inner }}

<div class="tabs-container" id="{{ $id }}">
  <ul class="tabs-nav">
    {{ $titleList := split $titles "," }}
    {{ range $i, $title := $titleList }}
      <li class="tab-nav" data-tab="{{ $i }}">{{ $title }}</li>
    {{ end }}
  </ul>
  <div class="tabs-content">
    {{ $delimiters := split $contents "<!-- tab -->" }}
    {{ range $i, $content := $delimiters }}
      <div class="tab-pane" data-tab="{{ $i }}">
        {{ $content | markdownify }}
      </div>
    {{ end }}
  </div>
</div>

<style>
  .tabs-container {
    font-family: Arial, sans-serif;
  }
  .tabs-nav {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
  }
  .tab-nav {
    padding: 10px;
    cursor: pointer;
    border: 1px solid #ccc;
    border-radius: 3px 3px 0 0;
    margin-right: 5px;
  }
  .tab-nav.active {
    background-color: #f1f1f1;
    border-bottom: 1px solid transparent;
  }
  .tabs-content {
    border: 1px solid #ccc;
    border-radius: 0 0 3px 3px;
    padding: 10px;
  }
  .tab-pane {
    display: none;
  }
  .tab-pane.active {
    display: block;
  }
</style>

<script>
  document.addEventListener('DOMContentLoaded', function () {
    const container = document.getElementById('{{ $id }}');
    if (!container) return;
    
    const tabs = container.querySelectorAll('.tab-nav');
    const panes = container.querySelectorAll('.tab-pane');

    tabs.forEach(tab => {
      tab.addEventListener('click', function () {
        tabs.forEach(t => t.classList.remove('active'));
        panes.forEach(p => p.classList.remove('active'));

        this.classList.add('active');
        const index = this.getAttribute('data-tab');
        container.querySelector(`.tab-pane[data-tab="${index}"]`).classList.add('active');
      });
    });

    // Activate the first tab by default
    if (tabs.length > 0) {
      tabs[0].classList.add('active');
      panes[0].classList.add('active');
    }
  });
</script>
```

Save it. Now you're good to go 🚀

To call it, simply use the following pattern in your markdown-based blog post:

```markdown
{{</* tabs id="example2" titles="Tab title 1, Tab title 2, Tab title 3" */>}}

Some text

<!-- tab -->

Some more text for tab 2. 

<!-- tab -->

And here's tab 3.

{{</* /tabs */>}}
```

This will generate the following version:

{{< tabs id="example2" titles="Tab title 1, Tab title 2, Tab title 3" >}}

Some text

<!-- tab -->

Some more text for tab 2. 

<!-- tab -->

And here's tab 3.

{{< /tabs >}}


What's important to remember when you're using the implementation:

1. Each panel set needs a new id. This will be set in `tabs id="your id"`. Think of ids as meaningful names for your tab collections.
2. In the `titles` section, separate tabs are separated by comma (`,`). See for instance `titles="Tab title 1, Tab title 2, Tab title 3"` in the example.
3. Inside the content, the content is separated by `<!-- tab -->`. 

Happy tabbing 🙌

![smaller_image](https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExdzVlM2RpYzEyNnZ6YWlpb2lsZDAxbHp3aDNmNGNjbjA1aTAwNjIyNSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/aYzxVt2lMrZXW/giphy.gif)


