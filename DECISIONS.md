# Decision Log
## Diamonds Price Analysis - BAN 6005, Wake Forest University

This log records the key analytical choices made during each assignment. It is maintained individually by Joshua C. Todd, though the underlying work was completed as a team.

---

## Assignment 2: Dataset Selection & Cleaning
*2026-06-XX*

**Dataset:** Diamonds Dataset - originally compiled from the `ggplot2` R package (Hadley Wickham), accessed via Kaggle.

**Main variable of interest:** `price` (continuous, USD).
We selected price because it is the central business outcome in any diamond retail or wholesale context - every other variable functions as a potential driver of price rather than an end in itself. Price is right-skewed (mean $3,945 vs. median $2,415), which raises natural questions about what separates premium diamonds from typical ones, and it is well-suited to descriptive comparison, hypothesis testing, and both linear and logistic regression approaches.

**Key recoding decisions:**
- `cut` was recoded from 5 levels to 3: Fair/Good → Low, Very Good → Medium, Premium/Ideal → High. This consolidation reduced noise while preserving the meaningful quality gradient.
- `color` was recoded from 7 levels (D–J) to 3: D–F → Colorless, G–H → Near Colorless, I–J → Faint. The original seven-level scale introduced more granularity than our sample size required for meaningful group comparisons.
- `clarity` was recoded from 8 levels to 3: I1/SI2/SI1 → Low, VS2/VS1 → Medium, VVS2/VVS1/IF → High. Same rationale as color - three levels preserve the substantive quality distinction without overfitting to category boundaries.

**Cleaning decisions:**
- Removed 145 exact duplicate rows - identical values across all 10 columns indicate data entry duplication, not legitimate repeat observations.
- Removed 17 rows with zero-value dimensions (x, y, or z = 0mm) - physically impossible for a real diamond; almost certainly data entry errors or sentinel values.
- Removed 3 extreme outlier rows where dimension values were wildly inconsistent with their carat weight - likely decimal-place entry mistakes (e.g., a dimension of 58mm for a 0.2-carat stone).
- Final dataset: **49,835 rows × 10 columns.**

**Why this dataset:** It offers a large enough sample (~50,000 observations) for all planned analyses, contains a clear continuous outcome variable, mixes continuous and categorical predictors well-suited to each assignment's methods, and has a direct business interpretation that makes findings meaningful rather than abstract.

---

## Assignment 3: Descriptive Statistics
*2026-06-XX*

**Cleaning carried forward:** The cleaned and recoded dataset from Assignment 2 was used unchanged. No additional rows were removed in this assignment.

**Most surprising pattern - depth is the only non-positively skewed variable:**
Every continuous variable in the dataset except `depth` showed positive skew - `price` most severely (skew = 1.61), followed by `carat` (1.11). This makes intuitive sense: most diamonds are small and inexpensive, with a long tail of large, costly stones. `depth`, however, had a skew of approximately −0.06, meaning its distribution is nearly symmetrical. The reason is structural: depth percentage is a ratio calculated from the diamond's physical proportions, and cutters actively aim for a specific depth range (roughly 59–63%) because it maximizes how light moves through the stone. That engineering constraint produces a balanced distribution rather than the market-driven skewness seen in price and carat.

**Bimodal pattern in physical dimensions:**
`x` (length) and `y` (width) both showed two distinct peaks - one around 4.23–4.73mm and another around 6.23–6.73mm - suggesting the dataset captures two separate size market segments rather than a continuous distribution. This was flagged as a potential consideration for regression modeling.

**Decision on summary statistics:** We reported mean, median, standard deviation, min, max, and skew for all continuous variables. For skewed variables (`price`, `carat`), median is the more representative central tendency measure; we reported both to allow comparison.

---

## Assignment 4: Probability
*2026-07-XX*

**Normal vs. empirical approach - and why:**
We used the empirical approach for all probability scenarios rather than assuming a normal distribution. The justification is straightforward: `price` and `carat` both showed significant positive skew (1.61 and 1.11 respectively), meaning a normal distribution assumption would systematically overestimate the likelihood of mid-to-high values. `depth` was the only variable that approximated normality (skew ≈ −0.06) and would have supported a normal model, but for consistency across the assignment we applied the empirical method throughout.

**Key probability findings:**
- Approximately 35% of diamonds fell in the lowest price band ($326–$1,325), reflecting the right-skewed distribution where most stones cluster at the low end.
- Roughly 25% of diamonds fell below $952 (Q1) and approximately 25% between the median and Q3, closely matching theoretical quartile expectations despite the non-normal distribution - confirming the empirical approach was appropriate.
- The bimodal pattern in physical dimensions identified in Assignment 3 appeared again in probability scenarios, reinforcing the conclusion that two distinct market segments are present in the data.

**Decision on price banding:** We used quartile boundaries ($952, $2,415, $5,325) as natural breakpoints rather than arbitrary round numbers, since these are data-driven thresholds with direct interpretive meaning.

---

## Assignment 5: Statistical Inference
*2026-07-XX*

**What we tested:** We conducted a one-sample t-test on diamond price to determine whether the population mean price differs from a specified benchmark value, and constructed a 95% confidence interval around the sample mean.

**Alpha level:** α = 0.05. We selected the conventional significance threshold because there is no domain-specific reason to be more conservative (α = 0.01) or more permissive (α = 0.10) in the context of diamond pricing research. The large sample size (n = 49,835) means the test has substantial power at α = 0.05.

**Conclusion:** With n = 49,835, the standard error of the mean is very small, which means the confidence interval is narrow and the t-statistic is large for any meaningful deviation from the benchmark. The test confirmed a statistically significant difference from the benchmark at α = 0.05. We noted, however, that statistical significance at this sample size does not necessarily imply practical significance - a difference of $10 would be statistically significant but commercially irrelevant.

**Key decision on the confidence interval:** We reported the 95% confidence interval alongside the hypothesis test rather than only the p-value, because the interval provides more actionable information about the plausible range of the true population mean price. A pricing manager needs to know the interval, not just whether significance was achieved.

---

## Assignment 6: Regression
*2026-07-XX*

**Model building approach:**
We began with a full model including all predictors (Model 1: all variables) and then identified variables to remove based on multicollinearity diagnostics.

**First predictor removed - and why:**
`z` (depth in mm) was the first variable removed. The correlation matrix showed that `x`, `y`, and `z` were correlated at 0.991–0.999 with each other - near-perfect multicollinearity. Physically, this makes sense: a diamond's length, width, and depth are mechanically related through the carat weight and cut proportions. Retaining all three inflates standard errors and makes individual coefficients unreliable. We removed `z` first because it had the highest pairwise correlations with both `x` and `y`.

**Multicollinearity resolution:**
After removing `z`, `x` and `y` remained correlated at 0.999. Both were subsequently removed from the final model. The physical dimension information is already partially captured by `carat` (which is a function of volume and density), and the categorical quality variables (cut, color, clarity) capture the grade-related information. Retaining `x` and `y` alongside `carat` would introduce redundant information without improving model interpretability.

**Final model retained predictors:** `carat`, `cut_medium`, `cut_high`, `color_near_colorless`, `color_faint`, `clarity_medium`, `clarity_high`, `depth`, `table`

**Final model performance:**
- R² = 0.9014 - the model explains approximately 90% of the variance in diamond price
- Adjusted R² = 0.9013 - consistent with R², confirming the model is not overfit
- F-statistic: highly significant (p ≈ 0), confirming the model as a whole is meaningful

**Key coefficient interpretations:**
- `carat` (+$10,892 per carat) - the dominant driver of price by a wide margin, consistent with industry knowledge that weight is the primary value determinant
- `color_faint` (−$2,060) - faint color diamonds command a significant price discount relative to colorless stones
- `clarity_high` (+$1,783) - high clarity stones command a meaningful premium
- `depth` (+$68 per percentage point) - small positive effect, suggesting slightly deeper stones price higher, though the practical effect is modest
- `table` (−$34 per percentage point) - wider tables slightly reduce price

**Decision on dummy variable reference categories:**
Low cut, Colorless color, and Low clarity were retained as reference categories. This means all dummy coefficients represent the price premium or discount relative to the lowest quality tier, which is the most interpretively natural framing for a pricing model.

---

*Log maintained by Joshua C. Todd - BAN 6005, Wake Forest University School of Business, Summer 2026.*
