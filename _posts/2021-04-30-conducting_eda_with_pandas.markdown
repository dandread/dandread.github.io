---
layout: post
title:      "Conducting EDA with pandas"
date:       2021-04-30 21:29:30 +0000
permalink:  conducting_eda_with_pandas
---

*Exploratory data analysis (aka EDA) is a critical part of the analysis process because it is where a data scientist will get a grasp on the data they will be working with, have the opportunity to clean up datasets, and begin performing so simple analyses.*

*Using concrete, real-life examples along the way, this post will serve as a brief tutorial of the EDA process, harnessing the power and simplicity of the pandas library to manipulate and clean data.*

## Background
Let's say you work as a data scientist for an insurance broker in Florida that deals primarily with personal insurance products. The company has noticed that while it does a good job selling a person both home and auto insurance, it isn't doing very well with umbrella policies (for more information on what an umbrella policy is, [click here](https://www.investopedia.com/articles/personal-finance/040115/how-umbrella-insurance-works.asp)!) Umbrella policies are usually a good idea for people with both home and auto insurance, so your company decides it wants to focus on selling this for the next few months. You are tasked with creating a list of eligible clients that can be given to each employee, so they can try to cross-sell to their clients.

## Getting Acclimated
Now that you have been presented with the project, it's time to get started! The very first step is finding the appropriate datasets to work with. You may not always be given data to work with, so it's up to you to gather what you need. Luckily, you have identified two datasets that you believe contain sufficient information for this project: *clientList*, which contains a list of the active clients, and *policyList*, which contains current policies. Let's go ahead and take a look at each!

```
clients = pd.read_csv('clientList.csv')
clients.head()
```

![](https://i.ibb.co/WgdQY6V/Screen-Shot-2021-04-30-at-5-17-19-PM.png)



