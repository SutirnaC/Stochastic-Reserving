# Stochastic-Reserving
Implement a stochastic reserving framework to quantify reserve risk, generate a full probability distribution of unpaid claims, and calculate the Value at Risk (VaR) for capital modeling.

Plaintext
/Stochastic-Reserving-Model
│
├── data/
│   ├── triangle_generator.py             # Script simulating lognormal/ODP claims
│   └── raw_claims_triangle.csv
│
├── notebooks/
│   ├── 01_Deterministic_Reserving_CL_BF.ipynb
│   ├── 02_Mack_Analytical_Variance.ipynb
│   └── 03_ODP_Bootstrap_Simulation.ipynb
│
├── dashboard/
│   └── Reserving_Capital_Dashboard.pbix
│
└── README.md

Executive Summary
This repository contains a complete, end-to-end stochastic reserving engine for long-tail General Liability and Third-Party Motor lines. 

In inflationary and highly litigious environments, relying solely on deterministic point estimates (like the Basic Chain Ladder) is dangerous. Management requires an understanding of **reserve uncertainty, tail risk, and required capital buffers**. This project bridges that gap by moving from a single "Best Estimate" to a full probability distribution of unpaid claims, enabling precise Value at Risk (VaR) calculations for Solvency II and IFRS 17 compliance.

Key Business Outcomes:

Capital Optimization:  Quantified the 99.5th percentile VaR (1-in-200-year event) to inform the Solvency Capital Requirement (SCR) for reserve risk.

Management Margins: Established a data-driven framework for setting management buffers at the 75th percentile of the simulated distribution.

Risk Identification: Isolated parameter variance from process variance using analytical (Mack) and simulation-based (Bootstrap) frameworks to highlight high-volatility accident years.

---

Repository Structure

```text
/Stochastic-Reserving-Model
│
├── data/
│   ├── triangle_generator.py                 # Script simulating ODP claims with calendar year noise

Methodology & Actuarial Framework
1. Deterministic Baseline
Chain Ladder (CL): Calculated volume-weighted Age-to-Age (ATA) factors to derive the central estimate for mature accident years.

Bornhuetter-Ferguson (BF): Applied an a priori Expected Loss Ratio (ELR) constraint to stabilize volatile, immature accident years where the CL factors are heavily leveraged.

2. Stochastic Modeling
Mack Chain Ladder: Implemented the distribution-free Mack model to calculate the closed-form analytical Standard Error (Process Error + Parameter Error).

Over-Dispersed Poisson (ODP) Bootstrap: Fit an ODP GLM to the incremental triangle. Extracted and standardized Pearson residuals, resampled them with replacement, and generated 10,000 "pseudo-triangles" to simulate the full predictive distribution of ultimate claims.

3. Capital & Validation Metrics
Coefficient of Variation (CoV): Calculated as Standard Deviation / Mean to quantify relative volatility.

Value at Risk (VaR): Extracted the 75th, 90th, and 99.5th percentiles directly from the Bootstrap simulation array to inform capital reserving strategies.

How to Run the Project
Clone the repository:

Bash
git clone [https://github.com/yourusername/Stochastic-Reserving-Model.git](https://github.com/yourusername/Stochastic-Reserving-Model.git)
cd Stochastic-Reserving-Model


Install required dependencies:

Bash
pip install pandas numpy chainladder matplotlib seaborn
Generate the data & run the notebooks:

Execute the triangle_generator.py script first to populate the data/ folder, then proceed through the Jupyter Notebooks sequentially.

Future Extensions
IFRS 17 Risk Adjustment: Building a module to calculate the exact Risk Adjustment based on the Bootstrap output using Tail Value at Risk (TVaR) metrics.

1-Year Solvency Horizon (Merz-Wüthrich): Implementing the Merz-Wüthrich formula to calculate the Claims Development Result (CDR) variance strictly over a 1-year horizon, separating it from ultimate run-off risk.

Copula Dependency Modeling: Introducing Gaussian Copulas to link marginal distributions across multiple correlated lines of business (e.g., Motor and Liability).

