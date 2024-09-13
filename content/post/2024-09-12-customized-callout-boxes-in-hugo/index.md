---
title: Add customized callout boxes to your Hugo blog posts
author: Cosima Meyer
date: '2024-09-11'
slug: customized-callout-boxes-in-hugo
categories:
  - Hugo
tags:
  - Hugo
postImage: images/single-blog/callout.png
featureImage: images/single-blog/callout.png
---

What I look for in text is when it's visually appealing. And I think callout boxes are a great way to direct the reader to important information. The Hugo Portio theme doesn't ship with them, so I implemented and customized them. If you want something as nice as this, here's your guide 🎉

{{< callout type="info" title="Information" >}}
This is an informational callout with default styling.
{{< /callout >}}

{{< callout type="success" title="Success" >}}
This is a success callout with default styling.
{{< /callout >}}


{{< callout type="error" title="Error" >}}
This is a error callout with default styling.
{{< /callout >}}

{{< callout type="warning" title="Caution" >}}
This is a warning message.
{{< /callout >}}

{{< callout type="custom" title="Custom Styled Callout" custom-class="my-special-class" >}}
This callout has custom styling applied.
{{< /callout >}}


In your `shortcodes` folder, create a new file called `callout.html`. You can do this manually or by using the following command:

```shell
touch callout.html
```

Open the file and add the following content:

```html
<div class="callout {{ .Get "type" }}">
  <div class="callout-content">
    <strong>{{ .Get "title" }}</strong>
    <p>{{ .Inner }}</p>
  </div>
</div>


<style>
  /* Base Callout Styling */
  .callout {
    position: relative; /* Needed for absolute positioning of the ::before element */
    padding: 20px;
    margin: 20px 0;
    border: 2px solid; /* Adds a border around the callout box */
    border-radius: 8px; /* Rounded corners for the box */
    font-family: Arial, sans-serif;
    background-color: #f9f9f9;
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  /* Add Font Awesome icon with ::before */
  .callout::before {
    content: "\f00c"; /* Default content, will be overridden by specific icons */
    font-family: "Font Awesome 5 Free"; /* Font Awesome 5 Free */
    font-weight: 900; /* Font Awesome icons use this weight */
    position: absolute;
    left: -40px; /* Position the icon to the left of the callout */
    top: 50%;
    transform: translateY(-50%);
    font-size: 24px; /* Size of the icon */
    color: inherit; /* Use the color from the callout */
  }
  
  /* Info Callout */
  .callout.info {
    background-color: #e0f7fa;
    border-color: #00acc1;
  }
  
  .callout.info::before {
    content: "\f05a"; /* Font Awesome info-circle icon */
  }
  
  /* Warning Callout */
  .callout.warning {
    background-color: #fff3e0;
    border-color: #ff6f00;
  }
  
  .callout.warning::before {
    content: "\f071"; /* Font Awesome exclamation-triangle icon */
  }
  
  /* Success Callout */
  .callout.success {
    background-color: #e8f5e9;
    border-color: #43a047;
  }
  
  .callout.success::before {
    content: "\f058"; /* Font Awesome check-circle icon */
  }
  
  /* Error Callout */
  .callout.error {
    background-color: #ffebee;
    border-color: #e53935;
  }
  
  .callout.error::before {
    content: "\f057"; /* Font Awesome times-circle icon */
  }
</style>
```

And add the following part to your `layouts/_default/baseof.html` (this will allow you to use fontawesome icons).

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
```

{{< callout type="warning" title="Caution" >}}
This is a tricky part - depending on if and how you implemented Fontawesome before, there may be conflicts. For example, I'm using a local version of fontawesome 4, so you may need to make some additional adjustments to get the code to work. Since this may be an exception, I'm adding the instructions on how to handle such a case as a foldout below.
{{< /callout >}}
{{< detail-tag "Alternative approach" >}}
Replace the `.callout::before` in your `callout.html`:

```html
  /* Add Font Awesome icon with ::before */
  .callout::before {
    content: "";
    font-family: "FontAwesome"; /* Font Awesome 4 */
    font-weight: normal;
    position: absolute;
    left: -40px;
    top: 50%;
    transform: translateY(-50%);
    font-size: 24px;
    color: inherit;
  }
```

Now there's no need to add the line to your `layouts/_default/baseof.html`.
{{< /detail-tag >}}

Now you're ready to use the shortcode:

```markdown
{{</* callout type="info" title="Information" */>}}
This is an informational callout with default styling.
{{</* /callout */>}}

{{</* callout type="warning" title="Caution" */>}}
This is a warning message.
{{</* /callout */>}}
```

Which gives you these two callout boxes:

{{< callout type="info" title="Information" >}}
This is an informational callout with default styling.
{{< /callout >}}

{{< callout type="warning" title="Caution" >}}
This is a warning message.
{{< /callout >}}

With `type="warning"` we define how the callout box looks like (you can choose between `info`, `warning`, `error`, `success`) or even define your own style using `type="custom"`. Here you can add your own pre-defined custom class using `custom-class="my-special-class"` as an addition to your call. 

With `title="Caution"` you define what's in bold (for consistency it's best to stick with the default titles, but you can customize them here). And of course you can add a message, as you can see above - so let's start adding this to your posts to guide your readers even further! 🤓
