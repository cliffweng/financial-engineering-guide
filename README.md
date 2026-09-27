# Financial Engineering Study Guide

A practical study guide for quantitative finance and financial engineering — for interview prep and self-study.

**Live site:** https://cliffweng.github.io/financial-engineering-guide/

## Roadmap

14 topics, one file each under [`topics/`](topics/), ordered foundational → applied:

1. TVM / discounting
2. Bonds & duration/convexity
3. Forwards / futures
4. Options payoff & Greeks
5. Black–Scholes intuition
6. Binomial trees
7. Monte Carlo pricing
8. Interest-rate curves / swaps intro
9. Credit risk / CDS intro
10. Portfolio theory & Markowitz
11. VaR / risk measures
12. Volatility & smiles
13. Hedging & Greeks in practice
14. Interview case patterns

## Interview hotspots

Every topic page carries a badge (🎯 Interview frequent / Interview occasional / Background) so you know where to spend prep time. If you're short on time, prioritize these:

- **TVM / discounting** — almost every pricing or "what's this cash flow worth" question starts here; interviewers use it to check you can move money through time without mixing compounding conventions.
- **Bonds & duration/convexity** — the workhorse of rates desks and risk; expect "why do prices fall when yields rise" and a duration/convexity approximation.
- **Forwards / futures** — cash-and-carry and the difference between OTC forwards and exchange-traded futures come up constantly.
- **Options payoff & Greeks** — payoff diagrams, put-call parity, and first-order Greeks are table stakes for any derivatives loop.
- **Black–Scholes intuition** — you rarely need to derive the PDE on a whiteboard, but you do need replication, risk-neutral pricing, and what \(N(d_1)\) / \(N(d_2)\) mean.
- **Monte Carlo pricing** — the default numerical method once payoffs are path-dependent; interviewers probe risk-neutral simulation, discounting, and variance.
- **Interest-rate curves / swaps** — bootstrapping, forward vs. par rates, and "what is a swap actually exchanging" are frequent.
- **Portfolio theory & Markowitz** — diversification, the efficient frontier, and why covariance (not just variance) is the engine.
- **VaR / risk measures** — definition, three computation methods, and why VaR is not coherent.
- **Hedging & Greeks in practice** — delta hedging, gamma/theta tradeoff, and why discrete rehedging leaks P&L.
- **Interview case patterns** — how to structure a pricing, risk, or "what's wrong with this book" case in 8–12 minutes.

**Occasional** (still worth knowing, less likely to anchor a whole interview): binomial trees, credit risk / CDS intro, volatility smiles.

This split is a judgment call based on what shows up in quant/FE interviews today, not a guarantee for any specific loop — adjust your prep if a role is unusually credit-, vol-, or rates-specialized.

## How to use this guide

Each topic page is designed to be read in **~10 minutes** and follows the same structure: why it matters, core concepts, a mental model, 3–5 interview questions with brief answer keys, and a short list of verified YouTube videos. Read them in order, or jump straight to what you need. No backend, no market data feed, no auth, no sign-up — just read the pages.

## How to contribute

See [CONTRIBUTING.md](CONTRIBUTING.md). In short: one topic per file, keep it under ~10 minutes to read, and only link to sources (especially YouTube videos) you've personally verified exist. Open an issue before proposing new topics or restructuring the curriculum.

## Decisions

These are the product locks this guide was built against — echoed here so future contributors don't accidentally relitigate them:

- **Audience**: quant/FE interview prep + self-study. Not a CFA crash course and not a live trading platform.
- **Time-boxed**: every topic is readable in 10 minutes or less. Depth is sacrificed for scannability; "further reading" links are where depth lives.
- **Learning + interview prep in one page**: each topic pairs core concepts with interview questions, rather than splitting them into separate tracks.
- **Real links only**: every YouTube link is verified to exist before being added. No invented URLs, ever.
- **Static site, GitHub Pages, Just the Docs**: no backend, no auth, no paid data, no quizzes. Cheap to host, cheap to maintain, easy to contribute to via plain Markdown + front matter.

## Enabling GitHub Pages

Just the Docs is configured via `_config.yml` (remote theme). After this lands on `main`:

1. Repo **Settings → Pages**.
2. **Build and deployment → Source**: **Deploy from a branch**.
3. Branch: `main` / folder: `/ (root)`.
4. Save. Site publishes to https://cliffweng.github.io/financial-engineering-guide/

## Local preview

```bash
bundle install
bundle exec jekyll serve
```

Then open `http://localhost:4000`.

## License

[MIT](LICENSE)
