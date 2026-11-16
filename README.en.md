# Forecasting Weekly Counter-Strike 2 Players with SARIMA

🇧🇷 [Português](README.md) | 🇺🇸 English

End-to-end time series analysis of the number of active **Counter-Strike 2 / CS:GO** players on Steam from 2016 to 2025, covering SARIMA modeling, outlier treatment and a 2026 forecast validated against real data from the first half of 2026.

Final project for the **Time Series** course at the Department of Statistics, **Federal University of São Carlos (UFSCar)**, Brazil (July 2026). The notebook and report are written in Portuguese.

## Key results

- **Final model:** SARIMA(1,1,2)(0,0,1)₅₂ fitted on the log of the series, with dummy regressors (`xreg`) for three outliers
- **Validation (trained through 2025, tested on the first 25 weeks of 2026):** MAPE of **5.91%**, MAE ≈ 94.6k players, RMSE ≈ 113.5k players
- **Outliers detected and explained:** a level shift in March 2020 (COVID-19 lockdowns), a transient change in May 2023 (BLAST.tv Paris Major, the last CS:GO Major) and another level shift around the transition to CS2
- **Comparison with other models:**

| Model        | MAE     | RMSE    | MAPE   |
|--------------|---------|---------|--------|
| SARIMA       | 94,606  | 113,451 | 5.91%  |
| Holt-Winters | 204,310 | 241,509 | 12.97% |
| ETS          | 65,892  | 83,149  | 4.33%  |

ETS had the lowest test error; SARIMA outperformed Holt-Winters, likely thanks to the explicit outlier treatment.

## Analysis steps

1. **Data preparation:** daily SteamDB records aggregated into weekly averages (522 weeks), plus a log transform to stabilize the variance
2. **Exploratory analysis:** time plot, descriptive statistics, STL decomposition, ACF/PACF
3. **Stationarity:** ADF and KPSS tests; the series becomes stationary after one difference
4. **Outlier detection:** `tsoutliers` (AO, LS and TC), with an interpretation of each event
5. **Modeling:** manual identification from the correlograms, confirmed by `auto.arima`
6. **Residual diagnostics:** Ljung-Box (no autocorrelation), Shapiro-Wilk and QQ-plot, Breusch-Pagan
7. **Forecasting:** 52 weeks ahead with 80% and 95% intervals, back-transformed to the original scale
8. **Hold-out validation** and comparison with Holt-Winters and ETS

## Files

| File | Contents |
|---|---|
| `serie_temporais_CS_log.ipynb` | Notebook (R kernel) with all code and outputs |
| `Grupo_..._Relatório___Análise_de_dado_real.pdf` | Final report with the full analysis and discussion |

## Tools

**R**: `forecast`, `tseries`, `tsoutliers`, `lmtest`, `dplyr`, `lubridate` · Jupyter Notebook

## How to reproduce

1. Download the CS2 player history from [SteamDB, app 730](https://steamdb.info/app/730/charts/) and save it as a `;`-separated CSV (columns `DateTime` and `Players`)
2. Install the packages:
   ```r
   install.packages(c("forecast", "tseries", "tsoutliers", "lmtest", "dplyr", "lubridate"))
   ```
3. Open the notebook with an R kernel (IRkernel) and set the CSV file name in the data-loading cell

## Authors

- Marcelo Xinhong Huang
- Letícia Yumi Ichibara
- Pedro Henrique de Araujo

Supervised by Prof. Maria Silvia de Assis-Moura (UFSCar)

## Data source

[SteamDB: Counter-Strike 2 Charts](https://steamdb.info/app/730/charts/)
