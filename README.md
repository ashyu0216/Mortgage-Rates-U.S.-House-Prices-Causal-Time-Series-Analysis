# Mortgage Rates and U.S. House Prices: An OLS Time-Series Analysis

This project examines the relationship between the 30-year fixed mortgage rate and U.S. national house prices using monthly Federal Reserve Economic Data (FRED) from 2000 to 2024. The main goal is to test whether higher mortgage rates are associated with lower house prices after controlling for unemployment and inflation.

## Research Question

How does the 30-year fixed mortgage rate affect the U.S. national house price index, holding unemployment and inflation fixed?

## Project Overview

Housing is one of the largest assets for most American households, and mortgage rates directly affect affordability and housing demand. When mortgage rates rise, borrowing becomes more expensive, which may reduce housing demand and put downward pressure on prices.

This project uses an OLS regression framework to estimate the relationship between mortgage rates and the Case-Shiller U.S. National Home Price Index. The analysis also checks for omitted variable bias, multicollinearity, heteroskedasticity, and robustness across different time periods.

## Data

The data comes from Federal Reserve Economic Data (FRED). The unit of observation is one U.S. calendar month.

The sample covers January 2000 to December 2024. After constructing the inflation variable, the final dataset contains 299 monthly observations.

### Variables

| Variable | Description                                               | Source               |
| -------- | --------------------------------------------------------- | -------------------- |
| `HPI`    | S&P CoreLogic Case-Shiller U.S. National Home Price Index | FRED: `CSUSHPISA`    |
| `MR`     | 30-Year Fixed Rate Mortgage Average in the U.S.           | FRED: `MORTGAGE30US` |
| `U`      | Civilian Unemployment Rate                                | FRED: `UNRATE`       |
| `pi`     | Annualized monthly CPI inflation                          | FRED: `CPIAUCSL`     |

Inflation is calculated as:

```text
pi = (CPI_t / CPI_{t-1} - 1) × 100 × 12
```

## Methodology

The main regression model is:

```text
HPI_t = β0 + β1 MR_t + β2 U_t + β3 π_t + u_t
```

where:

* `HPI_t` is the U.S. house price index in month `t`
* `MR_t` is the 30-year fixed mortgage rate
* `U_t` is the unemployment rate
* `π_t` is annualized monthly inflation

The coefficient of interest is `β1`, which measures the change in the house price index associated with a 1 percentage point increase in the mortgage rate, holding unemployment and inflation fixed.

The project uses:

* OLS regression with `statsmodels`
* HC0 heteroskedasticity-robust standard errors
* Variance Inflation Factor checks for multicollinearity
* Breusch-Pagan and White tests for heteroskedasticity
* Sub-period robustness checks for pre-GFC and post-GFC periods

## Main Findings

In the preferred full-sample model, a 1 percentage point increase in the mortgage rate is associated with about a 10.8-point decrease in the house price index, holding unemployment and inflation fixed.

The estimated coefficient on mortgage rates is negative and statistically significant under both classical and HC0 robust standard errors.

However, the relationship is not stable across time periods:

* Pre-GFC period, 2000–2007: the mortgage rate coefficient is strongly negative.
* Post-GFC period, 2010–2024: the mortgage rate coefficient becomes positive.

This suggests that the full-sample estimate should not be interpreted as a stable causal effect. Instead, it is better understood as an average association across different macroeconomic regimes.

## Key Results

| Model                         | Mortgage Rate Coefficient | Interpretation                                                   |
| ----------------------------- | ------------------------: | ---------------------------------------------------------------- |
| MR only                       |                     -3.68 | Weak negative association                                        |
| MR + Unemployment             |                    -11.02 | Stronger negative association after controlling for unemployment |
| MR + Unemployment + Inflation |                    -10.84 | Preferred full-sample model                                      |

The preferred model has an R² of approximately 0.25, meaning that mortgage rates, unemployment, and inflation explain about one quarter of the variation in the house price index.

## Robustness and Limitations

The results are robust to heteroskedasticity-robust standard errors, but they are not stable across sub-periods.

Important limitations include:

1. **Reverse causality**
   The Federal Reserve may respond to housing market conditions when setting monetary policy, which affects mortgage rates. Therefore, OLS cannot fully separate the effect of mortgage rates on house prices from the effect of house prices on policy and rates.

2. **Omitted variable bias**
   Important housing market factors such as housing supply, credit standards, inventory, and demographic demand are not included in the model.

3. **Regime instability**
   The relationship between mortgage rates and house prices changes substantially between the pre-GFC and post-GFC periods.

Because of these limitations, the results should be interpreted as associations rather than causal effects.

## Technologies Used

* Python
* pandas
* NumPy
* matplotlib
* seaborn
* statsmodels
* Stargazer
* Jupyter Notebook

## Project Structure

```text
.
├── econ320_final_project.ipynb
├── MORTGAGE30US.csv
├── CSUSHPISA.csv
├── UNRATE.csv
├── CPIAUCSL.csv
└── README.md
```

## How to Run

1. Clone the repository.

```bash
git clone https://github.com/your-username/your-repository-name.git
cd your-repository-name
```

2. Install the required Python packages.

```bash
pip install pandas numpy matplotlib seaborn statsmodels stargazer
```

3. Open the notebook.

```bash
jupyter notebook econ320_final_project.ipynb
```

4. Run all cells from top to bottom.

Make sure the four FRED CSV files are in the same folder as the notebook:

```text
MORTGAGE30US.csv
CSUSHPISA.csv
UNRATE.csv
CPIAUCSL.csv
```

## Conclusion

This project finds that higher mortgage rates are associated with lower U.S. house prices in the full sample from 2000 to 2024. However, the relationship changes significantly across macroeconomic periods, especially before and after the Global Financial Crisis. The findings suggest that mortgage rates matter for housing markets, but their effect depends heavily on the broader economic environment.

## Data Sources

* Federal Reserve Economic Data: https://fred.stlouisfed.org
* 30-Year Fixed Rate Mortgage Average: `MORTGAGE30US`
* Case-Shiller U.S. National Home Price Index: `CSUSHPISA`
* Civilian Unemployment Rate: `UNRATE`
* Consumer Price Index: `CPIAUCSL`
