# Understanding the Black-Scholes Model.

Here we will use the simple example of a European option for the analogy.

  

Essentially, we are pricing a **European option** which is basically a contract that pays only at maturity $T$.
* Call payoff at $T$
$$(S_T - K)^+$$

* Put payoff at $T$.
$$(K-S_T)^+$$

So the problem becomes:
> Given what I know today ($S_0$), what is the fair value today of that future payoff?

But that implies that, a **"Fair price today"** is just the present value of the expected future payoff, and this now raises a fundamental question:

#### Excpected under What Likelihood, or better, Probability?

And from this we realise that one of the elegant parts of the Black Scholes is that we don't use the real world probabilities, we instead use the risk neutral probability, where the drift becomes the risk free rate $r$.

But before all that, there is some basic concepts we need to understand:
For our European Option, at time $T$:
* If $S_T > K$ then we get, $S_T - K$$
* If $S_T \le K$ then we get, $0$

So mathematically:
$$
 payoff=max(S_T - K ,0)
 $$

### What exactly Does "Fair Price" mean?
Suppose we know the distribution the the stock price ,$S_t$, follows.
Then Logically, we can compute the expected payoff and discount it to today using the risk free rate.
$$
price = e^{-rT} \times \mathbb{E}[payoff]
$$

and we are back to the question we earlier mentioned, "Expected under what probability?"

### Core Insight of The Black Scholes.
Black Scholes does something very elegant and says:
>We shall not price using the real world drift $\mu$

Instead:
>We shall price assuming all assets grow at an average of the risk free rate.

This is the **Risk Neutral Probability**

Before I continue, don't get this wrong by thinking the investors are the ones who are risk neutral, it means, if no arbitrage exists, we can change the probability measure so that:
$$
\mu \rarr r
$$

This benefits us by making price both clean and arbitrage clean.

The main reason this is allowed is cause of replication.
If I can replicate the option payoff by stock snf risk free borrowing/lending, then the option must equal the cost of that strategy otherwise there is arbitrage.

This replication logic forces the pricing measure to behave like $drift = r$

