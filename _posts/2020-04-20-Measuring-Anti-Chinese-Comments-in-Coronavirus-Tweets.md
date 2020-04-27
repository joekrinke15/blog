---
layout: post
title: Measuring Anti-Chinese Comments in Coronavirus Tweets
image: https://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Flag_of_the_People%27s_Republic_of_China.svg/1200px-Flag_of_the_People%27s_Republic_of_China.svg.png
---
# Measuring Anti-Chinese Comments in Coronavirus Tweets

Covid-19 has spread across the globe, infecting milions of people and fundamentally changing how we work and interact with others. With the proliferation of this disease also came an unexpected byproduct; anti-Chinese racism in the United States.
![NYT Headline](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Racism.PNG?raw=true)
Since the first cases of the virus were in Wuhan, a number of individuals have blamed China and its government for the pandemic, going as far as to attack, harrass, and curse Chinese nationals and even Chinese-Americans. These flames of prejudice, some would argue, have been stoked by the U.S. president's rhetoric; he famously called Covid-19 the "Chinese Virus."
![Trump Tweet](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/TrumpTweet.PNG)

Some of my classmates (Jingjing Shi, Abdullah AlOthman, Felipe Buchbinder) and myself were interested in identifying the amount of prejudice that exists towards Chinese people and how it has changed over the course of the pandemic. We decided to use a dataset of Covid-19-related tweets collected by the [Panacea Lab](http://www.panacealab.org/covid19/) to tackle this question. We began by hydrating the tweets (taking a tweet's ID and adding the rest of its data) and subsetting them based on if they contained a keyword like Wuhan, China, or Chinese Virus. Our process can be seen below. ![Processing](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Processing.PNG)

After we collected the tweets, our next step was to determine how to measure the sentiment of their contents. We built a deep-learning model to classify tweets as being severly toxic, an identity attack, an insult, or a threat, using an annotated dataset provided by [Jigsaw's AI team.](https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification/data) This model achieved a .98 AUC. We then used this model to classify all of the tweets we gathered. We found that there appeared to be a spike in insults following the declaration of the pandemic worldwide.
![Trends](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Trends.png?raw=true)

China-related tweets appeared to have higher levels of insults, threats, toxicity, and identity attacks, confirming that Chinese people may be experiencing racism as a result of the Coronavirus. 

![Threat](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Threat.png?raw=true) ![Identity Attacks](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/identity%20attack.png?raw=true)
![Toxicity](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Tweet%20Toxicity%20(2).png) ![Threats](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Threat.png?raw=true)

Interestingly, individuals with no profile pictures also tweeted content that was much more negative and insulting- it is possible that such users may be bots created to manufacture political discord. We may try applying existing bot-detection algorithms to test this theory. 
![No Profile](https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/China%20Profile%20TF.png?raw=true)
