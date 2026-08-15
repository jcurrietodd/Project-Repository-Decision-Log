# Diamond Price Analysis - Group Project
### BAN 6005 · Wake Forest University School of Business · Summer 2026

> **Research Question:** Which physical characteristics and quality grades most strongly predict the price of a diamond?

---

## Project Overview

This repository documents a semester-long statistical analysis of diamond pricing using the Diamonds Dataset - a widely-used benchmark dataset originally compiled from the `ggplot2` R package (Hadley Wickham) and distributed via Kaggle. The analysis progresses from dataset selection and cleaning through descriptive statistics, probability modeling, hypothesis testing, and multiple linear regression, following the analytical workflow used by professional data analysts.

The project was completed as a group assignment. All analytical deliverables in this repository represent the team's shared work.

**Team Members:** Alicia, Bennett, and Joshua Todd
**Individual contributor (data cleaning and recoding):** Joshua Todd

---

## Dataset

| Attribute | Detail |
|:---|:---|
| **Source** | Kaggle - [Diamonds Dataset](https://www.kaggle.com/datasets/shivam2503/diamonds) (originally from ggplot2) |
| **Raw size** | 50,000 rows × 10 columns |
| **Cleaned size** | 49,835 rows × 10 columns |
| **Main variable** | `price` (USD, continuous) |
| **Removed** | 145 duplicate rows, 17 zero-dimension rows, 3 extreme outliers |

**Variables:**

| Variable | Type | Description |
|:---|:---|:---|
| `carat` | Continuous | Diamond weight (1 carat = 0.2g) |
| `cut` | Categorical (3 levels) | Low / Medium / High - recoded from original 5-level scale |
| `color` | Categorical (3 levels) | Colorless / Near Colorless / Faint - recoded from D–J scale |
| `clarity` | Categorical (3 levels) | Low / Medium / High - recoded from original 8-level scale |
| `depth` | Continuous | Total depth percentage |
| `table` | Continuous | Width of top facet as % of widest point |
| `price` | Continuous | Price in USD ($326–$18,823) |
| `x` / `y` / `z` | Continuous | Physical dimensions in millimeters |

---

## Repository Structure

```
diamonds-price-analysis/
├── README.md                          ← You are here
├── DECISIONS.md                       ← Analytical decision log
├── data/
│   └── diamonds_project_submission    ← Assignment 2: raw dataset + overview
├── assignment-03-descriptive-stats/
│   └── 3_diamonds_recoded_cleaned     ← Cleaned dataset + descriptive analysis
├── assignment-04-probability/
│   └── diamonds_recoded_cleaned_a4    ← Distribution analysis + probability scenarios
├── assignment-05-inference/
│   └── assignment5_stats              ← One-sample t-test + confidence intervals
└── assignment-06-regression/
    └── Assignment_6                   ← Multi-variable OLS regression models
```

---

## Analysis Summary

| Assignment | Method | Key Finding |
|:---|:---|:---|
| A2 - Dataset | Selection & cleaning | 49,835 rows after removing duplicates, zero-dimension rows, and outliers |
| A3 - Descriptive Stats | Summary statistics, distributions | `price` is right-skewed (mean $3,945 vs. median $2,415); `depth` is the only non-positively skewed variable |
| A4 - Probability | Empirical probability scenarios | Roughly 35% of diamonds fall in the lowest price band ($326–$1,325); bimodal pattern in physical dimensions |
| A5 - Inference | One-sample t-test, confidence intervals | Tested whether mean diamond price differs from a benchmark; set α = 0.05 |
| A6 - Regression | Multiple OLS with dummy variables | Final model R² = 0.9014; `carat` is the dominant predictor (coefficient: +$10,892 per carat); x, y, z removed due to near-perfect multicollinearity |

---

## Technical Notes

- All analysis was performed in Microsoft Excel
- Categorical variables were dummy-coded for regression (reference: Low cut, Colorless, Low clarity)
- Multicollinearity was assessed via correlation matrix; `x`, `y`, and `z` showed correlations of 0.999+ with each other and were removed
- Final model retained: `carat`, `cut_medium`, `cut_high`, `color_near_colorless`, `color_faint`, `clarity_medium`, `clarity_high`, `depth`, `table`

---

*Analysis completed as part of BAN 6005 - Business Analytics, Wake Forest University School of Business, Summer 2026.*
*Individual repository maintained by Joshua C. Todd.*
