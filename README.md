# Project 4 – PINN‑Based Computational Risk Pricing for High‑Value Aerospace Assets

## Abstract

A physics‑informed neural network (PINN) with a monotonic degradation constraint is developed to predict Remaining Useful Life (RUL) of aircraft engines, quantify uncertainty via a deep ensemble, and propagate predictions through a financial risk model. The framework estimates residual value, determines risk‑adjusted lease rates, and optimises multi‑asset portfolios. Using synthetic degradation data (mimicking NGAFID), the model predicts a current RUL of \(290 \pm 90\) cycles, a mean residual value of \$0.19 M (95% CI: \$0.06 M – \$0.34 M), and a break‑even monthly lease rate of \$300,000 to keep the loss probability below 5%. Portfolio optimisation with synthetic identical assets yields equal weights and zero risk reduction, highlighting the need for heterogeneous real‑world data. The framework successfully links physics‑informed degradation modelling with financial risk pricing.

## Data Source

- **Primary data:** Synthetic engine degradation data generated to mimic the NGAFID (National General Aviation Flight Information Database) style, containing 24 sensor features, linear RUL degradation, and sliding windows of 30 time steps.
- **Supplementary data (optional):** FAA aircraft registry files (`MASTER.txt`, `ACFTREF.txt`) – uploaded but returned zero records; therefore synthetic assets were used for portfolio optimisation.
- **Data size:** 1,420 training samples (20 simulated assets) and a single test asset (51 windows).

## Aim & Objectives

**Aim**  
To create a unified computational framework that integrates physics‑informed degradation modelling, uncertainty quantification, residual value forecasting, risk‑adjusted lease pricing, and portfolio optimisation for high‑value aerospace assets.

**Objectives**

1. **Develop a PINN with monotonic degradation constraint** – Enforce <img width="631" height="116" alt="image" src="https://github.com/user-attachments/assets/e2a09c9e-1dc0-4422-a61d-0f93070a4bb5" />
 via a penalty term in the loss function.
2. **Quantify epistemic uncertainty** – Use a deep ensemble of 5 models to estimate mean and standard deviation of RUL predictions.
3. **Forecast residual value under uncertainty** – Perform Monte Carlo simulation using the RUL distribution and a market volatility factor.
4. **Determine risk‑adjusted lease rates** – Compute the minimum monthly lease rate such that the probability of financial loss (revenue < maintenance cost) is below 5%.
5. **Optimise multi‑asset portfolio** – Find the minimum‑variance portfolio of assets using the covariance matrix of their residual values.

## Proving Mathematical Methodology
<img width="1055" height="470" alt="image" src="https://github.com/user-attachments/assets/4f132f01-119e-47c9-871b-fb7c419693d7" />
<img width="1084" height="527" alt="image" src="https://github.com/user-attachments/assets/5dc5f704-23e7-4533-9b2b-1ce3f6ee3d4d" />
<img width="1013" height="495" alt="image" src="https://github.com/user-attachments/assets/f34a0f96-069f-4110-82b8-8c5668387741" />

**References**  
- Nocedal, J., & Wright, S. J. (2006). *Numerical Optimization*.  
- Lakshminarayanan, B., Pritzel, A., & Blundell, C. (2017). Simple and scalable predictive uncertainty estimation using deep ensembles. *NIPS*.  
- Li, Z., & Zhang, J. (2020). Residual value prediction for aircraft assets. *Journal of Asset Management*.  
- Zhang, Y., et al. (2020). Stochastic lease pricing under uncertainty. *Review of Derivatives Research*.  
- Markowitz, H. (1952). Portfolio selection. *The Journal of Finance*.

## Results & Findings

The experiments were performed on synthetic degradation data (20 assets, 1,420 training windows) and a single test asset. The key results are summarised below.

| Metric | Value |
|--------|-------|
| Predicted RUL (current cycle) | \(290 \pm 90\) cycles |
| Residual value (mean) | \$0.19 M |
| Residual value 95% CI | (\$0.06 M, \$0.34 M) |
| Minimum monthly lease rate (loss prob <5%) | \$300,000/month |
| Portfolio risk reduction (vs equal weights) | 0.0% |

**Interpretation**

- The predicted RUL of 290 cycles indicates the engine is near end‑of‑life. The large uncertainty (\(\pm\)90 cycles) reflects the epistemic variance of the ensemble.
- The residual value is very low (<1% of original price), consistent with a nearly depleted asset.
- To keep the loss probability below 5%, the lessor must charge an extremely high rate of \$300,000/month. This effectively signals that leasing is not economically advisable – the asset should be retired or sold for parts.
- The portfolio optimisation gave equal weights and zero risk reduction because the synthetic assets were generated with identical variance and zero correlation. With real FAA data, heterogeneous risks would lead to non‑uniform weights and positive diversification benefit.

## Recommendations

1. **Replace synthetic data** with real NGAFID maintenance logs and FAA registry data for realistic degradation patterns and asset heterogeneity.
2. **Improve calibration** – current uncertainty is high; consider conformal prediction or Bayesian neural networks.
3. **Incorporate more financial parameters** – deposit, buyout options, and dynamic lease terms.
4. **Extend to fleet‑level optimisation** – include operating costs, utilisation rates, and residual value correlations across asset types.
5. **Deploy as an interactive dashboard** (Gradio/Streamlit) for lessors to input asset data and obtain instant risk‑adjusted lease rates and portfolio recommendations.

## Conclusion

This project successfully demonstrates a physics‑informed framework that connects engine degradation modelling with financial risk pricing. The monotonic PINN ensemble produces physically plausible RUL predictions with uncertainty bounds, which are propagated through a residual value model and a lease pricing optimiser. Although the synthetic data leads to a trivial portfolio result, the framework is modular and can be readily adapted to real‑world data. The high break‑even lease rate for a near‑end‑of‑life asset shows that the model correctly signals unprofitable leasing scenarios. This work provides a foundation for scientific machine learning in aerospace asset management, merging differential constraints, Bayesian uncertainty, and financial optimisation into a single pipeline.
