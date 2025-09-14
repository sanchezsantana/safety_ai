# Trajectory Similarity (Case-Based Reasoning)

This module retrieves **historically similar trajectories** using **embeddings (BGE-m3)** and similarity metrics,  
delivering **case-based explanations**: “this current situation looks like that past one which ended in an incident”.

---

## 1) Context — Why similarity in mining
- Supervisors and operators understand **concrete examples** more easily than abstract risk scores.  
- Enables **transfer of lessons learned** across areas, shifts, and operations.  
- Provides **intuitive explanations** for alerts: “we’ve seen this pattern before, and it led to an incident”.  
- Acts as a **memory system** that grounds predictive models in past reality.  

---

## 2) Architecture — Connections with Year-0, Synthetic Data, Text2Tabular, VDB/GDB, and QA Agent
- **Year-0**: Supplies trajectories with labeled outcomes (event/no-event) for indexing.  
- **Synthetic Data**: Adds diversity of narratives and noise to improve recall.  
- **Text2Tabular**: Converts textual narratives into sequences suitable for embedding.  
- **VDB (BGE-m3)**: Embedding index at window level (e.g., 4-week segments).  
- **GDB**: Provides metadata (risk, control, cause) for semantic filtering.  
- **QA Agent**: Answers “What past cases does this look like?” by retrieving top-k matches with context.  

Flow:
```
Current sequence → embedding per window → kNN search (cosine similarity) →
top-k historical trajectories → explanations + similarity scores
```

---

## 3) Technical Methodology
- **Representation**:  
  - Average or attention pooling of embeddings per day/week.  
  - Aggregates over critical signals (obs_cc, near misses, incidents).  
- **Indexing**:  
  - Simple kNN search initially.  
  - Scalable ANN methods (FAISS, Annoy) as volume grows.  
- **Metrics**: Recall@k, Mean Reciprocal Rank (MRR), coverage per risk/area.  
- **Filters**: Ontology-based (same risk/control/area) for relevance.  
- **QA**: Human-in-the-loop validation of retrieved cases for operational utility.  

---

## 4) Expected Results
- **Top-k list** of similar past trajectories with labels and outcomes.  
- **Explanations** highlighting dominant common factors (e.g., low CC observability, high near misses).  
- **Suggested playbooks** based on interventions from similar past cases.  
- **Dashboards**: carousel of “similar cases” shown alongside current alerts.  

---

## 5) Differentiation vs Other Lines
- The only **memory-based approach** grounded in real past evidence.  
- Complements `regression` and `trajectory_models` by providing **case-level explanations**.  
- Supports `explainability` with intuitive, field-friendly analogies.  
- Unlike `clustering`, it focuses on **specific similar cases**, not group patterns.  

---

## 6) Roadmap
- **MVP**: BGE-m3 embeddings + kNN; top-k retrieval with risk filters.  
- **V2**: FAISS-based indexing + integration with GDB metadata; automatic summaries of retrieved cases.  
- **V3**: Counterfactual reasoning: highlight differences in cases that did *not* end in incidents.  
