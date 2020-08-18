---
layout: post
title: Creating a GitHub Pages Website Using Jekyll
image: https://raw.githubusercontent.com/joekrinke15/JoeKrinke15.github.io/master/img/website.png
---

Creating a website can be a great way to showcase work in your portfolio, whether you are a data scientist, software engineer, or web developer. However, if you don't have much experience making a website, it can be difficult to find a low-cost option that is relatively simple to implement. GitHub pages combined with the static site generator Jekyll can help make the process of making a website much simpler. This post will outline the basics of creating a website by walking you through how I made a page about my dogs, Jericho and Lily. 

To begin making your website you must first have a GitHub account. You can easily register to create an account [here.](https://github.com/pricing) Once you have an account you can fork the [Jekyll repository](https://github.com/daattali/beautiful-jekyll) and begin editing your site. First, start by renaming the repository to yourwebsitename.github.io.

![Rename Repository](https://raw.githubusercontent.com/joekrinke15/JoeKrinke15.github.io/master/img/RenameSite.PNG)

Now that you've renamed your repository, take a look at the config.yml folder and read the comments to understand the website settings. This folder holds most of the important information about your website such as a navigation bar to show posts, your contact information, and what subsections exist. I started by altering my file to remove all of the contact information fields.

![Remove Contact Information](https://raw.githubusercontent.com/joekrinke15/JoeKrinke15.github.io/master/img/RemoveSocialMediaButtons.PNG)

Next, I deleted the two default posts found in the posts folder and created two new posts. You can create a new post using the new file button.
![New Post](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/CreateNewPost.PNG?raw=truePosts)

Posts are written using the [markdown](https://www.markdowntutorial.com/) language. Make sure your post names start with the date and end with ".md" to indicate they are markdown files. Here's an example of the markdown code for a post next to the end result. 

<img src="https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Threat.png?raw=true"  width="175" height="500"/>
<img src="https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/identity%20attack.png?raw=true"  width="175" height="500"/>

