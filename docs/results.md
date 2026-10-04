# Evaluation results

## Current results (not publication-ready)

The supplied confusion-matrix images show results on an 8,965-row test split of **oversampled data**:

| Model | TN | FP | FN | TP | Accuracy | Fraud precision | Fraud recall | Fraud F1 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| XGBoost classifier | 4,142 | 349 | 76 | 4,398 | 95.25% | 92.65% | 98.30% | 95.39% |
| Stacking classifier | 4,481 | 10 | 0 | 4,474 | 99.89% | 99.78% | 100.00% | 99.89% |

`Accuracy.txt` rounds the test scores to 95% and 99%.

## Why these are not final

The notebook oversamples before splitting. Duplicated minority records can therefore occur in both partitions, leaking training data into the test evaluation.

## Required re-evaluation

1. Stratify and split the original data first.
2. Fit encoders and oversampling only on the training partition.
3. Keep the test partition untouched until final evaluation.
4. Report precision, recall, F1-score, ROC-AUC, PR-AUC, threshold, and confusion matrix.
