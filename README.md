# E-Commerce Fraud Detection

A Flask web application that classifies an e-commerce transaction as **Fraudulent** or **Not Fraudulent** using an XGBoost classifier or a stacking classifier.

## Project scope

This project is **e-commerce fraud detection**. It is not a supply-chain-management or CatBoost project.

The application offers a single-transaction prediction form, CSV upload and preview, static dataset charts, and a performance page for the two trained classifiers.

## Models

| Model | Implementation in the notebook |
| --- | --- |
| XGBoost classifier | `xgboost.XGBClassifier()` |
| Stacking classifier | Random Forest + Decision Tree base estimators, with Logistic Regression as final estimator |

## Data

The training CSV, `Fraudulent_E-Commerce_Transaction_Data_2.csv`, has **23,634 rows** and **16 columns**. Its target is `Is Fraudulent` (22,412 non-fraud / 1,222 fraud records).

The model uses nine features: Transaction Amount, Payment Method, Product Category, Quantity, Customer Age, Device Used, Account Age Days, Transaction Hour, and Address Match. `Address Match` is derived as `1` when shipping and billing addresses match, otherwise `0`.

> **Data note:** The supplied data contains an invalid customer age of `-2`, and its source/licence is undocumented. Verify both before publishing the dataset.

## Repository layout

```text
ecommerce-fraud-detection/
├── app.py
├── requirements.txt
├── models/
│   ├── fraud_xg.pkl
│   ├── fraud_stack.pkl
│   └── README.md
├── notebooks/model.ipynb
├── data/README.md
├── templates/
├── static/
├── docs/
├── README.md
└── .gitignore
```

Move `model/model.ipynb` to `notebooks/`, retain one copy of each model in `models/`, and remove duplicate `.pkl` files from the repository root.

## Run locally

```bash
python -m venv .venv
# Windows PowerShell
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python app.py
```

Open `http://127.0.0.1:5000`. The original app expects models beside `app.py`; update it to load from `models/` when adopting this layout.

## Evaluation status — important

The notebook applies `RandomOverSampler()` **before** `train_test_split()`. This can put duplicated minority observations in both train and test sets, creating data leakage and overly optimistic scores.

| Model | Accuracy shown by matrix | Confusion matrix (TN, FP, FN, TP) | Status |
| --- | ---: | --- | --- |
| XGBoost | 95.25% | 4,142; 349; 76; 4,398 | Preliminary only |
| Stacking classifier | 99.89% | 4,481; 10; 0; 4,474 | Preliminary only; likely affected by leakage |

These figures match `Accuracy.txt` after rounding, but they must **not** be promoted as validated performance. Split original data first, then oversample the training partition only. Report precision, recall, F1-score, PR-AUC/ROC-AUC, and a confusion matrix.

## Improvements before publication

- Use valid dependency entries; the original requirements file contains `pip install` commands and omits `imbalanced-learn`.
- Preserve a fixed encoding/preprocessing pipeline with the model instead of relying on hard-coded category IDs.
- Add form validation and set `debug=False` outside development.
- Replace the hard-coded upload URL with Flask `url_for`.
- Confirm data source/licence; otherwise publish only a safe sample.
- Rebuild the training/evaluation workflow to remove leakage, then replace static results pages.

## Responsible use

This is an educational prototype. A model flag should be reviewed by a human and must not be the sole basis for blocking customers or payments.

## Suggested GitHub description

`Flask-based e-commerce fraud detection prototype using XGBoost and a stacking classifier.`
