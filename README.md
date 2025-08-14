# Safety AI Platform for Mining Operations

This repository contains the development of an **AI-powered cognitive safety platform** designed for mining operations.  
It integrates **vector databases (VDB)**, **graph databases (GDB)**, multi-agent QA systems, and **fine-tuned LLMs** to enable predictive and prescriptive safety analytics — even in scenarios with limited or poor-quality historical data.

## 📌 Project Overview
The platform leverages a **safety ontology** built from structured YAML files, which are indexed into a **Vector Database** for semantic retrieval and linked in a **Graph Database** for relational analysis.  
On top of these data layers, **LangChain QA agents** and **fine-tuned LLMs** are deployed to provide advanced reasoning and real-time safety insights.

## 🚀 Main Components
The repository is organized into four core modules:

1. **VDB (Vector Database)**
   - Ontology ingestion from YAML files.
   - Embedding generation (e.g., `BAAI/bge-m3`, `MiniLM`).
   - Persistent storage using ChromaDB.
   - Modular parsers for different ontology entities.

2. **GDB (Graph Database)**
   - Node and relationship creation from ontology.
   - Neo4j integration for querying and visualization.
   - Export and backup utilities.

3. **QA Agent**
   - LangChain-based RAG pipelines connected to the VDB.
   - Role-based and context-specific prompt templates.
   - Retrieval-augmented reasoning for mining safety queries.

4. **Fine-tuning**
   - Domain adaptation of LLMs (e.g., Gemma 2B) using synthetic and real safety data.
   - LoRA/QLoRA for resource-efficient training.
   - Dataset generation based on incidents, near-misses, and observations.

## 📂 Repository Structure
```
safety_ai/
│
├── vdb/
│   ├── ontology/        # YAML ontology files
│   ├── notebooks/       # VDB indexing notebooks
│   └── storage_vdb/     # (excluded) ChromaDB persistent storage
│
├── gdb/
│   ├── ontology/        # Ontology files for GDB
│   ├── scripts/         # ETL scripts for Neo4j
│   └── exports/         # Backups / snapshots
│
├── qa_agent/
│   ├── prompts/         # Prompt templates
│   ├── notebooks/       # QA pipeline notebooks
│   └── logs/            # Agent execution logs
│
└── fine_tuning/
    ├── dataset/         # Training datasets
    ├── checkpoints/     # (excluded) Model checkpoints
    └── logs/            # (excluded) Training logs
```

## ⚙️ Installation
Clone the repository and install dependencies:
```bash
git clone https://github.com/sanchezsantana/safety_ai.git
cd safety_ai
pip install -r requirements.txt
```

## 🗂 Requirements
Main dependencies include:
- `llama-index-core`
- `llama-index-vector-stores-chroma`
- `llama-index-embeddings-huggingface`
- `chromadb`
- `sentence-transformers`
- `langchain`
- `neo4j`
- `torch`
- `pyyaml`
- `pandas`

See [`requirements.txt`](requirements.txt) for the full list.

## 🔒 Repository Status
This repository is **private** and intended for internal development and experimentation.  
No datasets or proprietary models are publicly included.

## 📅 Next Steps
- Complete YAML ontology ingestion into VDB.
- Build the first GDB schema and load initial data.
- Develop LangChain QA agent integration with VDB/GDB.
- Fine-tune Gemma 2B on synthetic safety datasets.

---

**Author:** Eduardo Sánchez Santana  
**Company:** Day-tuh.ai  
**Status:** Active development (2025)
