# Algorithmic Monoculture in Machine Learning-Based Site-Specific Fertilizer Recommendation

<p align="center">
  <a href="https://econml26-workshop.github.io/">
    <img src="https://img.shields.io/badge/NeurIPS%202026-EconML%20Workshop-6f42c1" alt="Venue">
  </a>
  <img src="https://img.shields.io/badge/status-submitted-lightgrey" alt="Status">
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue" alt="Python">
  <img src="https://img.shields.io/badge/Jupyter-Notebook-orange" alt="Jupyter">
  <a href="https://colab.research.google.com/github/abdelghanibelgaid/algorithmic-monoculture-fertilizer/blob/main/algorithmic-monoculture-fertilizer.ipynb">
    <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab">
  </a>
</p>

Reproducibility notebook for **Algorithmic Monoculture in Machine Learning-based Site-Specific Fertilizer Recommendation**, submitted to the **Economics for Machine Learning (EconML) Workshop at NeurIPS 2026**.

## Overview

Machine learning recommendation systems are usually evaluated one model at a time, even when a single model may eventually influence decisions across a large population. In fertilizer recommendation, concentrated adoption can synchronize continuous nitrogen, phosphorus, and potassium prescriptions across farms, creating a form of systemic exposure that is not captured by individual predictive accuracy alone.

This repository contains the executable notebook used to reproduce the paper's empirical calibration, counterfactual adoption policies, sensitivity analysis, and figures.

The notebook uses Fertimap-derived fertilizer recommendation resources made available through the independent Turba Open Lab ecosystem. It does not retrain the fertilizer recommendation models. Instead, published benchmark performance from `turba-models` is converted into a dimensionless predictive-risk measure and used to calibrate the economic model.

## Abstract

Machine learning recommendation systems are commonly evaluated model by model, even when deployment occurs across many decision makers. Fertilizer recommendation creates a consequential setting because a common model can synchronize continuous NPK prescriptions across farms. A formal model is developed in which the recommendation error contains a model-specific common component and a farm-specific component. The expected social loss then contains an adoption concentration term proportional to the Herfindahl-Hirschman Index. Under equal predictive risk, diversified deployment strictly reduces systemic loss whenever the common model error and aggregate externality costs are positive. Under heterogeneous predictive risk, a threshold result establishes conditions under which universal adoption of the individually most accurate model is socially dominated by a mixed model portfolio. Benchmark results across nine model families and five Moroccan cereal systems calibrate the model. A central sensitivity setting with a 10% private risk regret cap reduces modeled social loss in all five crop systems, with reductions from 0.61% to 14.19%, while uniform diversification increases loss in three systems. The results identify model diversity as a potential economic risk control rather than an accuracy objective and motivate ecosystem-level evaluation before concentrated deployment of agricultural recommendation systems.

## Research question

The analysis asks a simple ecosystem-level question:

> When can universal adoption of the individually most accurate recommendation model create greater aggregate risk than a carefully constrained portfolio of models?

The framework separates recommendation error into:

- a **model-specific common component**, shared across farms adopting the same model;
- a **farm-specific component**, which can diversify across a large population.

Adoption concentration therefore matters when common model error and aggregate externality costs are nonzero.

## What the notebook reproduces

The notebook implements the complete empirical workflow used in the paper:

1. Loads the public Fertimap-derived dataset through `turba-data`.
2. Inspects pretrained fertilizer recommendation models exposed by `turba-models`.
3. Loads the public nine-model-family cross-model benchmark.
4. Constructs the dimensionless predictive-risk measure

   $$
  r_{cm}=\frac{1}{3}\sum_{t\in\{N,P_2O_5,K_2O\}}\left(1-R^2_{cmt}\right)
  $$

5. Retains the three lowest-risk model families for each crop system.
6. Reproduces the model-concentration loss function and analytical two-model threshold.
7. Solves the accuracy-constrained model-portfolio optimization problem.
8. Compares four adoption policies:
   - best-model monoculture;
   - uniform diversity;
   - accuracy-constrained diversity;
   - unconstrained social optimization.
9. Reproduces the central counterfactual results.
10. Runs the full sensitivity grid over the common-error share, externality weight, and private-risk regret cap.
11. Reproduces both paper figures.

## Empirical calibration

The benchmark covers **nine model families** across **five Moroccan cereal systems**.

### Crop systems

- Barley, rainfed
- Maize, grain
- Maize, silage
- Wheat, irrigated
- Wheat, rainfed

### Model families

- Extra Trees
- LightGBM
- CatBoost
- Random Forest
- XGBoost
- Linear Regression
- Ridge Regression
- Elastic Net
- AdaBoost

### Central scenario

| Parameter | Value | Interpretation |
|---|---:|---|
| Common-error share, $\rho$ | 0.25 | Sensitivity assumption |
| Externality weight, $\lambda$ | 20 | Illustrative sensitivity reference point |
| Population normalization, $n$ | 10,000 | Farms |
| Private-risk regret cap, $\delta$ | 0.10 | Maximum 10% increase in average private predictive risk |
| Candidate models | 3 | Lowest-risk model families per crop |

These values are calibration and sensitivity assumptions. They are not estimates of Moroccan welfare, environmental damages, or deployment conditions.

## Main results

Under the central calibration, accuracy-constrained diversification reduces modeled social loss in all five crop systems.

| Crop system | Reduction in modeled social loss |
|---|---:|
| Barley, rainfed | 14.19% |
| Maize, grain | 0.61% |
| Maize, silage | 2.19% |
| Wheat, irrigated | 7.85% |
| Wheat, rainfed | 10.70% |

The mean reduction is **7.11%**.

Uniform allocation across the three candidate models is not automatically beneficial. It increases modeled social loss in **three of the five crop systems**, demonstrating that model diversity is useful only when predictive quality is protected.

At $\rho=0.25$ and $n=10{,}000$, the two-model externality thresholds are:

| Crop system | Threshold $\lambda$ |
|---|---:|
| Barley, rainfed | 1.76 |
| Maize, grain | 13.67 |
| Maize, silage | 8.00 |
| Wheat, irrigated | 3.25 |
| Wheat, rainfed | 2.36 |

Across the full sensitivity grid, accuracy-constrained diversification reduces modeled social loss in **55.5% of crop-scenario combinations**. Benefits become more frequent as either the common-error share or the externality weight increases.

## Data and model resources

The analysis uses public resources from the independent Turba Open Lab ecosystem.

### `turba-data`

Repository: https://github.com/open-turba/turba-data

Dataset used:

```text
esa_worldcereal_morocco_cereals_medium
```

The notebook currently loads **132,017 rows and 22 columns** directly through the package. No manual dataset upload is required.

### `turba-models`

Repository: https://github.com/open-turba/turba-models

The cross-model benchmark is loaded programmatically from:

```text
https://raw.githubusercontent.com/open-turba/turba-models/main/reports/per_crop_model_leaderboard.csv
```

The benchmark contains **180 target-level rows**, covering **5 crop systems** and **9 model families**.

### Fertimap provenance

Fertimap is the Moroccan soil fertility map and fertilizer recommendation platform developed by OCP Group in collaboration with a consortium of agronomic research institutions and the Moroccan Ministry of Agriculture. Turba Open Lab is an independent open-source ecosystem. The use of Fertimap-derived resources does not imply an official partnership, endorsement, or affiliation between Fertimap and Turba Open Lab.

## Running the notebook

### Google Colab

Use the badge at the top of this README after the notebook is committed to the repository as:

```text
algorithmic-monoculture-fertilizer.ipynb
```

### Local execution

Clone the repository:

```bash
git clone https://github.com/abdelghanibelgaid/algorithmic-monoculture-fertilizer.git
cd algorithmic-monoculture-fertilizer
```

Start Jupyter:

```bash
jupyter notebook algorithmic-monoculture-fertilizer.ipynb
```

The notebook installs its main analysis dependencies directly:

```python
%pip install -q turba-data turba-models numpy pandas scipy matplotlib
```

An internet connection is required to install the public packages and load the benchmark file.

## Repository structure

```text
algorithmic-monoculture-fertilizer/
├── algorithmic-monoculture-fertilizer.ipynb
├── LICENSE
└── README.md
```

The notebook generates the figures used in the paper during execution.

## Reproducibility scope

This repository reproduces the paper's calibrated counterfactual analysis. It should not be interpreted as an estimate of realized agronomic or monetary welfare effects.

Important boundaries include:

- the common-error share $\rho$ is not estimated from row-level residual covariance;
- the externality weight $\lambda$ is a sensitivity parameter;
- benchmark fertilizer recommendations are treated as reference targets rather than experimentally verified causal optima;
- the analysis does not claim improvements in crop yield, farmer profit, nutrient-use efficiency, runoff, greenhouse-gas emissions, or public-health outcomes;
- the benchmark models are not retrained in this notebook.

A direct empirical extension would require common held-out predictions from all candidate models, residual covariance estimation, and externality valuation.

## Citation

The paper is currently under review. A final BibTeX citation will be added after the review process.

For now, please cite the repository title:

> *Algorithmic Monoculture in Machine Learning-Based Site-Specific Fertilizer Recommendation*. Submission to the Economics for Machine Learning Workshop at NeurIPS 2026.

## Venue

**Workshop:** Economics for Machine Learning (EconML)  
**Conference:** 40th Conference on Neural Information Processing Systems, NeurIPS 2026  
**Status:** Submitted

Workshop website: https://econml26-workshop.github.io/

## Related resources

- EconML 2026 Workshop: https://econml26-workshop.github.io/
- Turba Open Lab: https://github.com/open-turba
- `turba-data`: https://github.com/open-turba/turba-data
- `turba-models`: https://github.com/open-turba/turba-models
- `turba-client`: https://github.com/open-turba/turba-client
- Fertimap: http://www.fertimap.ma/

---

**Note:** The repository accompanies a calibrated mechanism study of model adoption concentration. The results are intended to motivate ecosystem-level evaluation of agricultural recommendation systems rather than prescribe a particular deployment policy.
