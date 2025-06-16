# Anomaly Detection in Network Graphs using Hypothesis Testing

This repository contains the code and methodology for detecting **malicious nodes in computer networks** using a combination of **graph-based structural analysis**, **unsupervised anomaly detection**, and **statistical hypothesis testing**. The proposed hybrid framework achieves **AUC of 0.9978** on real-world network traffic data.

> **Core idea:** Represent the network as a directed, weighted graph, extract local egonet features, and compute anomaly scores using OddBall, LOF, and Graph Deviance Score. Final detection is statistically validated using hypothesis testing.

---

![image](https://github.com/user-attachments/assets/2e04002f-893b-49d8-b848-244295d36fdb)

---


## Table of Contents

- [Overview](#overview)
- [Methodology](#methodology)
- [Dataset](#dataset)
- [Results](#results)
- [Installation](#installation)
- [Contributors](#contributors)
- [References](#references)

---

## Overview

Modern cyber threats often evade traditional rule-based systems. This project tackles the challenge of detecting unknown or subtle anomalies in network traffic **without using labels**.

We:
- Construct a **directed weighted graph** from network traffic flows.
- Extract **egonet features** for each node.
- Detect outliers via:
  - OddBall (structural deviations)
  - Local Outlier Factor (LOF)
  - Graph Deviance Score
- Combine these into a robust anomaly score.
- Validate separation between benign and attack nodes using **statistical hypothesis testing**.

---

## Methodology

1. **Egonet Feature Extraction**:  
   For each IP (node), extract:
   - Number of nodes in egonet (`Ni`)
   - Number of internal edges (`Ei`)
   - Sum of edge weights (`Wi`)
   - Top eigenvalue of weighted adjacency matrix (`λw`)

2. **Outlier Detection**
   - **OddBall**: Fit power-law models between features (e.g., `Ei` vs `Wi`) and compute deviation scores.
   - **LOF**: Measure local density differences using 4D egonet feature vectors.
   - **Graph Deviance**: Z-score based deviation from global statistical norms.

3. **Score Aggregation**  
   Normalize all scores to `[0, 1]` and compute a **final anomaly score** as the average.

4. **Hypothesis Testing**
   - Mann–Whitney U Test
   - T-test
   - Cohen’s d
   - ROC-AUC Curve

---

## Dataset

- **Source**: Network flow dataset from CICIDS2017 or similar.
- **Graph Construction**:
  - Nodes = Unique IP addresses
  - Directed edges = Communication flows
  - Edge weights = Total bytes transferred
- **Size**:
  - Nodes: 33,176
  - Edges: 402,161
  - Malicious Nodes: 11 (highly imbalanced)

---

## Results

| Metric              | Value          |
|---------------------|----------------|
| AUC                 | **0.9978**     |
| Mann–Whitney p-val  | 1.059 × 10⁻⁸   |
| T-test p-val        | 1.14 × 10⁻³²   |
| Cohen's d           | -3.59          |

> These values confirm **strong statistical separation** between benign and attack nodes.

---

## Installation

```bash
git clone https://github.com/your-username/graph-anomaly-detection.git
cd graph-anomaly-detection
pip install -r requirements.txt
```

## Contributors

- **Praanshu Patel**
- **Samarth Sonawane**
- **Akshat Shah**

---

## References

- [1] Leman Akoglu, Mary McGlohon, and Christos Faloutsos. *OddBall: Spotting anomalies in weighted graphs*, PAKDD 2010.
- [2] Markus M. Breunig, Hans-Peter Kriegel, Raymond T. Ng, and Jörg Sander. *LOF: Identifying density-based local outliers*, SIGMOD 2000.
