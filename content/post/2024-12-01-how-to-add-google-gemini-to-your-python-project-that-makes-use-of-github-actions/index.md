---
title: How to Add Google Gemini to Your Python Project That Makes Use of GitHub Actions
author: Cosima Meyer
date: '2024-12-01'
slug: how-to-add-google-gemini-to-your-python-project-that-makes-use-of-github-actions
categories:
  - Python-post
  - Python
tags:
  - NLP
  - ML
  - PyLadies
  - Python
  - Python-post
  - R-Ladies
postImage: images/single-blog/pyladies_bot_megaphone.png
featureImage: images/single-blog/pyladies_bot_megaphone.png
---

I extended the PyLadies and R-Ladies bot with a new feature using the support of Google Gemini. If you want to learn more about the bots and the project behind them, here are two posts that I wrote about [the bots and their architecture](/post/2023-09-17-building-mastodon-bots-and-promoting-the-community-part-2/) and the [Awesome PyLadies' repository (as well as Awesome R-Ladies' repository)](/post/2023-04-25-building-mastodon-bots-and-promoting-the-community/).

Catchy titles and emojis are great, but sometimes they don’t fully capture the heart of a blog post. That’s why I introduced a new feature for the PyLadies and R-Ladies bot on Mastodon and Bluesky. The bot now also share short summaries of the blog posts, giving readers a reason to click and explore further. 

### Why Google Gemini?
Choosing the right tool was crucial, and [Gemini](https://ai.google.dev/gemini-api/docs) ticked all the boxes I was looking for. First and foremost, I needed a powerful LLM. The bot crawls through a variety of content, some of which is quite technical. Gemini's ability to grasp these nuances and generate meaningful summaries was essential. Plus, it's incredibly easy to integrate. The plug-and-play approach meant I could get up and running quickly, and the modular design allows me to switch models down the line if needed. And let's not forget the cost! Being a passion project, I needed a solution that wouldn't break the bank. Gemini's free tier fit the bill perfectly, especially since I'm working with public data.

<!--The choice of Google Gemini for summarization stems from several key factors:

- **Power and Versatility**: [Google Gemini]() is a suite of powerful large language models (LLMs) capable of handling complex and technical language often found in data science blog posts.
- **Ease of Integration**: The seamless integration of Gemini into existing projects with a plug-and-play approach was a major advantage, minimizing the need for extensive modifications.
- **Modularity**: The ability to switch between different models within the Gemini suite provides flexibility and customization options.
- **Cost-Effectiveness**: Gemini's cost-effective nature, particularly the availability of a free-of-charge model for public data, aligned perfectly with the constraints of a personal project.
The specific model chosen was Google's Gemini 1.58B, a lightweight yet high-performing model that strikes a balance between capability and efficiency. This model was chosen for three primary reasons:

1. **Simplicity of Task**: Summarization is a relatively straightforward task compared to more complex LLM operations, making the 1.58B model suitable.
2. **Public Data Usage**: The project involves crawling and summarizing publicly available data, allowing the use of the free-of-charge model, which contributes to product improvement.
3. **Generous Rate Limits**: The free-of-charge model offers high rate limits, which the bot is unlikely to exceed.-->

### How It All Works

![small_image](/images/single-blog/architecture_bots.png)
{{< detail-tag "Alternative text" >}}Image showing the architecture of the bot. The bot's backend resides in a GitHub repository. GitHub Actions serves as a CRON trigger, running on a regular schedule. The CRON trigger activates a Python script named "promote_blog_posts.py". The script gathers RSS feed content from an RSS metadata pickle file containing basic information. Using the RSS feed text, the script interacts with Google AI Studio, where the Gemini API is connected. The script instructs the API to summarize the text and returns the summary. The script combines the title, author, hashtags, link, and summary to create a social media post. The post is then automatically published on Bluesky and Mastodon.{{< /detail-tag >}}

The whole system runs on a GitHub repository, with GitHub Actions acting as a daily trigger. When the trigger fires, my `promote_blog_posts.py` script comes in. It gathers all the RSS feed content, figures out who's next on the posting list, and grabs their latest unposted content. Here's where Gemini comes in: the script sends the RSS feed text over to Google AI Studio, where I've set up the Gemini API. Gemini then works its magic, generating a short and (hopefully) engaging summary of the post. This summary, along with the title, author, hashtags, and link, is packaged into a post and sent out to Bluesky and Mastodon. And that's it! But how do we get there from a technical perspective?

### Implementing Gemini 
Implementing Gemini was surprisingly straightforward. Here's a breakdown of the steps:

#### Generate an API Key

![small_image](/images/single-blog/api_google_ai_studio.png)
{{< detail-tag "Alternative text" >}}Image showing Google AI Studio landing page with an arrow in the top-left corner pointing to "Get API Key".{{< /detail-tag >}}

This was as simple as heading over to [Google AI Studio](https://aistudio.google.com/app/apikey), clicking "Get API Key," and copying the generated key. I stored this key securely in my environment variables and GitHub secrets, making sure it never touches GitHub directly.

#### Tweak the Python Script

A few lines of code were all it took. I imported the Gemini library, initialized the model using my API key, and chose the lightweight yet powerful Gemini 1.5 8B model. Then, I created a simple function to handle the summarization process. This function takes the RSS feed text, feeds it to the model with a prompt specifying the desired length and tone, and retrieves the generated summary. Of course, I added some safety checks to ensure the summary is appropriate and doesn't go off the rails. And that's it! 


```python
import google generativeai as genai

# Init model
genai.configure(api_key=os.environ['GEMINI_API_KEY'])
model = genai.GenerativeModel(model_name='gemini-1.5-flash')

def summarize_blog_post(text_from_rss_feed, model):
  
  # Build your prompt
  prompt = ["Summarize the content of the post in maximum 60 characters.",
            "Be as concise as possible and be engaging.", 
            text_from_rss_feed]
  
  # Get model response
  response = model.generate_content(prompt)
  
  # Perform additional checks and only accept those that are marked as OK
  safety_ratings = response.candidates[0].safety_ratings
  if all(rating.probability.name == 'NEGLIGIBLE' for rating in safety_ratings):
    return response_cleaned
  return ''
```

#### Update GitHub Actions
The final step was a minor adjustment to my GitHub Actions YAML file. All I did was add the Gemini API key, pulling it from my [GitHub secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions). 

```yaml
name: Promote PyLadies' blog posts

on:
  schedule:
    - cron: '0 7 1-31/2 * *' # will only run on odd days #'0 7 */3 * *' # '0 7 */3 * *' #'0 0 1 * *'  # "At 07:00 UTC every third day (which is 10 CET)"

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: checkout repo content
        uses: actions/checkout@v2 # Checkout the repository content

      - name: setup python
        uses: actions/setup-python@v4
        with:
          python-version: '3.9.16' # Install the python version needed

      - name: install python packages
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Execute py script for Bluesky 🦋
        env:
          PLATFORM: "bluesky"
          COUNTER: "pyladies_counter_bluesky.txt"
          ARCHIVE_DIRECTORY: "pyladies_archive_directory_bluesky"
          IMAGES: "pyladies_images"
          PICKLE_FILE: "pyladies_meta_data.pkl"
          CLIENT_NAME: "pyladies_bot"
          PASSWORD: ${{ secrets.BSKY_PYLADIES }}
          USERNAME: ${{ secrets.BSKY_USERNAME_PYLADIES }}
          GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }} # Add your Gemini API key
        run: python src/promote_blog_post.py
  
      - name: Commit files
        id: commit
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "github-actions"
          git add --all
          if [-z "$(git status --porcelain)"]; then
            echo "::set-output name=push::false"
          else
            git commit -m "Add changes" -a
            echo "::set-output name=push::true"
          fi
        shell: bash
          
      - name: Push changes
        if: steps.commit.outputs.push == 'true'
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.SECRET_WRITE }}
```

With that done, the bot was ready to start summarizing on schedule! And this is what it looks like:

![small_image](/images/single-blog/example_bot_gemini.png)
{{< detail-tag "Alternative text" >}}Image showing a post on Bluesky by the PyLadies bot including a summary.{{< /detail-tag >}}

### Lessons Learned Along the Way
This whole experience was a great learning opportunity, reinforcing the fact that incorporating powerful LLMs doesn't have to be a headache. The low-effort approach, combining GitHub Actions and Google AI Studio, saved me tons of time (and also financial resources).

### What's Next?

With summarization in place, I'm already brainstorming ways to make the bot even more awesome. Here's a sneak peek:

- **Summarizing YouTube Video Descriptions**: I want to expand the bot's capabilities to include summarizing YouTube video descriptions.
- **Introducing a "Judge AI"**: Imagine an AI that double-checks the generated summaries for accuracy and quality.
- **Using LLMs to Keep Things Tidy**: One challenge I face is dealing with overly long URLs and hashtags. I'm thinking of employing another LLM to automatically shorten summaries or other elements when needed, ensuring the post stays within character limits.
- **Bringing in More Content**: Blogs and YouTube videos are just the beginning. I'm eager to onboard other content types like podcasts and open-source software projects.
- **Opening the Door to Community Input**: This is a community project, and I'm always open to new ideas! ✨


<!-- 
### Generate an API Key

Here are the steps to generate a Gemini API Key:

1. [Access Google AI Studio](): Navigate to the Google AI Studio platform. You also have the option of using Vertex AI, but it's more complex and potentially more expensive.
2. Obtain API Key: Click on "Get API Key" to generate a unique numeric character combination.
3. Securely Store API Key:

  - Do not push this key to GitHub for security reasons.
  - Store it in your environment variables.
  - Store it within your GitHub secrets for use in your GitHub Actions workflow.

### Adjust Python Script

1. Import Library: Import the necessary library for using the Gemini API.
2. Initialize Model:
- In the config settings, initialize the model using the Gemini API key stored in your environment variables.
- Specify the model name (in this case, "Gemini 1.58B").
3. Create Summarization Function: Add a new function to the "promote_blog_posts.py" script. This function will:
- Take the RSS feed text and the initialized model as inputs.
- Define a prompt for summarization, specifying length, conciseness, and engaging tone. An example prompt is: `"Summarize the content of the post in a maximum of 60 characters and be as concise as possible and be engaging."`
- Send the text and prompt to the model for summarization.
- Receive the response payload from the model.
- Check the model's safety ratings to ensure the response is appropriate.
- Return the summary if all safety checks pass, otherwise return nothing.

### Adjust Your GitHub Actions YAML

1. **Add Gemini API Key**: Include the Gemini API key, retrieved from your GitHub secrets, in your GitHub Actions YAML file.
2. **Schedule CRON Job**: Ensure the bot runs on a regular schedule using a chron job definition within the YAML file.

## Lessons Learned
Integrating Google Gemini into this project yielded several valuable insights:
- **LLM Empowerment**: LLMs can easily enhance existing applications, particularly those with modular structures, requiring only minor adjustments.
- **Simplified Deployment**: Using GitHub Actions allows for a low-effort approach without the need for cloud migration or complex infrastructure setup.
- **Cost-Efficiency**: Google AI Studio offers cost-effective solutions, including a free tier, making it accessible for personal projects. The platform will automatically stop processing requests if the rate limit is reached, preventing unexpected charges.

## Future Possibilities

Of course I have more ideas in my on what comes next! Some are:

  - Summarizing YouTube video descriptions.
  - Implementing a "judge AI" to assess summary quality and reduce errors.
  - Utilizing LLMs to shorten summaries or content when necessary.
  - Expanding the project to include podcasts and open-source software.
  
This approach demonstrates how to leverage the power of Google Gemini to add valuable features and improve the user experience with minimal effort and cost. The modularity and ease of integration of Gemini make it a compelling choice for developers seeking to enhance their applications with advanced LLM capabilities.
-->