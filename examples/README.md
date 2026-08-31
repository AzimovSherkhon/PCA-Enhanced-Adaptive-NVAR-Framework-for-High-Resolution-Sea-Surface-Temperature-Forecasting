# Examples

This folder contains three notebooks demonstrating the SST forecasting workflow.

## Recommended order

Run the notebooks in this order:

```text
1. SST_normalize.ipynb
        ↓
2. Adaptive_NVAR.ipynb
        ↓
3. NG-RC.ipynb
```

## 1. SST_normalize.ipynb

Prepares the SST data for forecasting.

Steps:
1. Load the SST dataset.
2. Normalize the data.
3. Save the processed data for the forecasting notebooks.

Run this notebook first.

## 2. Adaptive_NVAR.ipynb

Demonstrates the PCA-enhanced Adaptive NVAR forecasting method.

Main steps:

```text
Normalized SST
      ↓
PCA
      ↓
Reduced SST dynamics
      ↓
NVAR feature construction
      ↓
Model training
      ↓
Forecast
      ↓
Reconstruct SST field
```

Run the notebook cells from top to bottom and inspect the forecast results.

## 3. NG-RC.ipynb

Demonstrates the NG-RC forecasting approach.

Main steps:

```text
Normalized SST
      ↓
PCA
      ↓
Delay coordinates
      ↓
Nonlinear feature construction
      ↓
Ridge regression
      ↓
Autonomous forecast
      ↓
Reconstruct SST field
```

> **Note:** Long autonomous NG-RC forecasts may become numerically unstable and produce `NaN` or `Inf` values. If this occurs, try a shorter forecast horizon, fewer PCA components, or stronger regularization.

## Quick start

From the project root:

```bash
pip install -r requirements.txt
jupyter notebook
```

Then open the notebooks under `examples/` and run the cells sequentially.
