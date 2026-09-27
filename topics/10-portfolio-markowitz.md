---
title: "10. Portfolio theory & Markowitz"
layout: default
nav_order: 11
---

# Portfolio theory and Markowitz
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

Markowitz's 1952 insight is still the interview version of "why not put everything in the highest-Sharpe name": *portfolio* risk depends on covariances, not just on weighted-average volatilities. Diversification is the only free lunch in the mean-variance world. Quant interviews use this to check that you can write portfolio variance, explain the efficient frontier, and know what CAPM does and does not claim.

## Core concepts

- **Portfolio return is linear; portfolio variance is not.** \\(R_p = \sum_i w_i R_i\\), so \\(E[R_p] = w^\top \mu\\). Risk:
  \\[
  \sigma_p^2 = w^\top \Sigma w = \sum_i \sum_j w_i w_j \sigma_i \sigma_j \rho_{ij}.
  \\]
  If \\(\rho < 1\\), \\(\sigma_p\\) is *less* than the weighted average of \\(\sigma_i\\). That gap is diversification.
- **Two-asset intuition.** Equal weights, \\(\sigma_1=\sigma_2=\sigma\\): \\(\sigma_p = \sigma\sqrt{(1+\rho)/2}\\). At \\(\rho=1\\), no benefit; at \\(\rho=-1\\), you can drive risk to zero. Correlation is the engine.
- **Feasible set vs. efficient frontier.** Plot \\((\sigma_p, \mu_p)\\) for all weights (long-only or with shorts). The *efficient frontier* is the upper half: maximum expected return for each volatility. Anything below is dominated. The left tip is the global minimum-variance portfolio.
- **Risk-free asset and two-fund separation.** Mix \\(r_f\\) with the *tangency* (max-Sharpe) portfolio of risky assets. The capital market line is that tangent. Every mean-variance investor holds a mix of cash and that one risky fund; risk aversion only changes the mix, not the fund (Tobin).
- **CAPM (what to actually say).** If everyone is mean-variance and agrees on \\(\mu,\Sigma\\), the tangency fund *is* the market. Then \\(E[R_i] - r_f = \beta_i (E[R_m]-r_f)\\) with \\(\beta_i = \text{Cov}(R_i,R_m)/\text{Var}(R_m)\\). Only systematic risk is priced; idiosyncratic risk is diversified away. CAPM is an equilibrium *story*, not a law — empirically, other factors exist (value, momentum, quality, ...).
- **What interviews want beyond the cartoon.** Estimation error in \\(\Sigma\\) (especially correlations) makes naive mean-variance weights wild; that's why people shrink, use risk-parity, or constrain. Diversification works until correlations go to 1 in a crash — a risk-management caveat, not a reason to ignore \\(\Sigma\\).
- **Beta vs. sigma.** A high-\\(\sigma\\) stock can have a low \\(\beta\\) (idiosyncratic noise). In CAPM, that extra sigma is not compensated. Hedging and [VaR](../11-var-risk-measures/) care about the whole \\(\Sigma\\); pricing of *expected return* (under CAPM) cares about \\(\beta\\).

## Mental model

```
  E[r]
    |                 *  individual assets
    |              .
    |           .     efficient frontier (upper limb)
    |        .
    |     .   *  (interior = dominated portfolios)
    |  .
    | *  global min-var
    +------------------------ sigma

  Add r_f: straight tangent from r_f to the tangency portfolio.
  That line is the new efficient set (CML).
```

Variance of a sum is the sum of a *matrix*. Off-diagonals are the whole point.

## Interview questions

1. **Write \\(\sigma_p^2\\) for two assets and explain in words why \\(\rho=0.2\\) beats \\(\rho=0.9\\) at the same weights and vols.**
   Answer: \\(\sigma_p^2 = w_1^2\sigma_1^2 + w_2^2\sigma_2^2 + 2 w_1 w_2 \sigma_1\sigma_2\rho\\). The covariance term is smaller when \\(\rho\\) is smaller, so the same weighted returns come with less volatility — the portfolio is inside the "average vol" line.

2. **Why is the efficient frontier a curve (hyperbola), not the straight line between two assets?**
   Answer: Because \\(\sigma_p\\) is *not* linear in weights unless \\(\rho=1\\). For \\(\rho<1\\) the \\((\sigma,\mu)\\) locus bows left (less risk for the blended return). Only perfect correlation gives a straight line.

3. **What is two-fund separation, and what breaks it in practice?**
   Answer: With a risk-free asset, all mean-variance investors hold the same tangency risky portfolio plus cash in different amounts. It breaks if investors disagree on \\(\mu,\Sigma\\), face constraints (no shorting, liabilities, taxes), care about more than variance (drawdowns, skew), or if there is no truly risk-free asset in their consumption currency.

4. **A stock has \\(\sigma=40\%\\) but \\(\beta=0.5\\). Is it 'high risk'? For whom?**
   Answer: High *standalone* risk, low *undiversifiable* risk. A diversified CAPM investor demands only a 0.5-sized market premium. A concentrated holder still feels the 40%. Risk systems (VaR, tracking error) will still see the 40% if the position is large.

5. **Why do optimized mean-variance weights often look insane, and what do practitioners do?**
   Answer: \\(\mu\\) and \\(\Sigma\\) are estimated with noise; the optimizer treats estimation error as opportunity and levers the noisy directions. Fixes: shrink \\(\Sigma\\) (Ledoit-Wolf), constrain weights, use risk-parity / equal-risk-contribution, or estimate expected returns very conservatively (Black–Litterman).

## Watch

- [4. Portfolio Diversification and Supporting Financial Institutions (CAPM Model)](https://www.youtube.com/watch?v=efPKwxZuLKY) — Yale Financial Markets (Robert Shiller). Diversification, the frontier, tangency portfolio, and CAPM in a full lecture.

## Further reading

- Markowitz, "Portfolio Selection" (1952).
- Any investments text (Bodie/Kane/Marcus or Cochrane's *Asset Pricing* for the equilibrium view) for CML vs. SML.
