# Year-0: Synthetic Baseline for Predictive Models

This module defines and generates **Year-0**, a synthetic representation of one full year of mining operations.  
Year-0 is the foundation for testing predictive models in the absence of real data, and also serves as a **retrofill mechanism** when client data is incomplete.

---

## 📌 Objectives
- Provide a **sandbox (Year-0)**: a synthetic year of observations, incidents, and interventions that serves as the baseline for model testing.  
  - Predictive models **do not access the predefined trajectories directly**; they only see daily tabular data and must **discover patterns** by themselves.  
- Develop **synthetic data generators** for observations, incidents, and trajectories using the safety ontology and LLMs.  
  - The **first batch of synthetic data** is generated to **replicate Year-0 in textual form** (reports, observations, incidents).  
  - Synthetic narratives are embedded with **BGE (bge-m3)** and **tabulated** to ensure alignment with the Year-0 structure before generating new scenarios.  
- Support **retrofill** of missing or poor-quality client data using synthetic trajectories consistent with Year-0.  
- Ensure Year-0 acts as a **reference framework** for all predictive approaches, feeding directly into regression, clustering, leading indicators, and sequence models.  

---

## 📂 Structure
```
year0/
│
├── catalogo_trayectorias.yaml     # Archetypes of trajectories (T1–T9 with variants)
├── targets.yaml                   # Annual targets (ratios, frequencies, distributions)
├── interventions.yaml             # Post-event interventions (latency, intensity, verification)
├── area_role_task_map.yaml        # Mapping of areas, roles, tasks, risks, and controls
│
├── generator_year0.py             # Script to generate the Year-0 calendar
├── qa_year0.py                    # Quality assurance checks (balance, ontology consistency)
├── feature_store.py               # Feature generation (windows, slopes, lags)
│
├── examples/                      # Sample CSV/Parquet outputs of Year-0
└── README.md                      # This file
```

---

## 📌 Development Specification — Predictive Year-0

### 1. Definitions and Rules
- **HPOT (High Potential Event)** and **fatality potential** defined in `labeling_rules.yaml`.  
- Severity and potential levels:  
  - `severity_level ∈ {minor, serious, major}`  
  - `potential_level ∈ {low, high, fatality_potential}`  
- Each rule connected to risks, controls, and causes in the ontology.

### 2. Trajectory Catalog (V2)
**Archetypes with variants:**

1. **Stable / Safe**  
   - Variants: (a) high coverage, (b) medium with noise, (c) low coverage.

2. **Operational Noise**  
   - Variants: (a) noise in general obs, (b) in CC, (c) combined.

3. **Slow Degradation**  
   - Variants: (a) ends in minor incident, (b) ends in HPOT near miss, (c) ends in hazard.

4. **Sudden Crisis**  
   - Variants: (a) no signals, (b) dispersed signals, (c) failed controls unseen.

5. **Partial Recovery**  
   - Variants: (a) initial improvement with mild relapse, (b) sustained improvement with minor issues, (c) false recovery.

6. **Post-Event Relapse (Event → measures → new event)**  
   - Variants:  
     - (a) effective intervention → no relapse  
     - (b) weak intervention → early relapse  
     - (c) partial intervention → rebound + medium relapse  
     - (d) failed verification → relapse  
     - (e) second event by different cause  
     - (f) relapse under low observability

7. **Latent Accumulation**  
   - Variants: (a) ends in hazard, (b) ends in near miss, (c) ends in incident.

8. **Chronic Low Observability**  
   - Variants: (a) no data, (b) erratic coverage, (c) contradictory reports.

9. **Critical Audit**  
   - Variants: (a) independent audit → reduces risk, (b) superficial audit → paper-passes, (c) failed audit → event follows.

Each trajectory has phases (`start, degradation, crisis, recovery, stable`) and parameters:  
- `obs_general`, `obs_cc` (ranges/trends)  
- `cc_status`, `cc_uptime_ratio`  
- Incident probabilities (`near_miss_hpot_p`, `injury_hpot_p`, `fatality_potential_p`)  
- `coverage_reports`, `coverage_sensors`  
- Intervention and verification fields (`intervention_latency_d`, `intensity`, `verification_pass_rate`).

### 3. Annual Distribution
- 365 days × area × risk × task.  
- Concatenate trajectories with:  
  - Neutral periods (≥ 25% of the year).  
  - Global balance:  
    - Major incidents < 1% of days.  
    - Ratio `obs_general : obs_cc ≈ 3:1`.  
- Automatic validation against `targets.yaml`.

### 4. Noise and Variability
- Random walk with amplitude ±10–15% of the mean.  
- Noise episodes defined as Operational Noise trajectories.  
- Parametric variation of trajectories (e.g., T2a, T2b).

### 5. Observability and Missing Data
- Fields:  
  - `coverage_reports (0–1)`  
  - `coverage_sensors (0–1)`  
  - `data_quality_state ∈ {high, medium, low}`  
- Rules:  
  - If `coverage < 0.6` → `estado_dia = incierto`.  
  - Add `uncertainty ∈ [0,1]`.  
  - Flags for imputation.

### 6. Interventions and Verification
- `interventions.yaml` defines:  
  - Types: `audit_cc, toolbox_talk, maintenance, retraining, stop_work`.  
  - Parameters: `latency_d, intensity, rebound, decay_days, relapse_p`.  
- Post-event dynamics:  
  - Rebound of observations and uptime.  
  - Exponential decay in 7–21 days.  
  - Verification after 3–10 days.  
- **Spillover**: % effect transferred to neighbor areas.

### 7. Seasonality and Operational Context
- Seasonal signals (`sin/cos` for month/day).  
- Campaigns/stoppages in `calendar_ops.yaml`.  
- Modulation of `p(event)` by area/condition (e.g., dust, weather).

### 8. Automatic Validation (QA)
- `qa_year0.py`:  
  - Ontology consistency: risk ↔ control ↔ cause.  
  - Sanity checks:  
    - `p(event)` ↑ if `obs_cc ↓` or `cc_uptime ↓`.  
    - Annual rates within `targets.yaml`.  
    - Do not label “good day” if `coverage < 0.6`.  
    - No invalid overlaps of trajectories.

### 9. Feature Store and Causality
- 3/7/14/30-day windows: moving averages, slopes, ratios.  
- Context variables: `days_since_last_hpot`, `hpot_count_30d`.  
- Temporal split: Train (Q1–Q2), Validation (Q3), Test (Q4).  
- **Causal linter**: ensure no feature uses post-event info.

### 10. Metrics and Evaluation
- For rare events:  
  - **PR-AUC**  
  - **Brier score + calibration**  
  - **Cost-sensitive metrics** (FN≫FP)  
  - **Alert-fatigue curves**  
- Visualizations:  
  - Trajectories with events/interventions.  
  - Reliability diagrams.  
  - Area/risk heatmaps.

### 11. Uplift / Effect of Interventions
- Save synthetic `policy_log`.  
- Estimate impact with **T-Learner** or simple counterfactuals.

### 12. Reproducibility and Engineering
- Versioning: `catalogo_trayectorias_v1.yaml`, `interventions.yaml`, `targets.yaml`.  
- Reproducible seeds (`seed=42`).  
- Data dictionary per table.  
- Minimal unit tests (`qa/tests/`).  
- Save manifests and hashes (lineage).

### 13. Validation with Embeddings (textual synthetic data)
- Generate textual descriptions of observations/incidents with LLM.  
- Encode with **BGE (bge-m3)**.  
- QA:  
  - Silhouette score per risk.  
  - Top-k recall by `graph_label_riesgo`.  
  - Manual review of 30 samples.  

---

## 🚀 Roadmap
- **Sprint 1–2**: Trajectory catalog V2 + `targets.yaml` + generator + QA basic.  
- **Sprint 3–4**: Interventions + seasonality + visualizations + rare-event metrics.  
- **Sprint 5–6**: Uplift modeling + rolling experiments + reproducibility.  
- **Later**: Embeddings validation and retrofill with synthetic text.

---

**Maintainer**: Day-tuh.ai (Eduardo Sánchez Santana, Atul Sehgal)  
