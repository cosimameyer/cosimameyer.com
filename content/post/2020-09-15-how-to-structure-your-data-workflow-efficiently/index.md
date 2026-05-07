---
title: How to Structure Your Data Workflow Efficiently Using R
author: Cosima Meyer
date: '2020-09-15'
slug: how-to-structure-your-data-workflow-efficiently-using-r
category: [R-post, R, workflow]
postImage: images/single-blog/git.png
featureImage: images/single-blog/git.png
---

The positive aspects of a structured data workflow are at hand: You will always know where your things are -- no matter in which project you are currently working. If you stick to one work frame, you can also access your projects later and easily maneuver your way around. If you end up convincing your collaborators from your workflow or -- if you at least introduce them to yours -- they will also have a more comfortable life. In short, setting up a data workflow is extremely helpful.

There is no right or wrong, and various workflows are possible. We proposed one workflow in this [blog post on Methods Bites](https://www.mzes.uni-mannheim.de/socialsciencedatalab/article/efficient-data-r/). I have developed my own (simple) structure[^1] throughout the years that I use whenever I start a new project and that I am sharing with you here as a step-by-step guide.

### Work With R Projects and Github

I love **R projects**. They are great. You can have a neat folder structure, and it's easy to keep your files together (I also use it to maintain this website!). If you did not have the chance yet, I recommend giving it a try. 

In a similar vein, use **Github**! It's been a live saver in so many different ways. Github is the cloud version of the version control Git. It allows you to push and pull your files and collaborate with your colleagues or co-authors, keep track of changes, and, if you are a student, you'll benefit from the [education pack](https://education.github.com) and get a (free) premium account that allows you have "private repositories" (the default are public repositories) that only you and your collaborators can access. And, a last great point, if you work with [Overleaf](https://www.overleaf.com/), you can also [link your Github account to your Overleaf project](https://www.overleaf.com/learn/how-to/How_do_I_connect_an_Overleaf_project_with_a_repo_on_GitHub,_GitLab_or_BitBucket%3F) and access your figures directly!

The best thing about R Studio and R projects is that you can link them with Github easily. And I'll show you how:

#### On Github

1. If you don't already have an account, [sign up](https://github.com).
2. Click the "+". 
3. Select "New repository".
4. Then you'll see this form. Fill it out, give your repository a name (a meaningful one!), select whether it's private or public (I like it to be private first), and then you can initialize a README (I like this option because it allows you to add some more information -- here is [one example](https://github.com/cosimameyer/conflict-elections) and [another example](https://github.com/dennis-hammerschmidt/telegram-bot) how to use them).

![small_image](/images/single-blog/github-repository.png)

5. Once you have selected everything you want, click "Create repository".
6. Now, you'll need to copy the URL and save it -- you'll need this in a second.

#### In R Studio

1. Open R Studio
2. Click on "File > New Project ... > Version Control > Git"
3. Now, paste your URL to "Repository URL", and the name will be automatically created. You can now also decide where you want to store the project on your local machine.
4. Once you've done these steps, you're set up and click "Create Project" to open your new project. 

![small_image](/images/single-blog/git-control.png)

Your project is now automatically linked with your Github repository and your R project. You can see this in the Git panel -- whenever you have changes in your files, they will show up (as they show up now for ".gitignore" and "give-it-a-name.Rproj"). To sync them with Github, check all files that you want to sync and hit **Commit**. You will now have to add a meaningful commit message for your future self (required to keep track of the changes). 

![small_image](/images/single-blog/commit.png)

Hit "Commit" and -- and that's essential -- "push" to push it to your Github. If you or your co-author made changes on Github or in the files directly and you want to get the updated versions, hit "pull" to get all changes. 


<!-- Sometimes Github tends to be counterintuitive but here's a -->

### Set up Your Folder Structure

Once you're here, I usually set up my folder structure. It mainly follows the following pattern:

```
├── README.md
├── code
│   ├── 1_data_preparation.Rmd
│   ├── 2_merging.Rmd
│   ├── 3_analysis.Rmd
│   ├── 4_descriptives.Rmd
│   ├── 5_visualization.Rmd
│   └── helper.R
├── data
│   ├── processed
│   └── raw
├── figures
└── give-it-a-name.Rproj
```

There are two options to do this. You can either do it by hand (which is perfectly fine) or do it automatically -- which is extremely straightforward. If you want to have the same structure as I have, copy and paste the following code lines. You can also adjust them to your needs.

```r
# Set up the folder structure
folder_names <- (
    # Main folders
  c("data", "code", "figures",
    # Sub-folders
    "data/raw", "data/processed"))

for (j in seq_along(folder_names)) {
  dir.create(folder_names[j])
}
```
To add the files, use the following lines of code. The way I wrote it, you can decide whether you want `.Rmd` files (which I personally often prefer and is the default in the loop) or `.R` files. Just change the suffix if needed.

```r
# Add files to the folders
file_names <- (
  c(
   # For preparing your data 
    "1_data_preparation",
   # The merging file might also be combined 
   # with the first file
    "2_merging", 
   # For your descriptives
    "3_descriptives",
   # For your analysis
    "4_analysis", 
   # For your visualization
    "5_visualization"
  )
)

for (j in seq_along(file_names)) {
  file.create(paste0("code/", file_names[j], ".Rmd"))
}

# Create a helper function file
file.create("code/helper.R")
```

Now that we have the folders and files, I will walk you through them and tell you what I use them for.

#### `data` Folder

The `data/` folder has two sub-folders: `raw` and `processed`. As the names suggest, put your raw and untouched data in the `raw` data folder, and if you want to save any changes, store them in `processed`. This allows you to produce replicable results (if you document all your steps in your code files) and to redo the data wrangling again with your raw data.

#### `code` Folder

All the files that we've created above are stored in this folder. Again, similar to the data folder, the file names are relatively straightforward. The `1_data_preparation` file is meant to include all the pre-processing steps; in `2_merging` you can merge the data (both files can also be combined into one); you can fit all descriptives in `3_descriptives`; `4_analysis` contains your analysis and regression diagnostics; and create beautiful visualizations in `5_visualization`. I've also created a `helper.R` file where you can store all your helper functions.

As I said above, the code that I gave you is flexible, and you can set up the files that fit best for your workflow (and it might differ depending on the project you're working on). I like the enumeration because it allows me to easily follow my steps in the sequential order they are meant to be executed.

#### `figures` Folder
In this folder, you will store all your visualizations and use Overleaf and follow [this guide](https://www.overleaf.com/learn/how-to/How_do_I_connect_an_Overleaf_project_with_a_repo_on_GitHub,_GitLab_or_BitBucket%3F), you can link them directly to your writing project in Overleaf.

[^1]: You can always increase the complexity of the structure, depending on your needs. Here's [another example](https://www.r-bloggers.com/2018/08/structuring-r-projects/) of a more complex folder structure.