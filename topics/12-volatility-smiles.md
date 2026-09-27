---
title: "12. Volatility & smiles"
layout: default
nav_order: 13
---

# Volatility and smiles
{: .no_toc }

*~9 min read*

**Interview occasional**

## Why it matters

Black–Scholes assumes one \\(\sigma\\). Markets do not: if you invert the BS formula at every strike you get a **smile** (or equity **skew**). Implied vol is how the market *quotes* options; the surface is the object vol desks actually trade. Interviewers use this to see whether you understand that BS is a quoting convention, and why OTM puts are "expensive" in implied-vol terms.

## Core concepts

- **Two vols.** *Realized / historical* vol is a statistic of past returns (close-to-close, Parkinson, etc.). *Implied* vol is the \\(\sigma_{\text{imp}}(K,T)\\) you plug into BS to match the *market price* of that option. They are not the same thing; the difference is a risk premium plus model error (see [Black–Scholes intuition](../05-black-scholes-intuition/)).
- **How you extract implied vol.** Market price in, BS formula inverted numerically (Newton on vega). If the market were exactly BS with constant \\(\sigma\\), every strike would return the same number. It doesn't.
- **Smile vs. skew.** FX and some commodities: roughly U-shaped *smile* (OTM puts *and* calls richer). Equities: downward *skew* — low strikes (OTM puts) have higher implied vol than high strikes. The 1987 crash is the usual historical marker for persistent equity skew.
- **Why a skew exists (intuition, not a unique theorem).**
  - Crashophobia / demand for puts (portfolio insurance).
  - Leverage: as \\(S\\) falls, equity-value vol of a levered firm rises.
  - Jumps and fat left tails: a lognormal underprices left-tail payoffs, so those options must carry a higher BS \\(\sigma\\) to fit.
- **The surface, not a number.** \\(\sigma_{\text{imp}}(K,T)\\) or in delta space \\(\sigma_{\text{imp}}(\Delta,T)\\). Term structure: short-dated events (earnings, FOMC) spike front-end vol; longer tenors are smoother. A **sticky-strike** vs. **sticky-delta** rule is a statement about how the surface *moves* when spot moves — it changes your delta (the "vol-gauntlet" / vanna-volga story).
- **Local vol vs. stochastic vol (one-level deeper).** Dupire local vol \\(\sigma_{\text{loc}}(S,t)\\) is calibrated to *today's* surface and is a perfect fit for vanillas, but it has specific (often too sticky) dynamics. Stochastic vol (Heston, SABR, SVI parametrizations) gives better forward smiles and vol-of-vol. Interviews: know *why* one extra process exists, not the PDE.
- **What implied vol is *not*.** It is not a forecast of realized vol (though variance swaps and the VIX are related to a strip of vanillas). Selling a 20-delta put at 25% implied vs. 18% realized can still lose if the jump happens.
- **Put-call parity still holds.** European \\(C-P\\) does not depend on vol. You cannot have a different implied vol for a European call and put of the same \\(K,T\\) — they share \\(\sigma_{\text{imp}}\\). If quotes disagree, that's a data/American/early-exercise/borrow issue, not a new smile.

## Mental model

```
  implied vol
    |
    | *                    *
    |   *                *
    |     *            *
    |       *  *  *  *          <-- FX-style smile
    |              *
    |  *  *                     <-- equity: extra left-tail skew
    |    *
    +------ low K ---- ATM ---- high K ----->

  Each point: the σ that makes BS(K,T; σ) = market price(K,T)
  The curve is a quoting language for the risk-neutral density's shape.
```

Breeden-Litzenberger: the second strike-derivative of a call price is the risk-neutral density of \\(S_T\\). A smile/skew *is* a non-lognormal density, written in BS units.

## Interview questions

1. **If BS were true with constant \\(\sigma\\), what would the implied-vol plot vs. strike look like? What do we actually see in SPX?**
   Answer: A flat line. SPX: downward skew, OTM puts have higher \\(\sigma_{\text{imp}}\\) than OTM calls, and the level of the ATM vol itself moves over time.

2. **Why can a European call and put with the same \\(K,T\\) not have different implied vols?**
   Answer: Put-call parity fixes \\(C-P\\) independently of \\(\sigma\\). BS call and put with the same \\(\sigma\\) already satisfy parity, so inverting either market price (if parity holds) yields the same \\(\sigma_{\text{imp}}\\).

3. **Give two economic reasons equity skew is typically downward-sloping.**
   Answer: (1) Crash/put demand: investors bid up low-strike puts. (2) Leverage / inverse relationship of level and vol: down-moves come with higher vol, which a risk-neutral density with a fat left tail captures. Jumps in addition to diffusion produce the same qualitative skew.

4. **You hedge a short OTM put using BS delta at the *implied* vol of that put. What can still hurt you?**
   Answer: Realized path vs. implied (gamma/theta), *changes* in the surface (vega, vanna, volga), jumps over the strike, discrete hedging error, and the fact that the "correct" sticky rule (sticky strike vs. sticky delta) changes the effective delta. Implied-vol delta is not a complete hedge of a smile world.

5. **What is the difference between local volatility and stochastic volatility in one sentence each?**
   Answer: Local vol: \\(\sigma\\) is a deterministic function of \\(S\\) and \\(t\\), fitted to today's vanillas (Dupire). Stochastic vol: \\(\sigma\\) itself is a random process, which produces vol-of-vol, a better forward smile, and extra risk you cannot hedge with spot alone.

## Watch

- [Implied volatility](https://www.youtube.com/watch?v=VIHldsSmASU) — Khan Academy. Invert BS; implied vol as the market's packaged view.
- [FRM: Implied volatility smile](https://www.youtube.com/watch?v=KhX-Hh7IZWw) — Bionic Turtle. Smile vs. flat BS vol, and why quoted vols vary by strike.

## Further reading

- Gatheral, *The Volatility Surface* — the standard next book if a vol seat is on the table.
- Dupire (1994) for local vol; Heston (1993) for the textbook stochastic-vol model.
