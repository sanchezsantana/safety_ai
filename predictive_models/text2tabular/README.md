# Text2Tabular: Embeddings and Structured Extraction

This module transforms **synthetic narratives (text)** into **structured tabular data** aligned with the mining safety ontology.  
It is designed to simulate real-world conditions where client data often exists only as **textual reports**, not structured tables.

---

## 📌 Objectives
- Take as input **synthetic narratives** (observations, incidents, audits) of **mixed quality**.  
- Use **embeddings (BGE-m3)** and NLP pipelines to extract ontology-based fields:  
  - `fecha`, `área`, `rol`, `tarea`, `riesgo_asociado`, `control_crítico`, `causa`, `tipo_incidente`, `tipo_observacion`, `detalle_textual`.  
- Produce a **tabular dataset** that can be compared directly with **Year-0 structured data**.  
- Validate the accuracy and robustness of embeddings + extraction on **reports of different quality levels** (clear, incomplete, noisy).  
- Demonstrate capability to handle **text-only inputs** in real mining safety deployments.  

---

## 📂 Structure
```
text2tabular/
│
├── inputs/                     # Synthetic narratives (JSON, TXT) from synthetic_data/
├── outputs/                    # Tabulated results (CSV, Parquet)
│
├── embeddings/                 # Embedding utilities (BGE-m3 integration)
│   ├── embedder.py
│   └── retriever.py
│
├── extraction/                 # Parsing & classification logic
│   ├── risk_classifier.py
│   ├── control_matcher.py
│   ├── cause_extractor.py
│   └── tabulation_pipeline.py
│
├── validation/                 # QA scripts against Year-0
│   ├── compare_with_year0.py
│   ├── metrics.py
│   └── error_analysis.py
│
└── README.md                   # This file
```

---

## 🔗 Integration
- **Input**: Curated synthetic data merged with Year-0 (`synthetic_data/merged_with_year0/`).  
- **Embeddings (BGE-m3)**: Encode narratives for semantic similarity and ontology mapping.  
- **Extraction pipeline**:  
  - **NER (Named Entity Recognition)** or rule-based parsing for fields like `área`, `rol`, `tarea`.  
  - **Embedding similarity search** to map free text to risks, controls, and causes.  
  - **Classifier layer** (logistic regression, transformer fine-tuned, or rules) for incident/observation types.  
- **Output**: Tabular CSV/Parquet with reconstructed fields.  
- **Validation**: Compare reconstructed dataset vs Year-0 baseline:
  - Accuracy/recall per field (riesgo, control, causa).  
  - Distributional similarity (ratios, counts).  
  - Robustness under noisy inputs.  

---

## 🚀 Workflow
1. **Load narratives** from `synthetic_data/merged_with_year0/`.  
2. **Embed text** using **BGE-m3** → obtain vectors.  
3. **Ontology mapping**:  
   - Match embeddings against ontology catalogs (`riesgos, controles, causas, roles, tareas`).  
   - Select top-k matches with confidence scores.  
4. **Parse metadata**:  
   - Extract dates, areas, roles, tasks from structured hints in the narratives.  
   - Apply NER or regex for simple fields (e.g., fechas).  
5. **Reconstruct tabular record** with all fields.  
6. **Validation**:  
   - Compare tabular reconstruction vs Year-0 original.  
   - Compute metrics (precision, recall, F1, PR-AUC).  
   - Run error analysis on mismatches.  
7. **Iterate**: refine extraction rules and ontology mapping to improve quality.  

---

## ✅ Key Best Practices
- **Flat metadata + ontology anchors**: keep extracted fields traceable to ontology IDs (graph_id, graph_label).  
- **Error-tolerant extraction**: handle missing or noisy text by marking fields as `unknown` instead of forcing labels.  
- **Confidence thresholds**: keep a `confidence_score` per extracted field; use thresholds to balance precision/recall.  
- **Human-in-the-loop**: allow manual review of low-confidence extractions (critical for safety domain).  
- **Comparisons vs Year-0**: always validate reconstruction against a known baseline to measure progress.  
- **Scalable design**: modular pipeline (embedding → extraction → validation) so that future LLMs or embedding models can be swapped easily.  

---

## 🎯 Next Steps
- Implement minimal pipeline:
  - `embedder.py` → encode narratives with BGE-m3.  
  - `tabulation_pipeline.py` → extract risks, controls, causes, and map to ontology.  
  - `compare_with_year0.py` → compute validation metrics.  
- Test pipeline with **mixed-quality synthetic reports**.  
- Tune thresholds and rules to handle ambiguity.  
- Integrate into **QA Agent** for interactive querying and error review.  

---

**Maintainer**: Day-tuh.ai (Eduardo Sánchez Santana, Atul Sehgal)  
