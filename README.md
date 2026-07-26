# Wine Quality Prediction Model

A machine learning project that trains a Random Forest Regressor to predict wine quality scores. Experiment tracking is handled with MLflow and data versioning with DVC.

---

## Overview

This project demonstrates a simple MLOps workflow:

- **Data versioning** via DVC, keeping large CSV files out of Git history
- **Experiment tracking** via MLflow, logging parameters and metrics for every training run
- **Scikit-learn** for model training and evaluation

The model is trained on physicochemical properties of wine (acidity, sulphates, alcohol content, etc.) and predicts a quality score on a numeric scale.

---

## Project Structure

```
.
├── data/
│   ├── wine_sample.csv        # Wine dataset (tracked by DVC)
│   └── wine_sample.csv.dvc   # DVC pointer file
├── train.py                   # Main training script
├── utils.py                   # Data loading and feature helpers
├── requirements.txt           # Python dependencies
├── .dvc/                      # DVC configuration
└── .gitignore
```

---

## Requirements

- Python 3.8 or higher
- MLflow tracking server (default: `http://localhost:7006`)

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Getting Started

### 1. Pull the dataset

The CSV is versioned with DVC. Pull it before training:

```bash
dvc pull
```

### 2. Start the MLflow tracking server

```bash
mlflow server --host 0.0.0.0 --port 7006
```

You can override the tracking URI by setting the environment variable:

```bash
export MLFLOW_TRACKING_URI=http://localhost:7006
```

### 3. Train the model

Run with defaults:

```bash
python train.py
```

Or pass custom arguments:

```bash
python train.py \
  --csv data/wine_sample.csv \
  --target quality \
  --n-estimators 100 \
  --max-depth 8 \
  --test-size 0.2 \
  --random-state 42 \
  --experiment wine-prediction \
  --run run-1
```

---

## CLI Arguments

| Argument | Default | Description |
|---|---|---|
| `--csv` | `data/wine_sample.csv` | Path to the input CSV file |
| `--target` | `quality` | Name of the target column |
| `--experiment` | `wine-prediction` | MLflow experiment name |
| `--run` | `run-2` | MLflow run name |
| `--n-estimators` | `50` | Number of trees in the Random Forest |
| `--max-depth` | `5` | Maximum depth of each tree |
| `--test-size` | `0.2` | Fraction of data held out for testing |
| `--random-state` | `42` | Random seed for reproducibility |

---

## Metrics Logged

Each training run logs the following metrics to MLflow:

| Metric | Description |
|---|---|
| `mse` | Mean Squared Error |
| `rmse` | Root Mean Squared Error |
| `r2` | R-squared coefficient of determination |

Parameters logged: `n_estimators`, `max_depth`, `test_size`, `random_state`, `train_rows`, `test_rows`.

---

## Viewing Results

Open the MLflow UI in your browser after starting the server:

```
http://localhost:7006
```

Navigate to the `wine-prediction` experiment to compare runs, parameters, and metrics.

---

## Tech Stack

| Tool | Purpose |
|---|---|
| scikit-learn | Model training and evaluation |
| MLflow | Experiment tracking and run management |
| DVC | Data version control |
| pandas / numpy | Data loading and manipulation |

---

## License

This project is intended for educational and demonstration purposes.
