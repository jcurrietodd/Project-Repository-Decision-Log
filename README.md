# Diamonds Price Analysis
### What drives the price of a diamond? A semester-long statistical investigation.

**Course:** BAN 6005 - Business Analytics | Wake Forest University School of Business | Summer 2026
**Team:** Alicia, Bennett, and Joshua Todd
**Individual repository maintained by:** Joshua C. Todd
**Dataset:** 49,835 diamonds | Source: Kaggle (ggplot2 / Hadley Wickham)
**Main research question:** Which physical characteristics and quality grades most strongly predict diamond price?

---

## Quick Navigation

| Assignment | File | Method | Key Output |
|:---|:---|:---|:---|
| [A2 - Dataset Selection](#assignment-2---dataset-selection) | [Dataset Selection & Overview](diamonds_project_submission%20(1)%20(1).xlsx) | Data cleaning & recoding | 49,835 rows after cleaning |
| [A3 - Descriptive Stats](#assignment-3---descriptive-statistics) | [Descriptive Statistics](3%20diamonds_recoded_cleaned_assignment_3.xlsx) | Summary statistics | Price skew = 1.61; depth skew = -0.06 |
| [A4 - Probability](#assignment-4---probability) | [Probability Analysis](diamonds_recoded_cleaned_assignment_4%20(1).xlsx) | Empirical probability | ~35% of diamonds priced under $1,325 |
| [A5 - Inference](#assignment-5---hypothesis-testing) | [Hypothesis Testing & Inference](assignment5_stats%20(added-on%20Draft)%20(2).xlsx) | One-sample t-test + CI | Significant at alpha = 0.05 |
| [A6 - Regression](#assignment-6---regression-modeling) | [Regression Modeling](Assingment%206.xlsx) | OLS regression | R² = 0.9014; carat drives price |

---

## Dataset

| Attribute | Detail |
|:---|:---|
| Source | [Kaggle - Diamonds Dataset](https://www.kaggle.com/datasets/shivam2503/diamonds) |
| Origin | ggplot2 R package (Hadley Wickham) |
| Raw size | 50,000 rows x 10 columns |
| Cleaned size | 49,835 rows x 10 columns |
| Removed | 145 duplicates, 17 zero-dimension rows, 3 extreme outliers |
| Main outcome variable | `price` (USD, $326 - $18,823) |

### Variable Reference

| Variable | Type | Description |
|:---|:---|:---|
| `carat` | Continuous | Diamond weight (1 carat = 0.2g) |
| `cut` | Categorical (3) | Low / Medium / High |
| `color` | Categorical (3) | Colorless / Near Colorless / Faint |
| `clarity` | Categorical (3) | Low / Medium / High |
| `depth` | Continuous | Total depth percentage |
| `table` | Continuous | Width of top facet as % of widest point |
| `price` | Continuous | Price in USD - main outcome variable |
| `x` / `y` / `z` | Continuous | Physical dimensions in mm |

---

**Decision Log:** [View DECISIONS.md](DECISIONS.md)

### Executive Summary

Our team was tasked with finding a dataset, under the constraints of at least three categorical variables to compare with our continuous predictors. Our diamonds dataset spanned to 49,835 rows after cleaning to be compared against our set predictors as outlined in our project. We explored six different methods of analysis to better understand what attributes of a diamond best determine price. The dominant finding in our analysis was that the price of diamonds is strongly predicted by carat weight, with each additional carat adding roughly ~$10,892 to the price. This predictor had a very large relativity, essentially dwarfing the other predictors. Of the categorical variables, we assigned dummies and added them as binaries in later assessments of our data to allow them to work out in final regression outputs.

Working through the data, we found some interesting insights, based on this large sample of registered diamonds in the market, that low-quality cut diamonds dominate the market. The price is heavily right-skewed (1.61), meaning that most of the diamonds are inexpensive and only a small share of large, more expensive stones pulls the mean price upward. The mean ($3,945) is much larger than the median ($2,415), confirming that the small share of pristine and larger diamonds heavily skew averages upward. Roughly ~35% of the diamonds are observed in the lower band, representing the inexpensive price group. In our final regression, we saw that color was the only predictor pulling down our results. Our model was statistically significant with a high R Squared ~0.9014 and an adjusted value of ~0.0913. All our predictors carried significance, the only issue we ran into was in multicollinearity – expected for dimension variables, given x, y and z are needed to map the area of a diamond. We made the choice to drop these variables; carat was a better metric to explain size.

Our main takeaway, in a business context, a jeweler could use this model to estimate the price of a diamond, especially focusing on carat being the best predictor of what the price should be when assessing different diamond samples. Cut matters the least, shear quantity/amount per stone is the focus. The more weight a diamond carries the more valuable it becomes when predicting prices.


## Assignment-by-Assignment Summary

### Assignment 2 - Dataset Selection
**File:** [A2 - Dataset Selection](diamonds_project_submission__1___1_.xlsx)

We selected the Diamonds Dataset because it offers ~50,000 observations with a clear continuous outcome variable (price), a mix of continuous and categorical predictors, and a direct business interpretation. Price was chosen as the main variable of interest because it is the central outcome in any diamond retail context and is well-suited to all four analytical methods planned for the course.

**Recoding decisions:**
- `cut`: Fair/Good -> Low | Very Good -> Medium | Premium/Ideal -> High
- `color`: D-F -> Colorless | G-H -> Near Colorless | I-J -> Faint
- `clarity`: I1/SI2/SI1 -> Low | VS2/VS1 -> Medium | VVS2/VVS1/IF -> High

---

### Assignment 3 - Descriptive Statistics
**File:** [A3 - Descriptive Stat](3_diamonds_recoded_cleaned_assignment_3.xlsx)

```
Variable        Mean        Median      Std Dev     Skew
-------         -------     -------     -------     ----
price           $3,945      $2,415      $3,989      1.61   <- right-skewed
carat           0.798       0.700       0.474       1.11   <- right-skewed
depth %         61.75       61.80       1.433      -0.06   <- near-symmetric (only one)
table           57.46       57.00       2.235       0.80
```

**Key finding:** `depth` is the only variable without positive skew. All others cluster at low values with long right tails - consistent with a market where most diamonds are small and inexpensive, and large premium stones are rare.

---

### Assignment 4 - Probability
**File:** [A4 - Probability](diamonds_recoded_cleaned_assignment_4__1_.xlsx)

**Approach:** Empirical (not normal distribution)
**Reason:** `price` skew = 1.61 and `carat` skew = 1.11 - a normal distribution assumption would overestimate the likelihood of mid-to-high values. `depth` (skew = -0.06) was the only variable approximating normality, but we applied the empirical method consistently across all variables.

```
Price Band          Count       Share of Dataset
$326 - $952         ~12,459     ~25%   (below Q1)
$952 - $2,415       ~12,459     ~25%   (Q1 to median)
$2,415 - $5,325     ~12,459     ~25%   (median to Q3)
$5,325 - $18,823    ~12,458     ~25%   (above Q3)

~35% of diamonds priced under $1,325 (lowest band)
Bimodal pattern in x and y dimensions -> two distinct market size segments
```

---

### Assignment 5 - Hypothesis Testing
**File:** [A5 - Inference](assignment5_stats__added-on_Draft___2_.xlsx)

- **Test:** One-sample t-test on mean diamond price
- **Alpha:** 0.05 (conventional threshold; no domain-specific reason to be more or less conservative)
- **Sample size:** n = 49,835
- **Result:** Statistically significant at alpha = 0.05
- **Note:** With n = 49,835 the standard error is very small. Statistical significance does not imply practical significance at this scale - a $10 difference would be statistically significant but commercially irrelevant. The 95% confidence interval was reported alongside the p-value to provide actionable range estimates.

---

### Assignment 6 - Regression Modeling
**File:** [A6 - Regression](Assingment_6.xlsx)

#### Final Model Performance

```
Metric                  Value
------                  -----
R-squared               0.9014
Adjusted R-squared      0.9013
Standard Error          $1,254.87
Observations            49,835
F-statistic             37,941.4 (p < 0.001)
```

#### Coefficient Table - Final Model

```
Predictor               Coefficient     Interpretation
---------               -----------     --------------
Intercept               -$2,069         baseline
carat                   +$10,892        dominant driver - $10,892 more per carat
cut_medium              +$318           medium cut premium over low cut
cut_high                +$444           high cut premium over low cut
color_near_colorless    -$625           discount vs. colorless
color_faint             -$2,060         significant discount vs. colorless
clarity_medium          +$1,168         clarity premium over low clarity
clarity_high            +$1,783         strong clarity premium over low clarity
depth                   +$68            small positive effect per % point
table                   -$34            slightly negative per % point
```

**Variables removed:** `x`, `y`, `z` (length, width, depth in mm)
**Reason:** Near-perfect multicollinearity. Correlation matrix showed x-y = 0.999, x-z = 0.991, y-z = 0.991. All three dimensions are mechanically related through carat weight and cut proportions. Retaining them inflated standard errors without improving interpretability. Carat already captures the volume/weight information.

#### What This Model Tells a Jeweler

> A diamond's carat weight is by far the strongest predictor of price - worth nearly $11,000 per carat. After weight, clarity matters more than cut, and color penalizes more than it rewards. A faint-color diamond costs roughly $2,060 less than a comparable colorless stone, while a high-clarity stone commands $1,783 more than a low-clarity one.

---

## Key Findings Across the Project

| Question | Finding |
|:---|:---|
| What drives diamond price most? | Carat weight (+$10,892 per carat) by a wide margin |
| Which quality grade matters most? | Color penalizes most severely (-$2,060 for faint vs. colorless) |
| How well can we predict price? | R² = 0.9014 - the model explains 90% of price variance |
| Are the dimensions (x, y, z) useful? | No - multicollinearity (r = 0.999) makes them redundant given carat |
| Is price normally distributed? | No - skew of 1.61 required the empirical probability approach |
| What is a typical diamond worth? | Median $2,415; mean $3,945 (mean pulled up by rare large stones) |

---

## Tools Used

| Tool | Purpose |
|:---|:---|
| Microsoft Excel | All analysis - descriptive stats, probability, t-tests, regression |
| GitHub | Version control and portfolio documentation |
| Kaggle | Dataset source and access |

---

## Repository Structure

|- diamonds_project_submission (1) (1).xlsx     <- Assignment 2
|- 3 diamonds_recoded_cleaned_assignment_3.xlsx  <- Assignment 3
|- diamonds_recoded_cleaned_assignment_4 (1).xlsx <- Assignment 4
|- assignment5_stats (added-on Draft) (2).xlsx   <- Assignment 5
|- Assingment 6.xlsx                             <- Assignment 6
---

*BAN 6005 - Business Analytics | Wake Forest University School of Business | Summer 2026*
*Individual repository: Joshua C. Todd | Team: Alicia, Bennett, Joshua Todd*
