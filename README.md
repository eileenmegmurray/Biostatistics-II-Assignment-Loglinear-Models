# Loglinear Models for Vegetable Intake: NYC Community Health Survey 2019

**Course:** BIOS621 (Assignment 2), CUNY Graduate School of Public Health and Health Policy
**Author:** Eileen M. Murray, MPH
**Date:** October 2025
**Language:** R (R Markdown, knitted to HTML)

> This was completed as a course assignment. Predictor variables were randomly assigned using a reproducible seed rather than chosen on substantive grounds, so the analysis is a modeling exercise.
> 
## Overview

This project models a count outcome, the number of vegetables a respondent reported eating the previous day (`nutrition47`), using the 2019 NYC Community Health Survey (CHS). The assignment focused on choosing an appropriate count model when the data show overdispersion and excess zeros, then comparing unweighted and survey-weighted results.

## Data

The [NYC Community Health Survey](https://www.nyc.gov/site/doh/data/data-sets/community-health-survey.page) is an annual telephone survey run by the NYC Department of Health and Mental Hygiene (DOHMH) to track health behaviors and risk factors among adult New Yorkers. The 2019 public-use SAS file is read directly from the DOHMH website, so no data is stored in this repository.

Variables requiring a data use agreement, administrative variables, and variables requiring alternate weights were excluded before sampling predictors.

| Variable | Role | Description |
|---|---|---|
| `nutrition47` | Outcome | Number of vegetables eaten yesterday |
| `emp3` | Predictor | Employment status |
| `everasthma` | Predictor | Ever told you have asthma |
| `checkedbp19` | Predictor | Checked blood pressure in the past 30 days |
| `mood6` | Predictor | How often felt worthless in the past 30 days |
| `strata`, `wt20_dual` | Design | Survey strata and weights |

## Methods

1. **Data preparation.** Recoded predictors into labeled factors, with missing values kept as an explicit "Missing" level.
2. **Descriptive statistics.** Built a Table 1 of participant characteristics (N = 8,653 with a non-missing outcome) and examined the outcome's distribution. The mean (1.45) was well below the variance (about 2.78), and roughly 20% of responses were zero, pointing to overdispersion and possible zero inflation.
3. **Multicollinearity check.** Assessed correlations between predictors using a model matrix and a heat map; no meaningful multicollinearity was found.
4. **Model fitting.** Fit four unweighted count models: Poisson, negative binomial, zero-inflated Poisson, and zero-inflated negative binomial.
5. **Model selection.** Compared models using AIC and residual and Q-Q diagnostic plots. The standard negative binomial had the lowest AIC (about 26,648, vs. about 26,666 for the zero-inflated negative binomial), and its diagnostics were also more favourable, so it was selected.
6. **Survey-weighted model.** Refit the model with the CHS stratified design and weights using the `survey` package (`svyglm` with a quasi-Poisson family to account for overdispersion).

## Key Findings

- **Employment** was the most consistent predictor. Being employed was associated with higher vegetable intake in both the unweighted model (β = 0.153, 95% CI: 0.110 to 0.196) and the weighted model (β = 0.169, 95% CI: 0.101 to 0.236).
- **Feeling worthless most of the time** was associated with lower vegetable intake in the weighted model (β = −0.230, 95% CI: −0.454 to −0.006, p = 0.044). Additional mood categories were significant only in the unweighted model.
- **Checking blood pressure** was negatively associated with intake in the unweighted model (β = −0.056, 95% CI: −0.098 to −0.014). **Asthma history** showed no significant association in either model.
- In the weighted model, a **missing response** to the blood pressure question was associated with lower vegetable intake, which may point to non-response bias worth investigating further.

Overall, the results only partially supported the alternative hypothesis, and the null hypothesis was not fully rejected.

## Repository Contents

| File | Description |
|---|---|
| `MurrayE_BIOS621_A2 (1).Rmd` | R Markdown source code |


> GitHub displays HTML files as source code. To view the rendered report, download the file and open it in a browser, or enable GitHub Pages for this repository.

## Reproducing the Analysis

**R packages:** `haven`, `dplyr`, `forcats`, `table1`, `ggplot2`, `RColorBrewer`, `pheatmap`, `MASS`, `pscl`, `survey`

```r
install.packages(c("haven", "dplyr", "forcats", "table1", "ggplot2",
                   "RColorBrewer", "pheatmap", "MASS", "pscl", "survey"))
```

The dataset is downloaded within the code, and predictor sampling uses `set.seed(16245)` so the same four predictors are selected on every run.

## Skills Demonstrated

Count regression (Poisson, negative binomial, zero-inflated models) · Model comparison with AIC and residual diagnostics · Complex survey design and weighting · Data cleaning and recoding in R · Reproducible reporting with R Markdown
