---
title: How to Implement a Redirect From Your Old GitHub Page to Your New Website
author: Cosima Meyer
date: '2020-12-28'
slug: implement-a-redirect-from-your-old-github-page-to-your-new-website
category: [R-post, R, Hugo, website]
tags: []
subtitle: ''
summary: ''
authors: []
featured: yes
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
postImage: images/single-blog/redirect.png
featureImage: images/single-blog/redirect.png
---

Before hosting my website with [netlify](https://www.netlify.com) (which makes hosting so smooth!), I deployed my website manually using [GitHub pages](https://pages.github.com) for quite some time. GitHub Pages is a great way to host a (more or less) static website. But if you are like me and want to update your website every now and then, it can become challenging, and that's why I'm more than happy that I switched. However, to ensure that people who only have my old website (probably because I forgot to change it somewhere) are directed automatically to my new up-to-date website, I wanted to implement a redirect. To do this, I followed this great [step-by-step guide](https://dev.to/steveblue/setup-a-redirect-on-github-pages-1ok7) and added a bit of of a tweak to adjust it to my taste. If you are looking for an (only slightly) different solution, I recommend reading the original post -- it took me quite some time to find a way to implement the redirect, and I'm more than grateful for Steve's fantastic and straightforward blog post on it! 

And here's how I did it in 10 simple steps:

1. In case you are like me and have not deleted your old GitHub repository with your website, rename your old GitHub repository where you hosted your website (it should be something such as `username.github.io` where you replace `username` with your GitHub name). I renamed it to `username.github.io2` to make space for the new one :)
2. Open a new GitHub repository (make it `public` to make it easier accessible) called `username.github.io`
3. Open the terminal and clone your GitHub repository

```bash
git clone https://github.com/username/username.github.io.git
```

The terminal will most likely warn you that "[y]ou appear to have cloned an empty repository." That's correct -- because there's nothing in your repository yet :)

4. We'll change that now. Navigate to your repository first:

```bash
cd username.github.io
```

5. Create three empty files:

```bash
touch _config.yml
touch index.html
touch .htaccess
```

6. This is our first deviation from Steve's guide. The `.htaccess` file allows us to directly go for the `index.html` file when calling our GitHub page. To do this, open the `.htaccess` file and add the following line: 

```bash
DirectoryIndex index.html
```

`.htaccess` is likely to be hidden. To show the `.htaccess` file in your Finder press `Cmd` + `Shift` + `.` (dot) on a Mac. 

7. Now open the `index.html` file. I used [Sublime Text](https://www.sublimetext.com) to open it but other editors should also work perfectly well. Add the following and replace `yournewpage.com` with your redirect:

```bash
<!DOCTYPE html>
<meta charset="utf-8">
<title>Redirecting to https://yournewpage.com/</title>
<meta http-equiv="refresh" content="0; URL=https://yournewpage.com/">
<link rel="canonical" href="https://yournewpage.com/">
```

These steps follow very much what [Steve describes in his step-by-step guide](https://dev.to/steveblue/setup-a-redirect-on-github-pages-1ok7).

8.Open `_config.yml`. Here we define the outer appearance of our page even though we don't need it.

```bash
theme: jekyll-theme-cayman
````

9. Now, we deviate again from the original post. To be able to commit and push your changes, you first have to add them.

```bash
git add _config.yml 
git add index.html
git add .htaccess
```

10. Now we can commit and push them:
```bash
git commit -a -m "Updating and uploading files."
git push origin master
```

Well, and that's it. It might take a few minutes, but your redirect is now up-and-running!