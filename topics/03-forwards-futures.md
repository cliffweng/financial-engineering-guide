---
title: "03. Forwards / futures"
layout: default
nav_order: 4
---

# Forwards and futures
{: .no_toc }

*~8 min read*

**🎯 Interview frequent**

## Why it matters

A forward is the simplest derivative: an obligation to transact at a later date at a price agreed today. Futures are the exchange-traded, marked-to-market cousin. Cash-and-carry pricing and the forward–spot relationship show up in every asset class (equity index, FX, rates, commodities) and are a favorite "derive this on the whiteboard" question.

## Core concepts

- **Forward vs. futures.** A *forward* is an OTC contract: customized, usually zero value at inception, settled at \\(T\\), bilateral counterparty risk. A *future* is standardized, exchange-traded, cleared, and **marked to market daily** (variation margin). Payoffs are similar; financing of margin and daily settlement can make futures prices differ slightly from forwards when rates are stochastic.
- **The forward price \\(F(0,T)\\) is not a forecast.** It is the price that makes the forward contract worth zero today. Under no-arbitrage, it is pinned by cash-and-carry, not by anyone's expected future spot (unless you add risk premia and the real-world measure).
- **Cash-and-carry for an investment asset** (no income): borrow \\(S_0\\), buy the asset, hold to \\(T\\). Cost of carry is financing: \\(F(0,T) = S_0 e^{rT}\\) (continuous). If the quoted forward is higher, short the forward, buy spot, carry; if lower, reverse. That's the whole argument.
- **Income, dividends, storage, convenience.**
  - Continuous dividend / FX foreign rate \\(q\\): \\(F = S_0 e^{(r-q)T}\\).
  - Discrete dividends: \\(F = (S_0 - PV(\text{divs})) e^{rT}\\).
  - Commodity with storage cost \\(u\\) and convenience yield \\(y\\): \\(F = S_0 e^{(r+u-y)T}\\).
- **Contango vs. backwardation.** Contango: \\(F > S\\) (upward-sloping curve). Backwardation: \\(F < S\\). For commodities this often reflects storage + convenience, not "the market thinks the price will fall" — the curve is a snapshot of *today's* contract prices by expiry, not a path of future spots (Khan Academy's futures-curve videos hammer this).
- **Value of an existing forward.** After inception, a long forward struck at \\(K\\) is worth \\(e^{-rT}(F(t,T) - K)\\) (or \\(S_t - K e^{-rT}\\) for non-dividend assets). Mark-to-market is "replace \\(K\\) with the new fair forward and pocket the difference, discounted."
- **FX: covered interest parity** is cash-and-carry in two currencies: \\(F = S_0 e^{(r_d - r_f)T}\\). A violation is an FX-swap arbitrage, not a view on the currency.

## Mental model

```
  Cash-and-carry (no income):

  t=0                         t=T
  borrow S0 at r              repay S0 e^{rT}
  buy the asset  ------------ deliver asset vs K
  short forward @ K

  If K > S0 e^{rT}: this trade locks K - S0 e^{rT} > 0  (arb)
  If K < S0 e^{rT}: reverse it (short asset, lend S0, long forward)

  Fair K = F(0,T) = S0 e^{rT}
```

A futures curve is a *row of today's prices* for different delivery dates, not a prediction of the spot path.

## Interview questions

1. **A stock trades at 100, \\(r = 5\%\\) continuous, 1-year forward quoted at 108. What do you do?**
   Answer: Fair \\(F = 100 e^{0.05} \approx 105.13\\). Quoted 108 is rich. Short the forward, buy stock, finance by borrowing 100. At \\(T\\): deliver stock, receive 108, repay \\(\approx 105.13\\), pocket \\(\approx 2.87\\). Classic cash-and-carry.

2. **Why isn't the forward price the expected future spot?**
   Answer: No-arbitrage pins \\(F\\) from replicating the delivery with cash-and-carry. \\(F = E^Q[S_T]\\) under the *risk-neutral* measure (or \\(F = e^{(r-\mu)T} E^P[S_T]\\)). Real-world expected spot includes a risk premium; the forward does not have to equal it.

3. **Name three concrete differences between a forward and a futures contract.**
   Answer: (1) OTC vs exchange/cleared. (2) Custom vs standardized. (3) Settlement at \\(T\\) vs daily mark-to-market + margin. Bonus: counterparty risk vs default-fund through CCP; forwards can have value during life without cash moving until expiry.

4. **An equity index pays a 2% continuous dividend. How does that change \\(F\\)? Intuition?**
   Answer: \\(F = S_0 e^{(r-q)T}\\) with \\(q=0.02\\). Holding the index you *receive* dividends you would miss if you only held the forward, so the forward must be cheaper than the pure-financing forward. Same pattern as FX with a foreign rate.

5. **Gold is in contango. Does that mean the market expects the gold price to rise?**
   Answer: Not necessarily. Contango can be pure cost of carry (finance + storage). The futures curve is today's menu of delivery prices, not \\(E[S_t]\\) plotted against \\(t\\).

## Watch

- [Forward contract introduction](https://www.youtube.com/watch?v=H9UEZdAnnt8) — Khan Academy. A farmer and a baker lock a price; that's a forward.
- [Futures introduction](https://www.youtube.com/watch?v=3g6P0lRXotI) — Khan Academy. Standardization, exchange, and why futures exist (counterparty risk).
- [Arbitraging futures contract](https://www.youtube.com/watch?v=0jk6uLZ1Tdc) — Khan Academy. Cash-and-carry when the futures is mispriced versus spot.

## Further reading

- Hull, chapters on mechanics of futures/forwards and on determination of forward/futures prices.
- For FX: any covered-interest-parity derivation (same cash-and-carry, two money-market accounts).
