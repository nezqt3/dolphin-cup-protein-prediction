| ID | Hypothesis |
|---|---|
| **E001** | A different k-mer size may better capture functional patterns in protein sequences. |
| **E002** | Combining multiple k-mer sizes may capture both short and longer sequence motifs. |
| **E003** | Very rare k-mers may introduce noise; filtering them with `min_df` may improve generalization. |
| **E004** | Using `sublinear_tf=True` may reduce the excessive influence of frequently repeated k-mers and improve TF-IDF features. |
| **E005** | The current SGD model may be under- or over-regularized; tuning `alpha` may improve Macro F1. |
| **E006** | Class imbalance may hurt performance on rare labels; using `class_weight="balanced"` may improve their F1 scores. |
| **E007** | A single global threshold is unlikely to be optimal for all 500 labels; per-label thresholds may improve Macro F1. |
| **E008** | Per-label thresholds for rare labels may overfit the calibration set; shrinking them toward the global threshold may improve generalization. |
| **E009** | An improvement on a single validation split may be caused by randomness; evaluating the best configuration across several fixed splits/seeds can test its stability. |
| **E010** | Pretrained protein embeddings such as ESM or ProtBERT may capture sequence information that k-mer TF-IDF cannot represent. |