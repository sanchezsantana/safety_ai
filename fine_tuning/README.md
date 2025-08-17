# Fine-tuning

This folder contains all resources related to the **fine-tuning of LLMs** for mining safety applications.  
The current focus is on **Gemma 3B** as the base model, with fine-tuning carried out using **LoRA** and the **Unsloth framework** for efficiency and scalability.

## 🎯 Objectives
- Adapt Gemma 3B to the mining safety domain.
- Generate domain-specific datasets from the YAML safety ontology.
- Implement **instruction-tuning** using **Alpaca-like JSON pairs**.
- Optimize training with LoRA for resource efficiency on Google Colab.
- Store all model checkpoints and logs in **Google Drive**.

## 📂 Folder Structure
```
fine_tuning/
│
├── dataset/       # Training datasets (Alpaca-like JSON format)
├── checkpoints/   # Model checkpoints (saved in Drive, not versioned here)
├── logs/          # Training logs (not versioned here)
└── README.md      # This file
```

## 🛠 Methodology
1. **Dataset Creation**
   - Parse the YAML ontology into structured instruction/response pairs.
   - Convert into Alpaca-like JSON format compatible with Unsloth.

2. **Fine-tuning Process**
   - Base model: **Gemma 3B**.
   - Framework: **Unsloth**.
   - LoRA / QLoRA for parameter-efficient fine-tuning.
   - Training and evaluation run on **Google Colab**.

3. **Hyperparameter Planning**
   - Key hyperparameters (learning rate, batch size, epochs, LoRA rank, etc.) will be planned and documented before training.
   - Experiments will be tracked for reproducibility.

4. **Model Storage**
   - Final models and checkpoints stored in Google Drive.
   - Only configs and scripts kept in this repo.

## 🔒 Notes
- **Checkpoints** and **logs** are excluded from GitHub (see `.gitignore`).
- This repo contains only configs, scripts, and lightweight datasets for reproducibility.
- Ensure that `requirements.txt` includes dependencies such as:
  - `unsloth`
  - `transformers`
  - `peft`
  - `datasets`
  - `torch`
