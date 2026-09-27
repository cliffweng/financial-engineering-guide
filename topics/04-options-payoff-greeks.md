---
title: "04. Options payoff & Greeks"
layout: default
nav_order: 5
---

# Options payoff and Greeks
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

If you cannot draw a payoff diagram and name the first-order Greeks, you will not survive a derivatives interview. Options are nonlinear bets: limited loss if you buy, unbounded or large loss if you sell. Greeks turn that nonlinearity into a risk report — delta for direction, gamma for curvature, vega for vol, theta for time. Pricing models (Black–Scholes, trees, Monte Carlo) exist to turn these payoffs into a present value; the payoff itself is model-free.

## Core concepts

- **Call vs. put, long vs. short.** A European call pays \\(\max(S_T - K, 0)\\) at \\(T\\); a put pays \\(\max(K - S_T, 0)\\). Long option: you *paid* premium, payoff is floored at zero. Short option: you *received* premium and have the opposite payoff — that is where blow-ups live.
- **Intrinsic vs. time value.** Intrinsic is the immediate-exercise value \\(\max(\pm(S-K), 0)\\). Time value is extra you pay for optionality (vol, remaining time). At expiry, time value is zero and price = payoff.
- **Moneyness.** ITM / ATM / OTM is about \\(S\\) vs \\(K\\) (or forward vs \\(K\\)). A 1-delta call is deep ITM; a 25-delta put is OTM. Market makers quote by delta as often as by strike.
- **Put-call parity (European, same \\(K,T\\)):** \\(C - P = DF(T)\,(F - K) = S_0 e^{-qT} - K e^{-rT}\\). Equivalently \\(C + Ke^{-rT} = P + S_0\\) (no dividends). If it fails, you replicate the cheap side and short the rich side — model-free arb (see [Forwards / futures](../03-forwards-futures/) for \\(F\\)).
- **American vs. European.** American can exercise early. For a non-dividend call, early exercise is suboptimal (better to sell than exercise). Puts, and calls on dividend-paying underlyings, can rationally exercise early.
- **The Greeks (sensitivities of value \\(V\\)):**
  - **Delta** \\(\Delta = \partial V/\partial S\\): share-equivalent. Call delta in \\((0,1)\\), put in \\((-1,0)\\).
  - **Gamma** \\(\Gamma = \partial^2 V/\partial S^2 = \partial\Delta/\partial S\\): how fast delta moves. Long vanilla options have positive gamma.
  - **Vega** \\(\partial V/\partial\sigma\\): long options like vol. (Not a Greek letter; still called vega.)
  - **Theta** \\(\partial V/\partial t\\): usually negative for long options (time decay).
  - **Rho** \\(\partial V/\partial r\\): smaller, more relevant for long-dated.
- **Gamma–theta tradeoff.** Long gamma / long vega typically *pays* theta. You make money from big moves and bleed if the underlying sits still. Short options collect theta and get hurt by jumps and vol spikes.

## Mental model

```
  Long call payoff at T (ignore premium):

  P&L |
      |           /
      |         /
      |       /
      |_____/___________  S_T
            K

  Long put: mirror, pays when S_T < K.

  Greeks as a dashboard, not a personality:
    delta  -> "how many shares am I synthetically long?"
    gamma  -> "how fast will that share-equivalent change?"
    vega   -> "what if implied vol moves 1 point?"
    theta  -> "what does one day of sitting still cost me?"
```

An option *price* is the discounted risk-neutral expected payoff. The *payoff diagram* is the contract; the Greeks are the local Taylor expansion of the pricing function.

## Interview questions

1. **Draw the P&L at expiry of a long call you bought for premium \\(c\\). Where is breakeven?**
   Answer: Hockey stick: zero below \\(K\\), then \\(S_T - K\\). After premium, P&L is \\(\max(S_T-K,0) - c\\). Breakeven is \\(S_T = K + c\\). Below \\(K\\) you lose the whole premium, not more.

2. **State put-call parity and one arbitrage if \\(C - P > S - Ke^{-rT}\\) (no dividends).**
   Answer: Parity: \\(C + Ke^{-rT} = P + S\\). If call minus put is too expensive vs. forward, short call, long put, long stock, short the bond (i.e. borrow \\(Ke^{-rT}\\)). Terminal payoffs cancel; you keep the initial mispricing.

3. **Why is a vanilla call's delta between 0 and 1, and why is ATM delta not exactly 0.5 in Black–Scholes?**
   Answer: A call is a slice of the stock (never more than 1 share, never a short). In BS, \\(\Delta_{\text{call}} = e^{-qT} N(d_1)\\), which is \\(N(d_1)\\) with \\(q=0\\). ATM-spot (\\(S=K\\)) still has \\(d_1 = (r + \sigma^2/2)\sqrt{T}/\sigma > 0\\), so delta \\(> 0.5\\). ATM-forward is closer to 0.5, still not exactly because of the \\(\sigma^2/2\\) term.

4. **You are long a 1-month ATM straddle. What are you long/short in Greek space, and when do you make money?**
   Answer: Long gamma, long vega, short theta, delta roughly flat. You make money if realized vol is high (spot moves a lot) or implied vol rises; you lose if the spot sits still (theta) or implied vol falls.

5. **A trader says "I'm short gamma." What does that actually mean for P&L over a day?**
   Answer: Their delta-hedged P&L is roughly \\(\tfrac12 \Gamma (\Delta S)^2 + \Theta \Delta t + \text{vega}\, \Delta\sigma\\). Short gamma means they *lose* on a large \\(|\Delta S|\\) and typically *collect* theta if nothing happens. Jumps are lethal for short-gamma books.

## Watch

- [Call payoff diagram](https://www.youtube.com/watch?v=MZQxeQYQCUg) — Khan Academy. Hockey-stick payoff for the long call.
- [Put-call parity](https://www.youtube.com/watch?v=m4mrd7sHCPM) — Khan Academy. Stock + put vs. call + bond, same terminal payoff.
- [FRM: Stock option Greeks](https://www.youtube.com/watch?v=9EEGC9iJcFQ) — Bionic Turtle. Delta, gamma, vega, theta, rho as sensitivities, in eight minutes.

## Further reading

- Hull, chapters on properties of stock options and on the Greek letters.
- Next: [Black–Scholes intuition](../05-black-scholes-intuition/) for how the price (not just the payoff) is produced.
