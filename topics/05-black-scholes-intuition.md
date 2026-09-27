---
title: "05. Black–Scholes intuition"
layout: default
nav_order: 6
---

# Black–Scholes intuition
{: .no_toc }

*~10 min read*

**🎯 Interview frequent**

## Why it matters

Black–Scholes–Merton is the benchmark language of options: even when the market *doesn't* obey it, people quote prices as implied vols in a BS formula. Interviewers almost never want a PDE derivation from scratch; they want replication, risk-neutral pricing, what \\(N(d_1)\\) and \\(N(d_2)\\) mean, and which assumptions break in practice (see [Volatility & smiles](../12-volatility-smiles/)).

## Core concepts

- **The product is still just a payoff.** A European call is \\(\max(S_T-K,0)\\). BS is one way to compute the discounted risk-neutral expectation of that payoff when \\(S\\) is geometric Brownian motion with constant \\(\sigma\\).
- **Replication, not forecasting.** If you can dynamically trade the stock and a money-market account to match the option payoff, the option's price is the cost of that replicating portfolio. Otherwise there would be arbitrage. The stock's real-world drift \\(\mu\\) drops out because it is already in the stock you use to hedge.
- **Risk-neutral pricing is the other face of the same coin.** Change to a measure \\(Q\\) where the discounted (dividend-adjusted) asset is a martingale, i.e. the drift becomes \\(r-q\\). Then \\(V_0 = e^{-rT} E^Q[\text{payoff}]\\). You are not assuming people are risk-neutral; you are using the hedge to justify discounting at \\(r\\).
- **The formula (non-dividend European call):**
  \\[
  C = S_0 N(d_1) - K e^{-rT} N(d_2),
  \\]
  with
  \\[
  d_{1,2} = \frac{\ln(S_0/K) + (r \pm \tfrac12\sigma^2)T}{\sigma\sqrt{T}}, \quad d_2 = d_1 - \sigma\sqrt{T}.
  \\]
  Put: \\(P = K e^{-rT} N(-d_2) - S_0 N(-d_1)\\). Put-call parity still holds (see [Options payoff & Greeks](../04-options-payoff-greeks/)).
- **What \\(N(d_2)\\) and \\(N(d_1)\\) are.** \\(N(d_2) = Q(S_T > K)\\), the risk-neutral probability of finishing ITM. \\(N(d_1)\\) is the call's delta (no dividends): the number of shares in the replicating portfolio. So the formula reads: *shares \\(\times\\) stock minus borrowed cash \\(\times\\) bond*.
- **Inputs.** \\(S, K, r, T, q\\) are (mostly) observed. \\(\sigma\\) is not — it is the only free parameter, which is why the market inverts the formula for **implied vol**.
- **Assumptions that actually matter.** GBM with constant \\(\sigma\\); continuous trading / no jumps; no transaction costs; constant \\(r\\); European exercise. Reality: vol is not constant (smiles), markets jump, you rehedge discretely (see [Hedging & Greeks in practice](../13-hedging-greeks-practice/)).
- **Limits.** \\(\sigma \to 0\\) or \\(T \to 0\\): price \\(\to\\) discounted intrinsic (forward). \\(\sigma \to \infty\\): call \\(\to S_0 e^{-qT}\\). More vol helps the long option because payoff is convex (Jensen).

## Mental model

```
  Two equivalent stories:

  (A) Hedge              (B) Expectation
  ---------------------  -----------------------------
  sell 1 call            C = e^{-rT} E^Q [max(S_T-K,0)]
  hold  N(d1) shares
  borrow K e^{-rT} N(d2)
  rebalance as S,t move

  If (A) is possible, (B) must equal the cost of (A).
  mu never appears: it is already in the stock you hold.

  S_T under Q:  log S_T ~ N( ln S0 + (r-q-σ²/2)T , σ² T )
```

Think of BS as "the unique no-arbitrage price in a complete GBM world," not as a statistical forecast of \\(S_T\\).

## Interview questions

1. **Why doesn't \\(\mu\\) appear in the Black–Scholes formula? Doesn't a higher-drift stock make a call more valuable?**
   Answer: A higher \\(\mu\\) also makes the *stock* more expensive to short/long in the hedge. In the replicating argument the two effects cancel. Under \\(Q\\), drift is replaced by \\(r-q\\). A higher real-world drift *does* make the call more likely to finish ITM in the \\(P\\)-measure, but that extra chance is exactly the equity risk premium already priced in the stock.

2. **Interpret \\(N(d_1)\\) and \\(N(d_2)\\) in one sentence each.**
   Answer: \\(N(d_2)\\) is the risk-neutral probability the call finishes in the money. \\(N(d_1)\\) is the delta: the number of shares in the replicating portfolio (no dividends). The difference in the \\(d\\)'s is \\(\sigma\sqrt{T}\\), the lognormal width.

3. **A 1-year ATM-forward call with \\(\sigma=20\%\\), \\(r=0\\). Roughly, is the call worth 8% of spot, 20%, or 50%? How do you think?**
   Answer: ATM-forward, \\(r=q=0\\), \\(C \approx S_0 \sigma \sqrt{T/2\pi} \approx 0.08\, S_0\\) (the Brenner–Subrahmanyam / ATM approximation \\(0.4\,S\sigma\sqrt{T}\\) is the same order). So ~8% of spot, not 20% (that's vol, not price) and not 50% (that's a digital, or a deep-ITM delta).

4. **List three BS assumptions that fail in equity markets, and what we do instead.**
   Answer: (1) Constant vol → implied-vol smile/surface and local/stochastic vol models. (2) Continuous paths → jumps, fat tails, OTM puts expensive. (3) Continuous frictionless hedging → discrete rehedging, transaction costs, borrow fees. Practitioners still *quote* in BS implied vol.

5. **How is BS the continuum limit of a binomial tree?**
   Answer: Add more steps, set \\(u/d\\) from \\(\sigma\sqrt{\Delta t}\\), risk-neutral \\(p^*\\) from the drift \\(r-q\\). The binomial distribution of \\(\log S_T\\) converges to the BS lognormal, and the discounted expected payoff converges to the BS formula (see [Binomial trees](../06-binomial-trees/)).

## Watch

- [Introduction to the Black-Scholes formula](https://www.youtube.com/watch?v=pr-u4LCFYEY) — Khan Academy. Dissects the formula and why vol is the special input.
- [19. Black-Scholes Formula, Risk-neutral Valuation](https://www.youtube.com/watch?v=TnS8kI_KuJc) — MIT 18.S096 (Vasily Strela). Risk-neutral pricing from a one-step binomial through to BS.
- [Implied volatility](https://www.youtube.com/watch?v=VIHldsSmASU) — Khan Academy. Invert BS to read the vol the market is implying.

## Further reading

- Black & Scholes (1973); Merton (1973). You don't need the papers for interviews; you need the replication story.
- Hull, chapters on Wiener processes, BS-Merton, and Greek letters.
