---
layout: post
title:      "Breaking Down the Math Behind Linear Regression"
date:       2021-08-29 10:30:00 -0400
permalink:  math_behind_linear_regression
---

*Whether you are beginning to learn about linear regression or you're a seasoned regression expert, the underlying math behind regression analysis is crucial in understanding what your model means and how it works. With a variety of packages from statsmodels to sklearn, conducting regression analysis in Python is easy to do and doesn't require much knowledge of the mathematical underpinnings. While this opens the door for individuals of all backgrounds to conduct regression analysis, those who understand how regression analysis works from a mathematical perspective are better equipped to answer questions and make decisions based on the model.*

# Background
In this post, we will be looking at simple linear regression using ordinary least squares (OLS). This is the primary technique taught when getting started with regression analysis an is fairly straightforward - understanding how OLS works provides a good foundation for other regression techniques. OLS works by creating a best-fit line through a series of points; what does this mean? Essentially, this line minimizes the distance between a predicted point on the line and the actual point the model is trying to fit. Visually, you end up with something like this:

![errors](https://user-images.githubusercontent.com/41350313/131257038-0a31ba50-83bc-4102-99b1-ec8a85341065.png)

In this plot, there are a series of random points that form somewhat of a linear pattern, but it's not exact. The best-fit line (shown in red) created by OLS attempts to get as close to each point as possible so the distance between the line and the points (shown in grey) are minimized. It's pretty straightforward, but powerful; the math, while a bit intimidating at first, is easy enough to follow and unlocks some powerful insights behind this otherwise unassuming graph.

# Equation of a line
The first step in understanding the math behind linear regression takes us back to algebra and the formula for the equation of a line. If you recall, the equation for a line is $y = mx + b$.
