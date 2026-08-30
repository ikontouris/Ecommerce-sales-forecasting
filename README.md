# Time Series Sales Forecasting

This project builds and evaluates time-series forecasting models for daily e-commerce sales.

The goal was to predict the next business week of sales using historical daily sales data.  
The final evaluation used a walk-forward testing setup, where each model predicted one week ahead before the actual values were added back into the training history.

## Dataset

The original dataset comes from real e-commerce sales data.  
For privacy reasons, the public dataset has been anonymized and transformed into a sales index instead of actual revenue values.

The target variable is:

- `sales`: anonymized daily sales index

The final holdout period was:

- `2026-06-29` to `2026-08-22`

## Models Tested

The following models were compared:

- Naive weekly baseline
- ARIMA
- SARIMA
- Simple RNN
- GRU
- Calendar-aware GRU with weekday flags

## Evaluation Method

Models were evaluated using walk-forward validation.

For each week:

1. Train using only data available before that week
2. Predict the next Monday-Saturday period
3. Compare predictions with actual sales
4. Add the actual week to the history
5. Retrain and predict the next week

This avoids future data leakage and better simulates a real forecasting workflow.

## Results

| Model | MAE | RMSE |
|---|---:|---:|
| GRU | 29.05 | 41.19 |
| New GRU 28/7 | 31.51 | 43.25 |
| RNN | 32.22 | 45.51 |
| SARIMA | 37.69 | 52.51 |
| Naive | 38.83 | 51.49 |
| ARIMA | 50.15 | 65.15 |

The GRU model achieved the best overall performance, improving MAE by approximately 25% compared with the naive baseline.

## Interpretation

The neural recurrent models outperformed the classical statistical baselines.  
The standard GRU achieved the lowest error, while the calendar-aware GRU also performed well but did not beat the simpler GRU.

However, daily e-commerce sales are influenced by many external business factors that are not included in this dataset, such as:

- inventory availability
- stockouts
- supplier delays
- cash-flow limitations
- promotions
- holidays
- seasonal demand
- product launches
- large one-off orders

Because of this, the model should be treated as a forecasting support tool, not as a complete business decision system.

## Main Takeaway

Past sales patterns contain useful forecasting signal, but they do not fully explain future sales.  
A GRU model provided the best accuracy in this experiment, while simpler models remain useful as transparent baselines.# Time-Series
