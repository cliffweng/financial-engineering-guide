---
title: "07. Monte Carlo pricing"
layout: default
nav_order: 8
---

# Monte Carlo pricing
{: .no_toc }

*~9 min read*

**🎯 Interview frequent**

## Why it matters

Once a payoff depends on a whole path (barriers, Asians, cliquets, Bermudans with a twist) or on several underlyings, trees and PDEs struggle; Monte Carlo scales with the number of *paths*, not the dimension of the state, in the naive sense that matters for interviews. The idea is one line: simulate under the risk-neutral dynamics, average the discounted payoff. Interviewers probe whether you simulate the right drift, whether you discount inside the average, and how error falls with \\(N\\).

## Core concepts

- **The estimator.** For a European-style (path-dependent is fine) claim,
  \\[
  \hat V = \frac{1}{N}\sum_{i=1}^N e^{-rT} g(S^{(i)}),
  \\]
  where \\(S^{(i)}\\) are i.i.d. paths generated under \\(Q\\). By the law of large numbers this converges to \\(e^{-rT} E^Q[g]\\), which is the no-arbitrage price in a complete (or well-hedged) model.
- **Simulate the risk-neutral SDE, not the real-world one.** Under BS, \\(dS = (r-q)S\,dt + \sigma S\,dW^Q\\). Using \\(\mu\\) instead of \\(r-q\\) prices with the wrong measure — a classic fail (see [Black–Scholes intuition](../05-black-scholes-intuition/) and [TVM / discounting](../01-tvm-discounting/)).
- **Euler (and better) schemes.** Discrete GBM has an exact solution: \\(S_{t+\Delta} = S_t \exp\bigl((r-q-\sigma^2/2)\Delta + \sigma\sqrt{\Delta}\,Z\bigr)\\), \\(Z\sim N(0,1)\\). For local vol / stochastic vol / rates, you usually Euler-discretize and watch bias from \\(\Delta t\\).
- **Error has two pieces.** *Statistical* error \\(\sim \sigma_g / \sqrt{N}\\) (standard error of the mean). *Bias* from time-stepping, American approximation, etc. Quote a price with a standard error; doubling accuracy in RMSE costs \\(4\times\\) paths unless you reduce variance.
- **Variance reduction** (know the names): antithetic variates (\\(Z\\) and \\(-Z\\)); control variates (e.g. use the BS European you can price in closed form); importance sampling; stratified sampling; quasi-MC (Sobol). Interviewers love: "what's your control?"
- **Greeks from MC.** Bump-and-revalue (noisy, especially gamma). Pathwise derivatives when the payoff is smooth enough. Likelihood ratio / score method when it isn't. Common seed (same \\(Z\\)s) when bumping.
- **Early exercise is the Achilles heel.** Vanilla MC is forward-only; American needs an exercise decision. Longstaff–Schwartz: regress continuation value on basis functions of the state, using the *cross-section* of paths. Know that this exists and is biased; don't claim you "just take max at each step" on a single path (that's wrong — you'd be using future information).
- **When not to use MC.** One-factor European vanillas: use BS. Low-dimensional Americans: trees/PDE are faster and better for early exercise. MC shines at high-dimensional Europeans and path-dependent payoffs.

## Mental model

```
  under Q:
    draw Z1, Z2, ... -> path S(t)
    payoff g(path)
    discount to t=0

  repeat N times
  price ≈ average of the discounted payoffs
  stderr ≈ stdev / sqrt(N)

  Wrong:
    simulate with mu, then discount at r     (mixed measures)
    average first, discount never            (numeraire error)
    exercise using that path's own future    (lookahead bias)
```

Monte Carlo is a numerical integral of the discounted payoff against the risk-neutral density (or path measure).

## Interview questions

1. **Write the Monte Carlo estimator for a European call in BS, including the SDE you simulate.**
   Answer: \\(S_T = S_0\exp\bigl((r-q-\sigma^2/2)T + \sigma\sqrt{T}Z\bigr)\\), \\(Z\sim N(0,1)\\). \\(\hat C = e^{-rT} \frac1N \sum \max(S_T^{(i)}-K,0)\\). Drift is \\(r-q\\), not \\(\mu\\).

2. **You run \\(N = 10{,}000\\) paths and the stderr is 0.20. How many paths for stderr \\(\approx 0.02\\)?**
   Answer: Error scales \\(1/\sqrt{N}\\), so you need \\(100\times\\) paths: \\(N = 1{,}000{,}000\\). This is why variance reduction and quasi-MC matter in production.

3. **Why can't you price an American put by, on each path, taking \\(\max(K-S_t, \text{discounted remaining})\\) using that path's future?**
   Answer: That uses information from *after* \\(t\\) on the same path — not adapted, upward biased (you'd exercise with perfect foresight). You need an exercise policy that depends only on the current state, e.g. Longstaff–Schwartz regression across paths.

4. **Give one control-variate example for an Asian call.**
   Answer: Use the *geometric*-average Asian, which has a closed-form BS-like price, as control: \\(\hat V + \beta\bigl(\text{analytic geo} - \hat V_{\text{geo}}\bigr)\\). Highly correlated with the arithmetic Asian, so variance drops a lot. \\(\beta\\) is estimated or set to 1.

5. **Pathwise vs. bump-and-revalue for delta. When does pathwise fail?**
   Answer: Pathwise: differentiate the payoff along the path, \\(\partial g/\partial S_0\\), then average. Fails when \\(g\\) is not Lipschitz / the derivative doesn't exist in \\(L^1\\) — digital, barrier-on-touch, or "max" at a kink if you're not careful (call is OK almost everywhere; digital is not). Then use likelihood ratio or smoothing.

## Watch

- [6. Monte Carlo Simulation](https://www.youtube.com/watch?v=OgO1gpXSUzU) — MIT 6.0002 (John Guttag). What Monte Carlo *is*: inferential statistics, casinos, and why averaging random samples works.
- [Session 6B: Monte Carlo Simulations in Finance & Investing](https://www.youtube.com/watch?v=eXhCXobViJc) — Aswath Damodaran. Distributions instead of point estimates; how to read a simulated output.
- [Monte Carlo Simulation for Option Pricing with Python (Basic Ideas Explained)](https://www.youtube.com/watch?v=pR32aii3shk) — QuantPy. Risk-neutral paths to a discounted expected payoff.

## Further reading

- Glasserman, *Monte Carlo Methods in Financial Engineering* — the reference if you actually implement this.
- Longstaff & Schwartz (2001) for American Monte Carlo.
