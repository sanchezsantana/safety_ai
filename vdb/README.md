# VDB

This folder contains the resources and notebooks for building the **Vector Databases (VDBs)** that support semantic retrieval in the Safety AI platform.  
VDBs are created with **LlamaIndex (core v0.12.20)** and powered by **Qdrant** as the vector store backend.  
All persistent storage is maintained in **Google Drive** to ensure continuity across Colab sessions.

## 🧩 VDB Architecture

We maintain two main VDBs, each serving a different purpose:

1. **Ontology VDB**
   - Built from YAML ontology files (e.g., risks, controls, causes, BowTie structures).
   - Each YAML is indexed separately with **specific metadata** attached to each `TextNode`.
   - Embedding model: **BAAI/bge-m3** (multilingual, state-of-the-art semantic retrieval).
   - Metadata consistency: a single version of the embedding model is applied across all ontology indexes to ensure comparability.
   - Persistent storage: Qdrant collections stored in **Google Drive**.

2. **Documents VDB**
   - Built from mining safety manuals, procedures, and training documents.
   - Uses **LlamaParse** for semantic chunking (section-based parsing rather than fixed-length splits).
   - Chunk-level metadata is enriched using a **fine-tuned LLM** to infer roles, risks, and relevance to BowTie models.
   - Embedding model: **always BAAI/bge-m3**, ensuring alignment with the ontology VDB.
   - Persistent storage: Qdrant collections stored in **Google Drive**.

## ⚙️ Tech Stack

- **Framework:** [LlamaIndex](https://www.llamaindex.ai/) `core==0.12.20`
- **Vector Store:** [Qdrant](https://qdrant.tech/)  
  - Chosen for scalability, advanced filtering, and high-performance ANN search.
  - Supersedes Chroma, which was used in early prototypes.
- **Embeddings:**  
  - Standardized: `BAAI/bge-m3` for all VDBs (ontology and documents).  
- **LLM Fine-Tuning:**  
  - Applied only for **metadata generation** on document chunks, not for embeddings.
- **Storage:**  
  - **Google Drive** is mounted in Colab (`/content/drive/`) to persist Qdrant indexes and related metadata.

## 📂 Folder Structure

```
vdb/
│
├── ontology/        # YAML ontology files to be indexed
├── notebooks/       # Colab notebooks for VDB creation and querying
├── storage_vdb/     # Local mountpoint, synced with Google Drive
└── README.md        # This file
```

## 🚀 Workflow

### 1. Ontology Indexing
- Parse each YAML with a dedicated parser.
- Convert YAML entries into `TextNodes` with enriched metadata.
- Generate embeddings with **bge-m3**.
- Store vectors in **Qdrant** (persisted in Google Drive).

### 2. Document Indexing
- Load PDFs/Word documents.
- Use **LlamaParse** to extract semantically meaningful chunks.
- Enrich chunks with metadata using a fine-tuned LLM.
- Generate embeddings with **bge-m3**.
- Store vectors in **Qdrant** (persisted in Google Drive).

### 3. Querying
- Retrieval via LlamaIndex query engine.
- Hybrid search: semantic similarity + metadata filtering.
- RAG integration with QA agents.

## 🔒 Notes
- Ontology VDB is versioned; embedding model version must remain consistent across batches.
- Documents VDB may contain sensitive information → store only in private repositories/instances.
- Always back up Qdrant collections in **Google Drive** before re-indexing.
