---
title: "01. TVM / discounting"
layout: default
nav_order: 2
---

# Time value of money and discounting
{: .no_toc }

*~8 min read*

**🎯 Interview frequent**

## Why it matters

Almost every price in finance is a present value. Bonds, swaps, options, credit, even a "simple" loan — they all ask the same question: what is a future cash flow worth *today*, given time, interest, and risk. Interviewers use TVM to check that you can move money through time without mixing compounding conventions, and that you know *why* you are discounting, not just which button to press.

## Core concepts

- **A dollar today is worth more than a dollar tomorrow** because you can invest it (opportunity cost) and because future cash is uncertain. Discounting is how you convert a later cash flow into today's units.
- **Future value and present value are inverses.** With a periodic rate \(r\) over \(n\) periods: \(FV = PV \times (1+r)^n\) and \(PV = FV / (1+r)^n\). If you cannot write that from memory, stop and re-derive it before going further.
- **Compounding frequency is a convention, not a law of nature.** The same quoted rate produces different PVs depending on annual, semiannual, continuous, or day-count conventions. Always ask: *what is the compounding basis?*
- **Continuous compounding** is the limit of more frequent compounding: \(FV = PV \cdot e^{rt}\), so \(PV = FV \cdot e^{-rt}\). Quants use it because it composes cleanly and matches the math in Black–Scholes and short-rate models.
- **Net present value (NPV)** is the sum of discounted cash flows minus the price you pay today. A positive NPV at the *correct* discount rate is a free lunch only if that rate really is the opportunity cost of capital for that risk.
- **Discount rate ≠ risk-free rate by default.** For a certain cash flow, use the matching risk-free (or funding) rate. For a risky cash flow, either (a) discount expected cash under the real-world measure at a risk-adjusted rate, or (b) take the risk-neutral expectation and discount at the risk-free rate. Mixing the two is a classic interview fail (see [Black–Scholes intuition](../05-black-scholes-intuition/) and [Monte Carlo pricing](../07-monte-carlo-pricing/)).
- **Annuities and perpetuities** are TVM with a closed form. A perpetuity paying \(C\) each period, discounted at \(r\), is \(C/r\). Growing perpetuity: \(C/(r-g)\) if \(r>g\). You should be able to derive both as geometric series.

## Mental model

```
  cash at t=0          cash at t=1          cash at t=2
      |                    |                    |
      v                    v                    v
   [  CF0  ] --------> [  CF1  ] --------> [  CF2  ]
      ^                    |                    |
      |                    |                    |
      +---- discount ------+---- discount ------+
           / (1+r)              / (1+r)^2

   PV = CF0 + CF1/(1+r) + CF2/(1+r)^2 + ...
```

Think of a discount factor \(DF(t)\) as an exchange rate between "dollars at \(t\)" and "dollars today." Pricing is: convert every cash flow into today's currency, then add.

## Interview questions

1. **You can receive $100 in one year or $95 today. The one-year risk-free rate is 4% (annual compounding). Which do you take, and why isn't it "it depends on risk preference"?**
   Answer: PV of $100 in one year is \(100/1.04 \approx 96.15\). That is strictly more than $95, so take the $100 (or, equivalently, borrow $96.15 today against it). For a *certain* cash flow, preference doesn't enter — arbitrage against the risk-free rate decides.

2. **A bank quotes 6% "compounded semiannually." What is the equivalent annually compounded rate? The continuously compounded rate?**
   Answer: Two periods of 3%: effective annual rate is \(1.03^2 - 1 = 6.09\%\). Continuous: \(e^{r_c} = 1.0609 \Rightarrow r_c = \ln(1.0609) \approx 5.91\%\). Never treat "6%" as interchangeable across conventions.

3. **Why do we discount a risk-neutral expected payoff at the risk-free rate, rather than at a "risky" rate?**
   Answer: The risk adjustment is already in the *probabilities* (or in the drift of the simulated process). Discounting again at a spread would double-count risk. Real-world expectation + risk-adjusted discount, *or* risk-neutral expectation + risk-free discount — pick one consistent pair.

4. **A perpetuity pays $10 a year forever, first payment in one year. Rates jump from 5% to 4%. What happens to its value, and what's the intuition?**
   Answer: \(10/0.05 = 200\) becomes \(10/0.04 = 250\). Lower rates raise the present value of distant cash. This is the same mechanism behind [bond prices](../02-bonds-duration-convexity/) moving inversely with yields.

5. **You discount monthly cash flows with an annual rate divided by 12. What's the hidden assumption, and when does it bite?**
   Answer: You're assuming the quoted rate is a nominal annual rate with monthly compounding (or you're approximating). It bites when the quote is an effective annual rate, or when day-count (Actual/360 vs 30/360) disagrees with "divide by 12." Always match the quote convention to the cash-flow grid.

## Watch

- [Time value of money](https://www.youtube.com/watch?v=733mgqrzNKs) — Khan Academy. Sal Khan on why *when* you get money matters as much as *how much*.
- [Introduction to present value](https://www.youtube.com/watch?v=ks33lMoxst0) — Khan Academy. Discounting a future dollar and the role of the interest rate.
- [Present Value 4 (and discounted cash flow)](https://www.youtube.com/watch?v=6WCfVjUTTEY) — Khan Academy. Multiple cash flows and DCF as a sum of present values.

## Further reading

- Any first-principles TVM chapter (e.g. Brealey/Myers or Hull ch. 4 on compounding) is enough; the skill is fluency with conventions, not more formulas.
