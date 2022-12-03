---
layout: post
title: Variance of the sample mean and impact on optimal leverage
categories: risk
---

## Assumptions

1. Stable distributions
2. Distributions are normal
3. Assume $$\sigma$$ is known
4. There is no autocorrelation in logreturns

Let the *observed* logreturn $$\mu$$ be estimated as a mean of a series of $$n$$ independent logreturns.

Using the variance of the sample mean, $$\mu$$ is drawn from a normal distribution, $$\mu \sim \mathcal{N}(m, \sigma_{\mu}^2)$$, with $$\sigma_\mu = \sigma / \sqrt{n}$$.

The error is then

$$
    \mu - m \sim \mathcal{N}(0, \sigma^2 + \sigma_\mu^2) = \mathcal{N}(0, (1/n + 1) \sigma^2) .
$$

The Kelly leverage $$l$$ is then $$ l \leq \frac{\mu}{(1/n + 1) \sigma^2}$$.


## Reality

1. Distributions and the joint distribution are not stable
2. Distributions are not normal
3. $$\sigma$$ is not known

Of these, the first matters by far the most.
In practice the estimation of $$\mu$$ is both more important and more uncertain than that of $$\sigma$$.
