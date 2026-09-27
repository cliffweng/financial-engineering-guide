---
title: "14. Interview case patterns"
layout: default
nav_order: 15
---

# Interview case patterns
{: .no_toc }

*~10 min read*

**🎯 Interview frequent**

## Why it matters

Quant/FE interviews recycle a small number of *shapes*: price this, hedge that, find the arb, size the risk, explain the P&L. If you recognize the pattern, you can spend the 8–12 minutes on the actual model choice instead of flailing. This page is a map of those shapes, not a new theory — it pulls together [TVM](../01-tvm-discounting/) through [hedging](../13-hedging-greeks-practice/).

## Core concepts

- **Pattern A — "What's the fair price?"** Always: (1) write the payoff, (2) pick the measure and the discount, (3) name the dynamics, (4) name the method (closed form / tree / PDE / MC), (5) list the risks the model misses. Example: Asian call → MC under \\(Q\\), GBM or local vol, control = geometric Asian.
- **Pattern B — "There's an arb — or is there?"** Compare a quoted price to a replicating portfolio. Cash-and-carry, put-call parity, curve bootstrap consistency, CDS vs. bond basis. If it looks like free money, hunt friction: borrow, haircuts, CTD, early exercise, taxes, you cannot short the asset.
- **Pattern C — "Hedge this."** Translate the position into Greeks, then neutralize in order: delta with the underlier, gamma/vega with other options, residual curve with swaps/futures. State what is *left* (jump, skew, liquidity). "I'm delta-flat" is never the end of the answer.
- **Pattern D — "The book lost money overnight. Why?"** Use the P&L explain: \\(\Delta P \approx \Delta\cdot\Delta S + \tfrac12\Gamma(\Delta S)^2 + \text{vega}\,\Delta\sigma + \Theta + \text{carry} + \text{unexplained}\\). Then match to a market move (spot, curve, smile, spread). Unexplained P&L is a mapping bug until proven otherwise.
- **Pattern E — "Risk this."** Horizon, P&L definition (clean vs. dirty), factors, method (parametric / hist / MC), and a stress that the quantile misses. Quote VaR *and* a coherent alternative or a stress. See [VaR / risk measures](../11-var-risk-measures/).
- **Pattern F — "Build a curve / a vol surface."** Instruments, interpolation, what you discount vs. project, arbitrage checks (calendar, butterfly). Don't invent a parametric form you cannot defend.
- **Numbers hygiene.** Dimensions: bp vs percent vs vol points. Compounding. Day-count. Notional vs. PV. Sign of the position (long/short, payer/receiver, bought/sold protection). A wrong sign is a failed case even if the formula is right.
- **Talk like a desk, not a textbook.** "I'd sell the 3m10y straddle, delta-hedge, and I'm short gamma into the event" is better than reciting Itô's formula. Then be ready to go one level deeper if they push.

## Mental model

```
  hear the case
       |
       v
  [ what is the product / payoff / position? ]
       |
       v
  [ which pattern: price | arb | hedge | P&L | risk | curve ]
       |
       v
  [ replicating story  +  what can break it ]
       |
       v
  [ a number or a Greek  +  the residual risks ]
       |
       v
  [ one thing I'd check in data / a stress ]
```

Start from the contract, not from the fanciest SDE you know. Complexity is a last resort.

## Interview questions

1. **A client wants a 1y at-the-money-forward call on a single stock. Walk the pricing case in 60 seconds.**
   Answer: Payoff \\(\max(S_T - F, 0)\\) with \\(F = S_0 e^{(r-q)T}\\). Vanilla European → BS as quote, maybe a smile adjustment from listed options if they exist. Hedge: buy \\(\Delta\\) shares, finance, rebalance; residual vega/skew. Risks: dividends, borrow, jumps into earnings, discrete hedge. If they need an American, switch to a tree.

2. **Quoted 1y forward is 2% through cash-and-carry. List three reasons you still might not arb it.**
   Answer: You cannot actually short the asset (borrow fee, locates). Dividends/storage uncertain. Transaction costs + bid/ask on spot *and* forward. Corporate actions. For indexes, the replicating basket vs. the futures spec. "Through" might also be a quoting convention (premium vs. discount, day-count).

3. **You are long a 10y receiver swap and short a 10y Treasury of matched DV01. Rates rally, you still lose. Give two plausible reasons.**
   Answer: (1) Curve *shape*: swap spread widened (SOFR/LIBOR vs. Treasury) enough to swamp the parallel DV01. (2) Convexity mismatch / key-rate mismatch (the swap's risk is not a bullet at 10y). Also: financing of the short Treasury, and any optionality if the Treasury was CTD into a future.

4. **Overnight P&L on a short 1-month 25-delta put is very negative, spot barely moved. What do you look at first?**
   Answer: Implied vol / skew (vega and vanna): a vol spike or put-skew steepener marks you down without a spot move. Then: theta vs. calendar, borrow, and whether the position was actually delta-flat under the right sticky rule. Last: a jump that reverted by the close (intraday gamma) that the EOD spot doesn't show.

5. **Design a 99% 1-day VaR for a book of listed equity options. Which method, and what will it miss?**
   Answer: Full-reval historical or MC with a smile; delta-normal is insufficient (gamma). Misses: events not in the window, overnight jumps, liquidity/bid-ask on OTM, correlation breaks across names, and anything beyond the 99% quantile (add ES + a crash stress). Backtest exceptions.

6. **The interviewer says 'your Monte Carlo price is 2.13 ± 0.40.' What do you say next?**
   Answer: Standard error is unusable — widen \\(N\\), add a control variate (the European vanilla), maybe quasi-MC. Also ask: bias from \\(\Delta t\\)? American? Same seed for Greeks? Never ship a price whose error bar is 20% of the number.

## Watch

There is no single "FE interview cases" lecture worth pretending is canonical. Use the topic videos earlier in this guide, then practice out loud with a timer:

- Pricing: [Black-Scholes, risk-neutral valuation (MIT 18.S096)](https://www.youtube.com/watch?v=TnS8kI_KuJc)
- Arb / parity: [Put-call parity](https://www.youtube.com/watch?v=m4mrd7sHCPM)
- Risk: [What is value at risk (VaR)?](https://www.youtube.com/watch?v=mvl32w_y38I)

## Further reading

- Work every interview question on the previous thirteen pages out loud, with a number, a sign, and a residual risk.
- Hull's end-of-chapter questions are closer to this page than most "brainteaser" lists. For desk-style cases, collected quant-interview books (e.g. Joshi, *Quant Job Interview Questions and Answers*) are optional — only after the core pages are fluent.
