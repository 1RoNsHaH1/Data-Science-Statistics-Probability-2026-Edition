# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 3 — Descriptive Statistics
# Chapters 3.1–3.3: Central Tendency | Spread | Shape of Distributions

---

# Chapter 3.1 — Measures of Central Tendency

> **Where is the "center" of your data? Mean, Median, Mode answer this.**

---

## 💡 Intuition

Imagine you're a cricket selector reviewing 10 batsmen. Each has played 20 innings. You want one number that summarizes "how much does this player typically score?"

That single summary number = **measure of central tendency**.

Three options: **Mean**, **Median**, **Mode**.

They often agree. When they disagree, the disagreement tells you something important about the data.

---

## 3.1.1 — Mean (Arithmetic Average)

### 💡 Intuition
> The mean is the "fair share" — if you redistributed all the data equally, what would each person get?

If 5 friends earn ₹20k, ₹30k, ₹25k, ₹22k, ₹28k → Total = ₹1,25,000 → Divided equally = ₹25,000 each.

### 📖 Formula

**Population Mean:**
$$\mu = \frac{\sum_{i=1}^{N} x_i}{N}$$

**Sample Mean:**
$$\bar{x} = \frac{\sum_{i=1}^{n} x_i}{n}$$

| Symbol | Meaning |
|--------|---------|
| μ | Population mean (Greek: mu) |
| x̄ | Sample mean (x-bar) |
| Σxᵢ | Sum of all values |
| N or n | Total count |

### 📊 Worked Examples

**Example 1 — Simple:**
Scores: [70, 85, 90, 60, 75]
Mean = (70+85+90+60+75)/5 = 380/5 = **76**

**Example 2 — Effect of Outlier:**
Incomes: [₹30k, ₹32k, ₹31k, ₹29k, ₹28k, ₹5,00k (CEO)]
Mean = (30+32+31+29+28+500)/6 = 650/6 ≈ **₹108k**

The CEO's salary **pulled the mean up massively**. 5 of 6 people earn far below the mean. Is mean representative? NO.

### ⚠️ The Mean's Weakness
> **The mean is sensitive to outliers (extreme values).**

```
Dataset: [10, 11, 12, 13, 14]     Mean = 12  ← representative
Dataset: [10, 11, 12, 13, 1000]   Mean = 209 ← NOT representative!
```

### 🚀 Data Science Connection
- MSE (Mean Squared Error) — comparing mean prediction error
- Feature means used in standardization: z = (x - μ)/σ
- Gradient descent uses mean gradients across batches
- Weighted mean in ensemble models: ŷ = Σ wᵢ × ŷᵢ

---

## 3.1.2 — Median

### 💡 Intuition
> The median is the **middle value** when data is sorted. Exactly half the values are above, half are below.

The median is the value at the 50th percentile.

```
Sorted data: 10, 20, 30, 40, 50
                         ↑
                      Median = 30 (middle value, n=5, odd)

Sorted data: 10, 20, 30, 40
                   ↑  ↑
              Median = (20+30)/2 = 25 (average of two middle values, n=4, even)
```

### 📖 How to Calculate

1. Sort the data in ascending order
2. If n is odd: Median = value at position (n+1)/2
3. If n is even: Median = average of values at positions n/2 and n/2 + 1

### 📊 Worked Examples

**Example 1:**
Data: [3, 1, 4, 1, 5, 9, 2, 6, 5]
Sorted: [1, 1, 2, 3, **4**, 5, 5, 6, 9]
n=9 (odd), position = (9+1)/2 = 5th value = **4**

**Example 2 — Outlier Resistant:**
Incomes: [₹28k, ₹29k, ₹30k, ₹31k, ₹32k, ₹5,00k (CEO)]
Sorted: [28, 29, **30, 31**, 32, 500]
Median = (30+31)/2 = **₹30.5k** ← much more representative than mean of ₹108k!

### ⚡ Median vs Mean — When to Use Which

| Use Mean when... | Use Median when... |
|-----------------|-------------------|
| Data is symmetric | Data is skewed |
| No extreme outliers | Outliers present |
| Normal distribution | Income, house prices, salaries |
| Further math needed | Want robust summary |

> 📌 **Real World:** Government reports income using **median** not mean, precisely because a few billionaires would inflate the mean.

### 🚀 Data Science
- Median imputation for missing values (robust to outliers)
- MAE (Mean Absolute Error) is implicitly minimized by median
- Robust scalers use median for centering

---

## 3.1.3 — Mode

### 💡 Intuition
> The mode is the most **frequently occurring** value. The most popular outcome.

```
Shoe sizes sold today: 7, 8, 9, 8, 10, 8, 7, 9, 8, 6
Mode = 8 (appears 4 times — the best-selling size)
```

### 📊 Types of Mode

```
UNIMODAL: One peak
[1, 2, 2, 2, 3, 4] → Mode = 2

BIMODAL: Two peaks (could indicate two subgroups!)
[1, 2, 2, 2, 3, 4, 4, 4, 5] → Modes = 2 and 4

MULTIMODAL: Multiple peaks
Could indicate multiple sub-populations

NO MODE: All values appear equally often
[1, 2, 3, 4, 5] → No mode
```

### 🌍 When Mode is Most Useful
- **Categorical data:** What's the most common product category? Most common city? (Only central tendency measure valid for nominal data!)
- **Discrete data:** What shoe size sells most? What's the most common exam score?
- **Finding subgroups:** Bimodal distributions suggest two hidden groups in data

### 🚀 Data Science
- Mode imputation for categorical missing values
- Mode tells you the most probable outcome in a discrete distribution
- K-modes clustering (like K-means but for categorical data)

---

## 3.1.4 — Weighted Mean

### 💡 Intuition
> Some values deserve more "weight" in the average than others.

**Example:** Final grade = 30% midterm + 50% final + 20% project
Midterm: 70, Final: 85, Project: 90

```
Simple average: (70+85+90)/3 = 81.67
Weighted average: 0.30×70 + 0.50×85 + 0.20×90 = 21 + 42.5 + 18 = 81.5
```

### 📖 Formula

$$\bar{x}_w = \frac{\sum_{i=1}^{n} w_i x_i}{\sum_{i=1}^{n} w_i}$$

| Symbol | Meaning |
|--------|---------|
| wᵢ | Weight for value i |
| xᵢ | Value i |
| Σwᵢxᵢ | Weighted sum |
| Σwᵢ | Total weight (= 1 if weights are proportions) |

### 🚀 Data Science
- Ensemble predictions: ŷ = Σ wᵢ × modelᵢ(x)
- Class-weighted loss functions for imbalanced datasets
- Exponential moving averages in time series (recent data weighted more)
- WACC in finance analytics

---

## 3.1.5 — The Relationship Between Mean, Median, Mode

```
SYMMETRIC distribution (e.g., Normal):
Mean = Median = Mode (all equal)
         ↓
        ███
       █████
      ███████
     █████████
────────────────
    Mean=Median=Mode

RIGHT-SKEWED (positive skew — tail pulls right):
Mode < Median < Mean
Tail pulls mean to the right
Example: Income distribution, house prices

Mode  Median  Mean
  ↓     ↓      ↓
  ████████
  █████████
  ██████████────
────────────────────────→ (long right tail)

LEFT-SKEWED (negative skew — tail pulls left):
Mean < Median < Mode
Tail pulls mean to the left
Example: Exam scores in an easy test

 Mean Median Mode
   ↓    ↓     ↓
        ████████
      ██████████
──────██████████
←──────────────────────── (long left tail)
```

> 🎯 **Exam Tip:** In a right-skewed distribution: Mean > Median > Mode. In a left-skewed distribution: Mean < Median < Mode.

---

# Chapter 3.2 — Measures of Spread (Dispersion)

> **The center alone doesn't tell the full story. How spread out is the data?**

---

## 💡 Intuition

Two classes have the same mean exam score of 75:
- Class A: [73, 74, 75, 76, 77] — everyone's similar
- Class B: [20, 50, 75, 95, 99] — wildly different students

The mean (75) says they're the same. **Spread** reveals the difference.

```
Class A:  ████████████████████  (tight cluster)
              ↑ mean=75

Class B:  ██──────────────────██  (very spread out)
              ↑ mean=75
```

---

## 3.2.1 — Range

### 📖 Formula

$$\text{Range} = \text{Max} - \text{Min}$$

**Example:** [20, 35, 45, 60, 80, 95]
Range = 95 - 20 = **75**

### ⚠️ Weakness
Range is determined entirely by 2 values (min and max). One outlier destroys it.
[20, 35, 45, 60, 80, **10,000**] → Range = 9,980 (meaningless!)

---

## 3.2.2 — Variance

### 💡 Intuition
> Variance = the average of squared distances from the mean.

Why squared? Two reasons:
1. Squaring makes negatives positive (distances are always positive)
2. Squaring penalizes large deviations more than small ones

```
If data = [2, 4, 6, 8, 10], mean = 6:

Deviations:        2-6=-4,  4-6=-2,  6-6=0,  8-6=+2,  10-6=+4
Squared deviations:  16,      4,       0,       4,       16
Sum of squares:    16+4+0+4+16 = 40
Variance = 40/5 = 8 (population)
         = 40/4 = 10 (sample, divide by n-1)
```

### 📖 Formulas

**Population Variance:**
$$\sigma^2 = \frac{\sum_{i=1}^{N}(x_i - \mu)^2}{N}$$

**Sample Variance (Bessel's Correction):**
$$s^2 = \frac{\sum_{i=1}^{n}(x_i - \bar{x})^2}{n-1}$$

| Symbol | Meaning |
|--------|---------|
| σ² | Population variance |
| s² | Sample variance |
| (xᵢ - μ)² | Squared deviation of each value from mean |
| N or n | Population or sample size |
| n-1 | Degrees of freedom (Bessel's correction) |

### 🔍 Why n-1 for Sample Variance?

> **Bessel's Correction:** Dividing by n-1 (not n) corrects the underestimation of population variance from a sample.

When we compute the sample mean x̄ first, and then compute deviations from x̄, we've "used up" one piece of information. We only have n-1 independent deviations. Dividing by n-1 gives an unbiased estimator of σ².

```python
# Python:
import numpy as np
data = [2, 4, 6, 8, 10]
np.var(data)       # population variance: divides by n → 8.0
np.var(data, ddof=1)  # sample variance: divides by n-1 → 10.0
```

---

## 3.2.3 — Standard Deviation

### 💡 Intuition
> Standard deviation is the square root of variance. It's in the **same units** as the data.

Variance of exam scores is in "marks-squared" — doesn't make sense!
Standard deviation is in "marks" — interpretable.

### 📖 Formulas

$$\sigma = \sqrt{\sigma^2} = \sqrt{\frac{\sum(x_i - \mu)^2}{N}}$$

$$s = \sqrt{s^2} = \sqrt{\frac{\sum(x_i - \bar{x})^2}{n-1}}$$

### 📊 Worked Example

Scores: [60, 70, 80, 90, 100]. Mean = 80.

```
Deviations:        -20,  -10,   0,  +10,  +20
Squared:           400,  100,   0,  100,  400
Sum:               1000
Population Var:    1000/5 = 200
Population SD:     √200 = 14.14 marks

Interpretation: On average, scores deviate from the mean by ±14.14 marks.
```

### 📊 Interpreting SD

```
Small SD: Data is tightly clustered around mean
Large SD: Data is widely spread

Class A: μ=75, σ=3   → Nearly everyone scored 72-78
Class B: μ=75, σ=15  → Scores range roughly 45-105
```

### 🚀 Data Science Applications

| Application | How SD is Used |
|-------------|---------------|
| Standardization | z = (x - mean) / std |
| Anomaly detection | Flag values > 3 std from mean |
| Confidence intervals | x̄ ± 1.96 × (σ/√n) |
| Bollinger Bands (finance) | Moving mean ± 2 × moving SD |
| Feature scaling | StandardScaler uses mean and SD |
| Neural network init | Weights ~ N(0, σ²) |

---

## 3.2.4 — Interquartile Range (IQR)

### 💡 Intuition
> IQR is the range of the middle 50% of data. Ignores the top 25% and bottom 25%.

```
Sorted data:
Bottom 25% | Middle 50%  | Top 25%
──────────────────────────────────────────
           Q1           Q3
           ↓             ↓
    [__][___|____________|___][__]
           ←── IQR = Q3-Q1 ──→
```

**IQR = Q3 - Q1** (third quartile minus first quartile)

Since it focuses on the middle 50%, it's **completely unaffected by outliers**.

### 📊 Worked Example

Data: [10, 20, 25, 30, 35, 40, 45, 50, 60, 70, 1000]

Q1 = 25th percentile ≈ 25
Q3 = 75th percentile ≈ 55
IQR = 55 - 25 = 30

The outlier (1000) has ZERO effect on IQR!

### 📐 Outlier Detection with IQR

**Tukey's Method:** Any value is an outlier if:
```
Below: Q1 - 1.5 × IQR
Above: Q3 + 1.5 × IQR
```

Using the example: Q1=25, Q3=55, IQR=30
- Lower fence: 25 - 1.5×30 = 25 - 45 = **-20** (no outliers below)
- Upper fence: 55 + 1.5×30 = 55 + 45 = **100** (1000 > 100 → **OUTLIER**)

### 🚀 Data Science
- Box plot construction (IQR determines whiskers)
- Robust outlier detection
- IQR-based imputation for outlier treatment
- Robust scaler: uses Q1 and Q3 instead of mean and SD

---

## 3.2.5 — Comparison of Spread Measures

| Measure | Formula | Robust? | Units | Best For |
|---------|---------|---------|-------|---------|
| Range | Max - Min | No | Same | Quick glance |
| Variance | Avg(deviations²) | No | Squared | Mathematical use |
| Std Dev | √Variance | No | Same | General analysis |
| IQR | Q3 - Q1 | Yes | Same | Skewed data, outliers |

---

# Chapter 3.3 — Shape of Distributions

> **Not just where and how spread — but what SHAPE the distribution takes.**

---

## 3.3.1 — Skewness

### 💡 Intuition
> Skewness measures **symmetry**. Is the tail longer on the left or the right?

```
SYMMETRIC (skewness ≈ 0):
The two halves are mirror images.
       ████
     ████████
   ████████████
─────────────────
Mean = Median = Mode

RIGHT-SKEWED (positive skew > 0):
Long tail on the RIGHT.
  ██
  ████
  ███████──────────────────────
  (Salary, house prices, bug counts, wealth)
  Mode < Median < Mean

LEFT-SKEWED (negative skew < 0):
Long tail on the LEFT.
             ██
           ████
──────────████████
  (Exam scores on easy test, age at death in developed countries)
  Mean < Median < Mode
```

### 📖 Formula (Pearson's Second Coefficient)

Quick approximation:
$$\text{Skewness} \approx \frac{3(\bar{x} - \text{Median})}{\sigma}$$

Formal formula (third standardized moment):
$$\gamma_1 = \frac{1}{n}\sum_{i=1}^n \left(\frac{x_i - \bar{x}}{\sigma}\right)^3$$

| Skewness | Interpretation |
|----------|---------------|
| ≈ 0 | Symmetric |
| > 0 | Right-skewed (positive) |
| < 0 | Left-skewed (negative) |
| \|skew\| > 1 | Highly skewed |
| \|skew\| < 0.5 | Approximately symmetric |

### 🌍 Real-World Skewed Distributions

| Variable | Skewness | Reason |
|----------|----------|--------|
| Income | Right | Few billionaires pull tail right |
| House prices | Right | No floor but no upper limit |
| Exam scores | Left | Most students pass, few fail |
| Bug severity | Right | Most bugs minor, few critical |
| Response time | Right | Most responses fast, few very slow |
| Age at death | Left | Most people live to old age |

### 🚀 Data Science
- Detecting skewness → decide whether to apply log transformation
- Tree models handle skew naturally; linear models need symmetric features
- Skewed target variables need special treatment in regression
- Right-skewed data: consider log(x), sqrt(x), or Box-Cox transform

---

## 3.3.2 — Kurtosis

### 💡 Intuition
> Kurtosis measures the **heaviness of the tails** and the **sharpness of the peak**.

```
HIGH KURTOSIS (Leptokurtic — heavy tails):
More extreme values than normal
Very sharp peak, fat tails
        ▀
       ███
     █████ ← taller peak than normal
   ████████
──█████████████──  but fatter tails than normal

LOW KURTOSIS (Platykurtic — light tails):
Fewer extreme values than normal
Flat top, thin tails
    ████████
   █████████ ← flatter than normal
  ███████████
──████████████── thin tails

NORMAL KURTOSIS (Mesokurtic):
The Goldilocks middle
Kurtosis = 3 (or excess kurtosis = 0)
```

### 📖 Formula (Fourth Standardized Moment)

$$\gamma_2 = \frac{1}{n}\sum_{i=1}^n \left(\frac{x_i - \bar{x}}{\sigma}\right)^4$$

**Excess Kurtosis** = Kurtosis - 3 (Normal distribution has excess kurtosis = 0)

| Excess Kurtosis | Type | Shape |
|----------------|------|-------|
| = 0 | Mesokurtic | Normal tails (Normal distribution) |
| > 0 | Leptokurtic | Fat tails (more extreme values) |
| < 0 | Platykurtic | Thin tails (fewer extreme values) |

### 🌍 Examples

| Distribution | Kurtosis Type | Meaning |
|-------------|--------------|---------|
| Normal | Mesokurtic | Benchmark |
| Stock returns | Leptokurtic | Fat tails → crashes/booms more common than normal predicts |
| Student t-distribution | Leptokurtic | Used instead of Normal for small samples |
| Uniform | Platykurtic | Light tails |

### 🚀 Data Science
- Fat tails in financial data → normal distribution underestimates risk
- Leptokurtic residuals in regression → consider robust regression
- Outlier diagnosis: high kurtosis suggests many extreme values
- Risk management uses kurtosis to assess tail risk

---

## 3.3.3 — Outliers

### 💡 Intuition
> An outlier is a data point that is **far from the rest** — suspiciously different.

**Causes of Outliers:**
1. **Data entry error:** Someone typed 19000 instead of 1900
2. **Measurement error:** Broken sensor
3. **Natural extreme:** A genuine exceptional value (Virat Kohli in a bad slump, or a record-breaking score)
4. **Different population:** A CEO's salary in a worker dataset

### Methods for Detecting Outliers

**Method 1 — Z-score method:**
```
|z| > 3 → potential outlier
z = (x - mean) / std

Any value more than 3 standard deviations from mean is suspicious.
```

**Method 2 — IQR method (Tukey's fences):**
```
Lower fence = Q1 - 1.5 × IQR → below = mild outlier
Upper fence = Q3 + 1.5 × IQR → above = mild outlier

Extreme outliers:
Lower: Q1 - 3 × IQR
Upper: Q3 + 3 × IQR
```

**Method 3 — Visual (Box plot):**
```
          ┌──────┐
 ●        │      │         ●  ● ← outliers shown as dots
──────|────┤      ├────|─────────────────
     min  Q1    Q3  max (within fences)
          └──────┘
             ↑
          Median line inside box
```

### How to Handle Outliers

| Action | When Appropriate |
|--------|-----------------|
| Keep | Genuine extreme value; important for analysis |
| Remove | Confirmed data error |
| Cap/Winsorize | Replace with fence value; for robust analysis |
| Separate analysis | Model outliers separately |
| Transform | Log transform reduces impact of extreme values |

> ⚠️ **Never blindly remove outliers.** Always investigate WHY a value is extreme. An outlier might be the most important data point!

### 🚀 Data Science
- Outlier detection: IsolationForest, LOF, DBSCAN
- Robust loss functions: Huber loss less sensitive to outliers than MSE
- Winsorization in feature engineering
- Outliers in training data → poor model generalization
- Fraud = outlier detection

---

# 📝 Practice Questions — Chapters 3.1–3.3

## 🟢 Beginner (10 Questions)

**Q1.** Dataset: [5, 8, 12, 15, 9, 11]. Find mean, median, mode.
**Q2.** Which is more appropriate for a right-skewed salary dataset: mean or median? Why?
**Q3.** Dataset: [2, 4, 6, 8, 10]. Find variance and standard deviation (population).
**Q4.** Grades: [70, 70, 75, 80, 85, 90, 90, 90]. What is the mode?
**Q5.** What does a standard deviation of 0 tell you about a dataset?
**Q6.** If skewness > 0, is the tail on the left or right?
**Q7.** Q1=20, Q3=50. Find IQR and the outlier fences (Tukey's method).
**Q8.** Dataset: [10, 12, 11, 13, 100]. Mean=?, Median=?. Which is more representative?
**Q9.** Exam: μ=75, σ=10. Student scored 60. What is the z-score?
**Q10.** Is high kurtosis or low kurtosis associated with heavier tails and more outliers?

## 🟡 Intermediate (10 Questions)

**Q11.** Sales data: [100, 150, 120, 130, 140, 135, 125, 10000]. Compute mean, median. Which is better? Find IQR and determine if 10000 is an outlier.
**Q12.** Two models' MSE across 5 trials: Model A: [2.1, 2.0, 2.2, 1.9, 2.0], Model B: [1.5, 3.5, 2.0, 1.0, 5.0]. Same mean. Which is better and why? (Compute SD for each.)
**Q13.** Given five exam scores with mean=72 and SD=8, add a score of 100. How do the mean and SD change qualitatively?
**Q14.** In a normal distribution, what % of data lies within 2 standard deviations of the mean? If μ=50, σ=5, between what values does this 95% lie?
**Q15.** Describe the relationship between mean, median, and mode for income data in India. Justify.
**Q16.** Bessel's correction: why do we divide by n-1 for sample variance? What goes wrong if we divide by n?
**Q17.** Data: [1, 2, 3, 4, 5]. Compute the excess kurtosis. (Hint: normal distribution has kurtosis=3, excess=0.)
**Q18.** A dataset has skewness = 2.5 and kurtosis = 8. What transformation might help normalize it?
**Q19.** Revenue data: mean=₹10,000, median=₹6,500. What does this tell you about the distribution's shape?
**Q20.** Compute IQR for: [15, 20, 35, 40, 50, 55, 65, 70, 75, 85, 90]. Use it to identify outliers.

## 🔴 Advanced (10 Questions)

**Q21.** Prove that for any dataset, the sum of deviations from the mean equals zero: Σ(xᵢ - x̄) = 0.
**Q22.** Show that Var(aX + b) = a²Var(X) for a constant a, b.
**Q23.** Shortcut formula: Show that Σ(xᵢ-x̄)² = Σxᵢ² - n×x̄².
**Q24.** Coefficient of Variation (CV) = (σ/μ) × 100%. Why is CV useful when comparing datasets with different means? Give an example.
**Q25.** Median minimizes MAE: Show that the value c that minimizes Σ|xᵢ - c| is the median.
**Q26.** Trimmed mean: Remove top 10% and bottom 10% of values before averaging. When is this preferred over both mean and median?
**Q27.** Standardized skewness: If skewness = 1.8 and standard error of skewness = √(6/n), and n=100, is the skewness statistically significant?
**Q28.** Box-Cox transformation: y = (x^λ - 1)/λ for λ≠0, y=log(x) for λ=0. How does λ affect the transformation?
**Q29.** Huber loss is defined as: L = ½(y-ŷ)² if |y-ŷ|≤δ, else δ|y-ŷ|-½δ². Explain why this is less sensitive to outliers than MSE.
**Q30.** For a lognormal distribution (common for income), what is the relationship between the log-transformed mean/median/mode?

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** Customer LTV dataset has mean=₹8,500 and median=₹3,200. You're building a linear regression model. What preprocessing step would you apply to the target variable and why?

**DS2.** ML training loss: After 50 epochs, mean loss=0.35, SD=0.12, median=0.31. What does the gap between mean and median tell you? Should you report mean or median loss as the model metric?

**DS3.** Feature engineering: Column "transaction_amount" has skewness=3.8. List three transformations and explain which you'd try first and why.

**DS4.** Your model shows residuals with kurtosis=7.2. What problem does this indicate? What are two solutions?

**DS5.** A/B test results: Group A mean=₹450, SD=₹200. Group B mean=₹480, SD=₹350. Same mean difference (₹30). Which group's result would you trust more for decision-making and why? (Think about variance and uncertainty.)

---

# ✅ Key Solutions

**A1.** Sorted: [5,8,9,11,12,15]. Mean=(5+8+9+11+12+15)/6=60/6=10. Median=(9+11)/2=10. Mode=none.

**A3.** Mean=6. Deviations²: 16,4,0,4,16. Sum=40. Var=40/5=8. SD=√8≈2.83.

**A7.** IQR=50-20=30. Lower fence=20-45=-25. Upper fence=50+45=95. Outliers below -25 or above 95.

**A8.** Mean=(10+12+11+13+100)/5=146/5=29.2. Median=sorted[2,3rd pos]=[10,11,12,13,100] → 12. Median (12) is more representative of the 4 normal values.

**A9.** z=(60-75)/10=-1.5. Student is 1.5 SDs below mean.

**A11.** Mean=(100+150+120+130+140+135+125+10000)/8=10900/8=1362.5. Median=[100,120,125,130,135,140,150,10000] →(130+135)/2=132.5. Median is far better. Q1≈120, Q3≈145, IQR=25. Upper fence=145+37.5=182.5. 10000>182.5 → confirmed outlier.

**A12.** Model A: Mean=2.04, SD≈0.104. Model B: Mean=2.60, SD≈1.51. Model A is far more consistent despite similar or lower mean. Choose Model A for reliability.

**A21.** Σ(xᵢ-x̄) = Σxᵢ - n×x̄ = Σxᵢ - n×(Σxᵢ/n) = Σxᵢ - Σxᵢ = 0. ✅

**A24.** CV allows comparison across different scales. Example: Investment A: mean=₹100k, σ=₹20k, CV=20%. Investment B: mean=₹1M, σ=₹100k, CV=10%. B has larger absolute SD but smaller relative variability. CV reveals B is actually less risky proportionally.

**DS1.** Apply log transformation (or Box-Cox). The large gap between mean and median (mean=2.6× median) indicates strong right skew. Log transform will make the distribution more symmetric, improving linear regression performance and satisfying normality of residuals.

**DS3.** Options: (1) Log transform: log(x+1) — best first choice for right-skewed, all-positive data. (2) Square root: sqrt(x) — milder than log. (3) Box-Cox: finds optimal λ automatically. Try log first — it's interpretable and usually effective for skewness > 1.

**DS5.** Group A (SD=₹200) is more trustworthy. Group B's high variance (SD=₹350) means the ₹30 difference could easily be noise. To decide, compute standard error of difference and run a t-test. High variance = less statistical confidence in the observed difference.

---

# 💼 Interview Questions

**IQ1.** "When would you use median instead of mean for a business metric?"
Whenever data is skewed or has outliers. Classic examples: customer lifetime value (few high-value customers), salary reports, response times, property prices. Median gives a better sense of "typical" value.

**IQ2.** "What is Bessel's correction and why does it exist?"
When estimating population variance from a sample, dividing by n underestimates the true variance. Dividing by n-1 corrects this bias, giving an unbiased estimator. The intuition: we lose one degree of freedom because we use the sample mean (estimated from the same data) in our calculation.

**IQ3.** "How do you handle skewed features in machine learning?"
(1) Apply log transformation for right-skewed positive data. (2) Square root for mild right skew. (3) Box-Cox for flexible transformation. (4) Use tree-based models that don't require normality. (5) Winsorize to cap extreme values. Choice depends on model type and how severe the skew is.

**IQ4.** "What does it mean if residuals from your regression model have high kurtosis?"
It means there are more extreme residuals than a normal distribution would predict — fat tails. This violates the normal residuals assumption. Solutions: check for outliers in training data, use a robust regression (Huber loss), or use a model that doesn't assume normal residuals (like gradient boosting).

---

# 📌 Chapters 3.1–3.3 Summary

## ⚡ Quick Revision

**Central Tendency:**
- Mean = sum/count; affected by outliers; use for symmetric data
- Median = middle value; robust to outliers; use for skewed data
- Mode = most frequent; use for categorical data
- Right skew: Mean > Median > Mode

**Spread:**
- Range = max - min; sensitive to outliers
- Variance = average squared deviation; in squared units
- SD = √variance; same units as data; most used
- IQR = Q3-Q1; robust to outliers; used in box plots

**Shape:**
- Skewness: asymmetry; +ve = right tail; −ve = left tail
- Kurtosis: tail heaviness; leptokurtic (fat tails) vs platykurtic (thin tails)
- Outliers: z>3 or outside Tukey fences; always investigate before removing

## 📐 Formula Sheet

| Formula | Name |
|---------|------|
| x̄ = Σxᵢ/n | Sample mean |
| s² = Σ(xᵢ-x̄)²/(n-1) | Sample variance |
| s = √s² | Sample SD |
| IQR = Q3 - Q1 | Interquartile range |
| Outlier fence = Q1-1.5×IQR or Q3+1.5×IQR | Tukey's fence |
| z = (x-μ)/σ | Z-score |
| CV = (σ/μ)×100% | Coefficient of variation |

---
*Chapters 3.1–3.3 Complete | Next: Chapters 3.4–3.6 — Percentiles, Correlation, Visualization*
