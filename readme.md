In this article we will price the same European vanilla option with a very basic Monte Carlo solver in C++ and then compare our numerical values to the analytical case. We won't be concentrating on an extremely efficient or optimised implementation at this stage. Right now I just want to show you how to get up and running to give you an understanding of how risk neutral pricing works numerically. We will also see how our values differ from those generated analytically.

## Risk Neutral Pricing of a European Vanilla Option
The first stage in implementation is to briefly discuss how risk neutral pricing works for a vanilla call or put option. 

it might help to have a look at the article on Geometric Brownian Motion, which is the underlying model of stock price evolution that we will be using. It is given by the following stochastic differential equation:

$$
d S(t)=\mu S(t) d t+\sigma S(t) d B(t)
$$
where $S$ is the asset price, $\mu$ is the drift of the stock, $\sigma$ is the volatility of the stock and $B$ is a Brownian motion (or Wiener process).
You can think of $d B$ as being a normally distributed random variable with zero mean and variance $d t$. We are going to use the fact that the price of a European vanilla option is given as the discounted expectation of the option pay-off of the final spot price (at maturity $T$ ):
$$
e^{-r T} \mathbb{E}(f(S(T)))
$$
This expectation is taken under the appropriate risk-neutral measure, which sets the drift $\mu$ equal to the riskfree rate $r$ :
$$
d S(t)=r S(t) d t+\sigma S(t) d B(t)
$$
We can make use of Ito's Lemma to give us:
$$
d \log S(t)=\left(r-\frac{1}{2} \sigma^2\right) d t+\sigma d B(t)
$$
This is a constant coefficient $\underline{\mathrm{SDE}}$ and therefore the solution is given by:
$$
\log S(t)=\log S(0)+\left(r-\frac{1}{2} \sigma^2\right) T+\sigma \sqrt{T} N(0,1)
$$
Here I've used the fact that since $B(t)$ is a Brownian motion, it has the distribution as a normal distribution with variance $T$ and mean zero so it can be written as $B(t)=\sqrt{T} N(0,1)$.
Rewriting the above equation in terms of $S(t)$ by taking the exponential gives:
$$
S(t)=S(0) e^{\left(r-\frac{1}{2} \sigma^2\right) T+\sigma \sqrt{T} N(0,1)}
$$
Using the risk-neutral pricing method above leads to an expression for the option price as follows:
$$
e^{-r T} \mathbb{E}\left(f\left(S(0) e^{\left(r-\frac{1}{2} \sigma^2\right) T+\sigma \sqrt{T} N(0,1)}\right)\right)
$$
The key to the Monte Carlo method is to make use of the law of large numbers in order to approximate the expectation. Thus the essence of the method is to compute many draws from the normal distribution $N(0,1)$, as the variable $x$, and then compute the pay-off via the following formula:
$$
f\left(S(0) e^{\left(r-\frac{1}{2} \sigma^2\right) T+\sigma \sqrt{T} x}\right)
$$
In this case the value of $f$ is the pay-off for a call or put. By averaging the sum of these pay-offs and then taking the risk-free discount we obtain the approximate price for the option.

Reference:
https://www.quantstart.com/articles/European-vanilla-option-pricing-with-C-via-Monte-Carlo-methods/