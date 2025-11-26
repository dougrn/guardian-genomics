# Guardian Genomics: Autonomous AI Agent for Precision Medicine 🧬🤖

![Python](https://img.shields.io/badge/Python-3.11%2B-blue)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791)
![Ollama](https://img.shields.io/badge/AI-Ollama%20Local-black)
![Architecture](https://img.shields.io/badge/Architecture-Event%20Driven-orange)

## 📋 Project Overview

**Guardian Genomics** is an autonomous, on-premise ETL and AI pipeline designed to solve the "Genomic Data Deluge" problem.

With over 3,000 new biomedical papers published daily and databases like ClinVar updating weekly, manual monitoring of a personal genome (approx. 600k variants) is impossible. This system automates the surveillance, filtration, and clinical interpretation of genomic risks in real-time, operating entirely locally to ensure strict data privacy.

**Key Engineering Achievement:**
The pipeline successfully reduces the signal-to-noise ratio by **99.8%**, filtering ~56,000 raw candidate signals down to <50 clinically actionable insights per cycle, using a hybrid architecture of deterministic logic and probabilistic AI reasoning.

---

## 🏗️ Architecture & Stack

The system moves beyond simple RAG (Retrieval-Augmented Generation) by implementing a multi-stage validation pipeline:

### 1. The Radar (Ingestion Layer)
* **Sources:** Connects to **PubMed API** (unstructured text) and **ClinVar FTP** (structured data).
* **Strategy:** Implements state-aware fetching (idempotency) to prevent redundant processing and respects API rate limits.
* **Optimization:** Uses Pandas chunking to process large genomic datasets (>150MB) with minimal RAM footprint.

### 2. The Logic Core (Deterministic Layer)
* **False Positive Filtering:** A custom validation engine that cross-references user genotype data against known microarray error patterns.
* **Zygosity Check:** Implements strict biological rules to distinguish *Reference Homozygosity* (Normal) from *Pathogenic Heterozygosity* (Carrier status), eliminating thousands of false alarms common in raw data analysis.

### 3. The Brain (Generative Layer)
* **Engine:** **Llama 3.2 (3B)** running locally via **Ollama**.
* **Task:** Semantic analysis of medical abstracts. The model determines clinical relevance for humans (filtering out non-human studies) and validates the "Direction of Effect" (Risk vs. Protective alleles).
* **Performance:** Optimized for edge inference (~10s per paper).

### 4. Persistence & Delivery
* **Database:** **PostgreSQL** handles relational data and execution logs.
* **Reporting:** Generates executive PDF summaries (Delta Load) and dispatches alerts via **Telegram Bot**.

---

## 🔒 Privacy & Security

* **100% On-Premise:** No genomic data leaves the local server.
* **Local Inference:** All AI processing is performed locally, ensuring zero data leakage to third-party LLM providers.

---

## 🛠️ Setup (Proof of Concept)

*This repository contains the architectural documentation and sanitized core logic. Personal genomic data and production prompts are excluded for privacy.*

```bash
# Clone the repository
git clone https://github.com/dougrn/guardian-genomics.git

# Install dependencies
pip install -r requirements.txt

# Run the orchestrator
python 00_orchestrator.py

"Note: This repository contains a refactored reference implementation of the core pipeline. The actual production system currently running on my server utilizes a set of optimized batch scripts for ease of maintenance and rapid iteration."
