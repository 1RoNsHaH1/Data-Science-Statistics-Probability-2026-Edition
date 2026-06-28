# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 4 — Inferential Statistics
# Chapters 4.1–4.3: Sampling | Estimation | Hypothesis Testing

---

# Chapter 4.1 — Sampling Techniques

> **You can't measure everything. Learn to measure a little, and infer a lot.**

---

## 💡 Why Sampling?

Measuring an entire population is often:
- **Too expensive** (survey all 140 crore Indians)
- **Too slow** (testing every product destroys it)
- **Impossible** (predicting weather for every molecule of air)

So we **sample** — measure a representative subset, draw conclusions about the whole.

The quality of your sample determines the quality of your conclusions.

---

## 4.1.1 — Simple Random Sampling

### 💡 Intuition
Every member of the population has an **equal chance** of being selected. Like drawing lottery tickets.

```
Population: 1000 customers (numbered 1-1000)
Sample size: 100
Method: Use random number generator to pick 100 numbers

Each customer has exactly 1/1000 = 0.1% chance of selection
```

**Tools:** Random number table, Python's `random.sample()`, Excel RAND()

**✅ Advantage:** Unbiased — no systematic favoritism.
**❌ Disadvantage:** Might miss important subgroups if they're small.

---

## 4.1.2 — Stratified Sampling

### 💡 Intuition
Divide population into **strata** (subgroups), then randomly sample from each stratum proportionally.

```
Population: 1000 customers
  - Premium customers: 200 (20%)
  - Regular customers: 600 (60%)
  - Free tier: 200 (20%)

Stratified sample of 100:
  - Premium: 100 × 0.20 = 20 customers
  - Regular: 100 × 0.60 = 60 customers
  - Free tier: 100 × 0.20 = 20 customers

Ensures all segments are proportionally represented!
```

**✅ Advantage:** Every subgroup is guaranteed representation. Better precision.
**❌ Disadvantage:** Need to know strata boundaries before sampling.

**Used in:** Opinion polls (ensure gender, age, region balance), clinical trials (age groups), machine learning (stratified train-test split).

```python
# Python — Stratified split
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
# stratify=y ensures class proportions preserved in both sets
```

---

## 4.1.3 — Cluster Sampling

### 💡 Intuition
Divide population into **clusters** (natural groups), randomly select some clusters, sample **all** members from selected clusters.

```
School district survey:
  - 50 schools (clusters)
  - Randomly select 10 schools
  - Survey all students in those 10 schools

Instead of traveling to 50 schools, you only go to 10.
Much cheaper! But less precise than simple random.
```

**✅ Advantage:** Cost-effective for geographically spread populations.
**❌ Disadvantage:** Members within a cluster tend to be similar (homogeneous), reducing the diversity of information.

---

## 4.1.4 — Systematic Sampling

Sample every k-th element from a list.

```
Population: 10,000 employees, listed 1-10,000
Want: sample of 100
k = 10,000/100 = 100
Random start: pick number between 1-100 (say, 37)
Sample: 37, 137, 237, 337, ..., 9937
```

**✅ Simple to implement**
**❌ If the list has a pattern at interval k, bias occurs**

---

## 4.1.5 — Sampling Bias

> **Sampling bias occurs when the sample is systematically unrepresentative of the population.**

### Types of Bias

| Type | Description | Example |
|------|-------------|---------|
| **Selection bias** | Some members more likely to be chosen | Online survey excludes non-internet users |
| **Survivorship bias** | Only "survivors" included | Studying successful startups ignores failures |
| **Response bias** | Respondents answer differently than they'd act | People say they exercise more than they do |
| **Voluntary response bias** | Participants self-select (usually extreme opinions) | YouTube comments, online reviews |
| **Undercoverage bias** | Some groups rarely sampled | Phone surveys miss homeless people |

### The Literary Digest Disaster (1936)
- Mailed 10 million surveys (huge sample!)
- Got 2.4 million responses
- Predicted Alf Landon to win presidential election 57-43%
- FDR won 62-38% — a massive failure!
- **Why?** Survey mailed to telephone/car owners → wealthy, Republican-leaning. Voluntary response → opinionated people responded. Large n does NOT fix sampling bias.

### 🚀 Data Science
- Training data bias → model bias (biased model)
- Historical data reflects past biases (hiring, lending, criminal justice)
- Class imbalance is a form of sampling bias
- Test set must match real-world distribution

---

# Chapter 4.2 — Estimation

> **From sample statistics, we estimate population parameters. But how confident are we?**

---

## 4.2.1 — Point Estimates

### 💡 Intuition
A **point estimate** is a single number that best represents the population parameter.

| Population Parameter | Point Estimate (from sample) |
|---------------------|------------------------------|
| μ (population mean) | x̄ (sample mean) |
| σ (population SD) | s (sample SD) |
| p (proportion) | p̂ (sample proportion) |
| σ² (variance) | s² (sample variance) |

**Example:** You survey 200 customers and find 65% would recommend your product.
Point estimate: p̂ = 0.65

**Problem:** A single point estimate gives no indication of uncertainty. The true p could be 0.60 or 0.70. We need **interval estimates**.

---

## 4.2.2 — Confidence Intervals

### 💡 Intuition

> **A confidence interval is a range of plausible values for the population parameter, with a stated level of confidence.**

**The 95% Confidence Interval Explained:**

> "If we repeated this study 100 times with different random samples, our interval would capture the true population parameter in 95 of those 100 repetitions."

**Common Misinterpretation:** ❌ "There's a 95% probability the parameter is in this interval."
**Correct:** ✅ "This procedure captures the true parameter 95% of the time."

```
Imagine 20 different samples, each producing a CI:

Sample 1:  ───[═══════]──────   ← captures μ ✅
Sample 2:  ──────[═════════]──  ← captures μ ✅
Sample 3:  ────────[═══]──────  ← captures μ ✅
Sample 4:           [═══]──────  ← MISSES μ ❌
Sample 5:  ──[══════════]─────   ← captures μ ✅
...

μ (true value): │
                ↑

With 95% CI: on average, 19 of 20 capture μ. 1 misses.
```

### 📖 Formula — Confidence Interval for Mean (known σ)

$$\bar{x} \pm z^* \times \frac{\sigma}{\sqrt{n}}$$

| Symbol | Meaning |
|--------|---------|
| x̄ | Sample mean |
| z* | Critical z-value for chosen confidence level |
| σ/√n | Standard error of the mean |
| ± | Margin of error on both sides |

### Critical Z-Values

| Confidence Level | z* | Meaning |
|-----------------|-----|---------|
| 90% | 1.645 | Middle 90% of N(0,1) |
| 95% | 1.960 | Most commonly used |
| 99% | 2.576 | Very high confidence |

### 📊 Worked Examples

**Example 1:**
Sample of 64 students. Mean score x̄ = 72. Population σ = 10. Build 95% CI.

```
SE = σ/√n = 10/√64 = 1.25
Margin of error = 1.96 × 1.25 = 2.45

95% CI: (72 - 2.45, 72 + 2.45) = (69.55, 74.45)

"We are 95% confident the true population mean score 
falls between 69.55 and 74.45."
```

**Example 2 — Proportion:**
500 users surveyed, 320 prefer dark mode. 95% CI for p?

```
p̂ = 320/500 = 0.64
SE = √(p̂(1-p̂)/n) = √(0.64×0.36/500) = √0.000461 = 0.02147

95% CI: 0.64 ± 1.96 × 0.02147
       = 0.64 ± 0.042
       = (0.598, 0.682)

"Between 59.8% and 68.2% of users prefer dark mode."
```

### 🎯 Width of Confidence Interval

```
Width = 2 × z* × (σ/√n)

To NARROW the interval (more precision):
1. Increase n (larger sample) ← most practical
2. Decrease confidence level (e.g., 90% instead of 95%)
3. Reduce σ (better measurement instruments)
```

### 🚀 Data Science Applications

| Application | Confidence Interval |
|-------------|-------------------|
| A/B test results | CI for difference in conversion rates |
| Model accuracy | 95% CI for test accuracy |
| Feature importance | CI for coefficient in regression |
| Uplift modeling | CI for treatment effect |
| Revenue forecast | Prediction interval for next quarter |

---

## 4.2.3 — t-Distribution for Small Samples

When σ is unknown AND n < 30, use the **t-distribution** instead of z.

$$\bar{x} \pm t^*_{(n-1)} \times \frac{s}{\sqrt{n}}$$

- Degrees of freedom = n - 1
- t-distribution has heavier tails than normal → wider intervals
- As n → ∞, t-distribution → Normal distribution

```
t vs Normal:
             Normal: ───█████───
             t(df=5): ──███████──  (heavier tails)
             t(df=30): ──██████──  (close to normal)

Larger df → t approaches Normal
```

---

# Chapter 4.3 — Hypothesis Testing

> **The formal framework for making decisions from data. Used in every scientific study and every A/B test.**

---

## 💡 Intuition — The Courtroom Analogy

> **Hypothesis testing is like a trial. The null hypothesis is "innocent until proven guilty."**

```
COURT:                    STATISTICS:
─────────────────────────────────────────────────
Defendant is innocent     Null hypothesis H₀ is true
Presumption of innocence  Assume H₀ until proven otherwise
Prosecution presents      We collect data/evidence
  evidence
Burden of proof           Statistical significance required
Reasonable doubt          Significance level α
Verdict: Guilty           Reject H₀ → Accept H₁
Verdict: Not Guilty       Fail to reject H₀
```

**Critical:** "Not guilty" ≠ "innocent". "Fail to reject H₀" ≠ "H₀ is true."

---

## 4.3.1 — Setting Up the Hypotheses

### Null Hypothesis (H₀)
> The **default assumption** — no effect, no difference, status quo.

H₀ always contains an equality: =, ≤, ≥

**Examples:**
- "The new drug has NO effect (mean cure rate = 50%)"
- "The new webpage has SAME conversion rate as old (p_new = p_old)"
- "There is NO relationship between X and Y"

### Alternative Hypothesis (H₁ or Hₐ)
> What we're trying to **find evidence for** — there IS an effect.

**Examples:**
- H₁: "The new drug has an effect (cure rate ≠ 50%)" → two-tailed
- H₁: "New page converts better (p_new > p_old)" → one-tailed (right)
- H₁: "There IS a relationship between X and Y"

### Types of Tests

```
TWO-TAILED TEST:
H₀: μ = 100
H₁: μ ≠ 100  (could be higher OR lower)
Rejection region on BOTH tails
Used when: you care about ANY deviation

ONE-TAILED RIGHT:
H₀: μ ≤ 100
H₁: μ > 100   (only care if it's higher)
Rejection region on RIGHT tail only

ONE-TAILED LEFT:
H₀: μ ≥ 100
H₁: μ < 100   (only care if it's lower)
Rejection region on LEFT tail only
```

---

## 4.3.2 — The p-value

### 💡 Intuition

> **The p-value is the probability of observing data as extreme as what we saw, IF the null hypothesis were true.**

```
"If H₀ were true, how likely is it we'd see data this extreme by chance?"

Small p-value → Unlikely to see this data if H₀ is true → Evidence against H₀
Large p-value → Data is consistent with H₀ → No evidence against H₀
```

**What p-value is NOT:**
- ❌ Not the probability H₀ is true
- ❌ Not the probability of making an error
- ❌ Not the probability the result is practically significant

### 📊 Visualizing the p-value

```
H₀: μ = 100 (two-tailed test)
We observe x̄ = 108 from a sample.

                    p-value
                   ┌──────────────────────┐
                   │                      │
Normal curve:   │  ████████████████████  │  │
                │ █████████████████████████ │
                │██████████████████████████ │
                ──────────────────────────────
              84   92  100  108  116   124

Shaded area in both tails (≥108 and ≤92) = p-value

If this area is very small → our observation is extreme → evidence against H₀
```

### p-value Scale

```
p-value     Interpretation
─────────────────────────────────────
< 0.001     Very strong evidence against H₀
0.001-0.01  Strong evidence
0.01-0.05   Moderate evidence (conventional threshold)
0.05-0.10   Weak evidence
> 0.10      Little or no evidence against H₀
```

---

## 4.3.3 — Significance Level (α)

> **α is the threshold p-value below which we reject H₀.** Chosen BEFORE the test.

**Convention:** α = 0.05 (5%) is most common.
Also used: α = 0.01 (1%) for medical/high-stakes decisions.

```
Decision rule:
  If p-value < α → Reject H₀ (statistically significant)
  If p-value ≥ α → Fail to reject H₀ (not significant)
```

---

## 4.3.4 — Type I and Type II Errors

> **Two ways hypothesis testing can go wrong.**

```
Reality
                H₀ is TRUE    H₀ is FALSE
              ┌─────────────┬──────────────┐
Reject H₀    │  Type I     │   Correct    │
(significant)│  Error (α)  │  Decision    │
              │             │  (Power=1-β) │
              ├─────────────┼──────────────┤
Fail to      │  Correct    │  Type II     │
reject H₀    │  Decision   │  Error (β)   │
              │             │              │
              └─────────────┴──────────────┘
```

### Type I Error (α — False Positive)
> **Rejecting H₀ when it IS actually true.**
> "Declaring a drug effective when it actually isn't."

- Probability = α (significance level)
- α = 0.05 means 5% chance of Type I error
- Also called: false alarm, false positive

### Type II Error (β — False Negative)
> **Failing to reject H₀ when it IS actually false.**
> "Declaring a drug ineffective when it actually IS effective."

- Probability = β
- Controlled by sample size and effect size
- Also called: missed detection, false negative

### 🎯 The Trade-off

```
Lower α → Fewer Type I errors BUT more Type II errors
Higher α → More Type I errors BUT fewer Type II errors

Medical drug approval:
  - Type I: Approve ineffective drug → costly/dangerous → keep α = 0.01
  - Type II: Reject effective drug → patients miss treatment → want small β too

Spam filter:
  - Type I: Block legitimate email → annoying but manageable
  - Type II: Let spam through → user gets spam
  Depends on context which error is more costly!
```

### Power (1 - β)

> **Power = probability of correctly rejecting H₀ when H₁ is true.**
> Power = probability of detecting an effect that really exists.

```
Power = 1 - β

Power = 0.80 → 80% chance of detecting a true effect
Low power → We might miss a real effect (Type II error)
```

---

## 4.3.5 — Step-by-Step Hypothesis Testing Framework

```
STEP 1: State hypotheses
   H₀: [null — equality/status quo]
   H₁: [alternative — what you're testing for]

STEP 2: Choose significance level
   α = 0.05 (or 0.01 for high-stakes)

STEP 3: Choose test and check assumptions
   Is data normal? Are samples independent? Is σ known?

STEP 4: Compute test statistic
   z = (x̄ - μ₀) / (σ/√n)   [for known σ]
   t = (x̄ - μ₀) / (s/√n)   [for unknown σ]

STEP 5: Find p-value
   Look up in table or use Python

STEP 6: Make decision
   p < α → Reject H₀ → "Statistically significant"
   p ≥ α → Fail to reject H₀ → "Not significant"

STEP 7: Interpret in context
   Never just say "rejected H₀" — explain what this means!
```

---

## 📊 Complete Worked Example

**Problem:** A company claims its call center has average call time μ₀ = 8 minutes. You sample 36 calls and find x̄ = 9.2 minutes with s = 3.6 minutes. Is the true mean significantly different from 8 minutes? Use α = 0.05.

**Step 1:** H₀: μ = 8, H₁: μ ≠ 8 (two-tailed)

**Step 2:** α = 0.05

**Step 3:** n=36, s known, n≥30 → use z-test (or t-test — similar for large n)

**Step 4:**
```
z = (x̄ - μ₀) / (s/√n)
  = (9.2 - 8) / (3.6/√36)
  = 1.2 / (3.6/6)
  = 1.2 / 0.6
  = 2.0
```

**Step 5:**
```
Two-tailed test: p-value = 2 × P(Z > 2.0)
P(Z > 2.0) = 1 - 0.9772 = 0.0228
p-value = 2 × 0.0228 = 0.0456
```

**Step 6:** p = 0.0456 < α = 0.05 → **Reject H₀**

**Step 7:** "The sample data provides statistically significant evidence at the 5% level that the true mean call time differs from 8 minutes. The observed mean of 9.2 minutes appears to represent a genuine increase in call time."

---

# 📝 Practice Questions — Chapters 4.1–4.3

## 🟢 Beginner (10 Questions)

**Q1.** What is the difference between a point estimate and a confidence interval?
**Q2.** n=100, x̄=50, σ=10. Compute 95% CI for μ.
**Q3.** What does a 99% CI say about how often the interval captures the true parameter?
**Q4.** H₀: μ = 20. H₁: μ ≠ 20. Is this one-tailed or two-tailed?
**Q5.** p-value = 0.03 and α = 0.05. Do you reject H₀?
**Q6.** What is a Type I error? Give a real-world example.
**Q7.** What is a Type II error? Give a real-world example.
**Q8.** What happens to a CI width when you increase sample size?
**Q9.** Which sampling method guarantees every subgroup is represented?
**Q10.** A clinical trial shows p=0.06 with α=0.05. Is the drug effective?

## 🟡 Intermediate (10 Questions)

**Q11.** Pilot study: n=25, x̄=42, s=8. Build 95% t-CI (df=24, t*=2.064).
**Q12.** Survey: 600 respondents, 54% prefer Product A. Build 99% CI for proportion.
**Q13.** What is the effect on CI if you: (a) double n, (b) change from 95% to 99% confidence?
**Q14.** Pharmaceutical trial: H₀: drug has no effect. Which error is worse — Type I or Type II? Why?
**Q15.** Two factories: Factory A samples 50 items, 3 defective. Factory B samples 80, 7 defective. Which sampling method would give the most representative defect rate estimate?
**Q16.** p-value = 0.001. Write the interpretation carefully, avoiding common misinterpretations.
**Q17.** Why should significance level α be chosen BEFORE collecting data?
**Q18.** Explain why "fail to reject H₀" is NOT the same as "accept H₀."
**Q19.** If CI doesn't contain 0 for a difference (μ₁-μ₂), what can you conclude?
**Q20.** How does Bessel's correction affect the CI formula for small samples?

## 🔴 Advanced (5 Questions)

**Q21.** Derive the 95% CI formula from first principles using the sampling distribution of x̄.
**Q22.** Bonferroni correction: If you run 20 hypothesis tests, each at α=0.05, what is P(at least one false positive)? What α should each test use to maintain family-wise error rate = 0.05?
**Q23.** Statistical significance vs practical significance: a test shows p<0.0001 for a 0.01% improvement in conversion. Is this meaningful? How do you decide?
**Q24.** The bootstrap CI: Explain how resampling with replacement creates a CI without parametric assumptions.
**Q25.** If power = 0.80, α=0.05, and effect size = 0.3, what sample size is needed? (Cohen's formula approach)

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** A/B test: Variant A converts 8% from 500 visitors. Variant B converts 10% from 500. Build a 95% CI for the difference in proportions. Is the difference significant?

**DS2.** Model evaluation: Your model achieves 87.5% accuracy on a test set of 400 samples. Build a 95% CI for the true accuracy. Should you launch the model if you need >85% with 95% confidence?

**DS3.** You're running 100 hypothesis tests for feature importance. At α=0.05, how many tests might show false significance by chance? What correction should you apply?

**DS4.** Clinical AI system: P(false positive — healthy flagged as sick) must be < 1%. P(false negative — sick flagged as healthy) must be < 5%. Map these to Type I and Type II errors. How does this constrain your choice of α and power?

**DS5.** Sequential testing problem: You're checking A/B test results daily and stopping as soon as p<0.05. Why does this inflate Type I error? What is the correct approach?

---

# ✅ Key Solutions

**A2.** SE=10/√100=1. 95% CI: 50±1.96×1 = (48.04, 51.96).

**A5.** Yes. p=0.03 < α=0.05 → Reject H₀.

**A8.** Decreases (narrows). Width ∝ 1/√n — larger n → smaller SE → narrower CI.

**A11.** SE=8/√25=1.6. 95% CI: 42±2.064×1.6=42±3.3=(38.7, 45.3).

**A12.** p̂=0.54. SE=√(0.54×0.46/600)=0.0203. 99% CI (z*=2.576): 0.54±0.052=(0.488, 0.592).

**A13.** (a) Width halved (√4 factor in denominator). (b) Width increases (need z*=2.576 instead of 1.96).

**A16.** "Assuming H₀ were true, there is a 0.1% probability of observing data as extreme as we observed purely by random chance. This is very strong evidence against H₀." Do NOT say: "99.9% probability the drug works."

**A22.** P(at least one FP) = 1-(0.95)²⁰=1-0.358=64.2%! Bonferroni: use α' = 0.05/20 = 0.0025 for each test.

**DS1.** p̂_A=0.08, p̂_B=0.10. Difference=0.02. SE_diff=√(0.08×0.92/500+0.10×0.90/500)=√(0.0001472+0.00018)=0.0183. 95% CI: 0.02±1.96×0.0183=0.02±0.0358=(-0.016, 0.056). Zero is in the interval → NOT statistically significant at 95%.

**DS2.** p̂=0.875, SE=√(0.875×0.125/400)=0.0165. 95% CI: 0.875±1.96×0.0165=(0.843, 0.907). Lower bound 84.3% < 85%. Borderline. Should collect more data or use a one-sided test.

**DS3.** Expected false positives = 100 × 0.05 = 5. Apply Bonferroni: test each at α/100 = 0.0005. Or use Benjamini-Hochberg procedure (FDR correction).

---

# 💼 Interview Questions

**IQ1.** "What is a p-value? How do you explain it to a business stakeholder?"
The p-value is the probability of seeing our data (or more extreme) if there were actually no difference. A business explanation: "If the variants were identical, how likely would it be to see this big a difference just by chance? p=0.03 means 3% likely — we're fairly confident the difference is real."

**IQ2.** "What's wrong with p-hacking?"
P-hacking means running tests until you find p<0.05, then stopping and reporting. This inflates Type I error rate far beyond α=0.05. With enough tests, you'll always find a "significant" result by chance. Pre-register your hypotheses and analysis plan before collecting data.

**IQ3.** "When would you use a one-tailed vs two-tailed test?"
Two-tailed: when you want to detect any deviation from H₀ (both increase and decrease). One-tailed: when you only care about deviation in one direction AND you specified this before collecting data. One-tailed tests have more power but must be justified theoretically, not chosen to get a significant result.

---

# 📌 Chapters 4.1–4.3 Summary

## ⚡ Quick Revision

**Sampling:** Simple random (equal chance), Stratified (by subgroup), Cluster (natural groups), Systematic (every k-th). Sampling bias is systematic and doesn't decrease with n.

**Estimation:** Point estimate = one number (x̄). Confidence interval = range. 95% CI means 95% of such intervals capture true parameter. Width decreases with larger n and lower confidence level.

**Hypothesis Testing:**
- H₀ = null (default), H₁ = alternative
- p-value = P(data this extreme | H₀ is true)
- Reject if p < α
- Type I (α) = false positive; Type II (β) = false negative
- Power = 1 - β = probability of detecting real effect

## 📐 Formula Sheet

| Formula | Name |
|---------|------|
| CI: x̄ ± z* × σ/√n | Confidence interval (known σ) |
| CI: x̄ ± t* × s/√n | Confidence interval (unknown σ) |
| CI: p̂ ± z* × √(p̂(1-p̂)/n) | CI for proportion |
| z = (x̄-μ₀)/(σ/√n) | One-sample z-test |
| t = (x̄-μ₀)/(s/√n) | One-sample t-test |
| SE = σ/√n | Standard error |

---
*Chapters 4.1–4.3 Complete | Next: 4.4–4.6 — Statistical Tests, A/B Testing, Power*
