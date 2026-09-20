# Findings

This document contains conclusions learned from completed experiments.

Do not record experiment ideas here.
Ideas belong in `ideas.md`.
Raw experiment results belong in `experiments.csv`.

---

## Current Best

**Experiment:** E000  
**Branch:** `main`  
**Model:** TF-IDF k-mer + One-vs-Rest SGD  
**Validation metric:** Macro F1  
**Macro F1:** TBD

This is the current baseline. All new experiments should start from this version.

---

## Confirmed Findings

No confirmed findings yet.

A finding should be added only after an experiment provides enough evidence to support it.

Example:

### F001 — Per-label thresholds improve Macro F1

**Evidence:** E007  
**Result:** Macro F1 increased from `0.4213` to `0.4381`.

**Conclusion:**  
Using an individual threshold for each label performs better than using one global threshold.

**Decision:**  
Keep per-label threshold optimization in the main pipeline.

---

## Rejected Hypotheses

No rejected hypotheses yet.

Example:

### E006 — `class_weight="balanced"`

**Result:**  
Macro F1 decreased from `0.4381` to `0.4290`.

**Conclusion:**  
Class balancing did not improve the current pipeline.

**Decision:**  
Do not use `class_weight="balanced"` in the current configuration.

---

## Observations

### Dataset

- Multilabel classification problem.
- 500 target labels.
- Labels are highly imbalanced.
- Macro F1 is the primary competition metric.

### Validation

- All experiments must use the same validation split.
- All experiments must use the same Macro F1 implementation.
- Leaderboard score must not replace local validation.

### Current Pipeline

```text
Protein sequence
      ↓
k-mer TF-IDF
      ↓
One-vs-Rest SGD
      ↓
Probabilities
      ↓
Threshold calibration
      ↓
Binary predictions
      ↓
Macro F1