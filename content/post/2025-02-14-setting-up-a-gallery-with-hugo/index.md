---
title: Setting up a gallery with Hugo
author: Cosima Meyer
date: '2025-02-14'
slug: setting-up-a-gallery-with-hugo
categories:
  - Hugo
postImage: images/single-blog/gallery.png
featureImage: images/single-blog/gallery.png
---

I have been creating stats and other visualizations for a couple of years now and uploading them to my [GitHub repo](https://github.com/cosimameyer/illustrations). While this was a quick way to make them publicly available, I have to admit that a GitHub repo is probably not the most accessible place for everyone. This became even clearer to me when someone asked me for an overview of the ["Amazing Women in Tech"](https://gallery.cosimameyer.com/amazing-women-in-tech/), a series I am running that aims to make women more visible in the world of programming, statistics, and STEM in general. The illustrations are shared along with a short portrait on social media sites like LinkedIn, Mastodon, and Bluesky, so I was looking for a good way to integrate a showcase gallery - ideally on my website. Since my site is Hugo-based, it was natural for me to look for another Hugo-based approach. This is how I came across [Nico Kaiser's amazing Hugo gallery theme](https://github.com/nicokaiser/hugo-theme-gallery), which made it extremely easy to get up and running. I spent a few after-work hours this week playing around with the setup, and this is what it looks like:

[![small_image](/images/single-blog/gallery.png)](https://gallery.cosimameyer.com)
{{< detail-tag "Alternative text" >}} Screenshot of the Illustrations Gallery website showing a preview on "Amazing Women in Tech". It shows hexagon tiles with small portraits of different amazing women in tech.{{< /detail-tag >}}


I couldn't set it up as a separate theme in my site, so I took a different approach and registered a subdomain for my site (`gallery.cosimameyer.com`) where I publish the theme separately as a standalone. If you're looking for an alternative approach, here's how I did it.

### Set up GitHub Repository

Follow the standard way to [set up a GitHub repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories). [Clone your GitHub repository (https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) on your local machine. The links in the text will take you to easy-to-follow instructions.

```bash
git clone <repo-url>
```

### Initiate Hugo

First, we navigate to the folder containing your cloned repository:

```bash
cd <repo-folder-name>
```

Now make sure that Hugo is installed (if not, you can do so by [using Homebrew on a Mac](https://formulae.brew.sh/formula/hugo)). To start Hugo, just run this inside the folder:

```bash
hugo new site .
```

This creates a template for a Hugo site that we can populate in the next steps.

### Get Theme

Now we can get Nico's theme. I followed here the recommendation in the [GitHub README](https://github.com/nicokaiser/hugo-theme-gallery?tab=readme-ov-file#as-git-submodule):

```bash
git submodule add --depth=1 https://github.com/nicokaiser/hugo-theme-gallery.git themes/gallery
```

### Populate Content

Nico gives a detailed description of what structure your content in the `content/' folder should follow. If you want to read it, it's [here](https://github.com/nicokaiser/hugo-theme-gallery?tab=readme-ov-file#usage). Mine looks something like this: 

```
├── _index.md
├── amazing-women-in-tech
│   ├── ada_lovelace_small.png
│   │   ...
│   │   index.md
│   └── timnit_gebru_small.png
└── machine-learning
    ├── _index.md
    ├── feature.png
    ├── nlp
    │   ├── image1.png
    │   ├── image2.png
    │   ├── image3.png
    │   └── index.md
    └── xai
        ├── image1.png
        ├── image2.png
        └── index.md
```

In `_index.md` and `index.md` you can control the title, description, metadata and appearance of your gallery. If you're looking for more guidance, it's all described [here](https://github.com/nicokaiser/hugo-theme-gallery?tab=readme-ov-file#usage). To check how your gallery looks, you can preview your website after running the following command: 

```bash
hugo server --disableFastRender
```

### Publish Your Gallery

Once your gallery is populated, you can publish it. To do this, you have different options. For instance, I set up a subdomain with my web hosting provider. Once this was set up, I needed my FTP details (username, server and password). How you get this may vary depending on your provider. Once I had these, I set them up as secrets in the GitHub repo and added the following GitHub Actions workflow (it is stored in `.github/workflows/deploy_website.yaml` in my gallery repository):

```yaml
name: Deploy Website

on:
  push:
    branches:
      - hugo-portio-theme  # Set a branch to deploy
    paths:
      - "archetypes/**"
      - "content/**"
      - "data/**"
      - "i18n/**"
      - "layouts/**"
      - "resources/**"
      - "static/**"
      - ".github/**"
      - "config.toml"
      - "config.yaml"
      - "index.Rmd"

jobs:
  deploy:
    runs-on: ubuntu-24.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - uses: r-lib/actions/setup-r@v1
        with:
          r-version: 'release'
          rspm: "https://packagemanager.rstudio.com/cran/__linux__/focal/latest"

      - uses: r-lib/actions/setup-pandoc@v1
  
      - name: Install packages and build website
        run: |
          install.packages('blogdown')
          blogdown::install_hugo()
          blogdown::build_site()
        shell: Rscript {0}
        
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.89.1'
          extended: true

      - uses: actions/cache@v4
        with:
          path: /tmp/hugo_cache
          key: ${{ runner.os }}-hugomod-${{ hashFiles('**/go.sum') }}
          restore-keys: |
            ${{ runner.os }}-hugomod-

      - name: Build
        run: hugo --minify

      - name: 📂 Sync files
        uses: SamKirkland/FTP-Deploy-Action@v4.3.5
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          protocol: ftps
          local-dir: ./public/
          server-dir: <website>
```

Replace the `<website>` in `server-dir` with the server directory (the directory on your server from which your website will be deployed). Now, if you push all your changes to your GitHub repository, the GitHub Actions job will be triggered and your website will be built and deployed 🎉