# Leading Indicators & Rule-Based Alerts

This module codifies **early-warning rules** (thresholds, trends) based on safety observations, critical controls, and near misses.  
It produces **explainable alerts** that can be quickly implemented and communicated to operations.

---

## 1) Context — Why rules/indicators in mining
- Operations demand **simple, actionable signals** (traffic-light style) that supervisors can use immediately.  
- Provides a **fast MVP** to validate adoption with field teams before advanced ML models are ready.  
- Serves as a **baseline reference** against which predictive models can be compared.  
- Complements ML approaches in contexts with **low data volume or coverage gaps**.  

---

## 2) Architecture — Connections with Year-0, Synthetic Data, Text2Tabular, VDB/GDB, and QA Agent
- **Year-0**: Defines expected ranges and post-event windows for rule calibration.  
- **Synthetic Data**: Used to test robustness of rules under **different data qualities** (clear, incomplete, noisy reports).  
- **Text2Tabular**: Converts narrative reports into structured signals that feed rules.  
- **VDB (BGE-m3)**: Enables similarity-based rules (e.g., spike in distance from “safe situation” cluster).  
- **GDB**: Supplies control topology (links between causes and barriers) to design structural rules.  
- **QA Agent**: Explains *why* an alert was triggered, showing the active rule and values.  

Pipeline:
```
Year-0 / Synthetic / Text2Tabular → signal builder →
rule engine (thresholds, trends, logic) →
alerts + explanations →
QA Agent / Dashboards
```

---

## 3) Technical Methodology
- **Types of rules**:  
  - Absolute thresholds (e.g., > 5 near misses in 14 days).  
  - Relative changes (Δ% week-over-week).  
  - Slopes (minimum trend slope).  
  - Ratios (`obs_cc / obs_general`).  
  - Runs/streaks of unsafe days.  
  - Post-event monitoring windows.  
- **Composition**: logical AND/OR, scoring weights, **cooldown periods** to avoid alert spam.  
- **Calibration**: Grid search using Year-0 to balance recall vs alert-fatigue.  
- **Metrics**:  
  - Recall at top-N alerts/day.  
  - Precision of rule triggers.  
  - PR-AUC against future event labels.  
  - Expected cost (penalizing missed high-potential events).  

---

## 4) Expected Results
- **Traffic-light dashboard** (green/yellow/red) by area & risk, with textual explanations.  
- **Alert registry**: timestamps, triggered rule, values.  
- **Alert-fatigue curves**: tradeoff between number of alerts/day and recall.  
- **Playbooks**: recommended interventions linked to each rule trigger.  

---

## 5) Differentiation vs Other Lines
- Simplest and most communicable approach; requires **no training data**.  
- Provides a **baseline for adoption** while ML models mature.  
- Feeds into `explainability` with deterministic rule-based reasons.  
- Unlike `regression` or `trajectory_models`, it does not estimate probabilities — it generates **clear binary alerts**.  

---

## 6) Roadmap
- **MVP**: Define 8–12 core rules; implement basic traffic-light dashboard.  
- **V2**: Adaptive rules (EWMA, CUSUM); prioritize alerts by cost/impact.  
- **V3**: Automate intervention orchestration and verification via QA Agent.  
