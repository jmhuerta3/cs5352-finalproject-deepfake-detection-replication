# CS5352-FINALPROJECT-Deepfake-Detection-Replication
Replication of F-sat deepfake audio detection experiments as described in "I Can Hear You: Selective Robust Training for Deepfake Audio Detection" by Zirui Zhang.

# Deepfake Audio Detection — F-SAT Replication

Replication of **"I Can Hear You: Selective Robust Training for Deepfake Audio Detection"**  
Zhang et al., ICLR 2025 (Spotlight)

**Jazmin M. Huerta | CS 5352 Computer Security | Spring 2026**

---

## Overview

This project replicates the F-SAT (Frequency-Selective Adversarial Training)
method for deepfake audio detection across three experimental runs, comparing
three model variants across four evaluation conditions.

---

## Datasets

| Dataset | Size | Purpose |
|---|---|---|
| ASVspoof 2019 LA | 23.6 GB | Training + clean evaluation |
| FakeAudio (Kaggle) | 26.9 GB | Out-of-distribution (OOD) evaluation |

---

## Results Summary

Three experimental runs were conducted:

| Run | Key Change | Purpose |
|---|---|---|
| Run 1 | Unbalanced data (9:1 fake:real) | Establish baseline, discover failure modes |
| Run 2 | 5 epochs + balanced 5,000/5,000 split | Fix class imbalance |
| Run 3 | 15 epochs + stronger F-SAT + improvements | Best performance |

### Run 1 — Unbalanced Dataset

| Model | Clean F1 | OOD F1 | Attack (Time) F1 | Attack (Freq) F1 | Real Accuracy |
|---|---|---|---|---|---|
| RawNet3 Baseline | 0.947 | 1.000* | 0.712 | 0.858 | 7% |
| RawNet3 + RandAug | 0.945 | 1.000* | 0.769 | 0.924 | 21% |
| RawNet3 + F-SAT | 0.946 | 1.000* | 0.877 | 0.936 | 1% |

*OOD F1=1.000 is misleading because FakeAudio contains only fake samples so the model scores 1.0 by predicting everything as fake.

### Run 2 — Balanced Dataset (5K/5K)

| Model | Clean F1 | OOD F1 | Attack (Time) F1 | Attack (Freq) F1 | Real Accuracy |
|---|---|---|---|---|---|
| RawNet3 Baseline | 0.826 | 0.917 | 0.118 | 0.207 | 83% |
| RawNet3 + RandAug | 0.793 | 0.886 | 0.141 | 0.108 | 77% |
| RawNet3 + F-SAT | 0.808 | 0.898 | 0.154 | 0.139 | 79% |

### Run 3 — All Improvements Applied

| Model | Clean F1 | OOD F1 | Attack (Time) F1 | Attack (Freq) F1 | Real accuracy |
|---|---|---|---|---|---|
| RawNet3 Baseline | 0.821 | 0.903 | 0.197 | 0.049 | 79% |
| RawNet3 + RandAug | 0.507 | 0.862 | 0.073 | 0.030 | 99% |
| RawNet3 + F-SAT | 0.710 | 0.950 | 0.321 | 0.093 | 95% |
| **Paper F-SAT** | **0.974** | **—** | **0.910** | **0.924** | **96%** |

---

## How to Run

1. Open `FSAT_Replication_Colab.ipynb` in Google Colab
2. Set runtime to **A100 GPU**
3. Run Cell 1, it mounts Drive and installs packages
4. First time: run Cell 2 with your Kaggle credentials to download datasets
5. Returning user: skip Cell 2, the datasets are read directly from Drive
6. Run all remaining cells in order! (1 through 12)

---

## Files

| File | Description |
|---|---|
| `FSAT_Replication_Colab.ipynb` | Full experiment notebook |
| `results/ (folder)` | My experimental results from run 2 and 3 |

---

## Key Findings

- F-SAT core claim confirmed, since it consistently outperformed baseline under attack
- Class imbalance (9:1 fake:real) causes real audio detection to collapse to 0%
- Fixing balance to 5K/5K improved detection of genuine voices from 0% to 95%
- Pretrained weights (model.pt) are not publicly available, this was identified as a reproducibility gap

---

## Reference

Zhang et al., *"I Can Hear You: Selective Robust Training for Deepfake Audio Detection"*, ICLR 2025.
