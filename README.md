# Monte Carlo Insurance Loss Simulator

An interactive actuarial modeling tool built entirely in the browser — no server, no dependencies, no installation required.

**[Live Demo →](https://kiinuma.github.io/monte-carlo-insurance-simulator/)**

---

## What It Does

Two tabs. Two core actuarial methods.

**Simulator tab — Monte Carlo aggregate loss modeling**
Set portfolio parameters (drivers, claim probability, severity range) and run thousands of independent one-year scenarios. Outputs the aggregate loss distribution, a CLT-based solvency reserve, an empirical 95th percentile, and a per-driver premium calculator.

**Loss Triangle tab — Chain-ladder IBNR estimation**
Generate a synthetic loss development triangle and apply the chain-ladder method to project IBNR (Incurred But Not Reported) reserves by accident year. Development factors are calculated from the known upper-left triangle and applied to project the lower-right.

---

## The Models

### Monte Carlo (Simulator tab)

**Claim frequency** — each driver independently files a claim with probability p, so claim count follows Binomial(N, p).

**Claim severity** — each individual claim is drawn from Uniform(min, max).

**Aggregate loss** — total portfolio loss S is a compound random variable with moments derived from the Law of Total Expectation and Variance:

```
E[S]   = E[N] · E[X]
Var[S] = E[N] · Var[X] + Var[N] · E[X]²
Reserve = E[S] + 1.645 · Std[S]    (CLT 95th percentile)
```

### Chain-Ladder (Loss Triangle tab)

**Development factors** — weighted average age-to-age ratios from the known triangle:

```
f_j = Σ C[i, j+1] / Σ C[i, j]    (summed over rows where both periods are known)
```

**IBNR projection** — factors applied to the latest diagonal to project ultimate losses:

```
C[i, j] = C[i, j−1] × f_{j−1}
IBNR_i  = C[i, ultimate] − C[i, latest diagonal]
```

---

## Parameters

| Parameter | Description |
|---|---|
| **Drivers (N)** | Total policyholders. Larger portfolios produce tighter loss distributions as individual randomness averages out. |
| **P(claim)** | Annual claim probability per driver. At 5%, ~500 of 10,000 drivers file claims. |
| **Min / Max Severity** | Bounds of the Uniform claim cost distribution. Widening the range increases aggregate variance and pushes the reserve higher. |
| **Simulations** | Number of independent one-year scenarios. 10,000 is sufficient for stable results. |
| **Premium Loading** | Markup above expected losses to cover expenses and profit. At 20%, the insurer collects $1.20 per $1.00 of expected loss. |
| **Accident Years** | Number of rows in the loss triangle (3–7). |
| **Dev Pattern** | Conservative / Moderate / Volatile — controls how quickly losses develop across periods. |

---

## Why This Matters

Actuaries use these models to set premium rates and statutory reserves — two of the most consequential decisions in insurance. This project demonstrates:

- Application of compound probability distributions to real-world loss modeling
- CLT reserve derivation and validation against Monte Carlo simulation output
- Chain-ladder IBNR estimation from historical development patterns
- Interactive sensitivity analysis showing how assumptions affect reserve requirements

The gap between the CLT reserve and the empirical 95th percentile reveals how well the normal approximation holds — an important check for heavy-tailed or skewed severity distributions. The gap between the chain-ladder IBNR and the Monte Carlo reserve shows the two methods approaching the same reserving problem from different angles.

---

## Stack

- Vanilla JavaScript — all simulation and chain-ladder logic, no dependencies
- HTML / CSS — no framework
- Google Fonts (DM Sans, DM Mono) — loaded via CDN
- Hosted on GitHub Pages

---

## Author

Kazumasa Iinuma — Applied Mathematics & Data Science, Boston University  
[GitHub](https://github.com/kiinuma) · [LinkedIn](https://linkedin.com/in/kazumasa-iinuma)
