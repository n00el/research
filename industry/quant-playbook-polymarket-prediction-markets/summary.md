# Summary: The Quant Playbook for Polymarket

## One-Line Summary

A breakdown of 6 mathematical formulas (LMSR, Kelly Criterion, EV Gap, KL-Divergence, Bregman Projection, Bayesian Updates) that hedge funds use to systematically extract profits from prediction markets like Polymarket.

## Key Takeaways

- **LMSR Pricing Model** is the AMM engine behind Polymarket -- understanding its liquidity parameter (b) lets quants predict price impact and exploit thin pools before other traders react.
- **Kelly Criterion** provides optimal bet sizing for long-term compounding; fractional Kelly (0.25-0.5x) is recommended to buffer against probability estimation errors and avoid ruin.
- **EV Gap Detection** is the core mispricing scanner: bet only when your model's probability exceeds the market price by enough margin (EV > 0.05 after fees) to justify entry.
- **KL-Divergence** measures distributional distance between correlated markets to find cross-market arbitrage opportunities (e.g., correlated political outcomes).
- **Bregman Projection** enables multi-outcome arbitrage by projecting onto the probability simplex to detect inconsistencies across exponential outcome combinations.
- **Bayesian Updating** dynamically adjusts probability estimates as new evidence (tweets, polls, news) arrives, giving an edge in fast-moving markets over static models.
- **Risk management is paramount:** fractional Kelly sizing, walk-forward backtesting, out-of-sample validation, and 20% drawdown stops are essential guardrails against overfitting and ruin.

## Topics

`prediction-markets`, `quantitative-finance`, `polymarket`, `kelly-criterion`, `market-microstructure`, `arbitrage`, `bayesian-inference`, `hedge-funds`, `algorithmic-trading`
