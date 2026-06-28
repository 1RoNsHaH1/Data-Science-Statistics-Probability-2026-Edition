# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 3 — Descriptive Statistics
# Chapters 3.4–3.6: Percentiles | Covariance & Correlation | Visualization

---

# Chapter 3.4 — Percentiles & Quartiles

## 💡 Intuition

> **A percentile tells you: what percentage of data falls BELOW this value?**

If you scored in the **90th percentile** on JEE, it means 90% of test-takers scored lower than you. You outperformed 90% of students.

```
Data sorted from lowest to highest:
│──────────────────│────────────│
│  90% of data     │  10% above │
                   ↑
              90th percentile
```

---

## 3.4.1 — Percentiles

### 📖 Definition

The **p-th percentile** is the value below which p% of the data falls.

**Key percentiles:**

| Percentile | Common Name | Meaning |
|-----------|-------------|---------|
| 25th | Q1 (First Quartile) | 25% below this value |
| 50th | Q2 / Median | 50% below (half the data) |
| 75th | Q3 (Third Quartile) | 75% below this value |
| 10th, 20th, ... | Deciles | Every 10% |
| Any Pth | P-th percentile | P% below |

### 📊 How to Find a Percentile

**Step 1:** Sort data in ascending order
**Step 2:** Compute position: L = (P/100) × n
**Step 3:** If L is whole number: average values at position L and L+1. If L is decimal: round up to next whole number.

**Example:** Data = [10, 20, 25, 30, 35, 40, 45, 50, 60, 70], n=10

75th percentile:
```
L = (75/100) × 10 = 7.5 → round up to 8th value
8th value = 50
P75 = 50
```

25th percentile:
```
L = (25/100) × 10 = 2.5 → round up to 3rd value
3rd value = 25
P25 = 25
```

IQR = 50 - 25 = 25

### 🌍 Real-Life Uses

| Context | Percentile Used |
|---------|----------------|
| JEE/NEET rankings | 99th percentile = top 1% |
| Child growth charts | Height at 75th percentile for age |
| ML model latency | P99 latency = 99th percentile response time |
| Salary benchmarking | "You're at 80th percentile" |
| Load testing | P95, P99 response time thresholds |

> 🚀 **Data Science:** P99 latency — if P99 = 200ms, it means 99% of requests complete within 200ms, and 1% take longer. Critical for API SLA monitoring.

---

## 3.4.2 — Quartiles

Quartiles divide sorted data into **four equal parts**:

```
Data: ────────────────────────────────────────────
       0%     25%      50%      75%     100%
               Q1      Q2/Med   Q3
               ↑        ↑        ↑
           First      Second   Third
           Quartile   Quartile  Quartile

Q1: 25th percentile — bottom quarter boundary
Q2: 50th percentile — median (middle)
Q3: 75th percentile — upper quarter boundary
IQR: Q3 - Q1 (middle 50% range)
```

### 📊 Five-Number Summary

The **Five-Number Summary** captures the full picture of a distribution:

```
[ Min | Q1 | Median | Q3 | Max ]

Example: Student exam scores
Min=42, Q1=65, Median=75, Q3=85, Max=98

Interpretation:
- Lowest score: 42 (probably a special case)
- Bottom quarter scored below 65
- Half scored below 75
- Top quarter scored above 85
- Highest score: 98
```

---

## 3.4.3 — Box Plots (Box-and-Whisker Plots)

### 💡 Intuition

Box plots visualize the five-number summary in a single graphic.

```
          ┌──────────────────┐
    ●     │    ┃             │         ●   ●
──────────┤────┃─────────────├─────────────────
         Min  Q1   Median   Q3        Max
    ↑                                      ↑
  Outlier                              Outlier
(below Q1-1.5×IQR)              (above Q3+1.5×IQR)

Whiskers extend to most extreme non-outlier values
Dots represent outliers
```

### 📊 Reading Box Plots

**Box width** = IQR → wide box = high variability in middle 50%
**Median position in box:**
- Median closer to Q1 → right skewed
- Median closer to Q3 → left skewed
- Median centered → symmetric

**Comparing multiple groups with box plots:**
```
     Math    Science  English
     ┌──┐     ┌──┐    ┌──────┐
     │  │     │  │    │      │
 ●───┤  ├───  │  │    │      │
     │  │   ──┤  ├──  │      │──●
     └──┘     └──┘    └──────┘

Math: tight range, some low outliers
Science: similar range, tighter
English: very wide spread (diverse class)
```

---

# Chapter 3.5 — Covariance & Correlation

> **How do two variables move together?**

---

## 💡 Intuition

- Does studying more lead to higher scores? (**positive relationship**)
- Does eating more junk food lead to lower fitness? (**negative relationship**)
- Does shoe size predict IQ? (**no relationship**)

Covariance and correlation **quantify the relationship** between two variables.

---

## 3.5.1 — Covariance

### 💡 Intuition

> Covariance measures whether two variables tend to move **in the same direction** (both go up together, both go down together) or **opposite directions** (one goes up while the other goes down).

```
POSITIVE COVARIANCE:
When X is above its mean, Y tends to be above its mean too.
X ↑ → Y ↑  |  X ↓ → Y ↓
Example: Temperature ↑ → Ice cream sales ↑

NEGATIVE COVARIANCE:
When X is above its mean, Y tends to be below its mean.
X ↑ → Y ↓  |  X ↓ → Y ↑
Example: Temperature ↑ → Jacket sales ↓

ZERO COVARIANCE:
No consistent pattern between X and Y.
X and Y move independently.
```

### 📖 Formula

**Population:**
$$\text{Cov}(X, Y) = \frac{1}{N}\sum_{i=1}^{N}(x_i - \mu_X)(y_i - \mu_Y)$$

**Sample:**
$$\text{Cov}(X, Y) = \frac{1}{n-1}\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})$$

| Symbol | Meaning |
|--------|---------|
| (xᵢ - x̄) | How far X is from its mean |
| (yᵢ - ȳ) | How far Y is from its mean |
| Product | + if both above mean; + if both below; − if opposite |
| Average | Average tendency to move together |

### 📊 Worked Example

Hours studied (X): [2, 4, 6, 8, 10], x̄ = 6
Test scores (Y): [55, 65, 75, 85, 95], ȳ = 75

```
(xᵢ-x̄): -4, -2, 0, +2, +4
(yᵢ-ȳ): -20, -10, 0, +10, +20

Products: 80, 20, 0, 20, 80

Cov = (80+20+0+20+80) / (5-1) = 200/4 = 50

Positive covariance = positive relationship ✅
```

### ⚠️ The Problem with Covariance

Covariance has no fixed scale — it depends on the units of X and Y.

Cov = 50 for (hours, score)
Cov = 50,000 for (hours, income-in-rupees)

You can't compare covariances across different datasets or different unit systems. **This is why we use correlation.**

---

## 3.5.2 — Pearson Correlation Coefficient

### 💡 Intuition

> Correlation is **standardized covariance** — it removes the units and always gives a value between -1 and +1.

$$r = \frac{\text{Cov}(X, Y)}{\sigma_X \times \sigma_Y}$$

### 📐 Full Formula

$$r = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum(x_i-\bar{x})^2 \cdot \sum(y_i-\bar{y})^2}}$$

| Value | Relationship |
|-------|-------------|
| r = +1 | Perfect positive linear relationship |
| r = +0.7 to +0.9 | Strong positive |
| r = +0.4 to +0.6 | Moderate positive |
| r = 0 to ±0.2 | Weak or no linear relationship |
| r = -0.4 to -0.6 | Moderate negative |
| r = -0.7 to -0.9 | Strong negative |
| r = -1 | Perfect negative linear relationship |

### 🎨 Visual Guide to Correlation Values

```
r = +1.0        r = +0.7        r = 0.0         r = -0.7       r = -1.0
    *               *  *          * * *  *         *    *            *
   *             *    *          * *  *  *          *  *          *
  *            *       *        *  * *   *           * *        *
 *            *         *      *  *   *   *           **      *
*            *           *    *    * *     *            *   *
                                                          *

Perfect      Strong       No linear    Strong        Perfect
positive    positive      relation    negative       negative
```

### 📊 Worked Example

Using the same data as before:
- σ_X = √10 ≈ 3.162
- σ_Y = √250 ≈ 15.81
- Cov(X,Y) = 50

r = 50 / (3.162 × 15.81) = 50/49.99 ≈ **1.0**

Perfect positive correlation — makes sense since Y = 10X + 35 exactly.

### ⚠️ Critical: Correlation ≠ Causation

> **Correlation shows a relationship exists. It does NOT tell you WHY.**

**Famous Spurious Correlations:**
- Ice cream sales and drowning deaths are strongly correlated (both caused by hot weather — a confound)
- Nicolas Cage movies released per year correlates with pool drowning deaths
- Number of TVs per household correlates with life expectancy

```
CORRELATION:
Ice cream sales ──────────────────→ Drowning deaths
(r = 0.95, but NO causal link!)

REALITY:
         Hot Weather
        ↙           ↘
Ice cream sales    Swimming → Drowning deaths

Hot weather CAUSES both. No direct relationship.
```

---

## 3.5.3 — Spearman Rank Correlation

### 💡 Intuition

Pearson correlation measures **linear** relationships. Spearman measures **monotonic** relationships (any consistently increasing or decreasing trend, even if not linear).

Spearman works by **ranking** the data first, then computing Pearson on the ranks.

$$r_s = 1 - \frac{6\sum d_i^2}{n(n^2-1)}$$

Where $d_i$ = difference in ranks for each pair.

### When to Use Spearman vs Pearson

| Use Pearson when... | Use Spearman when... |
|--------------------|---------------------|
| Relationship is linear | Relationship is monotonic but not linear |
| Data is continuous, normal | Data is ordinal |
| No major outliers | Outliers present |
| Both variables numeric | Ranked data |

**Example:** Customer satisfaction rank vs. repeat purchase rank → Spearman is appropriate (ordinal data, not interval).

---

## 3.5.4 — Correlation Matrix

### 💡 Intuition

When you have multiple variables, a **correlation matrix** shows the correlation between every pair at once.

```
Example: Dataset with 4 features: Age, Income, Score, Spend

              Age    Income   Score   Spend
    Age   [  1.00    0.65    -0.12    0.31 ]
    Income[ 0.65    1.00    -0.05    0.78 ]
    Score [ -0.12  -0.05    1.00    -0.18 ]
    Spend [ 0.31    0.78   -0.18    1.00 ]

Reading:
- Age & Income: r=0.65 → Strong positive
- Income & Spend: r=0.78 → Strong positive (higher earners spend more)
- Score & Income: r=-0.05 → Almost no relationship
- Age & Score: r=-0.12 → Very weak negative
```

### 🚀 Data Science Applications of Correlation

| Application | How Correlation is Used |
|-------------|------------------------|
| **Feature selection** | Remove highly correlated features (multicollinearity) |
| **EDA** | Identify which features correlate with target |
| **PCA** | Correlation structure drives principal components |
| **Recommendation systems** | Item-item or user-user correlation |
| **Portfolio management** | Diversify assets with low correlation |
| **Multicollinearity check** | VIF uses correlation matrix |

---

# Chapter 3.6 — Data Visualization for Statistics

> **The best statistical insight is often a picture. Learn to read and create the right plot for the right situation.**

---

## 3.6.1 — Histograms

### 💡 Intuition

A histogram shows the **distribution of a single numerical variable** by dividing the range into bins and counting how many values fall in each bin.

```
Exam scores histogram (bin width = 10):

Frequency
40 │       ████
35 │     ████████
30 │   ████████████
25 │ ████████████████
20 │   ████████████████
15 │       ████████
10 │           ████
   └─────────────────────── Score
     40  50  60  70  80  90  100

What it reveals:
- Most students scored 60-80 (peak)
- Right skew: fewer high scorers
- No one scored below 40
```

### 🔑 Key Decisions

**Bin width matters:**
```
Too few bins:         Too many bins:      Just right:
████████ (loses       ||||||||||||||||     ████
  detail)             (too noisy)        ████████
                                        ██████████
```

**Rule of thumb:** Use √n bins for n data points, or Sturges' Rule: k = 1 + 3.322 × log₁₀(n)

---

## 3.6.2 — Kernel Density Estimation (KDE) Plots

### 💡 Intuition

A KDE plot is a **smoothed histogram** — instead of counting bars, it draws a smooth curve showing where data is concentrated.

```
Histogram:              KDE:
██                       /\
████                   /    \
████████              /      \
██████████           /        \
████████████        /            \
───────────    ──────────────────────

Bars = counts   Smooth curve = density
```

KDE is useful when:
- You want a smooth view of the distribution
- Comparing multiple groups on the same plot
- The underlying distribution is continuous

---

## 3.6.3 — Box Plots (Revisited in Visualization Context)

Best for:
- Comparing distributions across multiple groups
- Spotting outliers visually
- Seeing spread and skewness quickly

```
Income by Education Level:

High School    Bachelors    Masters     PhD
    ┌──┐         ┌──┐       ┌──┐      ┌──┐
●───┤  ├──     ──┤  ├──   ──┤  ├──   ─┤  ├────●
    └──┘         └──┘       └──┘      └──┘
   (narrow,     (wider,    (wider    (widest,
   lower)       higher)    still)    highest)
```

---

## 3.6.4 — Scatter Plots

Best for visualizing **relationships between two variables**.

```
Hours Studied vs Exam Score:

Score
100 │                            * *
 90 │                        *  *
 80 │                   * *  *
 70 │              * *  *
 60 │         * *  *
 50 │    * *  *
 40 │
    └──────────────────────────────── Hours
     1    2    3    4    5    6    7

Strong positive correlation visible
Each dot = one student
```

**Adding a third dimension:** color the dots by a third variable (e.g., student's attendance %).

---

## 3.6.5 — Violin Plots

A violin plot combines a box plot with a KDE — showing both the summary statistics AND the full distribution shape.

```
         Violin Plot:
         Group A         Group B

          │ │               │ │
         ╱   ╲             ╱   ╲
        │     │           │     │
       ╱       ╲         ╱       ╲
      │         │       │         │
       ╲       ╱         ╲       ╱
        │     │           │     │
         ╲   ╱             ╲   ╱
          │ │               │ │

Thin violin = few data points there
Fat violin = many data points there
```

---

## 3.6.6 — Heatmaps

Best for visualizing **correlation matrices** or any 2D grid of values.

```
Correlation Heatmap:

          Age   Income  Score  Spend
   Age  [████  ████▒▒  ░░░░░  ████▒]
Income  [████▒  █████  ░░░░░  ████▒]
 Score  [░░░░  ░░░░░   █████  ░░░░░]
 Spend  [████▒  ████▒  ░░░░░  █████]

█████ = dark red = high positive correlation (≈1.0)
░░░░░ = white    = near zero correlation (≈0.0)
▒▒▒▒▒ = dark blue = negative correlation
```

---

## 3.6.7 — Choosing the Right Plot

```
What do you want to show?
│
├── DISTRIBUTION of one variable?
│   ├── Continuous → Histogram or KDE
│   ├── Categorical → Bar chart
│   └── Summary stats → Box plot or Violin
│
├── RELATIONSHIP between two variables?
│   ├── Both numerical → Scatter plot
│   ├── One categorical, one numerical → Box plot grouped
│   └── Two categorical → Heatmap of counts
│
├── COMPARISON across groups?
│   └── Box plot, violin plot, grouped bar chart
│
├── TREND over time?
│   └── Line chart
│
└── CORRELATION between many variables?
    └── Correlation heatmap, pair plot
```

---

## 🐍 Python Code Examples

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Sample data
scores = np.random.normal(75, 10, 200)
df = pd.DataFrame({'score': scores, 'group': np.random.choice(['A','B'], 200)})

# --- HISTOGRAM ---
plt.figure(figsize=(8, 5))
plt.hist(scores, bins=20, edgecolor='black', color='steelblue')
plt.xlabel('Score')
plt.ylabel('Frequency')
plt.title('Distribution of Scores')
plt.show()

# --- KDE PLOT ---
sns.kdeplot(scores, fill=True)
plt.title('Score Density')
plt.show()

# --- BOX PLOT ---
sns.boxplot(x='group', y='score', data=df)
plt.title('Scores by Group')
plt.show()

# --- VIOLIN PLOT ---
sns.violinplot(x='group', y='score', data=df)
plt.title('Score Distribution by Group')
plt.show()

# --- SCATTER PLOT ---
x = np.random.normal(5, 2, 100)
y = 3 * x + np.random.normal(0, 2, 100)
plt.scatter(x, y, alpha=0.6)
plt.xlabel('Hours Studied')
plt.ylabel('Score')
plt.title('Study Hours vs Score')
plt.show()

# --- CORRELATION HEATMAP ---
data = pd.DataFrame(np.random.randn(100, 4), columns=['A','B','C','D'])
sns.heatmap(data.corr(), annot=True, cmap='RdYlGn', center=0)
plt.title('Correlation Matrix')
plt.show()
```

---

# 📝 Practice Questions — Chapters 3.4–3.6

## 🟢 Beginner (8 Questions)

**Q1.** Data: [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]. Find Q1, Q2, Q3, IQR.

**Q2.** If your height is at the 70th percentile for your age, what does that mean?

**Q3.** Cov(X,Y) = 35, σ_X = 5, σ_Y = 7. Find Pearson r.

**Q4.** True or False: Correlation of +0.2 means a weak relationship, while -0.8 means a strong relationship.

**Q5.** What type of plot would you use to compare exam score distributions of male and female students?

**Q6.** What plot best shows the relationship between advertising spend and sales revenue?

**Q7.** r = -0.95 between temperature and gas usage. Interpret this correlation.

**Q8.** When should you use a KDE plot instead of a histogram?

## 🟡 Intermediate (7 Questions)

**Q9.** Compute Cov(X,Y) for X=[1,2,3,4,5] and Y=[2,4,5,4,5].

**Q10.** Two datasets: Dataset A has r=0.90 between X and Y. Dataset B has r=0.90 between log(X) and Y. Which one has a linear relationship and which has an exponential relationship?

**Q11.** Correlation matrix shows r=0.95 between "total_rooms" and "total_bedrooms" in a housing dataset. What problem does this cause for linear regression? What should you do?

**Q12.** Why is it wrong to say "studying more CAUSES higher scores" based on a positive correlation alone?

**Q13.** P99 latency of an API = 450ms. What does this mean? If we want P99 ≤ 200ms, what percentile threshold is our current system failing?

**Q14.** Construct a box plot description for: Min=15, Q1=30, Median=45, Q3=60, Max=95, with one outlier at 130.

**Q15.** Anscombe's Quartet: Four datasets all have the same mean, SD, and correlation (r=0.816) but look completely different when plotted. What lesson does this teach?

## 🔴 Advanced (5 Questions)

**Q16.** Prove that Pearson r is always between -1 and 1 using the Cauchy-Schwarz inequality.

**Q17.** Point-biserial correlation: When one variable is binary (0/1) and the other is continuous, how does Pearson correlation work? When would Spearman be preferred?

**Q18.** Explain why removing outliers can artificially inflate a correlation coefficient. Give a numerical example.

**Q19.** Intraclass Correlation Coefficient (ICC): How does it differ from Pearson r, and when is it used?

**Q20.** Mutual Information vs Correlation: When does correlation fail to detect a relationship that Mutual Information captures?

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** Feature selection: A correlation matrix for 20 features shows 3 pairs with |r| > 0.90. You need to reduce features from 20 to around 17. Which features from each correlated pair do you keep and why?

**DS2.** You're building a churn model. Plot the distribution of "days_since_last_login" for churned vs non-churned users. What plot type would you use? What differences would you expect to see?

**DS3.** A/B test visualization: You have conversion rates for 1000 experiments with two variants each. What plots would you use to understand (a) distribution of conversion rates, (b) difference between variants, (c) whether the difference is consistent?

**DS4.** Correlation heatmap shows income has r=0.03 with churn. Does this mean income is not useful for predicting churn? What additional analysis would you do?

**DS5.** Anscombe's lesson applied to ML: Why should you always visualize your data even when summary statistics look good? Give an ML-specific example of where poor visualization led to a bad model.

---

# ✅ Key Solutions

**A1.** Sorted: [10,20,30,40,50,60,70,80,90,100]. Q1=25 (avg of 20,30). Q2=55 (avg of 50,60). Q3=75 (avg of 70,80). IQR=50.

**A3.** r = 35/(5×7) = 35/35 = **1.0**. Perfect positive correlation.

**A4.** True. The sign indicates direction; the magnitude (absolute value) indicates strength. |−0.8|=0.8 > |0.2|=0.2 → stronger relationship.

**A9.** x̄=3, ȳ=4. Deviations: x:(-2,-1,0,1,2), y:(-2,0,1,0,1). Products: 4,0,0,0,2. Sum=6. Cov=6/(5-1)=1.5.

**A11.** Multicollinearity: both features contain nearly identical information. Linear regression coefficients become unstable and uninterpretable. Solution: Drop one feature, use PCA to combine them, or use Ridge regression which handles multicollinearity.

**A13.** 99% of API requests complete within 450ms; 1% take longer. The system is meeting 99th percentile requirements but not the 200ms target. The current system is meeting P99≤450ms but failing to meet P99≤200ms.

**A15.** Always visualize your data! Summary statistics alone are insufficient. The same statistics can correspond to wildly different underlying data patterns. In ML: a model can have perfect training metrics but terrible real-world performance if you don't investigate the data distribution visually.

**DS1.** For each correlated pair (r>0.90): (1) Keep the one with higher correlation to the target variable. (2) Keep the one that's more interpretable. (3) Keep the one with less missing data. Never mechanically drop without checking target correlation.

**DS2.** Box plot or violin plot comparing the two groups. Expected: churned users will have higher values (longer time since login), more right-skewed distribution, higher variance. Alternatively, overlapping KDE plots for both groups.

**DS4.** No — r=0.03 only measures LINEAR relationships. Income might have a non-linear (U-shaped) relationship with churn (both very low and very high income users churn, middle-income users stay). Use: (1) Plot income vs churn rate by decile, (2) Use mutual information, (3) Try binning income and check chi-square test.

---

# 💼 Interview Questions

**IQ1.** "How would you detect multicollinearity in a regression model?"
Multiple methods: (1) Correlation matrix — look for |r|>0.8. (2) Variance Inflation Factor (VIF) — VIF>10 signals multicollinearity. (3) Condition number of feature matrix. (4) Check if adding/removing a feature drastically changes other coefficients.

**IQ2.** "Pearson r=0 means no relationship. Is this always true?"
No! r=0 means no LINEAR relationship. Non-linear relationships (quadratic, U-shaped, sinusoidal) can have r=0 while having a strong relationship. Always visualize. Use Spearman, mutual information, or other non-parametric measures.

**IQ3.** "What is the difference between a histogram and KDE plot? When would you use each?"
Histogram: discrete bins, counts exact frequencies, shows absolute counts. KDE: continuous smooth curve, shows probability density, better for comparing multiple distributions on same axes. Use histogram for raw data exploration; KDE for comparing distributions or presentations.

---

# 📌 Chapters 3.4–3.6 Summary

## ⚡ Quick Revision

- **Percentile:** P% of data falls below this value
- **Quartiles:** Q1=25th, Q2=50th (median), Q3=75th
- **IQR:** Q3-Q1, the range of the middle 50%
- **Five-number summary:** Min, Q1, Median, Q3, Max
- **Covariance:** Direction of relationship, not bounded
- **Pearson r:** Standardized covariance, always in [-1, +1]
- **Spearman:** Rank-based correlation, handles non-linear monotonic relationships
- **Correlation ≠ Causation** — always remember this!
- **Visualization:** Histogram (distribution), Scatter (relationship), Box/Violin (comparison), Heatmap (correlation matrix)

## 📐 Formula Sheet

| Formula | Name |
|---------|------|
| L = (P/100)×n | Percentile position |
| IQR = Q3-Q1 | Interquartile range |
| Cov(X,Y) = Σ(xᵢ-x̄)(yᵢ-ȳ)/(n-1) | Sample covariance |
| r = Cov(X,Y)/(σ_X × σ_Y) | Pearson correlation |
| rₛ = 1 - 6Σd²/(n(n²-1)) | Spearman rank correlation |
| Tukey lower fence = Q1-1.5×IQR | Outlier lower bound |
| Tukey upper fence = Q3+1.5×IQR | Outlier upper bound |

---
*Chapters 3.4–3.6 Complete | Part 3 DONE | Next: Part 4 — Inferential Statistics*
