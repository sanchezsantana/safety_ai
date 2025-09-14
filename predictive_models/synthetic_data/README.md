# Synthetic Data Generation for Mining Safety

This module manages the creation of **synthetic observations and incidents**, aligned with the mining safety ontology.  
Synthetic data enables predictive model development in scenarios with **no data** or **poor data quality**, and complements the Year-0 baseline.

---

## 📌 Objectives
- Generate synthetic safety observations, near misses, hazards, and incidents **consistent with the ontology (risks, causes, controls, roles, tasks)**.  
- Ensure that the **first batch of synthetic data is aligned with Year-0** in distribution and structure.  
- Use a **multi-stage process** to simulate real-world conditions:  
  1. **Seed data with ChatGPT** → curated small set of examples.  
  2. **Expand with Google AI Studio** → large-scale generation.  
  3. **Introduce multiple qualities** of reports (clear, incomplete, noisy, ambiguous).  
  4. **Select mixed-quality data** instead of only the best, to better simulate operational variability.  
  5. **Reconstruct Year-0 merge** → integrate Year-0 dates and structure with synthetic narratives, producing a realistic dataset that simulates client data.  

---

## 📂 Structure
```
synthetic_data/
│
├── seeds/                      # Initial seed data (ChatGPT outputs)
├── expanded/                   # Expanded data (Google AI Studio outputs)
├── curated/                    # Reviewed data with mixed quality
├── merged_with_year0/           # Synthetic data merged with Year-0 timeline
│
├── generators/                 # Scripts to generate synthetic observations and incidents
│   ├── generator_observations.py
│   ├── generator_incidents.py
│   └── generator_audits.py
│
├── prompts/                    # LLM prompt templates for safety data generation
│   ├── observations_prompt.txt
│   ├── incidents_prompt.txt
│   └── audits_prompt.txt
│
├── validation/                 # QA scripts for embeddings and consistency
│   ├── validate_embeddings.py
│   └── validate_tabulation.py
│
├── examples/                   # Sample outputs (JSON, CSV, Markdown narratives)
└── README.md                   # This file
```

---

## 🔗 Integration
- **Ontology (YAMLs)**: provides the base catalogs (risks, controls, causes, incidents, observations, audits).  
- **Year-0**:  
  - Synthetic data generation starts by aligning with Year-0 distributions.  
  - Narratives are merged with Year-0 dates and tabular structure to produce a **realistic dataset**.  
- **Text2Tabular module**:  
  - Receives the merged dataset (narratives + Year-0 structure).  
  - Validates embeddings and tabulation against the original Year-0.  
- **Predictive Models**:  
  - Once validated, structured synthetic data feeds regression, clustering, leading indicators, and sequence models.  
- **QA Agent**:  
  - Enables querying synthetic scenarios, comparing them to Year-0, and explaining predictive outputs.  

---

## 🚀 Workflow
1. **Seed generation** with ChatGPT → produce curated base examples.  
2. **Expand dataset** with Google AI Studio → hundreds/thousands of narratives.  
3. **Include mixed quality**: clear, incomplete, noisy, and ambiguous reports.  
4. **Select diverse-quality data** to simulate real-world reporting conditions.  
5. **Merge with Year-0**:  
   - Add Year-0 timeline and structure (dates, areas, risks, roles, tasks).  
   - Produce a synthetic dataset that mimics client reality.  
6. **Pass dataset to Text2Tabular** for embeddings and reconstruction.  
7. **Validate** by comparing the reconstructed dataset against the original Year-0.  
8. **Expand** later to new scenarios beyond Year-0 once pipeline is validated.  

---

## ✅ Key Benefits
- Provides **synthetic client-like data** for testing predictive models.  
- Includes **report variability** (good, poor, noisy reports) for realism.  
- Guarantees alignment with Year-0 through merging before tabulation.  
- Serves as **ground truth for Text2Tabular validation**.  
- Ensures scalability: from seed data → expanded → curated → merged → validated.  

---

**Maintainer**: Day-tuh.ai (Eduardo Sánchez Santana, Atul Sehgal)  
