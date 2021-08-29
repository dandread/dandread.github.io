---
layout: post
title:      "Breaking Down the Math Behind Linear Regression"
date:       2021-08-29 10:30:00 -0400
permalink:  math_behind_linear_regression
---

*Whether you are beginning to learn about linear regression or you're a seasoned regression expert, the underlying math behind regression analysis is crucial in understanding what your model means and how it works. With a variety of packages from statsmodels to sklearn, conducting regression analysis in Python is easy to do and doesn't require much knowledge of the mathematical underpinnings. While this opens the door for individuals of all backgrounds to conduct regression analysis, those who understand how regression analysis works from a mathematical perspective are better equipped to answer questions and make decisions based on the model.*

# Background
In this post, we will be looking at simple linear regression using ordinary least squares (OLS). This is the primary technique taught when getting started with regression analysis an is fairly straightforward - understanding how OLS works provides a good foundation for other regression techniques. OLS works by creating a best-fit line through a series of points; what does this mean? Essentially, this line minimizes the distance between a predicted point on the line and the actual point the model is trying to fit. Visually, you end up with something like this:

![RSS](https://user-images.githubusercontent.com/41350313/131257038-0a31ba50-83bc-4102-99b1-ec8a85341065.png)

In this plot, there are a series of random points that form somewhat of a linear pattern, but it's not exact. The best-fit line (shown in red) created by OLS attempts to get as close to each point as possible so the distance between the line and the points (shown in grey) are minimized. It's pretty straightforward, but powerful; the math, while a bit intimidating at first, is easy enough to follow and unlocks some powerful insights behind this otherwise unassuming graph.

# Equation of a line
The first step in understanding the math behind linear regression takes us back to algebra and the formula for the equation of a line. If you recall, the equation for a line is

<p align="center">
<img width="366" alt="eq_of_line" src="https://user-images.githubusercontent.com/41350313/131262360-5b8e7286-7527-4a34-ad3c-868ec496c8e9.png">
</p>

In this equation, a random value for x is chosen, multiplied by the slope of the line, and added to the y-intercept to output a y-value. As a quick refresh, the slope of a line measures a change in the y-value given a change in the x-value. This is best illustrated with an example. Suppose a bakery sells cookies for $2.00 each. If the x-axis measured the amount of cookies sold in one day, and the y-axis measured to total dollar sales of cookies, then for every additional cookie sold, total dollar sales increase by $2.00; thus the slope of the line is $2.00. If the bakery goes from selling 5 cookies to 6 cookies, their sales will increase from $10 to $12, which is shown below.

![slope_example](https://user-images.githubusercontent.com/41350313/131262765-c3ad0583-47d8-4753-b381-38b5853374b6.png)

The y-intercept, measured by *b*, is simply the value when *x* is zero. In the example above, if no cookies were sold, total dollar sales would be $0. Thus, putting it all together, the equation for the line representing total dollar sales per cookie sold is

<p align="center">
<img width="459" alt="cookie_example" src="https://user-images.githubusercontent.com/41350313/131262883-20e5ba25-b919-4cf3-8223-de1f8d05b50e.png">
</p>

With this equation, the bakery can predict, with 100% accuracy, the total dollar sales of cookies by entering in values for *x*. Want to know how much selling 50 cookies would bring in? Simply use the equation to determine $100 would be made. Unfortunately, in the real-world we don't have nice, simple equations like this to make predictions. This is where linear regression comes in; it will help us find a best-fit line, and hence equation, that can be used for predictions. Before moving on, a change in syntax is needed. The formulas used in linear algebra, the branch of math responsible for OLS, uses different symbols for the variable. 

<p align="center">
<img width="405" alt="eq_of_line_var_swap" src="https://user-images.githubusercontent.com/41350313/131263020-9d20fd89-b33c-4dee-8d39-8781ccab5db9.png">
</p>

Note that equation is exactly the same, but the variables are different. It may look alien, but as long as you understand what each variable represents, the rest follows naturally.

# Simple Linear Regression via OLS
Now, instead of the nice, neat cookie example above, suppose we are presented with a different set of data that looks like this:

![scatter](https://user-images.githubusercontent.com/41350313/131263088-a57d627e-3c27-4d1b-bdd2-ff8b85477e81.png)

In this case, there is no line that can be drawn that will pass through each individual point of the graph. Therefore, there is no one equation that will provide the y-value for each x-value. It is important to note that even if the data is not perfectly linear, it is linear in nature. Using OLS, we can come up with a line that best explains these data points, even if it is unable to pass directly through each.

## OLS Formula
As established above, there is no equation of the form discussed above that will work for this data. Luckily, there is an easy fix for that. By adding an error term to the basic equation of a line, we can create a formula that addresses the discrepencies between each point and the best-fit line. 

<p align="center">
<img width="505" alt="OLS_formula" src="https://user-images.githubusercontent.com/41350313/131263720-a003f426-7b40-451d-b516-004c14204ab0.png">
</p>

This is the OLS equation, and it's almost identical to the equation of the line discussed above, but it has an error term attached to it. Notice, too, that the *y* term and *beta* term have a symbol above them. This is read as "y-hat" and indicates that each of these terms are predictions. So, if we draw any line through these points, the error term measures the distance between a point and the best fit line. Graphically, this looks like:

![RSS](https://user-images.githubusercontent.com/41350313/131257038-0a31ba50-83bc-4102-99b1-ec8a85341065.png)

## Residual sum of squares
One question remains; how do we find the line? This is where Python comes in! There are a variety of packages to choose from, but essentially the OLS method is looking to make that error term as small as possible. As discussed above, the distance between each point and the line is the error term - this is also called the residual. If all of these error terms are added up, we can compare one line to another to see which one will produce the lowest error, and hence, the best-fit.

The residual sum of squares is fairly intuitive. If you want to see how far apart two point are, you subtract them - this is exactly how we find the residual sum of squares (RSS). RSS is a statistical technique used to measure the amount of variance not explained by the linear regression model, that is, the error term. The formula is given below:

<p align="center">
<img width="730" alt="RSS_formula" src="https://user-images.githubusercontent.com/41350313/131266791-08e76663-ee03-4c35-9fd2-b232ddc5e9cc.png">
</p>

For each point, we subtract the actual y-value from each predicted y-value, square the result (this is just to get rid of any negatives since we only care about the distance in absolute terms), and take the sum of this for all points, you have found the RSS. We can check this by rearranging the OLS formula.

<p align="center">
<img width="957" alt="resid_error" src="https://user-images.githubusercontent.com/41350313/131266860-fff90e28-54a6-419c-be6b-45634942bc68.png">
</p>

If we set everything equal to the error term, we find that the error term is equal to the predicted y-value minus the intercept minus the slope times the x-value. Grouping this second part, you should be able to see this is exactly the same as the predicted y-value minus the actual y-value. Seeing how these equations interplays makes them less intimidating; we are really just moving different pieces around. These equations are all inter-related.

So, if we created a line A with an RSS of 120 and another line B with an RSS of 200, we could safetly assume that line A is a better model. But, what if this amount was much larger, say 120,000 and 200,000 for lines C and D, respectively? Also, what if I told you lines A & C, B & D, were equal to each other in accuracy when fitted to the data? This would appear to fly in the face of what was just explained, but it highlights a critical aspect of the RSS - it is affected by scale. The difference between line A having an RSS of 120 vs 120,000 can be explained by the fact that 120 was in units of kilometers, and 120,000 was in units of meters. So, the only different between the two numbers is scale, but you get two drastically different RSS values. This is why the RSS value, why important, is a poor metric to use when comparing models because it doesn't tell us much.

## Using meters
![meters_example](https://user-images.githubusercontent.com/41350313/131267450-864d6252-14f5-4f1e-98ee-f9a613b647ed.png)

## Using kilometers
![kilometers_example](https://user-images.githubusercontent.com/41350313/131267455-ce6f0795-c131-4a23-8267-4e184a3b945a.png)




![TSS](https://user-images.githubusercontent.com/41350313/131261756-a43c5b08-cc68-47a7-9a86-a5b0e8a0eacd.png)

It does this by calculating something called an r-squared score (also known as coefficient of determination) which measures the proportion of the dependent variable that is explained by the independent variable. In other words, the r-squared score tells us how much of y-variable is explained by the x-variable. If we go back to the bakery example, the r-squared score would 100% because our sales (y) was completely determined by the number of cookies sold (x) and every single point falls on the line. Looking at the image above, the r-squared score will be less than 100% because not all of the points lie on the line.
