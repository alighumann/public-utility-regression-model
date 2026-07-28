# public-utility-regression-model
# Revenue Forecasting — H&S (Hotazel Steam)

Two seasonal regression models built to forecast monthly revenue for H&S, using seasonal dummy variables and interaction terms. Models are trained on 2011–2013 data and tested on held-out 2014 data, then compared on out-of-sample forecast accuracy (MAPE) and in-sample fit (adjusted R²).

## Overview

H&S sells steam whose demand is highly seasonal — heating in winter, cooling in summer. A single regression line on `production` alone can't capture this. This project builds dummy variables and interaction terms so that each season can have its own **intercept** (baseline revenue) and its own **slope** (how strongly production converts to revenue), then picks the model that forecasts 2014 best.

## Data

`AICPA_regressionAnalysisData.csv` — 48 monthly observations (2011–2014).

| Column | Description |
|---|---|
| `type` | `dt4training` (2011–2013) or `dt4testing` (2014) |
| `date` | Month-end date |
| `revenue` | Monthly revenue (target) |
| `production` | Units produced |
| `coolDD` | Cooling degree days |
| `heatDD` | Heating degree days |

The 2011–2013 rows are used to fit the models; 2014 is held out to measure forecast accuracy on data the models never saw.

## Approach

1. **Dummy variables** — `winter_DV` = 1 in Dec/Jan/Feb, `summer_DV` = 1 in Jun/Jul/Aug, 0 otherwise. A dummy shifts the intercept for that season.
2. **Interaction terms** — `production × dummy`, so each season can also change the slope on production.
3. **Fit, forecast, compare** — build two models, forecast 2014 revenue with each, and compare using MAPE and adjusted R².

## Models

**Model 1 — winter only**

```
revenue = β0 + β1·production + β2·winter_DV + β3·winter_interaction
```

**Model 2 — winter + summer** (recommended)

```
revenue = β0 + β1·production + β2·winter_DV + β3·summer_DV
             + β4·winter_interaction + β5·summer_interaction
```

Model 2 fitted equation (spring/fall is the baseline):

- Spring/Fall: `revenue = 4,060,734.59 + 18.87·production`
- Winter: `revenue = 5,427,514.34 + 27.67·production`
- Summer: `revenue = 5,452,753.63 + 12.59·production`

Interpretation: each unit of production earns the most in winter (27.67), a baseline amount in spring/fall (18.87), and the least in summer (12.59) — consistent with winter steam serving high-value heating demand.

## Results

| Model | Adjusted R² | MAPE (2014) |
|---|---|---|
| Model 1: production + winter | 0.75 | 0.159 |
| Model 2: production + winter + summer | 0.76 | 0.149 |

**Model 2 is recommended** — lower forecast error (~14.9% vs ~15.9%) and higher adjusted R². Adding the summer terms improves out-of-sample accuracy.

> Note: these figures are read directly from the notebook output and reflect this specific dataset/split. Re-run the notebook to reproduce them.

## Requirements

```
numpy
pandas
matplotlib
statsmodels
```

Install with:

```bash
pip install numpy pandas matplotlib statsmodels
```

## Usage

1. Place `AICPA_regressionAnalysisData.csv` in the same directory as the notebook.
2. Open `Quiz 8.ipynb` in Jupyter or Google Colab.
3. Run all cells top to bottom to reproduce the data prep, both models, the forecast comparison, and the plots.

## Notebook structure

- **Step 1** — Import libraries and load the data
- **Step 2** — Convert the date column to datetime and extract the month
- **Step 3** — Create the seasonal dummy variables
- **Step 4** — Create the production × dummy interaction terms
- **Step 5** — Split into training (2011–2013) and testing (2014)
- **Model 1** — Fit, interpret, and test on 2014
- **Model 2** — Fit, interpret, and test on 2014
- **Comparison** — Plot actual vs. both forecasts and summarize metrics
- **Final plot** — Visualize Model 2's three seasonal regression lines
