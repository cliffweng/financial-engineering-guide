---
title: "13. Hedging & Greeks in practice"
layout: default
nav_order: 14
---

# Hedging and Greeks in practice
{: .no_toc }

*~10 min read*

**🎯 Interview frequent**

## Why it matters

Pricing tells you a number; hedging is how a dealer *survives* that number. A short call that was sold at "fair" BS value still bleeds or blows up if you don't rebalance delta, if vol moves, or if the underlying jumps. Interviewers want the P&L identity of a delta-hedged option, the gamma–theta tradeoff, and why discrete hedging is not the textbook continuous hedge.

## Core concepts

- **Delta hedging, the production version.** A short vanilla call: buy \\(\Delta = N(d_1)\\) shares (no dividends), finance the rest in cash. As \\(S\\) and \\(t\\) move, \\(\Delta\\) changes, so you trade the stock again. In the *frictionless BS* world this replicating strategy's P&L locks the original premium vs. payoff. In the real world it does not (see below).
- **The delta-hedged P&L expansion** (the interview Taylor series), over a small \\(\Delta t\\):
  \\[
  dV - \Delta\, dS \approx \Theta\,\Delta t + \tfrac12 \Gamma (dS)^2 + \text{vega}\, d\sigma + \cdots
  \\]
  (rates/rho omitted). For a *short* option, flip the signs: you are short gamma, short vega, long theta.
- **Gamma–theta in BS.** The BS PDE says that if implied = realized and you delta-hedge continuously, \\(\Theta + \tfrac12 \sigma^2 S^2 \Gamma \approx r(V - \Delta S)\\) (up to dividends). Intuition: **theta is the insurance premium; gamma is the insurance payout on quadratic moves.** If realized vol \\(>\\) the implied you sold at, the \\(\tfrac12 \Gamma (dS)^2\\) term beats theta and the short-option hedge *loses*.
- **Discrete rehedging.** You rebalance at intervals, not continuously. Error \\(\sim\\) path variation between hedges. More frequent hedging cuts gamma leakage but costs more bid–ask. Optimal frequency is an engineering tradeoff, not "as fast as possible."
- **Jumps.** Delta hedging assumes locally Brownian moves. A jump over a strike is a gamma event you never got to rebalance through — lethal for short-gamma, especially short-dated OTM options that were "almost" worthless.
- **Vega and the rest of the book.** Spot hedging does not kill vol risk. You hedge vega with other options (usually closer to ATM, longer-dated for smoother vega). Then you have **vanna** (dDelta/dVol, or dVega/dS) and **volga** (dVega/dVol) — smile dynamics, not vanilla BS. A "delta-hedged, vega-hedged" book can still lose on a skew steepener.
- **Portfolio Greeks add.** Position Greek \\(= \sum_i q_i \times\\) (per-unit Greek). Signs: short calls \\(\Rightarrow\\) short delta, short gamma, short vega. Short puts \\(\Rightarrow\\) *long* delta, still short gamma/vega. This is why a short-straddle book is "delta-flat, short gamma."
- **What 'flat' means on a desk.** Delta-flat vs. a chosen sticky rule; gamma within a limit; vega within a limit; maybe bucketed by tenor and strike. Limits exist because the Taylor expansion is local.

## Mental model

```
  You sold a call at implied σ_imp, delta-hedged.

  each day:
      collect theta          (+)  for the short
      pay  1/2 Γ (ΔS)^2      (-)  realized quadratic
      pay/receive vega Δσ    (?)  implied vol mark
      plus hedge slippage, jumps, rates, dividends

  if realized vol ≈ σ_imp and the surface doesn't move
      and there are no jumps and you rehedge "enough":
          P&L ≈ 0  (you earned the fair premium)
  if realized >> σ_imp:  short-gamma bleed
  if the surface rallies: short-vega mark-to-market loss
```

Hedging is not "set and forget." It is a running replication that leaks wherever the world violates the model you used to compute \\(\Delta\\).

## Interview questions

1. **You sell an ATM call and delta-hedge. Spot rallies 5% in a day, implied vol unchanged. What's the leading P&L term, and the sign?**
   Answer: Short gamma: \\(\text{P\&L} \approx -\tfrac12 \Gamma (\Delta S)^2 < 0\\), plus a little theta earned. Delta was hedged, so the linear \\(-\Delta \Delta S\\) in the option is offset by the stock. The *curvature* is the loss. (If you didn't rehedge during the rally, that's the discrete-hedge version of the same fact.)

2. **Why does a delta-hedged short option typically *make* money on a quiet day?**
   Answer: Short theta is negative on the *long* option, so the short *collects* theta. With a small \\((\Delta S)^2\\), gamma loss doesn't eat the premium. That's the carry of being short vol, as long as realized stays low.

3. **Name three reasons a textbook continuous BS delta hedge still loses money in production.**
   Answer: (1) Discrete rehedging / transaction costs. (2) Realized vol \\(\neq\\) implied (model is wrong on \\(\sigma\\)). (3) Jumps. Bonus: stochastic vol / smile moves (vega, vanna), borrow/dividends, overnight gaps.

4. **How do you reduce gamma of a short-option book without flattening delta?**
   Answer: Buy options (usually closer to ATM or similar expiry) to buy back gamma/vega, then re-hedge the net delta with the underlying. Gamma from vanillas is always positive for longs, so you *buy* gamma; you cannot get gamma from the stock alone (stock gamma is 0).

5. **What is the difference between hedging to a sticky-strike delta and a sticky-delta delta?**
   Answer: Sticky strike: the implied vol of a given *K* stays put as \\(S\\) moves, so you use BS delta at that fixed \\(\sigma(K)\\). Sticky delta: the vol *smile in delta space* stays put, so a given strike's \\(\sigma\\) changes as it becomes a different delta — that extra \\(d\sigma/dS\\) (vanna) adjusts the hedge ratio. Wrong sticky assumption \\(\Rightarrow\\) systematic hedge error.

## Watch

- [FRM: Stock option Greeks](https://www.youtube.com/watch?v=9EEGC9iJcFQ) — Bionic Turtle. The five Greeks as sensitivities, the dashboard you hedge against.
- [Hedging (aka, neutralizing) option delta and gamma (FRM T4-19)](https://www.youtube.com/watch?v=GCAM8UyCitE) — Bionic Turtle. Making a book delta- and gamma-neutral with the underlying plus another option.
- [Option delta plus gamma (FRM T4-16)](https://www.youtube.com/watch?v=5BURfvsqwMc) — Bionic Turtle. Long vs. short gamma on calls and puts, and why short gamma is short volatility.

## Further reading

- Hull, "The Greek Letters" — the standard chapter for delta hedging, gamma, and portfolio insurance.
- For smile-aware hedges: Gatheral or a vol-desk primer on sticky rules, vanna, and volga.
