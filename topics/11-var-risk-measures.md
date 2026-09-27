---
title: "11. VaR / risk measures"
layout: default
nav_order: 12
---

# VaR and risk measures
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

Value-at-Risk is the industry's default one-number market-risk quote: "we don't expect to lose more than \\(X\\) in \\(N\\) days more than \\(\alpha\\) of the time." Interviewers want a precise definition, the three standard ways to compute it, and the punchline that VaR is **not coherent** — it can ignore the tail beyond the quantile and can discourage diversification. Expected shortfall (ES / CVaR) is the usual repair.

## Core concepts

- **Definition.** Let \\(L\\) be the loss over horizon \\(h\\). The \\(\alpha\\)-VaR (e.g. 99% 1-day) is the \\(\alpha\\)-quantile of \\(L\\):
  \\[
  \operatorname{VaR}_\alpha = \inf\\{ x : P(L \le x) \ge \alpha \\}.
  \\]
  In words: a number \\(x\\) such that \\(P(L > x) \le 1-\alpha\\). It does **not** say how bad the exceedances are.
- **Three computational families.**
  1. *Parametric / variance-covariance / delta-normal:* assume \\(L\\) is (locally) linear in risk factors, factors jointly normal, \\(\operatorname{VaR} = \sigma_L \Phi^{-1}(\alpha)\\) (plus mean if you use relative VaR). Fast; blind to options' gamma and to fat tails.
  2. *Historical simulation:* revalue the book under each of the last \\(N\\) historical factor moves; read the empirical quantile. No distributional assumption; very dependent on the window and on "the future looking like the past."
  3. *Monte Carlo:* simulate factors from a model (often with fat tails, stochastic vol, jumps), full reval, quantile. Flexible and expensive; model risk replaces window risk.
- **Horizon scaling.** Naive: \\(\sigma_{h} = \sigma_1 \sqrt{h}\\) if i.i.d. returns. This fails with autocorrelation, mean reversion, or nonlinear options (gamma scales closer to \\(h\\), not \\(\sqrt{h}\\)). Don't square-root a 1-day options VaR to 10-day without thinking.
- **Coherent risk measures** (Artzner et al.): translation invariance, positive homogeneity, monotonicity, **subadditivity** \\(\rho(X+Y) \le \rho(X)+\rho(Y)\\). Subadditivity = "diversification should not increase measured risk." **VaR can violate it.** Expected shortfall \\(\operatorname{ES}_\alpha = E[L \mid L \ge \operatorname{VaR}_\alpha]\\) (average of the tail) is coherent (for continuous distributions) and is what Basel uses in the Fundamental Review of the Trading Book.
- **What VaR is used for anyway.** Limits, regulatory capital (still, in places), backtesting (Kupiec, Christoffelsen: count exceptions vs. \\(1-\alpha\\)). A 99% 1-day VaR should "break" about 2–3 times per year. Clustering of breaks means your independence assumption is wrong.
- **P&L explain vs. VaR.** A good risk system maps the book to factors (see duration, delta, vega in earlier topics) so you can *explain* P&L with the same Greeks that fed VaR. If unexplained P&L is large, the VaR map is a toy.

## Mental model

```
  loss density
    |
    |     ****
    |    *    *
    |   *      *
    |  *        *   *
    | *           *    *||||  tail
    +------------------|-------------------> L
                      VaR_99%
                      <---- ES = average of this tail ---->

  VaR = a point on the axis
  ES  = how bad, given you're past that point
```

VaR is a *quantile*. Treating it as "maximum loss" is the most common verbal fail in interviews.

## Interview questions

1. **State 99% 1-day VaR in one sentence that a desk head would accept, and one mistake that sentence must not make.**
   Answer: "On 99% of days, the 1-day loss should not exceed \\(X\\); on about 1% of days it will be worse." Mistake: calling \\(X\\) the *maximum* loss, or saying the probability of losing \\(X\\) is 99%.

2. **Why can 99% VaR of a portfolio be *higher* than the sum of the standalone 99% VaRs? Sketch a counterexample.**
   Answer: Subadditivity failure. Two bonds that each default with 0.6% probability, independently: standalone 99% VaR can be ~0 (no default in the 99% mass), but the portfolio can have \\(P(\text{exactly one default}) > 1\%\\), so portfolio VaR includes a default loss. Digital / jump risks at frequencies near \\(1-\alpha\\) are the usual examples.

3. **Delta-normal VaR for a deep short-dated option book: what's wrong?**
   Answer: The P&L is not linear in \\(S\\) (gamma, vega) and not normal (truncated, jumps). A 1-day 99% move can be a gamma blow-up the linear map never sees. Full reval historical or MC, and/or delta-gamma-vega analytic with a better distribution, is required.

4. **Historical simulation: last 250 days, no crisis in the window. What is the risk of your 99% VaR?**
   Answer: The 99% sample quantile is the 2nd or 3rd worst day — extremely noisy — and if the window is calm, the quantile is too small. You are assuming the next year looks like this year. Stress overlays and longer/weighted windows exist because of this.

5. **Why did regulation move from VaR to expected shortfall for internal models?**
   Answer: ES is subadditive (coherent), sensitive to the *shape* of the tail beyond the quantile, and harder to game by stuffing risk just past the VaR threshold. Backtesting ES is harder than counting VaR exceptions — that's the tradeoff.

## Watch

- [What is value at risk (VaR)? FRM T1-02](https://www.youtube.com/watch?v=mvl32w_y38I) — Bionic Turtle. Definition and the "how bad in the tail" caveat.
- [FRM: Three approaches to value at risk (VaR)](https://www.youtube.com/watch?v=L2xzlvhkagk) — Bionic Turtle. Parametric, historical, Monte Carlo.
- [Coherent risk measures and why VaR is not coherent (FRM T4-5)](https://www.youtube.com/watch?v=_hbgj-F8Kuo) — Bionic Turtle. Subadditivity and the case for ES.

## Further reading

- Artzner, Delbaen, Eber, Heath, "Coherent Measures of Risk" (1999).
- Jorion, *Value at Risk*, for the industry implementation view.
