# Explainability: SHAP + Attention + Similarity

This module integrates explanations across all predictive approaches:  
- **Feature attributions (SHAP)** from regression and tree models.  
- **Temporal importance (attention/TFT)** from sequence models.  
- **Case-based similarity** from trajectory similarity retrieval.  
- **Rule triggers** from leading indicators.  

The goal is to provide a **single, consistent narrative** for each prediction or alert.

---

## 1) Context — Why an explainability layer in mining
- Safety requires **trust**: without clear explanations, predictive models will not be adopted in the field.  
- Different models produce **complementary signals**; this layer integrates them into a unified explanation.  
- Facilitates **audits, post-mortems, and continuous improvement** by documenting why alerts were generated.  
- Bridges the gap between **data science outputs** and **operational decision-making**.  

---

## 2) Architecture — Connections with Year-0, Synthetic Data, Text2Tabular, VDB/GDB, and QA Agent
- **Inputs**: outputs from `regression`, `trajectory_models`, `trajectory_similarity`, and `leading_indicators`.  
- **VDB (BGE-m3)**: provides semantic context to enrich explanations (e.g., nearest risks/controls in ontology).  
- **GDB**: enables navigation of cause–control–risk subgraphs to contextualize alerts.  
- **QA Agent**: front-end to deliver unified explanations interactively.  
- **Dashboards**: provide a **single panel** with risk score + top drivers + similar cases + triggered rules.  

Composition:
```
Scores (regression/trajectory) +
SHAP (drivers) +
Attention (temporal importance) +
Similarity (cases) +
Rules (leading indicators) →
Unified explanation → QA Agent / Dashboards
```

---

## 3) Technical Methodology
- **Evidence normalization**: transform contributions into comparable scales (z-scores, percentiles).  
- **Explanation templates**: structured NLG (per risk/area) to generate consistent narratives.  
- **Consistency checks**: flag contradictions (e.g., SHAP suggests low risk but rules are triggered).  
- **Metrics**:  
  - Explanation latency (time to generate).  
  - **Perceived utility** (feedback from supervisors).  
  - Stability across retrains (robustness of explanations).  

---

## 4) Expected Results
- **Narrative explanations** per alert: *why*, *when*, *similar cases*, and *recommended action*.  
- **Post-event reports**: full traceability of drivers, timelines, and similar past cases.  
- **Audit panel**: interactive dashboard showing feature contributions, temporal signals, and rule triggers.  

---

## 5) Differentiation vs Other Lines
- Not a predictive model, but the **integrative layer** across all approaches.  
- Provides the **operational language** supervisors need to understand model outputs.  
- Reduces the risk of **black-box perception**, enhancing adoption and trust.  
- Enables **cross-model consistency** by combining regression, sequence, similarity, and rules.  

---

## 6) Roadmap
- **MVP**: Templates + SHAP + rules integrated into QA Agent dashboard.  
- **V2**: Add attention-based explanations and similarity retrieval to unify evidence.  
- **V3**: Close the loop with human feedback, define utility metrics, and evolve explanations iteratively.  
