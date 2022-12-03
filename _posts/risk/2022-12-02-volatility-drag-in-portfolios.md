---
layout: post
title: Volatility drag and optimal leverage in portfolios
categories: risk
---

## Assumptions

1. Stable distributions
2. Distributions are normal
3. Distributions are known
4. The portfolio is continuously rebalanced

Let a continuously rebalanced portfolio be described by $$ \left\{ w, \mu, \Sigma \right\} $$, which are the vectors of weights and of logreturns, and the covariance matrix.

The portfolio covariance is then $$\bar{\sigma}^2 = w' \Sigma w $$ and the expected portfolio logreturn is $$ \bar{\mu} = w'\mu$$.

The expected value of the portfolio at time $$t$$ is then

$$
    p(t) = p_0 e^{(\bar{\mu} - \bar{\sigma}^2/2) t}.
$$

We would like to maximise the portfolio growth rate $$g$$,

$$
    g := \bar{\mu} - \frac{ \bar{\sigma}^2 }{2} = w'\mu - \frac{1}{2} w' \Sigma w
$$

So taking the (vector) gradient

$$
    \frac{dg}{dw} = \mu - \Sigma w
$$

which is zero at the maximum, gives the well known Kelly-Thorpe leverage

$$
    w_{opt} = \Sigma^{-1} \mu.
$$

Substituting back into the previous expression, the optimal expected value is then

$$
    p_{opt}(t) = p_0 e^{ \frac{1}{2} (\mu' \Sigma^{-1} \mu) t}.
$$

The covariance matrix can be written as the product of the (diagonal) matrix of standard deviations (volatilities) $$S$$, the correlation matrix $$R$$, and $$S$$, $$\Sigma = SRS$$.

Defining the vector of Sharpe ratios [^1] $$\varsigma$$ element-wise, $$\varsigma_i = \mu_i / \sigma_i $$, we can write this as

$$
    p_{opt}(t) = p_0 e^{ \frac{1}{2} (\varsigma' R^{-1} \varsigma) t}.
$$

We see that Sharpe ratios are important (no surprise there).

We also see that the correlation matrix is inverted. Highly correlated instruments are reflected in nearly parallel eigenvectors of $$R$$, which in turn will give rise to highly leveraged spread trades in those instruments.


## Risks and reality

As is well know, markets can gap and tails are fat. Large positions in quiescent instruments can blow up unexpectedly.

1. Distributions are not stable
2. Distributions are not normal
3. Distributions are not known
4. The portfolio is not continuously rebalanced

From the above, the source of large positions is twofold:
1. low-volatility positions that induce high leverage;
2. highly correlated instruments with (even slightly) differing expected Sharpe ratios.

So, in a sense, higher-volatility instruments are safer than lower-volatility ones, if you size accordingly.

Highly leveraged spread trades in correlated instruments may not be. The poster child for the latter is of course [LTCM], who put spread trades on between highly correlated, low-volatility bonds. They really should have known better.


[^1]: I've ignored the risk-free rate, if there is even such a thing.
[LTCM]: https://en.wikipedia.org/wiki/Long-Term_Capital_Management

