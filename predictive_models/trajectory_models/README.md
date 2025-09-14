# Trajectory Models (LSTM, TFT) for Mining Safety

This module develops **sequence models** to capture the **temporal dynamics of risk**.  
Unlike regression, which treats predictors as static snapshots, trajectory models learn **patterns of evolution**: degradation, sudden crisis, recovery, or chronic risk.

---

## 1) Context — Why sequence models are needed in mining
- Incidents rarely occur “out of nowhere”; they emerge from **progressive trajectories** of observations and control degradation.  
- Supervisors need **short-term forecasts** (e.g., 7–14 days ahead) that account for **lags and lead signals**.  
- Sequence models provide the ability to identify **phases of risk buildup** that regression cannot capture.  

---

## 2) Architecture — Connections with Year-0, Synthetic Data, Text2Tabular, VDB/GDB, and QA Agent
- **Year-0**: Supplies labeled sequences (daily by area & risk) including both safe and unsafe phases.  
- **Synthetic Data**: Adds noise, missing data, and varied observation quality to improve robustness.  
- **Text2Tabular**: Converts textual reports into tabular signals that can be embedded in sequences.  
- **VDB (BGE-m3)**: Provides semantic embeddings aggregated by window (e.g., 7d, 14d).  
- **GDB**: Supplies graph-based features (e.g., connectivity of controls, common causes).  
- **QA Agent**: Exposes forecasts as **risk trajectories** with temporal explanations (e.g., “last 14 days of low CC uptime drive current risk”).  

Pipeline:
```
Year-0 / Synthetic → sequence builder (lags, windows) →
LSTM / TFT models →
risk forecasts (7d, 14d horizons) +
temporal explanations (attention, feature importance) →
QA Agent / Dashboards
```

---

## 3) Technical Methodology
- **Models**:  
  - LSTM / GRU for sequence learning.  
  - **TFT (Temporal Fusion Transformer)**: multi-horizon forecasting with attention and gating.  
  - HMM/HSMM as lightweight baselines for phase detection.  
- **Features**:  
  - Multi-scale lags (3/7/14/30 days).  
  - Counters of events (e.g., near misses, HPOT).  
  - Estacionalidad: sin/cos for week/month cycles.  
  - Coverage variables (coverage_reports, coverage_sensors).  
- **Regularization**: dropout, early stopping, temporal augmentation (jitter, mask).  
- **Metrics**: PR-AUC by horizon, **time-to-event concordance index**, calibration curves, **earliness-recall tradeoff**.  
- **Explainability**:  
  - Attention weights (TFT).  
  - Captum (Integrated Gradients, DeepLIFT) for LSTM.  
  - Feature importances over time windows.  

---

## 4) Expected Results
- **Risk forecasts** per horizon (7d, 14d).  
- **Critical temporal windows** that drive current forecasts.  
- **Phase detection**: identification of degradation, stable, or crisis phases.  
- **What-if scenarios**: simulate interventions (e.g., CC restored) and forecast impact.  
- **Dashboards**: risk bands over time, annotated events, temporal contributions.  

---

## 5) Differentiation vs Other Lines
- Only approach that models **full temporal dynamics**.  
- Complements `regression` (static) and `trajectory_similarity` (case-based).  
- Benefits from `clustering` (clusters can act as trajectory-level features).  
- Provides temporal evidence that feeds into the `explainability` layer.  

---

## 6) Roadmap
- **MVP**: Simple LSTM with 7/14d horizon, PR-AUC evaluation, temporal visualization.  
- **V2**: TFT with multi-horizon outputs, attention-based explanations integrated into QA Agent.  
- **V3**: Advanced phase detection (HMM/HSMM), simulation of interventions, integration with GDB features.  
