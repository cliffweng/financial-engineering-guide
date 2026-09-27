---
title: "02. Bonds & duration/convexity"
layout: default
nav_order: 3
---

# Bonds, duration, and convexity
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

A bond is the simplest interesting present-value machine: a schedule of coupons plus principal, discounted on a curve. Duration and convexity are how you talk about *interest-rate risk* without re-pricing the whole book every time the curve twitches. Rates desks, risk, and almost every "fixed income 101" interview loop live here.

## Core concepts

- **Price is discounted cash flows.** For a coupon bond with cash flows \(C_{t_i}\) and discount factors \(DF(t_i)\): \(P = \sum_i C_{t_i}\, DF(t_i)\). If you use a single yield \(y\) (YTM), you are fitting one number that makes that sum equal the market price — YTM is an *internal rate of return*, not a curve.
- **Price and yield move inversely.** If required yields rise, every cash flow is worth less today, so the bond cheapens. Khan Academy's "bond prices vs interest rates" video is the intuition; the formula is just TVM (see [TVM / discounting](../01-tvm-discounting/)).
- **Macaulay duration** is the PV-weighted average time of the cash flows: \(D_{\text{Mac}} = \sum_i t_i \cdot w_i\) where \(w_i = C_{t_i} DF(t_i) / P\). A zero's Macaulay duration equals its maturity; a coupon bond's is strictly less.
- **Modified duration** is the first-order sensitivity: \(\frac{1}{P}\frac{dP}{dy} = -D_{\text{mod}}\). For a 1bp (0.01%) parallel yield rise, \(\Delta P / P \approx -D_{\text{mod}} \times 0.0001\). This is the number risk systems mean by "duration."
- **Convexity** is the second derivative: \(\frac{1}{P}\frac{d^2P}{dy^2}\). For vanilla (option-free) bonds, convexity is positive: the price-yield curve bends *up*. Duration underestimates the gain when yields fall and overestimates the loss when yields rise.
- **The Taylor approximation** interviewers want: \(\frac{\Delta P}{P} \approx -D_{\text{mod}}\,\Delta y + \tfrac12 \,\text{Conv}\,(\Delta y)^2\). Use duration for small moves; add convexity when the move is large or when comparing two bonds with similar duration.
- **What duration does *not* capture.** It assumes a parallel yield shift of a single YTM (or a parallel curve shift, in DV01 language). Real curves twist and butterfly. Option-embedded bonds (callables, MBS) can have *negative* convexity when the embedded option kicks in.
- **DV01 / PV01** is the dollar value of 1bp: \(DV01 \approx D_{\text{mod}} \times P \times 0.0001\). Portfolio risk is often quoted in DV01, not "duration years."

## Mental model

```
  Price
    |        actual price-yield curve  (convex, bows up)
    |              *  *
    |            *      *
    |          *          *
    |        *   tangent = -duration
    |      *  /
    |    *   /
    |  *    /
    | *    /
    +------------------------ yield
         y0

  small dy: duration (the tangent) is enough
  large dy: add +1/2 convexity (dy)^2  — you sit *above* the tangent
```

A barbell (cash in short + long zeros) has more convexity than a bullet (cash in the middle) at the same duration — that's why convexity has value when volatility of yields is real.

## Interview questions

1. **Why does a bond's price fall when yields rise? Give the one-line TVM answer, then the duration answer.**
   Answer: TVM: higher \(y\) shrinks every discount factor, so PV falls. Duration: \(\Delta P \approx -D_{\text{mod}} P \Delta y\), so a positive \(\Delta y\) produces a negative \(\Delta P\). Duration is the local linearization of that TVM fact.

2. **Two bonds, same duration, same yield. Why might you still prefer one?**
   Answer: Convexity (and cash-flow timing / spread / liquidity). Higher convexity is better for the long: you gain more if yields move a lot in either direction. Also credit, optionality, and whether duration was computed off YTM vs. a full curve.

3. **A 2-year 6% annual coupon bond, par, yield 6%. Rough Macaulay duration — bigger or smaller than 2? Why?**
   Answer: Smaller than 2. Some PV arrives as the year-1 coupon, which pulls the PV-weighted average time below maturity. Only a zero has \(D_{\text{Mac}} = T\).

4. **You hedge a long 10y bond with a short 2y note, matching DV01. What risk remains?**
   Answer: Curve *shape* risk (2s10s steepener/flattener), convexity mismatch (the 10y has more convexity), and any spread/credit basis. DV01 matching only kills a parallel 1bp shift.

5. **Callable bond: why can effective convexity go negative?**
   Answer: As yields fall, the issuer is more likely to call, so price stops rising toward the call price. The price-yield curve flattens or bends down on the low-yield side — that's negative convexity, and duration can drop as yields drop (unlike a vanilla bond).

## Watch

- [Introduction to bonds](https://www.youtube.com/watch?v=Qh-M3_L4xYk) — Khan Academy. What a bond is: par, coupon, and why a company issues one.
- [Relationship between bond prices and interest rates](https://www.youtube.com/watch?v=I7FDx4DPapw) — Khan Academy. Why prices and yields move inversely, with a simple coupon example.
- [Bond convexity](https://www.youtube.com/watch?v=yOwRgWhIn_g) — Bionic Turtle. Convexity as the second moment of the cash-flow times, and why the duration tangent sits below the curve.

## Further reading

- Hull, *Options, Futures, and Other Derivatives*, chapter on interest rates (duration, convexity, DV01).
- Fabozzi's fixed-income chapters if you need more on key-rate duration and curve risk.
