# Dataset

This folder contains the **training datasets** used for fine-tuning the Gemma 3B model on mining safety tasks.  
The datasets are derived from the **YAML-based safety ontology** and converted into **Alpaca-like JSON format** for compatibility with Unsloth.

## 📌 Dataset Format

Each dataset entry follows the Alpaca-style structure:

```json
{
  "instruction": "Describe the critical controls for working at height.",
  "input": "Risk: Fall from height. Role: Supervisor.",
  "output": "Critical controls include the use of certified harnesses, double anchorage points, inspection before use, and area demarcation."
}
```

- **instruction** → The task or query the model should answer.  
- **input** → Additional context (risk, role, task, or scenario).  
- **output** → The expected domain-specific response.  

## 🛠 Data Generation Workflow

1. **Ontology Parsing**  
   - Extract structured information (roles, tasks, risks, controls, incidents) from YAML ontology files.  

2. **Pair Construction**  
   - Convert the extracted knowledge into instruction-response pairs.  
   - Ensure coverage of all three primary risks:  
     - Fall from height  
     - Falling objects  
     - Contact with electrical energy  

3. **Alpaca-like Conversion**  
   - Transform pairs into the JSON format compatible with Unsloth.  
   - Validate JSON structure to ensure training compatibility.  

## 📂 Folder Structure
```
dataset/
│
├── train.json   # Main training set (Alpaca-like format)
├── val.json     # Validation split
└── README.md    # This file
```

## 🔒 Notes
- **Datasets** in this repo are lightweight samples for reproducibility.  
- Full-scale datasets for fine-tuning are generated dynamically from the ontology and stored in **Google Drive**.  
- Keep sensitive or large-scale data outside of GitHub.  
