---
layout: post
title: Investing in Ponzi schemes is sometimes rational
categories: risk
---

In a remarkable case of nominative determinism[^1], [Bernie Madoff](https://en.wikipedia.org/wiki/Madoff_investment_scandal) ran Madoff Investment Securities from 1960 - 2008, which was an elaborate Ponzi scheme with the appearance of a very successful wealth management firm.

Many individuals and organisations gave money to Madoff, even though the signs were obvious that his firm was a fraud. Why?


## Ponzi schemes

In the following, we will assume that we can estimate a probability of default in the next period, which we call $$p_d$$.
The nominal fractional payout after period if there is no default is $$r$$.
We will assume payout is binary and known ahead of time (approximately true for the case of Madoff's firm, since his returns were unnaturally steady).
In the case of default, we get the recovery rate, $$d$$.
We also assume that we can withdraw (or deposit) at the (fake) book value, any amount at any time before a default.

The amount at risk (the "stake") is then $$(1 - d)$$.

The Kelly bet is then

$$
    f = 1 - p_d - \frac{p_d (1-d)}{r}
$$

(where $$f$$ is the fraction of the bankroll).

For the case of Bernie Madoff, the [recovery rate](https://en.wikipedia.org/wiki/Recovery_of_funds_from_the_Madoff_investment_scandal) was, amazingly, about $12 billion of $65 billion, so $$d \simeq 0.18$$, and the "return" was about $$r=0.15$$ annually.

The probability of default over time, $$p_d(t)$$ is very difficult to know (we only have one historical path per scheme) but, being a massive fraudulent literal Ponzi scheme, approached 1 by the end. We know that such schemes can persist for some time --- Madoff's for nearly 50 years --- so $$p_d(t)$$ is surely quite low at the beginning.

The Kelly bet looks like this:

![Kelly fraction for Madoff investment securities](/assets/risk/madoff.svg)

It was rational to invest in Madoff securities, in the early days.


## But really, just don't

In the case of Madoff, there is the obvious ethical dimension to consider, where you would be a willing participant in a fraud. So I can't recommend it.
It's worth noting that [Ed Thorpe](https://en.wikipedia.org/wiki/Edward_O._Thorp), who understands Kelly betting probably better than anybody, chose not to "invest" in Madoff's fund in 1991, considering it likely fraudulent.
In contrast, [Renaissance Technologies](https://en.wikipedia.org/wiki/Renaissance_Technologies) did invest, and reduced its exposure in 2003 and then over time.


[^1]: In another remarkable case of nominative determinism, [Sam Bankman-Fried](https://en.wikipedia.org/wiki/Sam_Bankman-Fried) ran the cryptocurrency broker-dealer [FTX](https://en.wikipedia.org/wiki/FTX_(company)) which also relied on an increasing inflow of customer funds to continue existing.

[Sornette]: https://isbnsearch.org/isbn/9780691175959
