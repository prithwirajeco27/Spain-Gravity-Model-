# Spain's Bilateral Trade: An Augmented Gravity Model (2000–2025)

An empirical trade-economics project examining Spain's bilateral trade with its trading partners using the **augmented Gravity Model of Trade**. The project estimates the specified model using **Ordinary Least Squares (OLS)** and **Poisson Pseudo-Maximum Likelihood (PPML)** in Python.

## Project Overview

The dependent variable, **bilateral trade**, is defined as the sum of exports and imports between Spain and each trading partner. The analysis studies how trade relates to economic size, geographical distance, and selected bilateral characteristics over 2000–2025.

### Research question

To what extent do economic size, distance, contiguity, a common official language, colonial ties, and Regional Trade Agreement (RTA) status explain variation in Spain's bilateral trade—and how do the estimates compare under OLS and PPML?

## Objectives

- Estimate the augmented gravity specification using log-linear OLS.
- Estimate the same explanatory specification using PPML with trade measured in levels.
- Interpret coefficient signs, magnitudes, and statistical significance in economic terms.
- Compare the OLS and PPML estimates.
- Document the sample selection and variables used in the empirical workflow.

## Data

The working dataset is a partner-year panel covering **2000–2025**.

The project documentation identifies the following sources:

- **UN Comtrade** — bilateral export and import data used to construct total bilateral trade.
- **CEPII** — bilateral distance and geographical, language, and historical indicators.
- **World Trade Organization (WTO)** — Regional Trade Agreement information.
- **GDP data** — Spain's GDP and trading-partner GDP are included in the panel. Add the exact GDP database and extraction details used in data collection before publication.

### Dependent variable

`Bilateral Trade = Exports + Imports`

### Variables used in the estimated models

| Variable | Definition / transformation | Expected relationship |
|---|---|---|
| `Bilateral Trade` | Exports + imports; level outcome in PPML and logged outcome in OLS | Dependent variable |
| `GDP country A` / `ln_gdp_spain` | Spain's GDP / natural logarithm | Usually positive |
| `GDP TP` / `ln_gdp_partner` | Partner GDP / natural logarithm | Positive |
| `Distance` / `ln_distance` | Bilateral distance / natural logarithm | Negative |
| `contig` | Shared land border indicator | Usually positive |
| `comlang_off` | Common official language indicator | Usually positive |
| `colony` | Colonial relationship indicator | Ambiguous |
| `RTA` | Regional Trade Agreement indicator | Usually positive |

The dataset also contains variables such as `comcol` and `comlang_ethno`, but these are **excluded from the regression formulas** in this workflow.

## Methodology

### 1. Augmented Gravity Model — OLS

OLS is estimated with the natural logarithm of bilateral trade as the dependent variable:

```text
ln_trade ~ ln_gdp_spain + ln_gdp_partner + ln_distance
           + contig + comlang_off + colony + RTA
```

For logged continuous regressors, coefficients are interpreted as elasticities. For binary regressors in the log-linear model, the exact percentage difference is `100 × [exp(β) − 1]%`.

### 2. Augmented Gravity Model — PPML

PPML models bilateral trade in levels with a log link:

```text
Bilateral Trade ~ ln_gdp_spain + ln_gdp_partner + ln_distance
                  + contig + comlang_off + colony + RTA
```

PPML is widely used in gravity estimation because it models the conditional mean in levels, can accommodate zero trade flows, and avoids the log-linear OLS retransformation issue associated with heteroskedasticity. In this project's cleaned sample, however, no zero-trade observations remain, so that particular advantage is not exercised by the reported estimates.

### Estimation details

- Software: Python in Jupyter Notebook.
- OLS outcome: `ln_trade = ln(Bilateral Trade)`.
- PPML outcome: `Bilateral Trade` in levels.
- Standard errors: clustered by `Trade Partner`.
- Both estimators use the same seven explanatory variables.
- The workflow drops rows missing selected model variables and requires positive GDP values and positive distance.

## Main Results

The final cleaned estimation sample contains **5,092 observations**. The source workbook contains 5,886 rows and 235 unique trading partners before the notebook's cleaning and sample restrictions.

| Variable | OLS coefficient (p-value) | PPML coefficient (p-value) |
|---|---:|---:|
| `ln_gdp_spain` | -0.2318 (0.028) | 0.1993 (0.009) |
| `ln_gdp_partner` | 1.0796 (<0.001) | 0.8436 (<0.001) |
| `ln_distance` | -1.0586 (<0.001) | -0.9355 (<0.001) |
| `contig` | 0.2477 (0.516) | 0.4108 (0.062) |
| `comlang_off` | 1.6689 (<0.001) | 1.2725 (<0.001) |
| `colony` | -0.5372 (0.128) | -0.6669 (0.001) |
| `RTA` | 0.2036 (0.172) | 0.0694 (0.633) |

### Key takeaways

- **Partner GDP** is positive and statistically significant under both estimators.
- **Distance** is negative and statistically significant under both estimators, consistent with the conventional gravity relationship.
- **Common official language** is positive and statistically significant under both estimators.
- **Spain's GDP** changes sign across estimators: negative under OLS and positive under PPML. This result is estimator-sensitive and should be interpreted cautiously.
- **Contiguity** is not statistically significant under OLS and is only marginally significant at the 10% level under PPML.
- **Colony** is not statistically significant under OLS but is negative and statistically significant under PPML.
- **RTA** is statistically insignificant under both estimators. This does not prove that RTAs have no effect; it means the specified models do not estimate a statistically distinguishable conditional association in this sample.

The OLS model reports `R² = 0.847` and adjusted `R² = 0.846`. The notebook reports a PPML Cox–Snell pseudo-`R²` of 1.000; this is not directly comparable to OLS `R²` and should not be interpreted in the same way.

## Important Scope Notes

- This repository documents the **augmented model estimated using OLS and PPML**.
- The supplied workflow does **not** estimate a separate core gravity model. Therefore, a numerical core-versus-augmented comparison is not reported.
- `comcol` and `comlang_ethno` are excluded from both regression formulas.
- The same 5,092 observations are used in the reported OLS and PPML estimates. The cleaned estimation sample contains no zero-trade observations.
- The results are conditional associations, not necessarily causal effects. The specifications shown here do not include year or trading-partner fixed effects.
- No diagnostic-testing section is included in this project summary.

## Repository Structure

Suggested repository layout:

```text
spain-gravity-model/
├── README.md
├── spain 2000-2025.ipynb
├── Spain_Trade_Panel_2000_2025_RTA.xlsx
└── Spain_Gravity_Model_Econometric_Report.docx
```

Update filenames in the repository tree if you rename the uploaded files. Consider keeping large or licensed source datasets outside the repository and documenting how to obtain them.

## How to Reproduce

1. Clone or download this repository.
2. Install Python and Jupyter Notebook (for example, through Anaconda).
3. Install the packages imported by the notebook. Typical packages for this workflow may include `pandas`, `numpy`, `statsmodels`, `openpyxl`, and `scipy`; confirm the exact imports in the notebook.
4. Place the Excel workbook at the path expected by the notebook, or update the notebook's file path.
5. Run the notebook cells in order to reproduce the data preparation and estimation outputs.

## References and Data Sources

- United Nations — [UN Comtrade](https://comtradeplus.un.org/)
- CEPII — [CEPII datasets](https://www.cepii.fr/CEPII/en/bdd_modele/bdd.asp)
- World Trade Organization — [Regional Trade Agreements](https://rtais.wto.org/)
- Santos Silva, J. M. C., & Tenreyro, S. (2006). “The Log of Gravity.” *The Review of Economics and Statistics*, 88(4), 641–658.

Add the exact GDP source, data extraction dates, and dataset versions used in the project if available.

## Author

Add your name, institution, course, and contact or GitHub profile here if you want these details displayed publicly.
