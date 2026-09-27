---
title: "09. Credit risk / CDS intro"
layout: default
nav_order: 10
---

# Credit risk and CDS intro
{: .no_toc }

*~8 min read*

**Interview occasional**

## Why it matters

Credit is the risk that a promised cash flow is not paid in full. A credit default swap (CDS) is insurance on that event: the protection buyer pays a spread, the protection seller covers the loss given default. You will not be asked to build a full hazard-rate engine in most generalist FE interviews, but you *will* be expected to explain a CDS, read a par spread, and connect spread \\(\approx\\) hazard \\(\times\\) LGD.

## Core concepts

- **Two numbers: probability of default (PD) and loss given default (LGD).** Expected loss on a loan \\(\approx PD \times LGD \times EAD\\). Recovery \\(R = 1 - LGD\\) is often assumed 40% for senior unsecured in a pinch; it is not a law.
- **Credit spread is compensation for expected loss plus risk premium plus funding/liquidity.** A corporate yield minus the matched Treasury/swap rate is *not* pure expected loss. Risk-neutral (CDS-implied) PDs are usually higher than real-world (rating) PDs — same pattern as equity risk premium.
- **A CDS, mechanically.** Protection buyer pays a running spread \\(s\\) (quarterly, on notional) until default or maturity, plus maybe an upfront. On a credit event, seller pays \\(N \times (1-R)\\) (cash auction) or takes delivery of a defaulted bond vs. par. Standard SNAC/ISDA docs define events: bankruptcy, failure to pay, (sometimes) restructuring.
- **Par spread.** The \\(s\\) that makes the CDS worth zero at inception (upfront \\(\approx 0\\) on old running-spread conventions; today many names trade with a standardized coupon + upfront). Rough no-arbitrage: PV(spread leg) = PV(protection leg).
- **The interview approximation:** \\(s \approx \lambda_{\text{hazard}} \times LGD\\) for a flat hazard rate \\(\lambda\\) and continuous payment, when \\(\lambda T\\) is not huge. So a 100bp par spread at 40% recovery \\(\Rightarrow \lambda \approx 100\text{bp}/0.6 \approx 167\\)bp hazard per year. This is the credit analogue of "carry ≈ spread."
- **Survival probability.** With constant hazard, \\(P(\tau > t) = e^{-\lambda t}\\). Risky cash-flow PV \\(\approx \sum CF_i\, DF(t_i)\, e^{-\lambda t_i}\\) plus recovery on default (simplified). This is how a risky bond is a defaultable version of [TVM](../01-tvm-discounting/).
- **CDS vs. cash bond (basis).** Buying a bond and buying protection is roughly a synthetic risk-free (plus funding). CDS–bond basis = CDS spread − bond's Z-spread (signs vary by shop). Can be nonzero because of funding, cheapest-to-deliver, counterparty risk, and technicals.
- **Counterparty risk on the CDS itself.** Bilateral OTC; now a lot of index CDS is cleared. CVA is the topic behind "what if the protection seller defaults when you need them" — know the word; don't overclaim a full CVA engine unless the seat is XVA.

## Mental model

```
  Protection buyer                     Protection seller
  (long protection,                    (short protection,
   short credit)                        long credit)
         |                                    |
         |  pay spread s until default/T      |
         | ---------------------------------> |
         |                                    |
         |  if default: receive N(1-R)        |
         | <--------------------------------- |

  Rough:  s ≈ λ × LGD
  Bond yield ≈ risk-free + s  (plus premia / basis)
```

Think of the spread as the *carry* you collect (or pay) for being short (or long) default risk, with a jump-to-LGD on the event.

## Interview questions

1. **A 5y CDS par spread is 120bp, assumed recovery 40%. Rough hazard rate? Rough 5y risk-neutral PD?**
   Answer: \\(\lambda \approx 0.012 / 0.60 = 2\%\\) per year. Survival \\(\approx e^{-0.02\times 5} \approx 90.5\%\\), so PD \\(\approx 9.5\%\\). (Continuous, flat-hazard approximation; good enough for a screen.)

2. **You buy a corporate bond and buy CDS protection on the same name/maturity. What risk did you (approximately) kill, and what remains?**
   Answer: You killed jump-to-default / credit-spread risk of that issuer (to first order). Remaining: funding of the bond, CDS–bond basis, cheapest-to-deliver option in the CDS, counterparty/CVA, interest-rate duration if not hedged, and recovery vs. assumed recovery.

3. **Why are CDS-implied default probabilities typically higher than historical default rates for the same rating?**
   Answer: CDS prices under a risk-neutral measure: they include a credit risk premium, liquidity, and dealer balance-sheet costs. Historical PDs are real-world frequencies. Same wedge as equity implied vs. realized vol, or as \\(Q\\) vs. \\(P\\) in [Black–Scholes](../05-black-scholes-intuition/).

4. **What is a credit event, and why does 'restructuring' matter?**
   Answer: Typically bankruptcy and failure to pay; restructuring (modified-mod-R, etc.) may or may not be included depending on the contract (North America SNAC often omits it). It matters because it changes when protection pays and which deliverable obligations qualify.

5. **A trader says the name 'widened 20bp.' Who won if they sold protection?**
   Answer: Seller of protection is *short* protection / *long* credit: they collect spread but lose when spreads widen (mark-to-market) or when default hits. Widening 20bp hurts the protection seller, helps the buyer.

## Watch

- [Credit default swaps](https://www.youtube.com/watch?v=a1lVOO9Y080) — Khan Academy. Why someone would buy protection in order to lend, and the running premium in basis points.
- [Credit default swaps (CDS) intro](https://www.youtube.com/watch?v=ccaCl1GKdJ0) — Khan Academy. Short, mechanical intro to the contract.

## Further reading

- Duffie, *Credit Risk Modeling* (survey) or the CDS chapter in Hull.
- ISDA's publicly available CDS FAQ / definitions if you need contract detail for a credit-trading seat.
