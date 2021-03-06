---
layout: post
title: Analyzing ICU Admissions at Beth Isreal Deaconess Medical Center
full-width: true
image: https://raw.githubusercontent.com/joekrinke15/blog/master/img/icu.png
---
In this post I'll be discussing my final project for Biostatistics 823: Statistical Programming for Big Data, a course I took at Duke University. For this project, my classmates and I built an interactive dashboard to explore ICU admission trends at Beth Isreal Deaconess Medical center. My teammates on this project were [Alena Kalodzitsa](https://www.linkedin.com/in/alenakalodzitsa/), [Felipe Buchbinder](https://www.linkedin.com/in/felipe-buchbinder-a65a0199/), and [Chenxi Wu](https://www.linkedin.com/in/chenxi-wu-107452175/). This project would not have been possible without them. 

# MIMIC-III Dataset
The dataset we used is called [MIMIC-III](https://mimic.physionet.org/). This database contains over 60,000 ICU patient records collected from 2001-2012 at Beth Isreal Deaconess Medical Center. The data was compiled by the MIT Lab for Computational Physiology and preprocessed to protect patient identity. 

# Project Goals 
The overarching goal of our project was to examine two separate questions: do certain diseases tend to occur together and how does disease prevalence vary across demographic groups. Our target audience was hospital administrators and physicians, as they could use this information to change how their hospital responds to the needs of their community. For example, if it is found that most severe illnesses tend to occur with a preventable underlying condition (such as obesity, smoking, or mismanaged diabetes), a program could be developed to help the local community take ownership of their own health. These efforts could be further targeted by examining which demographic groups tend to experience a given health problem. 

# Project Flowchart
We began by launching a prebuilt [AWS stack](https://github.com/MIT-LCP/mimic-code) that allowed us to access the MIMIC data from an AWS account. We queried the data using Amazon Athena and stored the results in an S3 bucket. Then we read the data into our Streamlit application using Pandas. 

<p align="center">
  <img src="https://github.com/joekrinke15/blog/blob/master/img/Flowchart%20(5).png?raw=true" />
</p>

# Dashboard Components
In order to answer the two questions we are interested in, we separated our dashboard into 5 components: general trends, disease to demographics, demographics to disease, a market basket analysis, and a co-occurrence analysis. 

### General Trends
This section provides information on the most common diseases, patient demographic distributions, and hospital admission locations.

<p align="center">
  <img src="https://github.com/joekrinke15/blog/blob/master/img/generaltrends.PNG?raw=true" />
</p>

### Disease to Demographics
This portion of the dashboard allows you to select a disease and see typical patient characteristics. 

<p align="center">
  <img src="https://github.com/joekrinke15/blog/blob/master/img/diseasetodemo.PNG?raw=true" />
</p>

### Demographics to Disease
The opposite of disease to demographics, demographics to disease lets you input demographic information and see what kinds of diseases people with those characteristics suffer from. 

<p align="center">
  <img src="https://github.com/joekrinke15/blog/blob/master/img/demotodisease.PNG?raw=true" />
</p>

### Market Basket Analysis
This graph shows the results of a market basket analysis of diseases. Specifically, on display are the combinations of diseases that have the highest lift values. The lift value tells you if the odds of two diseases happening together is higher than if the diseases occurred entirely independently. High lift values indicate a strong relationship between two disease categories.  

<p align="center">
  <img src="https://github.com/joekrinke15/blog/blob/master/img/mktbsk.PNG?raw=true" />
</p>


### Co-occurrence Analysis
Here you can select a disease and view a network of conditions that people with this disease have. This can be used to identify different groups of individuals who all have the same diagnosis. The co-occurrence matrix used here was generated using the [Natural Language Toolkit (NLTK)](https://www.nltk.org/) package. The nodes and edges of the graph were constructed using [NetworkX](https://networkx.org/documentation/stable/index.html).

<p align="center">
  <img src="https://github.com/joekrinke15/blog/blob/master/img/cooccur.PNG?raw=true" />
</p>

# Sample Insights and Conclusion

While we were building the dashboard we came up with some sample insights to illustrate its potential value. Here's some of what we found:

* The majority of patients in the ICU have some form of hypertension.
  * Hypertension is disproportionately present in Black patients.
*  Asians are underrepresented compared to the Asian population where the hospital is located (2.5% to 7% in Boston, MA).
  * Asian patients tend to be young women who are having children.
* Only 49% of patients are admitted to the ICU from the emergency room.
  * Many patients are coming from long-term care facilities.
  
Overall, our dashboard could serve as a useful tool to physicians and hospital administrators. It can be used to guide potential community-health interventions and monitor the health of various demographic groups. If you're interested in looking at the dashboard feel free to check out the code [here](https://github.com/joekrinke15/MIMIC-Analysis).

# Technologies Used
<table class="tg">
<tbody>
  <tr>
    <td class="tg-c3ow"><img width="1604" alt="screen shot 2017-08-07 at 12 18 15 pm" src="https://miro.medium.com/max/644/1*l3DUScBKumyAE1e8dt95vQ.png"></td>
    <td class="tg-c3ow"><img width="1604" alt="screen shot 2017-08-07 at 12 18 15 pm" src="https://miro.medium.com/max/1125/1*B9CIOrxdROHvtdmouQA1_A.png"></td>
    <td class="tg-c3ow"><img width="1604" alt="screen shot 2017-08-07 at 12 18 15 pm" src="https://cdn.worldvectorlogo.com/logos/mysql.svg"></td>
  </tr>
  <tr>
    <td class="tg-c3ow"><img width="1604" alt="screen shot 2017-08-07 at 12 18 15 pm" src="https://assets.website-files.com/5dc3b47ddc6c0c2a1af74ad0/5e18182db827fa0659541754_RGB_Logo_Vertical_Color_Light_Bg.png"></td>
    <td class="tg-c3ow"><img width="1604" alt="screen shot 2017-08-07 at 12 18 15 pm" src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Plotly-logo-01-square.png/1200px-Plotly-logo-01-square.png"></td>
    <td class="tg-c3ow"><img width="1604" alt="screen shot 2017-08-07 at 12 18 15 pm" src="https://miro.medium.com/max/592/0*zKRz1UgqpOZ4bvuA"></td>
  </tr>
  <tr>
    <td class="tg-0pky"><img width="1604" alt="screen shot 2017-08-07 at 12 18 15 pm" src="https://networkx.org/_static/networkx_logo.svg"></td>
    <td class="tg-0pky"><img width="1604" alt="screen shot 2017-08-07 at 12 18 15 pm" src="https://pandas.pydata.org/static/img/pandas.svg"></td>
    <td class="tg-0pky"><img width = "1604" src= "https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/NumPy_logo_2020.svg/1024px-NumPy_logo_2020.svg.png"></td>
  </tr>
</tbody>
</table>
