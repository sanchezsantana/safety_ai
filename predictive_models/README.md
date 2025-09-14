# Predictive Models for Mining Safety

This module is part of the **Safety AI Platform** and contains the predictive modeling line of work.  
Its purpose is to explore and validate **predictive approaches to mining safety events**, integrating both **synthetic data** and **real operational data** when available.  

All predictive outputs are integrated with the **QA Agent**, which acts as the interface for querying, explaining, and visualizing results.

---

## 📌 Objectives
- Provide a **sandbox (Year-0)**: a synthetic year of observations, incidents, and interventions that serves as the baseline for model testing.  
  - Predictive models **do not access the predefined trajectories directly**; they only see daily tabular data and must **discover patterns** by themselves.  
- Develop **synthetic data generators** for observations, incidents, and trajectories using the safety ontology and LLMs.  
  - The **first batch of synthetic data** is generated to **replicate Year-0 in textual form** (reports, observations, incidents).  
  - Synthetic narratives are embedded with **BGE (bge-m3)** and **tabulated** via the `text2tabular` module to ensure alignment with the Year-0 structure before generating new scenarios.  
- Implement **six predictive approaches**:  
  1. **Regression models**: quantify relationships between leading indicators and the probability of incidents, using SHAP for feature-level explanations.  
  2. **Clustering methods**: discover unsupervised patterns of degradation, crisis, and recovery.  
  3. **Leading indicators**: define and test threshold/rule-based early warnings.  
  4. **Trajectory-based models**: model sequences of observations/events (HMMs, LSTMs, TFTs) to anticipate high-potential incidents.  
  5. **Trajectory similarity (case-based reasoning)**: retrieve historically similar situations using embeddings (BGE-m3) for intuitive, example-driven explanations.  
  6. **Explainability integration**: unify outputs from all approaches (SHAP, attention, rules, similarity) into **consistent narratives** for operators and managers.  
- Ensure that **all predictions and analyses are queryable via the QA Agent**, enabling real-time interaction, visualization, and interpretability.  

---

## 📂 Structure
```
predictive_models/
│
├── year0/                   # Synthetic Year-0 baseline
├── synthetic_data/           # Synthetic data generation (textual + tabular)
├── text2tabular/             # Module to convert narrative reports into tabular features
├── regression/               # Classical regression approaches + SHAP
├── clustering/               # Clustering of degradation patterns
├── leading_indicators/       # Threshold/rule-based leading indicators
├── trajectory_models/        # Sequence models (HMMs, LSTMs, TFTs)
├── trajectory_similarity/    # Case-based retrieval with embeddings
├── explainability/           # Unified explanation layer (SHAP + attention + similarity + rules)
└── README.md                 # This file
```

---

## 🔗 Integration with the Platform
- **Ontology (YAMLs)**: ensures that observations, incidents, risks, and controls are consistent with BowTie/ICAM.  
- **VDB (Vector Database)**: embeddings for semantic retrieval of synthetic and real documents.  
- **GDB (Graph Database)**: graph schema linking risks, causes, controls, and incidents.  
- **LLM fine-tuning**: synthetic data generation and classification pipelines.  
- **QA Agent**: serves as the **front-end for predictive models**, allowing:  
  - Querying predictions (e.g., “what is the risk of incident in Area A next week?”).  
  - Visualizing trajectories, clusters, and risk scores.  
  - Explaining outputs in natural language with unified narratives for supervisors and managers.  

---

## 🚀 Next Steps
1. Finalize the definition of Year-0 trajectories, noise models, and balance rules.  
2. Generate synthetic datasets aligned with ontology and BowTie/ICAM methodology.  
3. Implement baseline **regression and clustering** models on Year-0.  
4. Test the **text2tabular** pipeline with synthetic narratives to ensure embeddings and tabular features align.  
5. Integrate predictions with the **QA Agent** for interactive queries and dashboards.  
6. Expand towards **trajectory models, similarity-based reasoning, and integrated explainability**.  

---

**Maintainers**: Day-tuh.ai (Eduardo Sánchez Santana, Atul Sehgal)  

