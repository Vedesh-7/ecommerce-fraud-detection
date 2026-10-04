# Project audit — 4 October 2026

## Confirmed identity

The inspected project is an **E-Commerce Fraud Detection** Flask application. It uses `XGBClassifier` and `StackingClassifier`; no CatBoost code or supply-chain workflow was found.

## Confirmed assets

- `app.py`: Flask routes for upload preview, prediction, charts, and performance.
- `model/model.ipynb`: EDA, feature preparation, training, evaluation, and pickle export.
- `fraud_xg.pkl` (343 KB) and `fraud_stack.pkl` (28 MB): serialized models; duplicate copies occur under `model/`.
- Training CSV: 23,634 rows × 16 columns.
- Upload CSV: 4,249 rows × 17 columns; adds `Id`.
- Test CSV: eight labelled model-input rows.
- `templates/` and `static/`: Flask UI and static assets.

## Confirmed training workflow

1. Derive `Address Match` from shipping/billing address equality.
2. Drop identifiers, dates, location, IP, and raw address fields.
3. Factorize payment method, product category, and device.
4. Oversample the minority class.
5. Split, train, evaluate, and pickle both models.

## Critical issue

Oversampling occurs before the split, causing potential train/test leakage. Rebuild and re-evaluate before presenting current accuracy as project performance.

## Other issues

- Requirements are shell commands, not valid dependency lines, and omit `imbalanced-learn`.
- Dataset source/licence is unknown and there is a negative-age record.
- Pickled models are Python/dependency-version sensitive; only load trusted models.
- Model paths rely on the current directory; debug mode is enabled; numeric inputs lack range validation.
- Static pages hard-code rounded results rather than showing validated metrics.
