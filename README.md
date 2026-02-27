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

Now, suppose a a stock follows:
$$
dS = \mu Sdt+\sigma SdW
$$

Here, we have 2 parts,

$\mu Sdt \rarr$ Deterministic Drift
$\sigma SdW\rarr$ Random Shock

Alse, let the option price $V$ be $V(S,t)$, which implies that the option price depends on price of the underlying stock and time. By it$\hat{o}$s Lemma we know that:
$$
dV = \frac{\partial{V}}{\partial{t}}dt+\frac{\partial{V}}{\partial{S}}dS+\frac{\partial^2{V}}{\partial{S}^2}(dS)^2
$$

Substituting $dS$ :
$$
dV = \frac{\partial{V}}{\partial{t}}dt+\frac{\partial{V}}{\partial{S}}(\mu Sdt+\sigma SdW)+\frac{\partial^2{V}}{\partial{S}^2}(\mu Sdt+\sigma SdW)^2
$$

$$
dV = \frac{\partial{V}}{\partial{t}}dt+
\frac{\partial{V}}{\partial{S}}(\mu Sdt+\sigma SdW)+
\frac{\partial^2{V}}{\partial{S}^2}((\mu S(dt))^2+2(\mu Sdt)(\sigma SdW)+
(\sigma S(dW))^2)
$$

But recall:
$$
(dW)^2 = dt
$$
and
$$
(dt)^2 = 0
$$
$\dots$
The side with the second partial derivative reduces as follows:
$$
\frac{\partial^2{V}}{\partial{S}^2}(\mu ^2S^2(dt)^2+2(\mu Sdt)(\sigma SdW)+\sigma^2 S^2dt)
$$

-   $(μSdt)2∼dt2  →$ignore
    
-   $2(μSdt)(σSdW)∼∼dt\sqrt{dt​}$  → ignore
    
-   $(σSdW)^2=σ^2S^2(dW)^2$
then:
$$
\frac{\partial^2{V}}{\partial{S}^2}(\sigma^2 S^2dt)
$$

$\dots$

Now we have:
$$
dV = \frac{\partial{V}}{\partial{t}}dt+
\frac{\partial{V}}{\partial{S}}(\mu Sdt+\sigma SdW)+
\frac{\partial^2{V}}{\partial{S}^2}(\sigma^2 S^2dt)
$$
factoring out $dt$:
$$
dV =
(\frac{\partial{V}}{\partial{t}}+
\mu S\frac{\partial{V}}{\partial{S}}+
\sigma^2 S^2\frac{\partial^2{V}}{\partial{S}^2})dt+
\frac{\partial{V}}{\partial{S}}\sigma SdW
$$

Now, we can consider a *self financing* trading strategy or loosely we can call it a hedged portfolio.
Let's define self financing.

### Defining Self Financing.
A portfolio is self financing if:
>The portfolio value at any time is affected only by the assets inside it. i.e .,

- No adding external cash.
- No removing internal cash.
If we are holding $\Delta_t$  shares of stock and $B_t$ invested in a bond, then the portfolio value at any time $t$ is:
$$
\Pi_t = \Delta_tS_t + B_t
$$
Self Financing in this case means :
$$
d\Pi_t = \Delta_tdS_t + rB_tdt
$$

We essentially just replicate the option using a mix of stock and a rock free asset, like a bill or a bond.

By default, our bond grows deterministically, so:
$$
dB = rBdt
$$
Assuming no arbitrage, the this portfolio perfectly replicates the option:
$\therefore$
$$
V = \Pi
$$
and :
$$
dV = d\Pi
$$

We defined $dV$ as:
$$
dV = \frac{\partial{V}}{\partial{t}}dt+
\frac{\partial{V}}{\partial{S}}(\mu Sdt+\sigma SdW)+
\frac{\partial^2{V}}{\partial{S}^2}(\sigma^2 S^2dt)
$$

and $d\Pi$ as:
$$
d\Pi_t = \Delta_tdS_t + rB_tdt
$$

substituting $dS_t$ from our original equation for stock price, we have:
$$
d\Pi_t = \Delta_t(\mu S_tdt+\sigma S_tdW_t) + rB_tdt
$$

$$
d\Pi_t = \Delta_t(\mu S_tdt)+
\Delta_t(\sigma S_tdW_t) +
rB_tdt
$$
$$
d\Pi_t = (\Delta_t\mu S_t + rB_t)dt+
\Delta_t(\sigma S_tdW_t)
$$

$\dots$

We notice that:
- 
The random part of $d\Pi \rarr$ $\Delta_t(\sigma S_tdW_t)$ is similar to the random part of $dV \rarr$ $\frac{\partial{V}}{\partial{S}}σS_tdW_t$

So for us to replicate, we need:
$$
\Delta_t = \frac{\partial{V}}{\partial{S}}
$$
And here, delta hedging emerges beautifully. I hope you remember when we also gave this strategy the name *"Hedged portfolio"*.


