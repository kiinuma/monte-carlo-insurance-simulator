# Monte Carlo Insurance Loss Simulator
 
An interactive actuarial modeling tool built entirely in the browser — no server, no dependencies, no installation required.
 
**[Live Demo →](https://kiinuma.github.io/monte-carlo-insurance-simulator/)**
 
---
 
## Overview
 
This tool simulates the aggregate annual losses of an auto insurance portfolio using a compound probability model. Set the portfolio parameters — number of drivers, claim probability, and severity range — and the simulator runs thousands of independent one-year scenarios to estimate the distribution of total losses.
 
The key output is the **solvency reserve**: the amount an insurer must hold to cover losses with 95% confidence. Two methods are compared side by side:
 
- **CLT reserve** — a closed-form analytical estimate derived from the Central Limit Theorem
- **Empirical 95th percentile** — drawn directly from the simulated loss distribution
The gap between the two reveals how well the normal approximation holds — an important check for portfolios with heavy-tailed or skewed severity distributions.
 
---
 
## The Model
 
**Claim frequency** — each driver independently files a claim with probability *p*, so the number of claims follows a Binomial(N, p) distribution.
 
**Claim severity** — each individual claim amount is drawn from a Uniform(min, max) distribution, representing a plausible range of loss costs.
 
**Aggregate loss** — total portfolio loss S is the sum of all individual claim amounts, a compound random variable whose moments are derived using the Law of Total Expectation and Variance.
 
### Key Formulas
 
```
Frequency:   N ~ Binomial(n, p)
             E[N] = n · p
             Var[N] = n · p · (1 − p)
 
Severity:    X ~ Uniform(a, b)
             E[X] = (a + b) / 2
             Var[X] = (b − a)² / 12
 
Aggregate:   E[S] = E[N] · E[X]
             Var[S] = E[N] · Var[X] + Var[N] · E[X]²
             Reserve = E[S] + 1.645 · Std[S]
```
 
---
 
## Parameters
 
| Parameter | Description |
|-----------|-------------|
| **Drivers (N)** | Total policyholders in the portfolio. Larger portfolios produce tighter, more bell-shaped loss distributions as individual randomness averages out. |
| **P(claim)** | Annual probability any single driver files a claim. At 5%, roughly 500 of 10,000 drivers are expected to have a loss in a given year. |
| **Min Severity** | Lower bound of the Uniform claim cost distribution. |
| **Max Severity** | Upper bound. Widening the severity range increases aggregate variance and pushes the required reserve higher. |
| **Simulations** | Number of independent one-year scenarios generated. 10,000 is sufficient for stable results at most parameter settings. |
| **Premium Loading** | Markup applied on top of expected losses to cover expenses and profit. At 20% loading, an insurer collects $1.20 for every $1.00 of expected loss. |
 
---
 
## Premium Calculator
 
After each simulation run the tool derives two pricing outputs:
 
- **Premium per driver** — annual rate each policyholder is charged: `(E[S] / N) × (1 + loading)`
- **Total premium pool** — aggregate revenue across the portfolio: `E[S] × (1 + loading)`
Together these show whether the premium structure is sufficient to cover both expected losses and the solvency reserve — the central question in actuarial rate-setting.
 
---
 
## Why This Matters
 
Actuaries use models like this to set premium rates and statutory reserves. This project demonstrates:
 
- Application of compound probability distributions to real-world insurance modeling
- Derivation of closed-form reserve estimates using the Central Limit Theorem
- Validation of analytical results against Monte Carlo simulation output
- Interactive parameter sensitivity analysis showing how pricing assumptions affect reserve requirements
---
 
## Stack
 
- Vanilla JavaScript — browser-native, zero dependencies
- HTML/CSS — no framework
- Hosted on GitHub Pages
---
 
## Author
 
Kazumasa Iinuma — Applied Mathematics, Boston University  
[GitHub](https://github.com/kiinuma) · [LinkedIn](https://linkedin.com/in/kazumasa-iinuma)
