---
title:  "Random Number Generation and Transformation"
date:   2018-07-04 10:30
categories: mathematics, monte-carlo
---
In this post I'll talk a bit about random number generation and ways that we can generate random numbers that originate from a certain distribution. This will be the first in an assortment of posts  regarding monte carlo methods.

I'll assume you have working knowledge of calculus and some probability and statistics.

The most essential aspect of any form of monte carlo method is the generation of random numbers. Uniformly distributed random numbers on the interval [0,1] are what we'd like to generate first, as they are also what is required for the other types of distributions.

Generating random numbers using a pseudo random number generator (PRNG) means that we generate a sequence of numbers, using some particular seed value (some arbitrary constant) that initializes the random number generation algorithm. For this sequence to be considered to be of good quality, it should have certain properties, such as lack of predictibility (i.e. if we generate x values, and we can guess the next, this means the predctibility is possible) and equidistribution. For example if the mean of a large sequence of uniformly distributed numbers in the range [0,1] is not 0.5, then it is likely that something is wrong with the PRNG, given that due to the Law of Large Numbers, we would expect the arithmetic mean of this sequence to be similar to the expected mean. We won't focus on evaluating PRNGs and leave that to other minds, some useful links are http://pit-claudel.fr/clement/blog/how-random-is-pseudo-random-testing-pseudo-random-number-generators-and-measuring-randomness/ and http://simul.iro.umontreal.ca/testu01/tu01.html.

Let's look at a simple PRNG known ad the Linear Congruential Generator (which we'll now start referring to as LCG).

The LCG is an easy-to-implement PRNG, the function definition being:

X_n+1 = (a * X_n + c) mod m

where m is the modulus constant, and m > 0.
a is the multiplier, and 0 < a < m
c is te increment, 0 <= c <= m
and X_0 is the seed value, 0 <= X_0 < m.

The above constants are all integers. Careful selection of these values ensures that the sequence we get is of a high quality.

Here's an R listing of what the code for this should look like:

N is the number of random numbers we want to generate. Let's generate 10000 and plot a histogram to look at the distribution.

It's fair to say, this distribution looks good. Given that the generation of a value is dependent on the next let's see if we can see a pattern by plotting them in a time series.

That looks pretty random, so let's plot them in series, taking pairs as cartesian coordinates (x_2k-1, x_2k).

Well that's interesting, pairs line up as lines... what if we plot them in 3D?