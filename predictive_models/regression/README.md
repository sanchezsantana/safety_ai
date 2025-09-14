# Regression Models for Mining Safety

This module implements **classical predictive models** (logistic/Poisson regression, tree ensembles such as XGBoost/LightGBM) to estimate incident risk from leading indicators.  
It serves as an **interpretable baseline** and the reference point for comparing advanced approaches.

---

## 1) Context — Why regression is needed in mining
- Mining operations require **explainability and traceability** to support decision-making at all levels (HSE, supervisors, managers).  
- Severe events are **rare**; regression provides a calibrated way to estimate probabilities without overfitting.  
- Establishes a **baseline**: allows us to measure the incremental value of sequence models, clustering, similarity-based reasoning, and integrated explainability.  
- Regression + SHAP = the **minimum viable predictive model** that can be shown to clients and integrated in dashboards.

---

## 2) Architecture — Connections with Year-0, Synthetic Data, Text2Tabular, VDB/GDB, and QA Agent
- **Year-0**: Provides the first tabular dataset (daily/weekly by area & risk) to train regression models.  
- **Synthetic Data**: Adds stress tests with **different qualities of reports** (good, incomplete, noisy), merged with Year-0 timelines.  
- **Text2Tabular**: Enables regression models to use inputs reconstructed from textual reports/observations.  
- **VDB (BGE-m3)**: Supplies semantic similarity features (e.g., “distance to known hazardous trajectories”).  
- **GDB**: Provides ontology-anchored features (e.g., count of active controls, centrality of causes).  
- **QA Agent**: Exposes predictions as **risk scores** with **SHAP explanations** to operators in real time.  

Data flow:
```
Year-0 / Synthetic → feature_store (lags, windows, ratios) →
models/ (LR, XGB/LGBM) →
calibration (Platt/Isotonic) →
outputs (scores + SHAP) →
QA Agent / Dashboards
```

---

## 3) Technical Methodology
- **Models**: Logistic Regression (L2), Poisson/Negative Binomial for counts, **XGBoost/LightGBM** for nonlinearities.  
- **Explainability**:  
  - **SHAP** for local (per prediction) and global explanations.  
  - Permutation importance for sanity checks.  
- **Features**:  
  - Rolling windows (3/7/14/30 days).  
  - Slopes (`slope_obs_cc`), ratios (`obs_cc/obs_general`).  
  - Time since last HPOT (`days_since_last_hpot`).  
  - HPOT counts (`hpot_count_30d`).  
  - Estacionalidad: sin/cos transforms of day/month.  
- **Calibration**: Brier score, Isotonic/Platt scaling for rare event probabilities.  
- **Metrics (rare events)**: PR-AUC, Recall@k (top-N alerts/day), cost-sensitive metrics (FN ≫ FP), Expected Cost.  
- **Temporal validation**: Train (Q1–Q2), Validation (Q3), Test (Q4); rolling evaluation.  
- **Robustness**: Simulations with low coverage (`coverage_*`) and injected noise.

---

## 4) Expected Results
- **Risk scores** per (area, risk, day/week).  
- **Top drivers (SHAP values)** for each prediction and aggregated globally.  
- **Alert-fatigue curves**: number of alerts/day vs recall.  
- **Calibrated thresholds** for operational deployment.  
- **Dashboards/QA Agent**:  
  - “Why is the risk score 0.87?” → SHAP breakdown.  
  - “What factors increased risk this week?” → drivers ranked by contribution.  

---

## 5) Differentiation vs Other Lines
- The only **baseline model**: fast, interpretable, and easy to validate.  
- Does not capture **temporal dynamics** like `trajectory_models`.  
- Does not discover **new patterns** like `clustering`.  
- Does not provide **case-based reasoning** like `trajectory_similarity`.  
- Feeds into `explainability` as the **feature-level contribution layer**.  

---

## 6) Roadmap
- **MVP**: Logistic Regression + XGB, core features, SHAP local/global, calibration; simple dashboard via QA Agent.  
- **V2**: Survival models (time-to-event), expected cost modeling, prioritization of alerts.  
- **V3**: Integration with VDB/GDB features, drift monitoring, continuous retraining with new data.  
