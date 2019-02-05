---
title:  "Ray Tracing The Rest of Your Life: A reader's companion, Chapter 1"
date:   2019-01-07 10:30
categories: rendering, mathematics, monte-carlo
---
This blog series is a set of notes that expand on Dr Shirley's "Ray Tracing The Rest of Your Life" book. The book is short and sweet, introducing all the tools that one will need in their arsenal to build a sophisticated ray tracer. I found it very refreshing in terms of simplifying otherwise esoteric concepts and bridging the gap between simple mathematical concepts and how they are used in ray tracing. 

My journey through the book raised many questions, which Dr Shirley was very helpful in answering and explaining. Along the way I read many different articles and saw different videos to understand things. I am probably not the only person who needed help understanding things and won't be the last, so I thought it would be a good idea to share the information that I gathered.

The format of this post is unconventional  - you should have a copy of [the book](http://www.realtimerendering.com/raytracing/Ray%20Tracing_%20the%20Rest%20of%20Your%20Life.pdf){:target="_blank"} (ideally the pdf as it is the most recent updated) open in another tab. I’ll try to guide you as best as I can throughout the book by suggesting to look at the book and come back when you reach a certain point.
<p align="center" style="color:red;">
<b>These statements will be in bold and red! </b></p>

Re-reading is also very important - it’s very hard to understand everything at a first read. Don’t worry too much if you don’t get everything immediately. These are not straightforward concepts so just keep at it. With all that said, let’s start.

# Chapter 1: A Simple Monte Carlo Program

The first chapter is very straightforward. Demonstrating Monte Carlo by estimating $$ \pi $$ is a prevalent computational statistics example.

The code presented in the book very simple and self explanatory. A point is generated inside a square and if it is inside the circle, we increase the variable tracking the points inside the circle. We divide this final count by the total number of points we generate to get an average answer to $$ \pi $$. 

<p align="center" style="color:red;">
<b>
It’s time to go look at the book - read up until the you get to the 2nd code snippet, involving the running \(\pi\) calculation program.
</b>
</p>

One of the questions of those uninitiated in probability and Monte Carlo theory might be: How does this type of computation work and why give us an estimate of $$ \pi $$? This stems from what is known as the **expected value**. 

A classic example in explaining the expected value is trying to answer the question: 

<p align="center">
What is the probability that I will get a heads, when I flip a fair coin?
</p>

We know intuitively that it is 0.5. However what the expected value theory shows us is that if we 

1. get an actual fair coin, 
2. start flipping it, 
3. take note of each result, 
4. count the number of heads and tails, 
5. average them, 

We will probably get a result close to 0.5. The more times we toss the coin, the closer to 0.5 the answer will be.

The mathematical notation used to describe the expected value is:

$$
E[X] = \sum_{i=1}^k x_i p_i = x_1 p_1 + x_2 p_2 + ... + x_k p_k
$$

* $$ E[X] $$ is the expected value of $$ X $$, our statistical experiment.
* $$ x_i $$ is the result of each sample that we compute of our function
* $$ p_i $$ is the probability weight of this sample.

 The reason why this works is a theorem known as the **Law of Large Numbers**. The weak law of large numbers states that if we take a number of samples and average them, it will *PROBABLY* converge to the expected value. The strong law of large numbers states that it will converge *ALMOST SURELY* (with probability 1). We’re not going to go down this rabbit hole, the one of interest to us in the weak law.

An important term that we should define is **variance**. In our example, we were trying to compute $$ \pi $$, which makes this  the mean we’re trying to estimate. The variance, in very informal language, measures how spread out our sample computations are from this mean. The lower the variance, the less spread out the samples are and the more accurate our computed mean is to the expected value (the expected value is also known as the mean). If we have high variance, this means our samples are spread out, and we might be using an ineffective function to computationally find our mean.

The **Law of Diminishing Returns** explains the idea that the more samples we take, the more we need than before, to get an accurate answer.  In terms of numbers, if you want to half the error that you’re getting, you’ll need to quadruple the amount of samples. Here is how the error between our estimated value of $$ \pi $$ and actual $$ \pi $$ decreases as we compute more samples.

<p align="center">
<img src="{{ site.url }}/assets/posts/rt_rc_chapter1/pi_estimate.png" alt=" $$ \pi $$ Estimate error">
</p>

This doesn't hold a lot of significance for now but it's good to keep this in mind.

<p align="center" style="color:red;">
<b>Now would be a good time to go read the rest of the chapter.</b>
</p>

Stratification is a way of intelligently placing your samples to estimate your function better. If you look at my blog post about random numbers, I show that a good PRNG (Pseudo-random number generator) should generate uniformly distributed points. However to ensure true uniformity, without having clustered samples, we stratify the samples we take. 

The careful selection of samples to minimize variance is an ongoing area of research, that is an absolutely fascinating subject and one of my favourite topics in CG. Dr. Shirley expands a bit on this is in this blog post: [Flavors of sampling in ray tracing](http://psgraphics.blogspot.com/2018/10/flavors-of-sampling-in-ray-tracing.html){:target="_blank"}.
Some more links from me about this topic. I’ll re-visit this at the end of Chapter 2, once we define the Monte Carlo Integral.


The next post will be on Chapter 2, where I'll delve a bit more into Monte Carlo theory and we'll start performing some more interesting computations.

If you have any questions and feedback, feel free to comment or contact me via twitter/email.
