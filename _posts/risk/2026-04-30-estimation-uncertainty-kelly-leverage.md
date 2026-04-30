---
layout: post
title: Estimation uncertainty and optimal Kelly leverage
categories: risk
---

It turns out that the optimal leverage using an estimated mean is unaffected by how small your sample is. Even though you know you will underperform.

## The assumptions

1. Stable distributions
2. Distributions are normal
3. The variance $\sigma^2$ is known
4. There is no autocorrelation in log-returns

Let the *observed* log-return $\hat{\mu}$ be estimated as the mean of $n$ independent log-returns.
The true mean $m$ is unknown; from the data, $\hat{\mu} \sim \mathcal{N}(m, \sigma^2/n)$.

## The result

The Kelly-optimal leverage given the observed data is

$$
l^* = \frac{\hat{\mu}}{\sigma^2}
$$

The formula is unchanged by estimation uncertainty. What changes is the expected growth rate, which depends on whether we condition on the observed data or average over possible datasets.

## The derivation

With leverage $l$, log-wealth evolves as

$$
d(\log w) = \left(l \cdot m - \frac{1}{2} l^2 \sigma^2\right) dt + l\sigma\, dW.
$$

The growth rate is a quadratic function of $l$,

$$
g(l) = l \cdot m - \frac{1}{2} l^2 \sigma^2.
$$

This is a downward parabola with curvature $\sigma^2$ and peak at $l^* = m/\sigma^2$.

We don't know $m$, but we observe $\hat{\mu} \sim \mathcal{N}(m, \sigma^2/n)$. Taking expectation over the posterior on $m$,

$$
\mathbb{E}[g \mid \text{data}] = l \cdot \hat{\mu} - \frac{1}{2} l^2 \sigma^2,
$$

and maximizing,

$$
\frac{\partial}{\partial l}\mathbb{E}[g] = \hat{\mu} - l\sigma^2 = 0 \quad \Rightarrow \quad l^* = \frac{\hat{\mu}}{\sigma^2}.
$$

The growth parabola is symmetric around its peak. An estimation error $\delta$ to the left or right of the true optimum inflicts the same penalty. Because the posterior on $m$ is also symmetric about $\hat{\mu}$, there is no directional bias, so your best estimate $\hat{\mu}$ is also your optimal action. You cannot improve expected growth by systematically shifting leverage away from $\hat{\mu}/\sigma^2$.

Your estimation error in leverage space is

$$
\hat{l} - l^* = \frac{\hat{\mu} - m}{\sigma^2} \sim \mathcal{N}\left(0, \frac{1}{n\sigma^2}\right).
$$

For a parabola, the loss from missing the peak by distance $\delta$ is $\frac{1}{2} \cdot \text{(curvature)} \cdot \delta^2$. Averaging over your estimation error,

$$
\mathbb{E}[\text{penalty}] = \frac{1}{2} \cdot \sigma^2 \cdot \mathbb{E}[(\hat{l} - l^*)^2] = \frac{1}{2} \cdot \sigma^2 \cdot \frac{1}{n\sigma^2} = \frac{1}{2n}.
$$

The expected growth averaged over possible datasets is then

$$
\mathbb{E}[g^*] = \frac{1}{2} \cdot \frac{m^2}{\sigma^2} - \frac{1}{2n}.
$$

The penalty $\frac{1}{2n}$ is independent of $m$ and $\sigma$.

Conditioned on your observed $\hat{\mu}$, the optimal leverage is $l^* = \hat{\mu}/\sigma^2$ and your expected growth is $g^* = \frac{1}{2} \cdot \frac{\hat{\mu}^2}{\sigma^2}$. The estimate enters the leverage formula exactly as if it were the true mean.

If instead you average over all possible datasets before seeing any data, the expected growth is lower by $\frac{1}{2n}$,

$$
\mathbb{E}[g^*] = \frac{1}{2} \cdot \frac{m^2}{\sigma^2} - \frac{1}{2n}.
$$

The leverage formula is the same in both cases. The penalty $\frac{1}{2n}$ is the cost of not knowing where the peak of the parabola lies. It appears only when you average over possible datasets before the data is observed.

## The reality

- Distributions are not stable
- Distributions are not normal
- $\sigma$ is not known and must also be estimated

Of these, the first matters by far the most. You're unlikely to get a situation where $n$ is large and $m$ remains the same. Non-stationarity dwarfs the $\frac{1}{2n}$ correction derived here.

---

*An earlier version of this post (December 2022) contained an error in the variance calculation.*
