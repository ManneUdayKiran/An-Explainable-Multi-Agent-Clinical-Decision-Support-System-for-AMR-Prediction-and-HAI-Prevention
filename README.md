# SentinelAI

## Explainable Multi-Agent AI System for Antimicrobial Resistance and Healthcare-Associated Infection Prevention

SentinelAI is an AI-based clinical decision-support system designed to support the analysis of **Antimicrobial Resistance (AMR)** and **Healthcare-Associated Infections (HAIs)**.

The system combines **Machine Learning, Multi-Agent AI, Explainable AI (XAI), and Retrieval-Augmented Generation (RAG)** to analyze healthcare-related data, identify AMR and HAI risks, detect unusual infection patterns, explain model predictions, and provide evidence-based infection prevention guidance.

> **Important:** SentinelAI is a research and clinical decision-support prototype. It is not intended to replace qualified healthcare professionals or make autonomous medical decisions.

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Objectives](#objectives)
- [Proposed Solution](#proposed-solution)
- [System Architecture](#system-architecture)
- [Data Flow](#data-flow)
- [System Modules](#system-modules)
- [Dataset and Data Processing](#dataset-and-data-processing)
- [Machine Learning Pipeline](#machine-learning-pipeline)
- [Model Selection](#model-selection)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [Running the Project](#running-the-project)
- [Model Training](#model-training)
- [RAG Pipeline](#rag-pipeline)
- [Explainable AI](#explainable-ai)
- [Expected Outputs](#expected-outputs)
- [Evaluation](#evaluation)
- [Security and Privacy](#security-and-privacy)
- [Limitations](#limitations)
- [Future Enhancements](#future-enhancements)
- [Project Status](#project-status)
- [References](#references)
- [Team](#team)

## Overview

Antimicrobial resistance occurs when microorganisms become resistant to antimicrobial drugs, making infections more difficult to treat. Healthcare-associated infections are infections acquired during the process of receiving healthcare and may contribute to complications, longer hospital stays, and increased healthcare costs.

Healthcare environments generate many types of data, including:

- Patient and admission information
- Laboratory and microbiology results
- Antibiotic susceptibility data
- Infection surveillance data
- Genomic or AMR-related information
- Hospital surveillance information
- Medical and infection-prevention guidelines

These sources often use different formats and require preprocessing and integration before they can be analyzed effectively. SentinelAI brings these capabilities together in a modular platform for research and decision support.

## Problem Statement

Healthcare institutions generate large amounts of patient, laboratory, microbiology, infection-related, and surveillance data. However, these data sources can be difficult to analyze together. This makes it challenging to:

- Identify patients at higher infection risk
- Analyze antimicrobial resistance patterns
- Identify unusual infection trends
- Understand the reasons behind AI predictions
- Connect predictions with trusted infection-prevention guidance

SentinelAI addresses these challenges by supporting AMR prediction, HAI risk assessment, potential outbreak signal detection, explainable predictions, and evidence-based guidance retrieval.

## Objectives

### AMR Prediction

Develop and compare machine-learning and deep-learning models using relevant clinical and genomic/AMR data to predict antimicrobial resistance classes:

- Resistant
- Intermediate
- Susceptible

### HAI Risk Prediction

Use relevant patient, clinical, and infection-related information to estimate HAI risk levels:

- Low
- Medium
- High

### Outbreak Detection

Analyze infection surveillance and time-based data to identify unusual patterns that may represent potential outbreak signals. The system flags patterns for investigation; it does not automatically declare an outbreak.

### Explainable AI

Use SHAP as the primary explainability method, with LIME considered where appropriate, to communicate the factors influencing model predictions.

### RAG-Based Decision Support

Retrieve relevant information from trusted medical and public-health documents, such as WHO and CDC guidance, and use the retrieved evidence to support responses.

## Proposed Solution

SentinelAI follows a modular multi-agent architecture:

```text
                         DATA SOURCES
                              |
                              v
                  PREPROCESSING & INTEGRATION
                              |
                              v
                     MULTI-AGENT SYSTEM
                              |
              +---------------+---------------+
              |               |               |
              v               v               v
        AMR Prediction    HAI Risk       Outbreak
           Agent         Prediction       Detection
                            Agent           Agent
              |               |               |
              +---------------+---------------+
                              |
                              v
                       EXPLAINABILITY
                         SHAP / LIME
                              |
                              v
                       RAG KNOWLEDGE
                        WHO / CDC
                              |
                              v
                       DECISION SUPPORT
                              |
                              v
                            OUTPUT
```

## System Architecture

The system is organized into independent components so that data ingestion, prediction, surveillance, explainability, and knowledge retrieval can evolve independently.

1. **Data sources:** clinical, laboratory, microbiology, surveillance, genomic/AMR, and guideline data.
2. **Preprocessing and integration:** validation, cleaning, normalization, feature engineering, and data integration.
3. **Multi-agent orchestration:** routes requests to the appropriate specialist agent.
4. **Specialist agents:** perform AMR prediction, HAI risk prediction, and outbreak signal detection.
5. **Explainability:** produces feature-level explanations for supported model predictions.
6. **Knowledge retrieval:** finds relevant passages from trusted guidance documents.
7. **Decision support:** presents predictions, confidence, explanations, evidence, and recommended next steps for qualified users to review.

## Data Flow

```text
Raw data -> Validation -> Cleaning and normalization -> Feature engineering
         -> Specialist agents -> Explanations and evidence retrieval
         -> Decision-support output -> Human review
```

## System Modules

### 1. Data Sources

Potential sources include structured clinical records, laboratory results, microbiology and antimicrobial susceptibility records, infection surveillance records, genomic/AMR datasets, and trusted public-health guidance.

### 2. Data Preprocessing and Integration

Typical processing steps include schema validation, missing-value handling, duplicate detection, categorical encoding, numerical scaling, temporal alignment, feature engineering, and dataset integration.

### 3. Multi-Agent System

The orchestration layer coordinates specialized agents and combines their outputs into a traceable decision-support response.

### 4. AMR Prediction Agent

Predicts the likely antimicrobial susceptibility category for a relevant organism, antimicrobial, or clinical context. Outputs should include the prediction, confidence or uncertainty information, and the features used by the model.

### 5. HAI Risk Prediction Agent

Estimates the likelihood of HAI risk using available patient, admission, clinical, device, laboratory, and infection-related features.

### 6. Outbreak Detection Agent

Analyzes time-based and location-based infection patterns using statistical or machine-learning methods. Alerts are signals for epidemiological investigation, not clinical conclusions.

### 7. Explainability Module

Uses SHAP and, where appropriate, LIME to show which features most influenced a prediction. Explanations are intended to improve transparency and support expert review.

### 8. RAG Knowledge Agent

Indexes trusted reference documents, retrieves relevant passages for a query, and returns evidence-linked context for decision support. Source provenance should be retained with every generated response.

### 9. Decision Support

Presents agent results in a human-readable format, including risk category, prediction confidence, explanation, retrieved evidence, limitations, and suggested review actions.

## Dataset and Data Processing

The repository includes `Dataset.zip` for project data. Before using any dataset, confirm its license, provenance, de-identification status, schema, and suitability for the intended evaluation.

Recommended processing stages:

1. Inspect schemas and data quality.
2. Remove or protect direct and indirect identifiers.
3. Standardize clinical, microbiology, and temporal fields.
4. Handle missing values and class imbalance explicitly.
5. Split data by patient and, where appropriate, by time to reduce leakage.
6. Version the processed datasets and record preprocessing decisions.

## PDF Data Extraction

Guideline PDFs may require text extraction, cleaning, chunking, metadata tagging, and quality checks before indexing. Tables, references, page numbers, document titles, publication dates, and source URLs should be preserved where possible so retrieved evidence can be traced back to the original document.

## Machine Learning Pipeline

```text
Data ingestion
    -> Validation and preprocessing
    -> Feature engineering
    -> Patient/time-aware train-validation-test split
    -> Model training
    -> Hyperparameter tuning
    -> Evaluation and calibration
    -> Explainability analysis
    -> Model packaging
    -> Agent integration
```

The pipeline should prevent patient-level leakage, track preprocessing artifacts, compare against simple baselines, and record model versions and evaluation datasets.

## Model Selection

Candidate models may include:

- Logistic regression and other interpretable baselines
- Decision trees and random forests
- Gradient-boosted models
- Support-vector machines
- Neural networks where dataset size and validation support their use
- Time-series or anomaly-detection methods for surveillance data

Model selection should consider discrimination, calibration, interpretability, robustness, subgroup performance, computational cost, and clinical relevance rather than accuracy alone.

## Technology Stack

The intended technology areas are:

- **Python:** data processing and machine learning
- **Scikit-learn / XGBoost:** conventional predictive models
- **SHAP / LIME:** explainability
- **PyTorch or TensorFlow:** optional deep-learning models
- **LangChain or LlamaIndex:** optional RAG orchestration
- **FAISS, Chroma, or another vector store:** document retrieval
- **FastAPI:** backend API layer
- **React:** frontend interface
- **Docker:** reproducible development and deployment

The final implementation should document the exact versions used by the project.

## Project Structure

The implementation can be organized as follows:

```text
SentinelAi/
├── data/                 # Raw and processed data references
├── notebooks/            # Exploration and experiments
├── src/
│   ├── agents/           # AMR, HAI, and outbreak agents
│   ├── data/             # Ingestion and preprocessing
│   ├── explainability/   # SHAP and LIME integrations
│   ├── models/           # Training and inference code
│   ├── rag/              # Document indexing and retrieval
│   └── api/              # Backend services
├── frontend/             # User interface
├── tests/                # Unit and integration tests
├── Dataset.zip
└── README.md
```

This is a target structure; directories should be added as the implementation develops.

## Installation

Create and activate a Python environment, then install the dependencies defined by the project’s dependency file:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

When frontend code is present, install its dependencies from the frontend directory:

```powershell
cd frontend
npm install
```

## Environment Variables

Do not commit secrets, patient data, or private service credentials. A future implementation may use variables such as:

```text
MODEL_PATH=
VECTOR_STORE_PATH=
LLM_API_KEY=
RAG_COLLECTION=
API_HOST=127.0.0.1
API_PORT=8000
```

Store local values in an untracked `.env` file and provide a `.env.example` containing placeholders.

## Running the Project

Run commands appropriate to the implemented modules. A typical local workflow is:

```powershell
# Backend
uvicorn src.api.main:app --reload

# Frontend, in a separate terminal
cd frontend
npm run dev
```

The exact commands may change as the implementation is added.

## Model Training

Training workflows should document the dataset version, feature set, split strategy, model configuration, metrics, random seeds, and generated artifacts. Trained models should be evaluated on held-out data and checked for calibration and clinically meaningful subgroup behavior before integration.

## RAG Pipeline

A typical RAG workflow is:

1. Collect trusted guidance documents.
2. Verify source, publication date, and licensing.
3. Extract and clean document text.
4. Chunk text while preserving document metadata.
5. Generate embeddings and store them in a vector index.
6. Retrieve relevant passages for each query.
7. Generate a response grounded in retrieved evidence.
8. Display citations, source metadata, and uncertainty to the user.

The RAG component must not be treated as a substitute for current institutional policy or professional judgment.

## Explainable AI

For each supported prediction, the system should expose:

- The prediction and confidence or uncertainty
- The most influential features
- The direction and relative contribution of those features
- The model and data version
- Known limitations and applicability constraints

Explanations describe model behavior; they do not establish causation.

## Expected Outputs

Potential outputs include:

- AMR susceptibility prediction
- HAI risk category
- Potential outbreak signal
- Feature-level explanation
- Retrieved guideline evidence
- Confidence and uncertainty information
- Human-review recommendations

## Evaluation

Evaluation should include:

- Accuracy, precision, recall, F1-score, and AUROC where appropriate
- Calibration and decision-curve analysis where relevant
- Confusion matrices and class-specific performance
- Temporal and patient-level holdout testing
- Subgroup and fairness analysis
- Outbreak signal precision and lead-time analysis
- Retrieval relevance and citation-grounding checks
- Expert review of explanations and decision-support usability

## Security and Privacy

Healthcare data must be handled according to applicable laws, regulations, institutional policies, and dataset licenses. The implementation should apply access control, encryption, audit logging, de-identification or anonymization, secure secret management, least-privilege access, and retention controls. Real patient data should not be placed in source control or used in development without appropriate authorization.

## Limitations

- Data quality and representativeness may limit model performance.
- Retrospective performance does not establish prospective clinical benefit.
- Predictions may be affected by dataset shift and local epidemiology.
- Correlation-based explanations do not prove causation.
- Outbreak signals require expert epidemiological investigation.
- Retrieved guidance may become outdated and must be versioned and reviewed.
- The prototype is not an autonomous diagnostic or treatment system.

## Future Enhancements

- Prospective and multi-site validation
- Integration with standards-based healthcare interfaces
- Improved temporal and genomic AMR modeling
- Human-in-the-loop feedback and audit workflows
- Monitoring for drift, calibration, and subgroup performance
- Role-based dashboards for infection prevention teams
- Stronger document provenance and guideline update monitoring
- Privacy-preserving and federated learning research

## Project Status

This repository currently contains the project dataset and documentation. The software architecture and workflows described here define the intended research direction; implementation details will be added as development progresses.

## References

- [World Health Organization: Antimicrobial resistance](https://www.who.int/health-topics/antimicrobial-resistance)
- [Centers for Disease Control and Prevention: Healthcare-associated infections](https://www.cdc.gov/healthcare-associated-infections/)
- [SHAP documentation](https://shap.readthedocs.io/)
- [LIME documentation](https://lime-ml.readthedocs.io/)

Reference documents used by the RAG pipeline should be recorded with their title, publisher, version or publication date, and source URL.

## Team

Add project contributors and their responsibilities here.

---

SentinelAI is intended for research, education, and clinical decision-support prototyping. It does not provide medical advice and must not be used to make autonomous clinical decisions.
