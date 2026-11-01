# Fraud Detection & Explainable Machine Learning

An end-to-end machine-learning demonstration for classifying synthetic financial transactions and explaining model predictions with SHAP. The repository includes data generation, preprocessing, model training, evaluation, explanation utilities and a Streamlit interface.

> **Important:** The included pipeline generates synthetic data. Results from this dataset are for demonstrating the workflow only; they are not evidence of real-world banking or payment-fraud performance and must not be used to make live financial decisions.

## What the project does

1. Generate a configurable synthetic transaction dataset (50,000 rows and a 2% fraud ratio by default).
2. Split the data into training and test sets and preprocess the features.
3. Handle class imbalance using the configured strategy (SMOTE by default).
4. Train a configurable classifier: XGBoost, Random Forest or Logistic Regression.
5. Evaluate predictions using metrics suited to imbalanced classification.
6. Generate global and local feature explanations using SHAP.
7. Save the trained model, preprocessing object, metrics and plots under `models/`.
8. Load the artifacts in a Streamlit app for interactive transaction scoring and model exploration.

## Features

- Configurable synthetic data generation and random seed.
- Standard, MinMax or Robust scaling options in the configuration.
- SMOTE / ADASYN / no-resampling configuration.
- XGBoost, Random Forest and Logistic Regression model options.
- Precision, recall, F1, ROC-AUC, PR-AUC, specificity and confusion-matrix metrics.
- SHAP global feature importance and per-transaction explanations.
- Streamlit transaction analyzer and model dashboard.
- Dockerfile and pytest pipeline tests.

## Pipeline architecture

```text
Synthetic transaction generator
          |
          v
Preprocessing + train/test split
          |
          v
Class-imbalance handling
          |
          v
Configured classifier
          |
          +----> Evaluation metrics
          |
          v
SHAP explainability
          |
          v
Saved model + preprocessor + metrics
          |
          v
Streamlit dashboard
```

## Tech stack

| Area | Technology |
|---|---|
| Language / data | Python, NumPy, Pandas |
| Machine learning | scikit-learn, XGBoost |
| Imbalanced learning | imbalanced-learn (SMOTE / ADASYN) |
| Explainability | SHAP, LIME dependency |
| Visualization | Matplotlib, Seaborn, Plotly |
| Interactive app | Streamlit |
| Model persistence | Joblib |
| Configuration | YAML |
| Testing | pytest |
| Containerization | Docker |

## Run locally (Windows PowerShell)

**Prerequisites:** Python 3.11+ and pip.

1. Clone the repository:

   ```powershell
   git clone https://github.com/visheshmalhotraTHECODER/Fraud-Detection-Explainable-ML.git
   cd Fraud-Detection-Explainable-ML
   ```

2. Create and activate a virtual environment:

   ```powershell
   py -3.11 -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

3. Install dependencies:

   ```powershell
   python -m pip install --upgrade pip
   pip install -r requirements.txt
   ```

4. Train the model and generate artifacts:

   ```powershell
   python train.py
   ```

   The default configuration creates synthetic data and writes model artifacts under `models/`. Training may take several minutes depending on your machine.

5. Launch the dashboard:

   ```powershell
   streamlit run app.py
   ```

6. Open the local URL printed by Streamlit (normally [http://localhost:8501](http://localhost:8501)).

## Configuration

Edit [config.yaml](config.yaml) to change:

- Dataset size, fraud ratio and random seed.
- Test-set size, scaling method and imbalance strategy.
- Model family and hyperparameters.
- SHAP sample count and number of top features.
- Artifact output paths.

## Evaluation

The training script calculates:

| Metric | Why it matters |
|---|---|
| Precision | How many predicted fraud cases are actually fraud in the test data |
| Recall | How many fraud cases in the test data are detected |
| F1 | Harmonic mean of precision and recall |
| ROC-AUC | Ranking quality across classification thresholds |
| PR-AUC | Precision-recall performance, useful for imbalanced data |
| Specificity | How many legitimate cases are correctly identified |

Do not compare these synthetic-data metrics directly with published production fraud-detection results. A realistic evaluation requires a representative, time-aware dataset, leakage checks, threshold selection, calibration and monitoring.

## Tests

```powershell
python -m pytest tests/ -v
```

## Repository layout

```text
app.py                  # Streamlit interface
train.py                # End-to-end training entry point
config.yaml             # Pipeline and model configuration
requirements.txt        # Python dependencies
src/
  data_loader.py        # Synthetic data generation and loading
  preprocessor.py       # Scaling and imbalance handling
  model.py              # Model building, training and evaluation
  explainer.py          # SHAP explanations and plots
tests/
  test_pipeline.py      # Pipeline tests
data/                   # Generated dataset (not committed)
models/                 # Generated model artifacts (not committed)
```

## Limitations and responsible use

- The default data is synthetic and may not represent real transaction behavior.
- High scores on generated data do not establish generalization to unseen real fraud patterns.
- SHAP explains model behavior; it does not establish causality or prove that a prediction is correct.
- Do not upload personal, payment-card or confidential transaction data to this demo.
- A production fraud system would need privacy controls, secure data handling, drift monitoring, human review, model governance and independent validation.

## Attribution and license

This repository adapts **[fraud-detection-xai](https://github.com/tokumag/fraud-detection-xai)** by **[Tokuma Gebissa](https://github.com/tokumag)**. The upstream MIT license and copyright notice are retained in [LICENSE](LICENSE).

**Portfolio maintainer:** [Vishesh Malhotra](https://github.com/visheshmalhotraTHECODER).

Please preserve the upstream copyright and license notice when redistributing this adaptation.
