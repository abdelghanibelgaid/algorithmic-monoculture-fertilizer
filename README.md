# Algorithmic Monoculture in Machine Learning-Based Site-Specific Fertilizer Recommendation

<p align="center">
  <a href="https://econml26-workshop.github.io/"><img src="https://img.shields.io/badge/NeurIPS%202026-EconML%20Workshop-6f42c1" alt="Venue"></a>
  <a href="https://openreview.net/forum?id=cxXP3bI6LC"><img src="https://img.shields.io/badge/Paper-OpenReview-8c1b13" alt="OpenReview"></a>
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue" alt="Python">
  <a href="https://colab.research.google.com/github/abdelghanibelgaid/algorithmic-monoculture-fertilizer/blob/main/algorithmic-monoculture-fertilizer.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a>
  <img src="https://img.shields.io/badge/Jupyter-Notebook-orange" alt="Jupyter">
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-green" alt="MIT License"></a>
</p>

## Abstract

Machine learning recommendations are often evaluated one model at a time, despite deployment across interconnected decision makers. For continuous NPK fertilizer prescriptions, correlated errors can aggregate across farms and generate systemic costs. A model of shared and farm-specific recommendation errors links expected social loss to model adoption shares; the Herfindahl-Hirschman Index characterizes concentration effects when common model errors are independent and predictive risks are equal. A covariance-generalized formulation captures the dependence between model families. A two-model threshold characterizes when privately optimal monoculture is socially dominated, and a convex portfolio rule limits average private predictive regret. Nine model families across five Moroccan cereal systems provide a benchmark-based calibration. Under illustrative parameters and a 10\% regret cap, accuracy-constrained diversification lowers modeled social loss in all five systems by 10.89-41.53\% (mean 19.41\%). Uniform allocation reduces modeled loss in this setting, but breaches the regret cap in every crop. Correlation sensitivity shows that highly dependent model errors attenuate the benefits of diversification. These counterfactual outcomes are not estimates of agronomic or monetary welfare; the framework identifies adoption architecture and error dependence as ecosystem-level evaluation variables.

## Research questions

- When can the individually lowest-risk model generate greater aggregate loss under concentrated adoption?
- Can model diversity limit systemic exposure without exceeding a specified average private predictive-regret cap?
- How do assumed common-error shares, externality intensity, and cross-model error correlations affect the result?

## Data and models

The analysis uses the independent [Turba Open Lab](https://github.com/open-turba) ecosystem: [turba-data](https://github.com/open-turba/turba-data) provides recommendation scenarios, and [turba-models](https://github.com/open-turba/turba-models) provides fixed benchmark results. The input comprises five crop systems, nine model families, and N, P₂O₅, and K₂O targets. Candidate portfolios include the three lowest-risk families per crop. The notebook does not retrain the benchmark models.

The leaderboard is read from `turba-models` commit `c4376f99587cb00d7ae9de92389914ecc29c1412` and its SHA-256 is reported in the notebook. Targets are recommendation-system outputs, not observed crop-yield responses.

## Results

For the illustrative setting $\rho=0.25$, $\lambda=20$, $n=10{,}000$, and an average private-regret cap of $\delta=0.10$, constrained portfolios reduce modeled social loss across the five crop systems:

| Crop system | Modeled loss reduction |
|---|---:|
| Maize (Silage) | 41.53% |
| Maize (Grain) | 21.34% |
| Wheat (Rainfed) | 12.04% |
| Barley (Rainfed) | 11.25% |
| Wheat (Irrigated) | 10.89% |

The five-crop arithmetic mean is 19.41%. Across 600 prespecified sensitivity scenarios, 69.0% show a positive modeled benefit. Uniform allocation violates the 10% private-risk cap in every crop under the central scenario. Assumed positive correlation between common model errors attenuates or removes the gains.

## Run the notebook

1. Open [`algorithmic-monoculture-fertilizer.ipynb`](./algorithmic-monoculture-fertilizer.ipynb) in Kaggle, Jupyter, or Colab with internet access enabled.
2. Run all cells, including installation of `turba-data` and `turba-models`.
3. Inspect the inline benchmark validation, allocation tables, diagnostic checks, three figures, and sensitivity results.

Python 3.10+ and an internet connection are required for the benchmark download and package setup. The notebook displays the software versions, pinned benchmark commit, and checksum.

## Citation

> Belgaid, A. (2026). *Algorithmic Monoculture in Machine Learning-based Site-Specific Fertilizer Recommendation*. NeurIPS 2026 Workshop on Economics for Machine Learning (EconML).

## Related resources

- [Turba technical report [arXiv:2610.05949]](https://arxiv.org/abs/2610.05949)
- [turba-data](https://github.com/open-turba/turba-data)
- [turba-models](https://github.com/open-turba/turba-models)
- [EconML workshop](https://econml26-workshop.github.io/)

## License
Code is released under the MIT License. See [LICENSE](./LICENSE).