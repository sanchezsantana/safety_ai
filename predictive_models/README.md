# Predictive Models for Mining Safety

This module is part of the **Safety AI Platform** and contains the predictive modeling line of work.  
Its purpose is to explore and validate **predictive approaches to mining safety events**, integrating both **synthetic data** and **real operational data** when available.  

All predictive outputs are integrated with the **QA Agent**, which acts as the interface for querying, explaining, and visualizing results.

---

## 📌 Objectives
- Provide a **sandbox (Year-0)**: a synthetic year of observations, incidents, and interventions that serves as the baseline for model testing.
- Develop **synthetic data generators** for observations, incidents, and trajectories using the safety ontology and LLMs.
- Implement **four predictive approaches**:
  1. **Regression models**: quantify relationships between leading indicators and the probability of incidents.
  2. **Clustering methods**: discover patterns of degradation and risk evolution.
  3. **Leading indicators**: define and test thresholds/rules for early warning.
  4. **Trajectory-based models**: model sequences of observations and events (HMMs, transformers) to anticipate high-potential incidents.
- Ensure that **all predictions and analyses are queryable via the QA Agent**, enabling real-time interaction and interpretability.

---

## 📂 Structure
```
predictive_models/
│
├── year0/                  # Synthetic Year-0 baseline
├── synthetic_data/          # Synthetic data generation (textual + tabular)
├── regression/              # Classical regression approaches
├── clustering/              # Clustering of degradation patterns
├── leading_indicators/      # Threshold-based leading indicators
├── trajectory_models/       # Sequence models (HMMs, transformers)
└── README.md                # This file
```

---

## 🔗 Integration with the Platform
- **Ontology (YAMLs)**: ensures that observations, incidents, risks, and controls are consistent.
- **VDB (Vector Database)**: embeddings for semantic retrieval of synthetic and real documents.
- **GDB (Graph Database)**: graph schema linking risks, causes, controls, and incidents.
- **LLM fine-tuning**: synthetic data generation and classification pipelines.
- **QA Agent**: serves as the **front-end for predictive models**, allowing:
  - Querying predictions (e.g., “what is the risk of incident in Area A next week?”).
  - Visualizing trajectories, clusters, and risk scores.
  - Explaining outputs in natural language for supervisors and managers.

---

## 🚀 Next Steps
1. Finalize the definition of Year-0 trajectories and balance rules.
2. Create synthetic datasets aligned with the ontology and BowTie/ICAM methodology.
3. Implement baseline regression and clustering models on Year-0.
4. Integrate predictions with the QA Agent for interactive queries and dashboards.
5. Expand towards trajectory models with sequence learning.

---

**Maintainer**: Day-tuh.ai (Eduardo Sánchez Santana, Atul Sehgal)
