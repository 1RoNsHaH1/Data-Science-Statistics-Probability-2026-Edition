# 🎯 DS Statistics Interview Q&A — Batch 3
# Topic: Descriptive Statistics
# Level: Basic → Advanced | Cross-questions included

---

## 🗂️ Legend
- 🔥 = Frequently asked | ⭐ = Very important | 🔥⭐ = Both

---

# SECTION 1 — BASIC LEVEL

---

## Q1. 🔥⭐ What are measures of central tendency? Compare mean, median, and mode.

**A:**
Measures of central tendency describe the "center" or "typical value" of a dataset. The three main measures:

**Mean (Arithmetic Average):**
x̄ = Σxᵢ / n
- Uses all data values
- Sensitive to outliers — one extreme value can pull the mean far from typical values
- Best for symmetric, roughly normal distributions
- Enables further mathematical operations (variance, z-scores)

**Median:**
- Middle value when data is sorted (50th percentile)
- Completely unaffected by outliers
- Better "typical value" for skewed distributions
- Best for income, house prices, salaries (skewed data)

**Mode:**
- Most frequently occurring value
- Only measure valid for nominal (categorical) data
- Can have 0, 1, or multiple modes
- Best for categorical data and finding "most common" value

**The Skewness-Central Tendency Relationship:**
```
Right skew:  Mode < Median < Mean  (tail pulls mean right)
Symmetric:   Mode ≈ Median ≈ Mean
Left skew:   Mean < Median < Mode  (tail pulls mean left)
```

| Situation | Use This |
|-----------|---------|
| Symmetric, no outliers | Mean |
| Skewed data (income, prices) | Median |
| Categorical data | Mode |
| Outliers present | Median |
| Need to do further math | Mean |

↳ **Cross Q:** Why does the Indian government report median household income rather than mean?
↳ **Cross A:** India's income distribution is extremely right-skewed — a small percentage of billionaires (Mukesh Ambani, Gautam Adani, etc.) have incomes hundreds of times larger than most citizens. If the mean were reported, these extreme values would inflate the average far above what most families actually earn. For example, if 99 families earn ₹3L/year and 1 family earns ₹1 crore, the mean is ₹12.97L — misleadingly implying most families earn that much. The median (₹3L) accurately reflects the typical family. This is why median is the standard for income reporting in economics.

---

## Q2. 🔥⭐ What is variance? What is standard deviation? Why do we use both?

**A:**
**Variance (σ² or s²):** The average squared deviation from the mean.

Population: σ² = (1/N)Σ(xᵢ - μ)²
Sample: s² = (1/(n-1))Σ(xᵢ - x̄)²

**Standard Deviation (σ or s):** Square root of variance.

σ = √(σ²), s = √(s²)

**Why we need both:**

**Variance:**
- Used in mathematical formulas (Var(X+Y) = Var(X) + Var(Y))
- Input to many statistical tests (F-statistic, ANOVA)
- Always non-negative (squared units remove sign)
- Penalizes large deviations more than small ones (squaring)

**Standard Deviation:**
- Same units as the data → interpretable
- Used for "±" ranges (μ ± 2σ for 95% interval under normality)
- Used to compute z-scores: z = (x - μ)/σ
- Used for confidence intervals: x̄ ± z*(σ/√n)

**Example:** Exam scores: [70, 75, 80, 85, 90]
Mean = 80, Deviations = [-10, -5, 0, 5, 10]
Variance = (100+25+0+25+100)/4 = 62.5 marks² (unintuitive)
SD = √62.5 ≈ 7.9 marks (interpretable: "typical deviation is ±7.9 marks")

↳ **Cross Q:** Why does sample variance divide by n-1 instead of n?
↳ **Cross A:** This is Bessel's correction. When we compute the sample mean x̄ first and then measure deviations from x̄ (not from the true μ), we've already used the data to estimate the center. This causes the sample deviations to be systematically smaller than deviations from the true μ — the sample is "pulled toward its own mean." Dividing by n-1 corrects this downward bias. Mathematically: E[s²] = σ² (unbiased) vs E[(1/n)Σ(xᵢ-x̄)²] = σ²(n-1)/n < σ² (biased). With n=2: two points perfectly determine a line, leaving zero residual variance if we divide by 2. Dividing by n-1=1 avoids this. As n→∞, the correction becomes negligible.

---

## Q3. 🔥⭐ What is the IQR? How is it used to detect outliers?

**A:**
**IQR (Interquartile Range) = Q3 − Q1**

Where:
- Q1 = 25th percentile (value below which 25% of data falls)
- Q3 = 75th percentile (value below which 75% of data falls)
- IQR covers the middle 50% of the data

**Why IQR is robust:**
IQR uses only the middle 50% of data, so extreme values in the tails have zero effect. The top 25% and bottom 25% are completely ignored.

**Tukey's Fences (Outlier Detection):**
- Lower fence: Q1 − 1.5 × IQR
- Upper fence: Q3 + 1.5 × IQR
- Values outside these fences = **mild outliers**
- Values beyond Q1 − 3×IQR or Q3 + 3×IQR = **extreme outliers**

**Example:**
Salaries (₹L): [3, 4, 4, 5, 5, 6, 6, 7, 8, 50 (CEO)]
Q1 = 4, Q3 = 7, IQR = 3
Lower fence = 4 − 4.5 = −0.5 (no outliers below)
Upper fence = 7 + 4.5 = **11.5**
CEO salary (50) > 11.5 → **OUTLIER**

**Box plot:** The box spans Q1 to Q3, with a line at the median. Whiskers extend to most extreme non-outlier values. Outliers plotted as individual dots.

↳ **Cross Q:** Why not just use mean ± 3σ for outlier detection?
↳ **Cross A:** Mean ± 3σ is the z-score method. It fails when: (1) The data is non-normal (z-scores assume normality). (2) The outlier itself inflates the mean and SD, "masking" itself. Example: [1,2,3,4,5,100]. Mean≈19, SD≈37. z-score for 100 = (100-19)/37=2.2 < 3 → NOT flagged! IQR is resistant to masking because Q1 and Q3 are medians of halves — outliers don't distort them. Rule: for skewed data or suspected outliers, IQR method is more reliable. For confirmed normal data with no extreme outliers, z-score method is fine.

---

## Q4. 🔥 What is skewness? How do you identify and handle it?

**A:**
**Skewness** measures the asymmetry of a distribution around its mean.

**Formula (Fisher's moment coefficient):**
$$\gamma_1 = \frac{1}{n}\sum_{i=1}^n \left(\frac{x_i - \bar{x}}{s}\right)^3$$

| Value | Meaning |
|-------|---------|
| γ₁ ≈ 0 | Symmetric |
| γ₁ > 0 | Right-skewed (positive): long tail to the right |
| γ₁ < 0 | Left-skewed (negative): long tail to the left |
| |γ₁| > 1 | Highly skewed |
| |γ₁| < 0.5 | Approximately symmetric |

**Visual identification:**
- Right skew: peak left of center, long right tail → Mean > Median
- Left skew: peak right of center, long left tail → Mean < Median
- Histogram is the best first check

**Handling skewness in ML:**

| Skew Type | Transformation |
|-----------|---------------|
| Right-skewed, positive values | log(x), √x, x^(1/3) |
| Right-skewed, with zeros | log(x+1) |
| Strong right skew | Box-Cox with λ<1 |
| Left-skewed | x², x³, or reflect + transform |
| General | Yeo-Johnson (handles negatives) |

↳ **Cross Q:** Why does skewness matter for ML models?
↳ **Cross A:** Skewed features cause multiple problems: (1) Linear models assume relationships are linear and residuals are normal — heavy skew in features/target violates these. (2) Gradient-based optimization is sensitive to feature scale and distribution — very skewed features can cause instability. (3) Distance-based models (KNN, K-means) are distorted by skewed features where most variation is in one part of the range. (4) Neural networks learn faster when features are roughly symmetric and normalized. Tree models (Random Forest, XGBoost) are the exception — they find thresholds by splitting, so skewness doesn't affect them directly. Rule: for linear/logistic regression and neural networks, always handle skewness before training.

---

## Q5. 🔥 What is kurtosis? What are leptokurtic and platykurtic distributions?

**A:**
**Kurtosis** measures the "tailedness" of a distribution — how heavy the tails are and how sharp the peak is.

**Formula (fourth standardized moment):**
$$\gamma_2 = \frac{1}{n}\sum_{i=1}^n \left(\frac{x_i - \bar{x}}{s}\right)^4$$

**Excess kurtosis** = kurtosis − 3 (subtracts Normal distribution's kurtosis of 3)

| Type | Excess Kurtosis | Description |
|------|----------------|-------------|
| **Mesokurtic** | ≈ 0 | Normal-like tails (Normal distribution) |
| **Leptokurtic** | > 0 | Heavy tails + sharp peak (more extreme values than Normal) |
| **Platykurtic** | < 0 | Light tails + flat peak (fewer extremes than Normal) |

**Examples:**
- **Leptokurtic (fat tails):** Stock returns, Student's t-distribution. More crashes and booms than Normal predicts.
- **Platykurtic (thin tails):** Uniform distribution, bounded distributions.
- **Mesokurtic:** Normal distribution (baseline).

**Why it matters:**
- Fat-tailed distributions undermine risk models that assume normality (2008 financial crisis — "100-year events" happened far too frequently)
- High kurtosis in residuals → model doesn't capture all systematic variation
- For ML: high kurtosis → many outliers → consider robust loss functions (Huber instead of MSE)

↳ **Cross Q:** What is the Jarque-Bera test?
↳ **Cross A:** Jarque-Bera tests the null hypothesis that data comes from a Normal distribution by combining skewness and kurtosis: JB = (n/6)[γ₁² + (γ₂)²/4] ~ χ²(2) under H₀. Advantages: uses both skewness and excess kurtosis simultaneously, so can detect non-normality from either source. Disadvantages: poor power for small samples (n<50), can reject normality with large n even for nearly-normal distributions. Alternatives: Shapiro-Wilk (best for n<50), Anderson-Darling (good for any n, sensitive to tails), D'Agostino-Pearson (similar to JB but robust to small n). In practice: always complement formal tests with QQ plots — a visual check often reveals the nature of non-normality better than a p-value.

---

## Q6. 🔥⭐ Explain Pearson correlation. What are its limitations?

**A:**
**Pearson correlation coefficient r** measures the strength and direction of the linear relationship between two continuous variables.

$$r = \frac{\sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum(x_i-\bar{x})^2 \cdot \sum(y_i-\bar{y})^2}}$$

Equivalently: r = Cov(X,Y) / (σ_X × σ_Y)

**Interpretation:**
| r value | Interpretation |
|---------|---------------|
| +1.0 | Perfect positive linear relationship |
| +0.7 to +0.9 | Strong positive |
| +0.4 to +0.6 | Moderate positive |
| +0.1 to +0.3 | Weak positive |
| 0 | No linear relationship |
| −0.1 to −0.3 | Weak negative |
| −0.7 to −0.9 | Strong negative |
| −1.0 | Perfect negative linear relationship |

**Key limitations:**
1. **Only measures linear relationships:** r = 0 doesn't mean independent — Y = X² has r ≈ 0 but perfect dependence.
2. **Sensitive to outliers:** One extreme point can dramatically change r.
3. **Not robust to non-normality:** Assumes bivariate normality for inference.
4. **Doesn't imply causation:** Correlation ≠ causation (ice cream & drowning).
5. **Scale matters:** Transforming variables changes r.
6. **Doesn't capture non-monotonic relationships:** Quadratic, sinusoidal patterns have low r.

↳ **Cross Q:** When would you use Spearman instead of Pearson correlation?
↳ **Cross A:** Spearman rank correlation rₛ computes Pearson correlation on the RANKS of the data, not the raw values. Use Spearman when: (1) Data is ordinal (e.g., customer satisfaction: Poor/Fair/Good/Excellent). (2) Relationship is monotonic but not linear (Y increases as X increases, but not proportionally). (3) Outliers are present (Spearman is robust because outliers don't get extreme ranks). (4) Data is non-normal. Spearman detects any monotonic relationship; Pearson detects only linear. Trade-off: Pearson has more statistical power when relationship IS linear. Formula shortcut: rₛ = 1 - 6Σdᵢ²/[n(n²-1)] where dᵢ = rank difference for each pair.

---

# SECTION 2 — INTERMEDIATE LEVEL

---

## Q7. 🔥⭐ What is the five-number summary? How does a box plot represent it?

**A:**
The **five-number summary** captures a complete picture of a distribution with five statistics:

[Min | Q1 | Median (Q2) | Q3 | Max]

**Box plot components:**
```
         ┌──────────────────┐
         │       │          │
 ○────────┤  Q1  │  Median  ├────│──────────────── ○
         │       │          │   Max (non-outlier)
         └──────────────────┘
      Min (non-outlier)   Q3
      
○ = Outlier (beyond 1.5×IQR from box)
│ = Whisker (to most extreme non-outlier)
Box = IQR (Q1 to Q3)
Line inside box = Median
```

**Reading a box plot:**
- **Box width** = IQR → measures spread of middle 50%
- **Median position in box** → indicates skewness:
  - Median near Q1 → right-skewed
  - Median centered → symmetric
  - Median near Q3 → left-skewed
- **Whisker length** → spread of non-outlier tails
- **Dots outside whiskers** → outliers

**When to use box plots:**
- Comparing distributions across multiple groups (e.g., salary by department)
- Quick outlier detection
- Seeing skewness of multiple distributions simultaneously
- Before/after comparison

↳ **Cross Q:** What does it mean if the box plot has no visible whiskers?
↳ **Cross A:** Very short or invisible whiskers mean Q1 or Q3 is equal to or very close to the min/max non-outlier value. This can indicate: (1) Very low spread in the non-outlier data (nearly all data compressed in one region). (2) Or that the "minimum" or "maximum" value is exactly Q1 or Q3 (e.g., lots of zeros creating a spike at Q1). Example: A fraud flag column (99.9% zeros, 0.1% ones) would have a box plot with Q1=Q2=Q3=0, no visible box, and a dot at 1 as an outlier. In such cases, a box plot is a poor choice — use a bar chart of counts instead.

---

## Q8. 🔥 What is the coefficient of variation (CV)? When is it more useful than standard deviation?

**A:**
**Coefficient of Variation:** CV = (σ/μ) × 100%

It expresses standard deviation as a percentage of the mean — a relative measure of variability.

**When CV is more useful than SD:**

1. **Comparing variability across different scales:**
   - Revenue SD = ₹10,000 vs Employee count SD = 50. Which is more variable? Can't compare SDs directly.
   - CV_revenue = 10000/50000 = 20%. CV_employees = 50/200 = 25%. Employees more variable relatively.

2. **Comparing datasets with different units:**
   - Weight in kg vs height in cm — SDs are incomparable.
   - CV is dimensionless — comparable across units.

3. **Process quality control:**
   - CV < 10%: Low variability, process under control
   - CV 10-30%: Moderate variability
   - CV > 30%: High variability, investigate

**When NOT to use CV:**
- When mean ≈ 0 (CV explodes to infinity)
- When mean can be negative
- When measurements are on interval scale (e.g., temperature in Celsius — a mean of 2°C gives misleading CV)

↳ **Cross Q:** How is CV used in feature selection for ML?
↳ **Cross A:** Features with very low CV (near-zero variance) carry almost no information — they're nearly constant across all samples. These are excellent candidates to drop: (1) Near-zero variance features won't help the model discriminate between classes. (2) They add noise and increase dimensionality. In scikit-learn: VarianceThreshold(threshold=0.01) removes features with variance < 1% of their mean-squared. This is a quick preprocessing step that can significantly reduce feature dimensionality with zero information loss. CV-based filtering is especially useful for sensor data, gene expression data, and text TF-IDF matrices where many features may have near-zero variance.

---

## Q9. 🔥⭐ What is a percentile? How do you compute the 90th percentile?

**A:**
The **p-th percentile** is the value below which p% of the data falls.

**Computing the p-th percentile (n data points, sorted):**

Step 1: Compute the rank position: L = (p/100) × n
Step 2:
- If L is a whole number: average the L-th and (L+1)-th sorted values
- If L is fractional: round up to next integer → value at that position

**Example:** Data = [15, 20, 25, 30, 35, 40, 45, 50, 55, 60], n=10

**90th percentile:**
L = (90/100) × 10 = 9 → whole number
P90 = average of 9th and 10th values = (55+60)/2 = **57.5**

**75th percentile (Q3):**
L = (75/100) × 10 = 7.5 → round up to 8
P75 = 8th value = **50**

**Critical percentiles in industry:**
- P50 (median): typical user experience
- P95, P99: tail latency for API performance (SLA)
- P99.9: extreme tail — "nines" reliability
- P25, P75 (Q1, Q3): box plot construction

**Deciles:** P10, P20, ..., P90 divide data into 10 equal parts
**Quartiles:** P25, P50, P75 divide into 4 equal parts
**Quintiles:** P20, P40, P60, P80 divide into 5 equal parts

↳ **Cross Q:** Why does Netflix care about P99 latency instead of mean latency?
↳ **Cross A:** Mean latency hides tail behavior. If 99% of requests complete in 50ms but 1% take 10 seconds, the mean might be 150ms — acceptable-sounding. But P99 = 10 seconds means 1 in 100 user interactions is very slow — for a service with 100M requests/day, that's 1M terrible experiences daily. This is why SLAs specify "P99 < 200ms" rather than "mean < 200ms." The insight from queueing theory: slow requests create backlogs that cascade — a single slow database query can slow all requests behind it. P99 monitoring catches these before they affect user satisfaction (NPS, churn). Always monitor P95 and P99 for any user-facing service.

---

## Q10. 🔥⭐ What is the difference between correlation and causation? How do you establish causation?

**A:**
**Correlation:** Two variables X and Y move together statistically. Measured by r, mutual information, etc. Does NOT require a causal mechanism.

**Causation:** X causes Y — intervening on X (changing X while holding everything else constant) changes the expected value of Y.

**Why correlation ≠ causation:**

**1. Confounding:** A third variable Z causes both X and Y.
- Example: Shoe size and reading ability correlate in children — but AGE causes both.

**2. Reverse causation:** Y causes X, not X causing Y.
- Example: Ice cream sales correlate with murder rates — but hot weather causes both, AND murder rates might be correlated with ice cream simply through seasonality.

**3. Coincidence (spurious):** Random chance.
- Nicolas Cage movies vs pool drownings, r = 0.66 (2000-2009).

**Establishing causation — methods in order of rigor:**

1. **Randomized Controlled Trial (RCT):** Gold standard. Random assignment eliminates all confounding (observed and unobserved).

2. **Instrumental Variables (IV):** Find Z that affects X but doesn't directly affect Y (other than through X). Uses variation in Z to isolate causal effect of X on Y.

3. **Difference-in-Differences (DiD):** Compare change over time in treatment vs control group. Assumes parallel trends.

4. **Regression Discontinuity (RD):** Use threshold-based treatment (e.g., minimum test score for scholarship). Compare just above vs just below threshold.

5. **Propensity Score Matching:** Balance treated and control groups on observed confounders.

6. **Controlled regression:** Control for all known confounders (but can't control for unknowns).

↳ **Cross Q:** "Our model shows that users who see more ads convert more. Should we show more ads?" — What's the causal question here?
↳ **Cross A:** This is a classic confounding problem. The observed correlation "ads seen → conversion" could be: (1) Causal: ads persuade users to buy (what we want). (2) Reverse: users who intended to buy (high purchase intent) engaged more with the site → saw more ads. (3) Confounded: mobile users see more ads AND convert more (both caused by engagement level). To establish causation: run an A/B test where some users are randomly shown MORE ads (treatment) vs fewer (control). If treatment group converts more, the causal effect is confirmed. Without randomization, showing more ads to all users might have ZERO effect if the correlation was driven by selection (users who see more ads were already going to buy).

---

## Q11. 🔥 What is Anscombe's Quartet? What lesson does it teach data scientists?

**A:**
**Anscombe's Quartet** (1973) consists of four datasets that are nearly identical in standard statistical summary:

| Statistic | Dataset I | Dataset II | Dataset III | Dataset IV |
|-----------|-----------|-----------|------------|------------|
| Mean of X | 9.00 | 9.00 | 9.00 | 9.00 |
| Mean of Y | 7.50 | 7.50 | 7.50 | 7.50 |
| SD of X | 3.32 | 3.32 | 3.32 | 3.32 |
| SD of Y | 2.03 | 2.03 | 2.03 | 2.03 |
| Correlation | 0.816 | 0.816 | 0.816 | 0.816 |
| Regression line | y=3+0.5x | y=3+0.5x | y=3+0.5x | y=3+0.5x |

Yet when plotted, they look completely different:
- Dataset I: Standard linear relationship (linear regression valid)
- Dataset II: Curved quadratic relationship (need polynomial)
- Dataset III: Linear with one massive outlier
- Dataset IV: Vertical scatter with one leverage point driving the line

**Lesson:** Summary statistics NEVER replace visualization. The same numbers can hide wildly different data structures. Always plot your data before analysis!

**ML implications:**
- Features with similar means/SDs can have completely different distributions
- Two models with identical validation MSE can have very different behavior on specific subgroups
- Always visualize: distributions, scatter plots, residuals, learning curves
- "The Datasaurus Dozen" (2017): extended Anscombe to 13 datasets — all with identical statistics but wildly different scatter plot shapes

↳ **Cross Q:** If two regression models have the same R², how do you choose between them?
↳ **Cross A:** R² only measures overall fit — equal R² can hide very different model behaviors. Additional diagnostics: (1) Residual plots: plot residuals vs fitted values — should be random scatter. Systematic patterns (curves, fans) indicate model misspecification. (2) QQ plot of residuals: checks normality assumption. (3) Scale-location plot: checks homoscedasticity. (4) Cook's distance / leverage plots: identifies influential points. (5) Prediction intervals: check if they're appropriate width. (6) Out-of-sample performance (test set R²): prevents overfitting comparison. (7) Complexity: prefer simpler model (parsimony) if R² is similar. The residual plot is the single most important diagnostic — study it before trusting any R² value.

---

## Q12. 🔥 What is the Empirical Rule? When can you use it?

**A:**
The **Empirical Rule** (68-95-99.7 rule) applies to normally distributed data:

- 68% of data falls within μ ± 1σ
- 95% of data falls within μ ± 2σ (precisely: 1.96σ)
- 99.7% of data falls within μ ± 3σ

**Conditions for use:**
1. Data must be approximately normally distributed
2. Mean and standard deviation must be defined and finite
3. Sample must be large enough for mean/SD to be reliable

**Applications:**

1. **Quality control:** A process with μ=100, σ=5 should produce products in [85,115] ≈ 99.7% of the time. Values outside this range trigger investigation.

2. **Anomaly detection:** Values beyond μ ± 3σ are flagged as anomalies (~0.3% rate).

3. **Finance risk:** "1% VaR" = loss exceeded 1% of the time ≈ μ − 2.33σ.

4. **Grading on a curve:** If scores ~ N(μ,σ²): A: above μ+1σ (~16%); B: μ to μ+1σ (~34%); C: μ-1σ to μ (~34%); etc.

**When NOT to use:**
- Heavy-tailed distributions (stock returns, insurance claims): 99.7% rule fails badly
- Skewed distributions: asymmetric, rule doesn't apply
- Small samples where normality is unclear

↳ **Cross Q:** What is Chebyshev's inequality and how does it compare to the Empirical Rule?
↳ **Cross A:** Chebyshev's inequality: P(|X-μ| ≥ kσ) ≤ 1/k² — works for ANY distribution. For k=2: P(outside 2σ) ≤ 25%. For k=3: P(outside 3σ) ≤ 11.1%. Comparison: Normal distribution (Empirical Rule) is much tighter — P(outside 2σ) = 4.55%, not 25%; P(outside 3σ) = 0.27%, not 11.1%. Chebyshev provides a distribution-free bound (worst case). When the distribution IS known to be normal, use the empirical rule (tighter bound). When distribution is unknown, use Chebyshev as a conservative guarantee. In ML: Chebyshev can bound generalization error without distributional assumptions — useful in PAC learning theory.

---

## Q13. 🔥⭐ What is a correlation matrix and what are its properties? How is it used in ML?

**A:**
A **correlation matrix** R is a p×p symmetric matrix where Rᵢⱼ = Pearson r between variables Xᵢ and Xⱼ.

**Properties:**
1. **Diagonal = 1:** Every variable is perfectly correlated with itself
2. **Symmetric:** Rᵢⱼ = Rⱼᵢ
3. **Bounded:** All entries in [−1, +1]
4. **Positive semi-definite:** All eigenvalues ≥ 0
5. **Full rank = p if no perfect multicollinearity**

**Example (4-variable dataset):**
```
          Age    Income   Score   Spend
Age    [  1.00    0.65    -0.12    0.31 ]
Income [  0.65    1.00    -0.05    0.78 ]
Score  [ -0.12   -0.05    1.00    -0.18 ]
Spend  [  0.31    0.78   -0.18    1.00  ]
```

**ML uses:**

1. **Feature selection:** Remove one feature from each highly correlated pair (|r|>0.9)
2. **Multicollinearity detection:** High correlations → VIF issues in linear regression
3. **PCA:** PCA decomposes the correlation/covariance matrix
4. **Feature engineering insight:** High correlation → consider creating ratio feature
5. **EDA:** First step in understanding relationships between variables
6. **Portfolio optimization:** Asset correlation matrix used to minimize portfolio variance

↳ **Cross Q:** What is the difference between correlation matrix and covariance matrix? Which does PCA use?
↳ **Cross A:** Covariance matrix Σ: entries are Cov(Xᵢ,Xⱼ) — units are products of variable units. Scale-dependent — features with large variance dominate. Correlation matrix R: entries are Pearson r — unitless, scale-free. PCA on Σ: gives principal components in the original scale — features with high variance dominate even if they're unimportant. PCA on R (or equivalently, on standardized data): gives principal components that treat all features equally. Rule: if features are in different units (income in ₹, age in years, height in cm), use correlation-based PCA (standardize first). If features are in the same unit and you care about variance differences, use covariance-based PCA. In practice: almost always standardize features before PCA.

---

# SECTION 3 — ADVANCED LEVEL

---

## Q14. ⭐ What is the difference between descriptive and inferential statistics? Why does this matter?

**A:**
**Descriptive Statistics:** Summarizes and describes the data you HAVE. No generalization.
- Computes: mean, median, SD, percentiles, correlation, frequencies
- Valid only for the specific dataset (population or sample)
- No uncertainty involved — these are exact facts about your data
- Examples: "The average exam score in THIS class is 75." "42% of OUR customers churned last month."

**Inferential Statistics:** Uses sample data to make inferences about the broader POPULATION.
- Involves uncertainty: estimates, confidence intervals, hypothesis tests
- Requires sampling assumptions (random, representative)
- Results are probabilistic: "We estimate with 95% confidence that the true mean is 70-80."
- Examples: "Based on 500 surveyed customers, we estimate ~45% of ALL customers would buy this."

**Why the distinction matters:**

1. **Common mistake:** Treating descriptive statistics as if they apply to unseen data. "Our training accuracy is 92%" is descriptive — it tells you nothing about future (test) performance.

2. **Train-test split:** Training metrics = descriptive. Test metrics + confidence intervals = inferential.

3. **Model deployment:** A model's performance on test set is descriptive of test set. Inferential statistics (confidence intervals, hypothesis tests) needed to claim it will generalize.

4. **Business decisions:** "Our sample shows 5% conversion" is descriptive. "We estimate 4.2-5.8% true conversion with 95% confidence" is inferential — and more honest.

↳ **Cross Q:** When is a descriptive statistic also an inferential claim?
↳ **Cross A:** When the "population" IS your dataset — e.g., you have census data for all employees, or all transactions in a period. Then mean salary of ALL employees is purely descriptive. But if you use LAST YEAR's data to infer THIS YEAR's pattern, you're making an inferential claim (using one time period to generalize to another). The distinction also matters in ML: a model's training accuracy is purely descriptive of training data. Test accuracy is descriptive of test data AND inferential about future performance (assuming test data is representative of future data — this assumption is often violated, e.g., temporal drift).

---

## Q15. 🔥⭐ How do you handle multicollinearity in regression? How do you detect it?

**A:**
**Multicollinearity:** Two or more predictor variables are highly correlated, making it impossible to isolate individual effects.

**Effects of multicollinearity:**
1. Coefficients become unstable (large standard errors)
2. Hypothesis tests for individual coefficients become unreliable
3. Coefficient signs may flip or have counterintuitive values
4. Predictions may still be accurate (multicollinearity doesn't bias predictions, only coefficient estimates)

**Detection methods:**

**1. Correlation matrix:** If |rᵢⱼ| > 0.8 between predictors → warning

**2. Variance Inflation Factor (VIF):**
VIF_j = 1/(1-R²_j) where R²_j = R² from regressing Xⱼ on all other X variables.
- VIF = 1: No multicollinearity
- VIF = 5: Moderate (warning)
- VIF > 10: Severe multicollinearity (action required)

```python
from statsmodels.stats.outliers_influence import variance_inflation_factor
vif_data = pd.DataFrame()
vif_data["feature"] = X.columns
vif_data["VIF"] = [variance_inflation_factor(X.values, i) 
                    for i in range(len(X.columns))]
```

**3. Condition number** of X matrix: > 30 suggests multicollinearity; > 1000 severe

**Solutions:**
| Solution | When to Use |
|----------|------------|
| Drop one correlated feature | When features are redundant |
| Create ratio/difference feature | When features measure similar things |
| Ridge regression (L2) | Keep all features but shrink coefficients |
| PCA on correlated features | Transform to orthogonal components |
| Partial Least Squares | PCA-like but maximizes covariance with Y |

↳ **Cross Q:** Why doesn't multicollinearity affect prediction accuracy?
↳ **Cross A:** Multicollinearity affects INTERPRETATION of coefficients, not prediction. Even if we can't separately estimate β₁ and β₂ when X₁ and X₂ are correlated, we can still accurately estimate β₁X₁+β₂X₂ (the combined effect) when the new data has similar X₁,X₂ correlation. The model doesn't need to know how to "split" the effect between X₁ and X₂ — it just needs to predict Y. This is why: (1) RF and XGBoost handle multicollinearity fine for prediction. (2) Ridge regression gives biased (but stabilized) coefficients that still predict well. (3) The predictive failure occurs only when new data has DIFFERENT X₁,X₂ correlation than training data (out-of-distribution).

---

## Q16. ⭐ What is the concept of statistical power in feature selection?

**A:**
When testing whether a feature has a significant relationship with the target (e.g., using correlation test, chi-square, ANOVA), statistical power matters.

**Power = P(correctly detecting a real relationship)**

**Low power leads to:**
- Features with real predictive value deemed "not significant" (Type II errors)
- Important features incorrectly dropped
- Model missing key predictors

**Factors affecting power in feature selection:**
1. **Sample size n:** More data → higher power → detect weaker relationships
2. **Effect size:** Larger true relationship → easier to detect
3. **Significance level α:** Higher α → more power but more false discoveries
4. **Number of tests:** With p features, multiple testing problem arises

**Multiple testing in feature selection:**
Testing p=100 features at α=0.05: expect 5 false positives even if no features are related.

**Corrections:**
- **Bonferroni:** α_corrected = 0.05/100 = 0.0005 per test (very conservative)
- **Benjamini-Hochberg (FDR control):** Control false discovery rate at q% rather than false positive rate. More powerful than Bonferroni. Recommended for feature selection.

**Practical guidance:**
With small datasets (n<500), statistical feature selection has low power — many real relationships will be missed. Better approach: use regularization (L1) which implicitly selects features, or domain knowledge + cross-validated model selection.

↳ **Cross Q:** What is the difference between feature selection and feature importance?
↳ **Cross A:** Feature selection: binary decision — include or exclude a feature from the model. Feature importance: continuous measure of how much a feature contributes to model performance. Feature importance from a fitted model: (1) Tree importance: average decrease in Gini/entropy from splits on this feature. (2) Permutation importance: how much does model accuracy decrease when this feature is randomly shuffled? (3) SHAP values: game-theoretic attribution of prediction to each feature per sample. Feature selection based purely on statistical tests ignores feature interactions — a feature might have low univariate correlation with the target but high importance in combination with other features. Model-based feature selection (L1, recursive feature elimination, SHAP) is generally better because it captures these interactions.

---

## Q17. 🔥 What is a QQ plot and how do you interpret it?

**A:**
A **QQ (Quantile-Quantile) plot** compares the quantiles of your data against the quantiles of a theoretical distribution (usually Normal).

**How to construct:**
1. Sort data x₁ ≤ x₂ ≤ ... ≤ xₙ
2. Compute theoretical quantiles for a standard Normal at positions (i-0.5)/n
3. Plot: x-axis = theoretical quantiles, y-axis = sample quantiles
4. If data perfectly matches theoretical distribution → straight line y=x

**Interpretation patterns:**

```
NORMAL (good):          HEAVY TAILS:           LIGHT TAILS:
                        (leptokurtic)          (platykurtic)
  •                  •                            •
 •                       •                    • •
• •                   •••                  ••
•                   •••                 •••
                  •••
Data on diagonal  S-shape curving       Reverse S-shape
→ normally dist.  outward at ends       inward at ends

RIGHT SKEW:             LEFT SKEW:
    •                           •
   •                           •
  •                           •
 ••                          ••
•••                        •••
Curve upward right     Curve downward left
```

**Common patterns and what they mean:**
- **Straight diagonal:** Data follows assumed distribution ✅
- **S-curve (sigmoidal):** Heavy tails (kurtosis > 3)
- **Reverse S-curve:** Light tails (kurtosis < 3)
- **Upward curve at right end:** Right skewness
- **Downward curve at left end:** Left skewness
- **Steps/jumps:** Discrete data or rounding
- **Single outlier point:** One extreme outlier

↳ **Cross Q:** What should you do if regression residuals show a curved QQ plot?
↳ **Cross A:** A curved QQ plot of residuals means the normality assumption for OLS regression is violated. Actions depend on the pattern: (1) Right-skewed residuals (curve up at right): transform Y — try log(Y) or √Y. The response distribution may be log-normal or Gamma. (2) Heavy-tailed residuals (S-curve): outliers or truly heavy-tailed errors. Options: robust regression (Huber loss), remove verified outliers, or use a model that doesn't assume normal errors (tree models). (3) Left-skewed residuals: try Y² or exp(Y) transformation. (4) If none work: consider GLM with appropriate error distribution (Gamma, Inverse Gaussian for positive right-skewed Y). Remember: non-normal residuals don't affect coefficient estimates (they're still unbiased via OLS), but they DO invalidate confidence intervals and hypothesis tests on coefficients.

---

# 📌 QUICK REFERENCE — Descriptive Statistics Cheatsheet

```
CENTRAL TENDENCY
Mean     x̄ = Σxᵢ/n              ← sensitive to outliers
Median   middle value sorted      ← robust to outliers
Mode     most frequent value      ← for categorical data
Rule:    skewed data? → Median. Normal? → Mean. Categorical? → Mode

SPREAD
Range          Max - Min                  ← most sensitive to outliers
Variance       Σ(xᵢ-x̄)²/(n-1)            ← squared units
Std Dev        √Variance                  ← same units
IQR            Q3 - Q1                    ← robust, for box plots
CV             (σ/μ)×100%                 ← scale-free comparison

SHAPE
Skewness  γ₁ > 0: right tail  γ₁ < 0: left tail  γ₁ ≈ 0: symmetric
Kurtosis  γ₂ > 3: heavy tails (leptokurtic)  γ₂ < 3: light tails (platykurtic)
          excess kurtosis = γ₂ - 3; Normal has excess kurtosis = 0

OUTLIER DETECTION
IQR method: outside Q1-1.5×IQR or Q3+1.5×IQR
Z-score:    |z| > 3 (for normal data)
Both methods often disagree on borderline cases — investigate manually

CORRELATION
Pearson r:   linear relationships, sensitive to outliers
Spearman rₛ: monotonic relationships, robust
r = 0 ≠ independent — only means no LINEAR relationship

MUST-KNOW RELATIONSHIPS
Right skewed:  Mean > Median > Mode
Left skewed:   Mean < Median < Mode
Normal/symmetric: Mean ≈ Median ≈ Mode
Multicollinearity: VIF > 10 = severe. Use Ridge or drop features.
```

---

*Batch 3 Complete: 17 Questions | Basic (6) → Intermediate (7) → Advanced (4)*
*Next: Batch 4 — Inferential Statistics & Hypothesis Testing*
