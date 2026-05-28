# MD&A Volatility Forecasting
### Leakage-Safe Contextual NLP for Short-Horizon Equity Risk Prediction

**MSc Dissertation | Cardiff University | MAT099 | Data Science and Analytics**
**Student ID: 24043659 | Supervisor: Anqi Liu**

---

## Research Question

Do textual risk signals from MD&A improve future equity volatility forecasting
beyond market-based predictors, and under what conditions are they most
informative?

---

## Key Results

| Model | RMSE_log | R2_log | vs Benchmark |
|---|---|---|---|
| Naive Lag Vol (benchmark) | 0.5001 | -0.048 | — |
| RF Model 1 — Market only | 0.4418 | 0.182 | — |
| RF Model 2 — + LM Dictionary | 0.4405 | 0.187 | -0.3% |
| RF Model 3 — + FinBERT PCA | **0.4371** | **0.199** | **-12.6%** |
| RF Model 4 — + Semantic Surprise | 0.4377 | 0.197 | -11.9% |

Diebold-Mariano test (M1 vs M3): p = 0.034 — statistically significant
improvement under squared-error loss.

Conditional findings:
- FinBERT gain is 2x larger under market stress (0.0088) vs calm (0.0039)
- FinBERT adds 14x more value when LM uncertainty is low vs high
- Hard-case error reduction is 4.6x the overall average

---

## Pipeline Overview

![Research Pipeline](figures/fig3_1_pipeline_diagram.png)

Sample: 2,058 filing events | 122 firms | May 2019 to December 2024
Holdout: 786 observations | 2023 to 2024 | evaluated once only

---

## Repository Structure

notebooks/
├── 01_firm_universe/        — S&P 500 firm selection and SEC identifier matching
├── 02_mda_extraction/       — MD&A text extraction with iterative repair
├── 03_price_data/           — CRSP price alignment and target construction
├── 04_feature_engineering/  — LM dictionary, FinBERT embeddings, semantic surprise
├── 05_modelling_development/— Exploratory modelling rounds 1 to 3
└── 06_final_modelling/      — Gold-standard locked evaluation framework

figures/                     — All 10 dissertation figures
results/                     — Final holdout performance metrics
data/                        — Data requirements and sample format

---

## How to Run

Step 1: Install dependencies
pip install -r requirements.txt

Step 2: Set up your data
See data/README_data.md for CRSP and SEC EDGAR data requirements.

Step 3: Run notebooks in order
01_firm_universe     → build firm list (148 firms, 132 after coverage filter)
02_mda_extraction    → extract MD&A text (2,265 final usable sections)
03_price_data        → align CRSP prices (2,247 observations)
04_feature_engineering → LM features, FinBERT embeddings, semantic surprise
05_modelling_development → exploratory rounds
06_final_modelling   → 12_dataset_v4.ipynb — final locked evaluation

---

## Methodology

Text representations tested:
- Loughran-McDonald (2011) dictionary: negative, positive, uncertainty proportions
- FinBERT (Araci, 2019) contextual embeddings: 768-dim to 15 PCA components
- Semantic surprise features: firm-relative embedding distances

Anti-leakage design:
- Strict chronological holdout (2023 to 2024, never seen during development)
- Train-only PCA fitting — no holdout information in dimensionality reduction
- Three expanding validation folds for hyperparameter tuning

Models: Linear Regression, Elastic Net, Random Forest, XGBoost
Evaluation: RMSE_log, MAE_log, R2_log, bootstrap CIs, DM-style tests

---

## Data Availability

CRSP data is proprietary (WRDS subscription required).
SEC EDGAR filings are publicly available.
MD&A text files are reproducible using the extraction pipeline.

---

## Citation

Cardiff University MSc Dissertation, MAT099
Student ID: 24043659
Title: Do MD&A Textual Risk Signals Improve Equity Volatility Forecasting?
Year: 2025
Supervisor: Anqi Liu

---

## Licence

MIT License — see LICENSE file for details.