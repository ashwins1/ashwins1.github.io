---
layout: post
title:  "The Illusion of Fairness"
date:   2015-11-04 23:19:44 -0800
categories: math
---

Today's post will deal with two separate, fairly well-known brainteasers that
deal with simulating fair (i.e., uniform) randomness using biased random bits
as input.

The first question is a new take on the classic trading interview question:
given a biased random coin which comes up heads with probability \\(p\\),
simulate an unbiased coin flip. The equally-classic answer is readily available
online and in every trading interview prep book ever written, but on the
off-chance (no pun intended) that you haven't heard it before, it's worth
taking a minute to think about before you spoil the answer for yourself.

The classic answer is the so-called "von Neumann trick". Basically, you
repeatedly flip the coin twice. Every time it comes up \\(HH\\) or \\(TT\\), go
back and flip the coin a second time. If the coin comes up \\(HT\\), output
that your simulated fair coin is heads. If the coin comes up \\(TH\\), output
that the simulated fair coin is tails.

This works because either of the latter two options (i.e., \\(HT\\) or \\(TH\\)
on the biased coin) occurs with equal probability, namely \\(p (1-p)\\).
Therefore, there is an equal chance of either scenario terminating the process
and yielding an output of the simulated fair coin. Unfortunately, it can take a
while to get there: for each given pair of flips of the biased coin, we will
stop the game with probability \\(2 p (1-p)\\), i.e., if and only if the
two flips are either \\(HT\\) or \\(TH\\). The total number of flips until
we stop the game, then, is a geometric random variable with parameter
\\(2 p (1-p)\\). If \\(p\\) is very large (close to 1) or small (close to 0),
the expected value of this random variable can be very large indeed:
as an example, if \\(p = 0.9999\\), the expected number of flips before we
can output a head or tail of the simulated coin is slightly more than 5000.

Clearly this is not a very efficient way of generating fair coin flips if
our initial source of randomness is heavily biased towards heads or tails.
How can we generate fair coin flips while using fewer flips of the biased
coin? Alternatively, can we prove that no other algorithm can do better
than this?

The other problem for today is similar: pick some number of coins \\(k\\),
each of which has its own bias (probability of heads), \\(p_1, \ldots, p_k\\).
Now, using these coins, simulate a roll of a fair \\(n\\)-sided die.


This problem has several degrees of freedom: you are free to choose the number
of coins \\(k\\) to be as high as you want, and you can choose the bias on each
coin to be whatever you like. As you might expect, there are many ways to select
the coins and biases to make this work.

A fairly intuitive approach is as follows: you can think of the problem of
rolling a fair \\(n\\)-sided die as the problem of generating an integer in the
range \\([0, n)\\) uniformly and at random.  First, figure out how many bits
are needed to represent all the numbers in that range; this is simply \\(\lceil
\log_2 (n) \rceil\\). Let \\(k\\) (the number of coins) be equal to this
quantity. For simplicity, set the bias of each of the coins to \\(0.5\\).
Now generate the uniform random number as follows:

  1. Flip all of the coins and interpret the result as a binary number:
     the \\(i^\text{th}\\) coin (where \\(i \in \left\{ 1, \ldots, k \right\}\\))
     maps to the \\(i^\text{th}\\) digit of the number, where \\(H\\) is represented
     as 1 and \\(T\\) as 0.
  2. If the resulting binary number is outside the desired range (i.e., if it is
     greater than or equal to \\(n\\)), go back to step 1. Otherwise, output the
     binary number as the result of the die roll.

It should be intuitively clear that this approach is fair: since each of the
coins is fair, all binary numbers between 0 and \\(2^k - 1\\) have an equal
shot at being selected. Some of those numbers might be outside the range of
values that we want to generate; if we hit one of those numbers, we will simply
try again. That is, we take a uniform distribution over the numbers \\((0,
2^k]\\) and (possibly) "clip" it by setting to 0 the probabilities of all
numbers greater than \\(n-1\\).  The distribution over the remaining numbers
(i.e., the numbers in \\([0, n)\\) will still be uniform. Hence, conditional on
having stopped the procedure, we will know that the output is within the
appropriate range, and that it will have been selected from among all numbers
in that range with uniform probability.

As with the previous problem, however, there is an inefficiency here. As an
exercise, come up with a pathological case, i.e., one that maximizes the
expected number of times we have to flip all the coins before we generate a
simulated roll of the fair die.  Asymptotic worst cases are fine. Then, find a
way to fix the bug: given that we are allowed to choose any number of coins,
and give them any bias we like, how can we minimize the number of times we
have to flip all the coins? Can you make it so that we only flip the coins once?
