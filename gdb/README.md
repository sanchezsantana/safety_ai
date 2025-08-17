# Graph Database (GDB)

This folder contains the scripts and ontology resources used to build and manage the **Graph Database (GDB)** for the Safety AI platform.  
The GDB is implemented using **Neo4j AuraDB**, a fully managed cloud database service, and is accessed directly from Google Colab.

## 🌐 Why AuraDB?

- **Cloud-based**: No need to install or maintain Neo4j locally.  
- **Free tier**: AuraDB Free supports up to ~200,000 nodes and ~400,000 relationships, which is sufficient for prototyping.  
- **Automatic backups and updates**: Fully managed by Neo4j.  
- **Pause/resume**: Database pauses when inactive, preserving data.

## ⚙️ Connecting to Neo4j AuraDB from Colab

### 1. Get your credentials
When you create an AuraDB instance, you receive:
- A **URI** like:  
  ```
  neo4j+s://xxxxxx.databases.neo4j.io
  ```
- A **username** (default: `neo4j`).  
- A **password** (shown only once at creation – save it securely).  

### 2. Install the Python driver
In a Colab cell:
```python
!pip install neo4j
```

### 3. Connect to the database
Use the official Neo4j driver:

```python
from neo4j import GraphDatabase

# Replace with your AuraDB credentials
URI = "neo4j+s://xxxxxx.databases.neo4j.io"
AUTH = ("neo4j", "your_secret_password")

driver = GraphDatabase.driver(URI, auth=AUTH)
driver.verify_connectivity()
print("✅ Connection established successfully")
```

### 4. Run queries
You can create sessions and run **Cypher queries** to insert ontology data (nodes, relationships) or explore the graph.

```python
def test_query():
    with driver.session() as session:
        result = session.run("RETURN 'Hello from Neo4j' AS message")
        for record in result:
            print(record["message"])

test_query()
```

## 📘 Ontology for GDB

The GDB structure is defined by a **dedicated ontology YAML file**:  

- `99_schema_neo4j_v3.yaml` → Defines the classes, relationships, and schema rules for creating the Graph Database.  

This file ensures consistency between the **safety ontology** and the Neo4j data model.  
ETL scripts in the `scripts/` folder use this schema as a reference to generate nodes and relationships.

## 📂 Folder Structure

```
gdb/
│
├── ontology/        # Ontology files adapted for GDB
│   └── 99_schema_neo4j_v3.yaml   # GDB schema definition
│
├── scripts/         # ETL and data loading scripts for Neo4j
├── exports/         # Snapshots / backups of graph
└── README.md        # This file
```

## 🔒 Notes
- Do **not** commit your AuraDB credentials to GitHub.  
- For sensitive projects, use environment variables or secret managers.  
- AuraDB Free tier is recommended for learning and prototyping. For production, upgrade to a paid tier.  
