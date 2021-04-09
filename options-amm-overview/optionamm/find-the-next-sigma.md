---
description: >-
  This section explains how the protocol implemented a numeric method to find
  the next sigma considering a new pool imbalance.
---

# Find The Next Sigma

### Numerical Methods

As you read in the Pricing section, the AMM uses an algorithm to update the options price based on factors such as the spot price of the underlying asset, time to maturity, and implied volatility. This section explains how to programmatically estimate the implied volatility based on the new pool position considering the trade that will happen.

The new sigma is how we refer to implied volatility, and the system calculates it based on the new option target price \(which is the new "equilibrium BlackScholes price"\) for the pool.

The algorithm uses a numerical method to do that.

Numerical methods are mathematical ways to solve particular problems. They allow us to approximately solve math problems, specifically ones we can not solve analytically \(or at least not quickly\).

In our case, the Black Scholes formula is one example of an equation that can be analytically solved when calculating the option price. Still, it is tough when you already have the option price \(in this case, "the equilibrium price of the pool"\) and want to find one of the variables missing that resulted in this price. In this case, the missing variable at that point is the new sigma \(IV\).

### Our Numerical Method

In our case, the numeric method used is proposed by Yan Wang in his paper called [A Well-Posed Algorithm to Recover Implied Volatility](https://www.semanticscholar.org/paper/A-Well-Posed-Algorithm-to-Recover-Implied-Wang/ed2f3b452c3734f9ea766ad0210063c116eb580f).

For those familiar with search algorithms, this method resembles a [Binary Search](https://en.wikipedia.org/wiki/Binary_search_algorithm#:~:text=In%20computer%20science%2C%20binary%20search,middle%20element%20of%20the%20array.), where you should first define upper and bottom boundaries. After that, your next guess will be somewhere between those boundaries. If you have not found the target result inside this loop, the result found becomes one of the boundaries, and then we repeat the process.

Find below the steps the algorithm takes to find the new sigma applying Yan Wang numeric method.  

#### a\) Defining the boundaries

We need to define the upper and bottom boundaries for the sigma$$\sigma$$and its respective option price. We call these variables $$\sigma_{higher}$$, $$\sigma_{lower}$$, $$price_{higher}$$and $$price_{lower}$$.

There is no magic trick to define the initial boundaries. You can guess a very high sigma, calculate the Black Scholes equation using this sigma, and then check if the price is higher than the new target price. If the price you found is higher than the target price, this price became your initial upper boundary. To find the bottom boundary, repeat the same process with a very low sigma.

#### b\) Find the next sigma

To find the following result of this loop we use the following equation:

#### $$\displaystyle \sigma_{next}=\sigma_{lower}+\frac{(price_{target}-price_{lower}) *(\sigma_{higher} - \sigma_{lower} )}{price_{higher}-price_{lower}}$$

![In the SigmaGuesser contract, you can find this equation](../../.gitbook/assets/screen-shot-2021-04-02-at-09.54.25.png)

#### c\) Run Black Scholes and compare results

Now that we have found our `nextSigma` $$\sigma_{next}$$, we can re-calculate the Black Scholes and check if the calculated price is equal enough to the target price. In our case, we define that we accept 1% difference tolerance. This means that if the calculated price matches the target price within a 1% range, we accept them as "equal."

#### d\) Repeat if needed 

Suppose the calculated price differs more than 1% compared to the target price. We need to repeat the process, redefining the new boundaries. That is relatively straightforward. If the calculated price is below the target price, then the calculated price \(and the respective sigma\) became the lower boundary. If not, the calculated price became the upper boundary.

### How to avoid high gas costs

In environments such as Ethereum, you need to aim for efficient algorithms, otherwise, you will end up paying a lot to run a transaction. 

We work around this problem by running the `sigmaInitialGuess` contract before executing the transaction. You can perform a `call()` method with the trade that you want to execute.

Since `call()`methods are free, we run this numeric method and return the`nextIV`to the caller. Then, when he does the actual trade, he can pass the`sigmaInitialGuess`as a parameter that will be used as one of the initial boundaries or even be the right answer before even running this process.

