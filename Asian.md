In this article I'm going to discuss how to price a certain type of Exotic option known as a Path-Dependent Asian in C++ using Monte Carlo Methods. It is considered "exotic" in the sense that the pay-off is a function of the underlying asset at multiple points throughout its lifetime, rather than just the value at expiry. An Asian option actually utilises the mean of the underlying asset price sampled at appropriate intervals as the basis for its pay-off, which is where the "path-dependency" of the asset comes from. The name actually arises because they were first devised in 1987 in Tokyo as options on crude oil futures.

There are two types of Asian option that we will be pricing. They are the arithmetic Asian and the geometric Asian. They differ only in how the mean of the underlying values is calculated. We will be studying discrete Asian options in this article, as opposed to the theoretical continuous Asian options. The first step will be to break down the components of the program and construct a set of classes that represent the various aspects of the pricer. Before that however, I will brief explain how Asian options work (and are priced by Monte Carlo).

Overview of Asian Options
An Asian option is a type of exotic option. Unlike a vanilla European option where the price of the option is dependent upon the price of the underlying asset at expiry, an Asian option pay-off is a function of multiple points up to and including the price at expiry. Thus it is path-dependent as the price relies on knowing how the underlying behaved at certain points before expiry. Asian options in particular base their price off the mean average price of these sampled points. To simplify this article we will consider $N$ equally distributed sample points beginning at time $t=0$ and ending at maturity, $T$.
Unlike in the vanilla European option Monte Carlo case, where we only needed to generate multiple spot values at expiry, we now need to generate multiple spot paths, each sampled at the correct points. Thus instead of providing a doub le value representing spot to our option, we now need to provide a std : : vector $<$ doub le> (i.e. a vector of double values), each element of which represents a sample of the spot price on a particular path. We will still be modelling our asset price path via a Geometric Brownian Motion (GBM), and we will create each path by adding the correct drift and variance at each step in order to maintain the properties of GBM.
In order to calculate the arithmetic mean $A$ of the spot prices we use the following formula:
$$
A(0, T)=\frac{1}{N} \sum_{i=1}^N S\left(t_i\right)
$$
Similarly for the geometric mean:
$$
A(0, T)=\exp \left(\frac{1}{N} \sum_{i=1}^N \log \left(S\left(t_i\right)\right)\right)
$$
These two formulae will then form the basis of our pay_off_price method, which is attached to our Asian0ption class.


Reference:
https://www.quantstart.com/articles/Asian-option-pricing-with-C-via-Monte-Carlo-Methods/

barrier
http://www.quantstart.com/articles/Monte-Carlo-Simulations-In-CUDA-Barrier-Option-Pricing/