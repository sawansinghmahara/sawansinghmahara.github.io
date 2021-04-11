---
layout: page
title: "Memorylessness of the Geometric Random Variable"
categories: Probability
date:   2020-10-31 21:21:21 +0530
tags:
  - Probability
  - Problems
---
I wrote this explanation for some teaching assistant work for undergraduates during my Master's. It might be a bit sloppy, but here it goes! 

So let's look at a contrived game. Considering winning a lottery, you either win it or you don't. Let the probability of winning the lottery be $$p$$ and the probability of losing it will thus be $$(1-p)$$.

I have the genius strategy buying tickets until I win. Then I stop.

Now I ask the question **what is the probability that the  $$n^{th}$$ ticket I buy makes me win?**

The answer to it is actually uncertain, and so we model it using the help of a random variable called the **geometric random variable**, denoted by $$X$$

## Understanding the PMF

Let us  take an example, **what is the probability that the  $$3^{rd}$$ ticket I buy makes me win?**

This means we have to fail on the first, with a probability $$(1-p)$$  

AND

Fail on the second, with a probability of $$(1-p)$$

AND

Finally win on the third with a probability $$p$$

So the question $$\mathbb{P}(X=3)$$ is nothing but

$$\quad \quad \quad \quad \: \;\mathbb{P}(X=3)=(1-p)\cdot(1-p)\cdot p $$

$$\;\:\mathbb{P}(X=3)=(1-p)^{2}p$$

$$\;\;\;\;\;\mathbb{P}(X=3)=(1-p)^{3-1}p$$

So the answer to **what is the probability that the  $$n^{th}$$ ticket I buy makes me win?** is

 $$\mathbb{P}(X=n)=(1-p)^{n-1}\cdot p$$ 

This is the **PMF of the Geometric Random Variable**

 

## CDF

The CDF of this random variable will be

$$\mathbb{P}(X\leq n)=\overset{n}{\underset{i=1}{\sum}}(1-p)^{i-1}\cdot p$$

which can be simplified to be

$$\mathbb{P}(X\leq n)=1-(1-p)^{n-1}$$

Which means that

$$\mathbb{P}(X\geq n)=(1-p)^{n-1}$$

## Memorylessness

Memorylessness in the case of a geometric random variable would imply that the probability of winning after say, my $$10^{th}$$ try would be the same as winning after my $$15^{th}$$ try, given that I have already used up 5 tries.

Or in other words, telling me that I have used up and not won in my last $$t$$ tries, is as useful as not telling me anything at all.

The probability that I win in the next, say $$s$$ tries, is the same as that of me winning in the next $$s+t$$ tries, after you tell me I have not won anything in my first $$t$$ tries. Mathematically speaking, we mean

$$\mathbb P(X \geq s+t \mid X \geq t)=\mathbb P(X \geq s)$$

## Proof

How do we justify this?

Let's use Baye's theorem to help us out. Using it, we find that 

$$\mathbb P(X \geq s+t \mid X \geq t)=\frac{\mathbb P(X \geq s+t , X \geq t)}{\mathbb P(X\geq t)}$$

But what is $$\mathbb P(X \geq s+t , X \geq t)$$?

It is the same as asking **what is the probability that I win after $$s+t$$ trials and also after $$t$$ trials?**

The first question of the probability of winning after $$s+t$$ trials implies that

- We *consider winning after $$s+t$$* trials

and  the second question of winning after $$t$$ trials considers two cases

- *Winning after $$s+t$$ trials*
- Winning between the trials $$t$$ and $$s+t$$

So the only common event in these two questions (when they are jointly asked) is winning after $$s+t$$ trials

To generalise, we mean 

$$\mathbb P(X \geq s+t , X \geq t)=\mathbb P(X \geq s+t)$$

So, $$\mathbb P(X \geq s+t \mid X \geq t)$$  can be simplified as

$$\mathbb P(X \geq s+t \mid X \geq t)=\frac{\mathbb P(X \geq s+t , X \geq t)}{\mathbb P(X\geq t)}$$
$$\quad \quad \quad \quad \quad \quad \quad \quad \quad $$

$$=\frac{\mathbb P(X \geq s+t)}{\mathbb P(X\geq t)}$$

$$\quad \quad \quad \quad \quad \quad \quad \quad \quad $$

$$=\frac{\mathbb P(X \geq s+t)}{\mathbb P(X\geq t)}$$

But from earlier while deriving the CDF, we know that $$\mathbb{P}(X\geq n)=(1-p)^{n-1}$$ so substituting this, we get

$$\mathbb P(X \geq s+t \mid X \geq t)=\frac{(1-p)^{s+t}}{(1-p)^{t}}$$

$$\quad \quad \quad \quad \quad \quad \quad \quad \quad \;$$

$$=(1-p)^{s}$$

Which is nothing but $$\mathbb P(X \geq s)$$, so this implies

$$\mathbb P(X \geq s+t \mid X \geq t)=\mathbb P(X \geq s)$$

Which shows that the geometric random variable is in fact memoryless.
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
