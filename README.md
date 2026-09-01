# Time Series Sales Forecasting

This project builds and evaluates time-series forecasting models for daily e-commerce sales.

The goal is to predict the next business week of sales using historical daily sales data.  
The final evaluation uses a walk-forward testing setup, where each model predicts one week ahead using only the data available before that week.

## Dataset

The original dataset comes from real e-commerce sales data.

For privacy reasons, the public dataset has been anonymized and transformed into a sales index instead of actual revenue values.

The target variable is:

- `sales`: anonymized daily sales index

The final holdout period is:

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

For each forecast week:

1. Train using only data available before that week
2. Predict the next Monday-Saturday period
3. Compare predictions with the actual sales values
4. Move to the next week and repeat the process

This setup avoids future data leakage and better simulates a real forecasting workflow.

## Results

| Model | MAE | RMSE |
|---|---:|---:|
| New GRU 28/7 | 29.44 | 41.49 |
| GRU | 31.19 | 43.15 |
| RNN | 32.72 | 45.33 |
| SARIMA | 37.69 | 52.51 |
| Naive | 38.83 | 51.49 |
| ARIMA | 50.15 | 65.15 |

The calendar-aware GRU achieved the best overall performance, with the lowest MAE and RMSE.

Compared with the naive baseline, it reduced MAE by approximately 24.2% and RMSE by approximately 19.4%.

## Interpretation

The recurrent neural network models outperformed the classical statistical baselines.

The calendar-aware GRU performed best in this final run, while the standard GRU produced very similar results. This suggests that recurrent models were able to capture useful temporal patterns in the sales series.

However, the difference between the two GRU models was not very large. In practice, this means that the simpler GRU is still a strong option, while the calendar-aware GRU may be useful when weekday effects and closed-day patterns are important.

Daily e-commerce sales are affected by many external business factors that are not included in this dataset, such as:

- inventory availability
- stockouts
- supplier delays
- cash-flow limitations
- promotions
- holidays
- seasonal demand
- product launches
- large one-off orders

Because of this, the models do not always capture sudden mid-week sales spikes or drops.

## Business Use Case

The forecasts should not be treated as exact daily sales predictions.

Their main value is as a baseline decision-support tool. In a business context, this type of forecast can help with short-term planning, especially cash-flow management and payment scheduling.

## Main Takeaway

Past sales patterns contain useful forecasting signal, but they do not fully explain future sales.

The calendar-aware GRU achieved the best accuracy in this experiment, while the standard GRU remained very competitive. Classical models and naive baselines are still useful for comparison because they provide simple and transparent reference points.
