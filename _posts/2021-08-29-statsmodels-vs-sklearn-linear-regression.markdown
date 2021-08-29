---
layout: post
title:      "Comparing statsmodels and scikit-learn for Linear Regression"
date:       2021-08-29 10:30:00 -0400
permalink:  statsmodels_vs_sklearn_for_linear_regression
---

*If you are just starting to explore linear regression analysis using Python, you will likely have heard of two different packages that can be used: statsmodels and sklearn. It can be confusing at first to understand the differences, use cases, and strengths/weaknesses of each. This post will explore the different ways linear regression can be conducted using both packages while highlighting the use cases for each.*

# Background
In this post, we will be looking at simple linear regression using ordinary least squares (OLS). This is the primary technique taught when getting started with regression analysis an is fairly straightforward. OLS creates a best-fit line through a series of points; what does this mean? Essentially, this line minimizes the distance between a predicted point on the line and the actual point the model is trying to fit. Visually, you end up with something like this:

