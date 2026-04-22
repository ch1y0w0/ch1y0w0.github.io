---
layout: post
title:  "Understanding Linear Regression"
date:   2025-03-01 12:40:00 +0100
categories:
---

# What is Linear Regression?

**Linear Regression** is basically a method to find the best-fitting line through points of data. As we know in algebra, a line has the equation:

$$y = mx + b$$

Where $m$ is the slope of the line and $b$ is the intercept*(Where the line starts at $x = 0$)*.

In theory, if all our data points perfectly follow the equation, they form a line. Why? As we know in high-school geometry, a line is basically infinite points on a 2D space. If we give a number $x$ ($x \in R$) to our line equation, it gives us a $y$. The input which is called the $x$, is the position of that point on the $x$ axis and the output which is called $y$ is the position of that point on the $y$ axis, in our 2-Dimensional space. $x$ and $y$ together, form the coordinate of that point:

$(x, y)$

### Simple Example
Let's assume the following equation:
$$y = 3x + 2$$

Now let's calculate the output of $x=2$:
$$y = 3 . 2 + 2 = 8$$
So the coordinates are:
$$(2, 8)$$
Let's plot the coordinate:

![Plotting point $(2,8)$](https://github.com/ch1y0w0/ch1y0w0.github.io/blob/master/_posts/Pasted%20image.png)

Now imagine calculating the $y$ for all $x \in R$:

![Plotting all numbers to infinity](https://github.com/ch1y0w0/ch1y0w0.github.io/blob/master/_posts/Pasted%20image(2).png)

They form a line.

### What's the Relationship to Linear Regression?

By using **Linear Regression**, we can find the line through our dataset. Because in real-world applications, we use predictive models to predict the outcome of something that can't be perfectly calculated through formulas. If we find the best-fitting line, we can simply predict the future outcomes. So before, we reached the output by using the equation, and now we use the want to reach the equation using the outputs. The more data points we have in our dataset, the closer we can get to that equation.

### There's a Catch

In real-world applications, we almost never certainly reach the exact line because the perfect line doesn't exist. You ask why? because in real-world events, we always have noise. For example let's say we want to predict the price of a ride based on the distance traveled. Sometimes there's traffic, sometimes it's rainy and you must go slower. So there's always noise in most of the datasets and a dataset without noise doesn't need prediction.

### Simple Example

Let's say we have a dataset of the year-to-height relation of the Coastal Redwood tree. The dataset tells us that this specie of the tree has the height $y$ at it's age $x$. The number of data points is $n=1000$.

The noise? Sunlight, genetic, soil quality etc.

We show the equation with noise as:
$$y = (mx + b) + \epsilon$$
Where $\epsilon$ is our noise. When the model is trying to find $m$ and $b$, it ignores the noise because we can't predict the noise as it's a mixture of hundreds of small variables and even unknown ones.

### So, How Does a Model Learn Despite Having Noise?

That's where we get into the **Machine Learning** territory. As the name suggests, the machine learns to minimize the error. What is the error? Due to the noise, the predicted value has a difference with the actual value. The model tries to minimize the error as much as possible to reach the closest value to the actual line. That's where we use **Optimizing Algorithms**.

# Quick Look Into Optimization Algorithms

An **Optimization Algorithm** is an algorithm that uses a **Cost Function** to minimize the difference between the actual value and the predicted value:

$$error = y_{actual} - y_{prediction}$$

This step will repeat until the model reaches a point where the error can't be minimized further.

### What Are Cost Functions?

**Cost functions** is basically a function that gets all the errors and squashes them into a single number. Then the **Optimization Algorithm** tries to reduce that error by adjusting the $m$ and $b$ according to our $\alpha$ which is our **Learning Rate**.

Here are some common **Cost Functions**:

1. MSE(Mean Squared Error)
2. MAE(Mean Absolute Error)

# Normalization

Imagine a dataset with a big ratio(For example the height of the tree in millimeters according to it's age in years). As we need to 



# Coding Time

First, we do a quick coding in Python and then we implement it fully from scratch in C, cause i love C.

So we need to get the dataset, 
