# Episode-Level Modeling for Housing Outcomes: A Comparative Prototype

![Status](https://img.shields.io/badge/Status-Active_Prototype-green)
**Focus:** Methodological Structure, Temporal Analysis, & Data Ethics

## Overview
This repository contains a working analytical prototype designed to evaluate **episode-level homelessness service data**. It structures raw administrative logs into a coherent modeling spine to support two complementary analytical workflows:

1.  **The Baseline:** An interpretable regression model (Snapshot Approach).
2.  **The Innovation:** A sequence-based deep learning model (Temporal Approach).

The primary goal is to demonstrate **feasibility** and **architectural rigor**. This is not a production-ready system, but rather a framework for comparing how data representation (static vs. sequential) influences predictive performance in social safety net contexts.

## Problem Context
Public agencies often rely on **aggregated summaries** or **point-in-time counts** to evaluate housing outcomes. While computationally simple, these approaches obscure critical signals contained in the **trajectory** of a client's journey.

* **The Limitation:** Aggregation destroys temporal context. A client with 50 shelter stays in a single month looks identical to a client with 50 stays spread over three years.
* **The Hypothesis:** Preserving the sequence of service events (the "order of operations") improves predictive accuracy and reduces false positives among high-utilizers.

This prototype explores how to operationalize this hypothesis while maintaining strict alignment with real-world administrative data constraints (HMIS).

## Technical Methodology

### 1. The "Shared Spine" Architecture
To ensure a fair comparison, both models ingest the exact same raw data tables (`episodes`, `services`, `clients`). We enforce a **Single Source of Truth** logic:
* **The Anchor:** One row per Episode ID.
* **The Outcome:** Explicitly derived from exit destination codes (binary classification: *Exit to Permanent Housing*).
* **The Split:** The divergence happens only at the feature engineering stage.

### 2. The Dual-Path Experiment

| Feature | Path A: Baseline (Snapshot) | Path B: Deep Learning (Sequence) |
| :--- | :--- | :--- |
| **Data Structure** | Tabular / Aggregated | 3D Tensor (Samples, Timesteps, Features) |
| **Time Representation** | Total Duration (`days_enrolled`) | Inter-event Gaps (`delta_t`) |
| **Service Representation** | Summed Counts (`count_crisis_events`) | Ordered Sequence (`[Event_A, Event_B...]`) |
| **Model Architecture** | Logistic Regression / Random Forest | LSTM / Transformer (Proof of Concept) |
| **Goal** | Interpretability & Benchmarking | Pattern Recognition & Trajectory Sensitivity |

## Data Privacy Notice
This repository utilizes **synthetic/mock data** that structurally mimics real HMIS schemata but contains **no real Personally Identifiable Information (PII)**. All client records, IDs, and service logs are generated for demonstration and architectural validation purposes only.

## Repository Structure
* `/notebooks`: Jupyter notebooks detailing the data prep and model training comparisons.
* `/src`: Helper scripts for tensor construction and data cleaning.
* `requirements.txt`: Python dependencies required to reproduce the analysis.
