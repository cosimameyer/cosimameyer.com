---
title: Building Mastodon Bots and Promoting the Community - Part 2
author: Cosima Meyer
date: '2023-09-17'
slug: [building-mastodon-bots-and-promoting-the-community-part-2]
postImage: images/single-blog/rladies-pyladies-bots.png
featureImage: images/single-blog/rladies-pyladies-bots.png
category: [R, R-post, Python, Python-post, Community, Mastodon]
tags: [R, Python, PyLadies, RLadies, Community, Mastodon]
---
As mentioned in the [first post in this series](https://cosimameyer.com/post/2023-04-25-building-mastodon-bots-and-promoting-the-community/), this post will cover how to build a Mastodon bot. This can be fairly straightforward, but - as always - there are multiple ways to "make it work". Fortunately, there are already some fantastic tutorials out there that can help you get started (and they have definitely helped me out a lot!). I have listed them in the [References](#references) for you to get some more inspiration. There are two main components to building Mastodon bots:

- Building the code base that creates and posts the toots,
- Deploying the code to run automatically somewhere.

This post is about the first part, and I'll share my methods for the deployment of the code in a third blog post.

### Understanding the Architecture of My Mastodon Bot

In its current state, the bot does three different things:

- Boosting and favoriting posts that are tagged with `#pyladies' or `#rladies'.
- Boosting and favoriting posts explicitly tagged by the bot.
- Promoting blog posts by R-Ladies and PyLadies from two curated lists (yes, those are the [Awesome PyLadies](https://github.com/cosimameyer/awesome-pyladies-blogs) and [Awesome R-Ladies](https://github.com/rladies/awesome-rladies-blogs) repositories!)

And this is what it looks like:

[![small_image](/images/single-blog/diagram-mastodon.png)](/images/single-blog/diagram-mastodon.png)
{{< detail-tag "Alternative text" >}}Diagram showing the architecture of the Mastodon bots (PyLadies and R-Ladies bot). Both bots rely on several Python scripts that allow them to collect metadata (including RSS feeds) and use that information to promote, reblog, and boost posts and mentions, among other things.{{< /detail-tag >}}

We'll talk more about how the individual scripts are orchestrated and executed automatically in the next post (in short, you can use whatever framework works best for you - I mainly rely on GitHub Actions, but started with AWS Lambda) - this is also intertwined with what the first part of the diagram covers. This post will cover how to set up a script that allows your Mastodon bot (the far right rectangle in the diagram) to act. 

But before we get there, we need to create a bot first. There are a few instances that allow bots - I chose [botsin.space](https://botsin.space). Setting up a bot there is a fairly straightforward process.

#### Setting Up a Mastodon Bot Account

1. Set up a Mastodon account for your bot - the [botsin.space](https://botsin.space) instance is highly recommended. It's a dedicated instance for bots, and it's easy to set up an account there. If you are not sure if your bot is a good fit, contact the maintainer. I did this for my bot because I wanted to make sure it was in line with the policies of the instance. Getting a response was really quick, and discussing open questions was great. 

![small_image](/images/single-blog/mastodon-signup-botsin.png)
{{< detail-tag "Alternative text" >}}Screenshot of what is required when setting up a Mastodon bot at [botsin.space](https://botsin.space) (display name, username, email address, password, and reason for wanting to join.){{< /detail-tag >}}

2. Settings on Mastodon

- Upload Description
- Upload an image
- Upload a background image
- Mark your bot as "bot" (this is netiquette to make it clear to other users that they are interacting with a bot)

In practice it looks like this 

![small_image](/images/single-blog/bot-settings.png)

{{< detail-tag "Alternative text" >}}Screenshot of what you can add once your account is active:

- Upload Description
- Upload an image and a background image
- Mark your bot as "bot" (this is netiquette to make it clear to other users that they are interacting with a bot)

{{< /detail-tag >}}

To make sure the bot is recommended to others and reaches a wider audience, I also checked the suggestion box. In the description, I've also added who created the bot (and a disclaimer that it's still experimental). This is to make sure that people know who to contact if they need to. 

### `mastodon` - The Perfect Python API That Helps You Publish Your Post

As you can see in the diagram above, each script follows a similar scheme:

- I authenticate with Mastodon
- Create the post
- Publish the post

![small_image](/images/single-blog/diagram-zoom.png)
{{< detail-tag "Alternative text" >}}Diagram showing the architecture of the Mastodon bots' scripts. They typically follow the following sequence:

- Authenticate with Mastodon API
- (Reading helper data)
- Build post
- Publish post to Mastodon
{{< /detail-tag >}}

We'll walk through these steps with a simple example that will allow you to build your own bot later.

A common task for a bot might be to repost posts that mention the bot, and that's what we know. We will use the [`boost_mentions.py` script] (https://github.com/cosimameyer/mastodon-bots/blob/main/src/boost_mentions.py). We'll dive into the three core parts individually and then bring them together. The code is heavily inspired by [Joshua Quirk's `Rebooster` bot](https://github.com/Lambdanaut/Rebooster) - if you want to see another example that follows a similar setup, head over to the `Rebooster` repository.

#### Authenticating With Mastodon

Here we rely on a config file (for general settings) and environment variables (for secrets and credentials). Let's take a look at `config.py` first:

```python
##########################################################
# Config file
##########################################################

# The URL of the mastodon instance your bot is on
API_BASE_URL = 'https://botsin.space'

# Define the visibility on Mastodon
MASTODON_VISIBILITY = 'public'
```

The rest of the information is stored in your environment. You can get all the information in the Mastodon interface by following these steps:

1. Go to the "Development" section in your [botsin.space account](https://botsin.space)

![small_image](/images/single-blog/mastodon-development.png)
{{< detail-tag "Alternative text" >}}
Screenshot showing the "Your applications" page at Mastodon. The "Development" tab is selected on the left-hand side and allows the user to click the "New application" button.
{{< /detail-tag >}}

2. Add an application name (it can be any name)

![small_image](/images/single-blog/mastodon-add-application-name.png)
{{< detail-tag "Alternative text" >}}
Screenshot showing the "New application" page at Mastodon. The name `rladies_bot` is already filled in but it can be any name as it just helps you to identify for which application you use your newly generated keys.
{{< /detail-tag >}}

3. And then your application is successfully created (make sure it has read and write permissions!)

![small_image](/images/single-blog/mastodon-application.png)
{{< detail-tag "Alternative text" >}}
Screenshot showing the "Your applications page". It shows that the application `rladies_bot` was successfully created with the scopes "read write follow". "Follow" is optional and not required for our purpose.
{{< /detail-tag >}}

4. And this is where you get your keys (make sure not to post them anywhere publicly - and if you do, renew them)

![small_image](/images/single-blog/mastodon-keys.png)
{{< detail-tag "Alternative text" >}}
Screenshot showing the "Application: rladies_bot" page at Mastodon. The client key, client secret and access token are blurred but those will be the ones that you will use to authenticate to Mastodon.
{{< /detail-tag >}}

To pass them to the bot, add them to your environment by putting them in an `.env' file.

With this information, we then run the `boost_mentions.py` script and authenticate with Mastodon. You can easily rely on the Python library [`mastodon`](https://mastodonpy.readthedocs.io), which has many features to make your life easier when posting.

```python
import os
import config
from mastodon import Mastodon
                
if __name__ == "__main__": 
    
    MASTODON_VISIBILITY = config.MASTODON_VISIBILITY
    MASTODON_URL = config.API_BASE_URL
    MASTODON_VISIBILITY = config.MASTODON_VISIBILITY
    ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
    PASSWORD = os.getenv("PASSWORD")
    USERNAME = os.getenv("USERNAME")
    CLIENT_NAME = os.getenv("CLIENT_NAME")
    CLIENT_CRED_FILE = os.getenv('BOT_CLIENTCRED_SECRET') 
    
    TIMELINE_DEPTH_LIMIT = 40  # How many of the latest statuses to pull per tag. 

            
    print(f"Initializing {CLIENT_NAME} Bot")
    print("-----------------------------------------------")
    print(f" > Connecting to {MASTODON_URL}")
    
    client_id, client_secret = Mastodon.create_app(
                CLIENT_NAME,
                api_base_url = MASTODON_URL)
                
    # Create client
    mastodon = Mastodon(
        client_id = client_id, 
        access_token = ACCESS_TOKEN,
        client_secret = client_secret,
        api_base_url = MASTODON_URL,
    )   

    print(f" > Logging in as {USERNAME}.")
    mastodon.log_in(
        USERNAME,
        PASSWORD
    )

    print(" > Successfully logged in")
    print(" > Fetching account data")

    account = mastodon.me()
```

#### Capture Feed and Create Post (and Publish Them)

Now that we have authenticated with Mastodon, we can read the feed. There are several options - since we want to capture all posts that mention the bot, we choose `types=['mention']`. 

```python
    notifications = mastodon.notifications(types=['mention'])
    print(f" > Fetched account data for {account.acct}")
    print("-----------------------------------------------")
```

Now that we have all the notifications collected, we can go through them, check if the bot has already favorited or boosted them (we don't want to boost or favorite them again), and then post them using [`mastodon.status_reblog`](https://mastodonpy.readthedocs.io/en/stable/15_everything.html#mastodon.Mastodon.status_reblog) and [`mastodon.status_favorite`](https://mastodonpy.readthedocs.io/en/stable/15_everything.html#mastodon.Mastodon.status_favourite). There are other features that help you write and publish posts. I have collected the ones I use for the bots below:

| Function                                                                                                                                                                           	| What it does                                                                                                                                              	|
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|-----------------------------------------------------------------------------------------------------------------------------------------------------------	|
| `mastodon.status_reblog`                                                                                                                                                           	| Reblogging statuses previously catched                                                                                                                    	|
| | |
| `mastodon.status_favourite`                                                                                                                                                        	| Favoriting statuses previously catched	|
|   | |
| `mastodon.status_post(toot_txt)`                                                                                                                                                   	| Post a text (the text is a simple string in `toot_txt`)                                                                                                   	|
| | |
| `mastodon.media_post(filename)` 	| Capturing a file (`filename`),  	|
| `mastodon.media_update(media_upload_mastodon, description = en['alt_text'])`| updating it by adding an alternative text, |
| `mastodon.status_post(toot_txt, media_ids = media_upload_mastodon)`| and posting a post with an image (the post is a string in the object `toot_txt`)|


I added a try-catch to make sure the bot does not fail, but posts a message if it does (there may be scenarios where we definitely want the bot to fail. In those cases a try-catch isn't the best way to go, but for this setting it's fine).

```python
    print(" > Reading statuses to identify tootable statuses")
    for notification in notifications:
        if not notification.status.favourited and \
                notification.status.account.acct != account.acct:
            try:
                print(f"- Boosting new toot by {notification.account.username} viewable at: {notification.status.url}")
                mastodon.status_reblog(notification.status.id)
                mastodon.status_favourite(notification.status.id)
            except:
                print(f"- Boosting new toot by {notification.account.username} did not work - going to the next toot.")
```
As mentioned above, the code is heavily inspired by [Joshua Quirk's `Rebooster` bot](https://github.com/Lambdanaut/Rebooster). I follow his approach and print some logs directly. We could also initiate a logger and capture all logs, which is definitely a good idea when putting things into production. 

{{< detail-tag "Here are all relevant files together" >}}
##### `config.py`

```python
##########################################################
# Config file
##########################################################

# The URL of the mastodon instance your bot is on
API_BASE_URL = 'https://botsin.space'

# Define the visibility on Mastodon
MASTODON_VISIBILITY = 'public'
```

##### `boost_mentions.py`

```python
import os
import config
from mastodon import Mastodon
                
if __name__ == "__main__": 
    
    MASTODON_VISIBILITY = config.MASTODON_VISIBILITY
    MASTODON_URL = config.API_BASE_URL
    MASTODON_VISIBILITY = config.MASTODON_VISIBILITY
    ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
    PASSWORD = os.getenv("PASSWORD")
    USERNAME = os.getenv("USERNAME")
    CLIENT_NAME = os.getenv("CLIENT_NAME")
    CLIENT_CRED_FILE = os.getenv('BOT_CLIENTCRED_SECRET') 
    
    TIMELINE_DEPTH_LIMIT = 40  # How many of the latest statuses to pull per tag. 

            
    print(f"Initializing {CLIENT_NAME} Bot")
    print("-----------------------------------------------")
    print(f" > Connecting to {MASTODON_URL}")
    
    client_id, client_secret = Mastodon.create_app(
                CLIENT_NAME,
                api_base_url = MASTODON_URL)
                
    # Create client
    mastodon = Mastodon(
        client_id = client_id, 
        access_token = ACCESS_TOKEN,
        client_secret = client_secret,
        api_base_url = MASTODON_URL,
    )   

    print(f" > Logging in as {USERNAME}.")
    mastodon.log_in(
        USERNAME,
        PASSWORD
    )

    print(" > Successfully logged in")
    print(" > Fetching account data")

    account = mastodon.me()
    
    notifications = mastodon.notifications(types=['mention'])
    print(f" > Fetched account data for {account.acct}")
    print("-----------------------------------------------")
    
    print(" > Reading statuses to identify tootable statuses")
    for notification in notifications:
        if not notification.status.favourited and \
                notification.status.account.acct != account.acct:
            try:
                print(f"- Boosting new toot by {notification.account.username} viewable at: {notification.status.url}")
                mastodon.status_reblog(notification.status.id)
                mastodon.status_favourite(notification.status.id)
            except:
                print(f"- Boosting new toot by {notification.account.username} did not work - going to the next toot.")
```

{{< /detail-tag >}}
<br><br>

### What's Next?

Overall, the setup is now complete, bringing your bot to life and making it fully operational. To launch your bot, you can run the scripts locally. Alternatively, as I will explain in an upcoming post, you have the option to streamline and automate this process. This can be done using many different tools - I used both AWS Lambda and GitHub Actions, but you can essentially tailor the choice to your specific needs and preferences.

### References

- [Corso, Frank: Deploying a Twitter bot with AWS Lambda](https://frankcorso.dev/aws-lambda-python-twitter-bot.html) 
- [Duggan, Mathew: Deploying a Mastodon bot with AWS Lambda](https://matduggan.com/make-a-mastodon-bot-on-aws-free-tier/)
- [Eden, Terence: Building a Mastodon bot](https://shkspr.mobi/blog/2018/08/easy-guide-to-building-mastodon-bots/)
- [Quirk, Joshua: Rebooster bot](https://github.com/Lambdanaut/Rebooster/) 

<!-- - [Meyer, Cosima: GitHub repository - `mastodon-bots`]() -->
