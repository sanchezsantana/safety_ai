# Clustering of Degradation Patterns

This module applies **unsupervised learning** to discover hidden patterns in safety observations and incidents.  
It helps identify **degradation trajectories, crisis profiles, and recovery types** that may not be visible in labeled data.  
Clustering is a way to surface **emergent archetypes** that complement supervised models.

---

## 1) Context — Why clustering in mining
- Not all patterns are labeled; **novel modes of failure** may appear in operations.  
- Provides an **empirical taxonomy** of situations that supervisors can understand and act upon.  
- Supports design of **control policies** and safety strategies per cluster type.  
- Bridges between **raw trajectories** and actionable **risk categories**.  

---

## 2) Architecture — Connections with Year-0, Synthetic Data, Text2Tabular, VDB/GDB, and QA Agent
- **Year-0**: Supplies sequences with simulated archetypes (T1–T9). Serves as ground truth for initial clustering validation.  
- **Synthetic Data**: Adds variability (noise, missing values, different report qualities) to ensure cluster robustness.  
- **Text2Tabular**: Converts narrative reports into tabular sequences that can be clustered.  
- **VDB (BGE-m3)**: Provides semantic embeddings to enable **semantic clustering** of similar situations.  
- **GDB**: Supplies graph-derived features (e.g., failed controls, cause–effect chains) to enrich cluster definition.  
- **QA Agent**: Answers questions such as “Which cluster dominates in Area X this quarter?” and shows cluster profiles.  

Pipeline:
```
Year-0 / Synthetic / Text2Tabular → feature/embedding builder →
Clustering (K-Means, HDBSCAN, Spectral) →
Cluster profiles (labels, stats) →
QA Agent / Dashboards
```

---

## 3) Technical Methodology
- **Representation**:  
  - Aggregated windows (7/14/30 days).  
  - Semantic embeddings from VDB.  
  - Normalized features (z-scores).  
- **Algorithms**:  
  - **K-Means** (baseline).  
  - **HDBSCAN** (density-based, discovers outliers).  
  - **Spectral clustering** (leverages graph structure).  
- **Cluster quality**: Silhouette score, Davies–Bouldin index, stability under resampling.  
- **Labeling**: Rules or heuristics to assign interpretable names (e.g., “Pre-crisis with low CC observability”).  
- **External validation**: Correlation with HPOT rates, interventions, recovery speed.  

---

## 4) Expected Results
- **Cluster catalog** with operational descriptions.  
- **Daily/weekly assignment** of each (area, risk) to a most-likely cluster.  
- **Indicators per cluster**: average risk level, time-to-event, common control failures.  
- **Dashboards**: heatmaps showing cluster prevalence by area/time, evolution of cluster mix across the year.  

---

## 5) Differentiation vs Other Lines
- Only approach that can **discover new patterns without labels**.  
- Provides inputs for:  
  - `leading_indicators`: define thresholds/rules per cluster.  
  - `trajectory_models`: clusters as features for sequence models.  
- Complements `trajectory_similarity` by showing **groups of cases**, not individual examples.  
- Feeds into `explainability` by offering **cluster prototypes** as reference archetypes.  

---

## 6) Roadmap
- **MVP**: Implement K-Means + Silhouette; build initial cluster descriptions.  
- **V2**: Add HDBSCAN for density-based clustering, automatic naming, cluster dashboards.  
- **V3**: Multimodal clustering (tabular + embeddings + graph features); online novelty detection to flag new archetypes.  
