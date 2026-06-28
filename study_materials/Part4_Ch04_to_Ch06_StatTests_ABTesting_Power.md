# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 4 — Inferential Statistics
# Chapters 4.4–4.6: Statistical Tests | A/B Testing | Statistical Power

---

# Chapter 4.4 — Statistical Tests

> **Choose the right test for the right situation. Using the wrong test gives the wrong answer.**

---

## 4.4.1 — Z-Test

### 💡 When to Use
- Testing if a **sample mean** differs from a known population mean
- **n ≥ 30** OR population σ is known
- Data approximately normal (or CLT applies)

### 📖 Formula

$$z = \frac{\bar{x} - \mu_0}{\sigma / \sqrt{n}}$$

**Two-sample z-test:**
$$z = \frac{(\bar{x}_1 - \bar{x}_2) - \Delta_0}{\sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}}}$$

### 📊 Example
Claim: Population mean exam score = 70. Sample of 100 students, x̄=73.5, σ=12.
z = (73.5-70)/(12/√100) = 3.5/1.2 = 2.917
p-value (two-tailed) = 2×P(Z>2.917) ≈ 0.0035 < 0.05 → Reject H₀. Mean is significantly ≠ 70.

---

## 4.4.2 — T-Test

### 💡 When to Use
- Population σ is **unknown** (the usual case in practice!)
- Small samples (n < 30) or any size
- Three types: one-sample, two-sample independent, paired

### 📖 Formulas

**One-sample t:**
$$t = \frac{\bar{x} - \mu_0}{s / \sqrt{n}}, \quad df = n-1$$

**Two-sample independent t (Welch's):**
$$t = \frac{\bar{x}_1 - \bar{x}_2}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}}$$

**Paired t (before/after, or matched pairs):**
$$t = \frac{\bar{d}}{s_d / \sqrt{n}}, \quad df = n-1$$

Where $\bar{d}$ = mean of (after - before) differences.

### 📊 Which T-Test?

```
ONE SAMPLE: Compare one group mean to a known value
  "Is this class average significantly different from the national average of 70?"

TWO SAMPLE (INDEPENDENT): Compare means of two unrelated groups
  "Do men and women have different average salaries?"
  (Different people in each group)

PAIRED: Compare means where observations are linked
  "Did a training program improve scores?"
  (Same people measured before and after)
  
Paired t-test is MORE POWERFUL because it controls for between-person variability
```

### 📊 Assumptions of t-test

1. Data is approximately normal (or n is large)
2. Observations are independent
3. For two-sample: similar (or equal) variances (check Levene's test)
4. Continuous numeric data

### 📊 Worked Example — Paired t-test

A training program effect on 8 employees. Scores before/after:

```
Employee:   1    2    3    4    5    6    7    8
Before:    65   72   58   80   75   68   82   70
After:     70   78   65   82   80   75   85   78

Differences (after-before):
d = 5, 6, 7, 2, 5, 7, 3, 8

d̄ = (5+6+7+2+5+7+3+8)/8 = 43/8 = 5.375

s_d = √[Σ(dᵢ-d̄)²/(n-1)] = √(30.875/7) ≈ 2.1

t = d̄ / (s_d/√n) = 5.375 / (2.1/√8) = 5.375/0.742 = 7.24

df = n-1 = 7
For df=7, t=7.24 → p-value << 0.001

Conclusion: Training program significantly improved scores.
```

---

## 4.4.3 — Chi-Square Test

### 💡 Two Uses

**1. Chi-Square Goodness-of-Fit:** Does observed categorical data match an expected distribution?

**2. Chi-Square Test of Independence:** Are two categorical variables related?

### 📖 Formula

$$\chi^2 = \sum \frac{(O - E)^2}{E}$$

| Symbol | Meaning |
|--------|---------|
| O | Observed count in each cell |
| E | Expected count if H₀ is true |
| Σ | Sum over all cells |

Degrees of freedom:
- Goodness of fit: df = k-1 (k categories)
- Independence: df = (rows-1) × (cols-1)

### 📊 Worked Example — Independence Test

Q: Is gender related to product preference?

```
Observed:
            Product A   Product B   Total
Male             80         70        150
Female           60         90        150
Total           140        160        300

Expected (if independent): E = row_total × col_total / grand_total
E(Male, A) = 150×140/300 = 70
E(Male, B) = 150×160/300 = 80
E(Female, A) = 150×140/300 = 70
E(Female, B) = 150×160/300 = 80

χ² = (80-70)²/70 + (70-80)²/80 + (60-70)²/70 + (90-80)²/80
   = 100/70 + 100/80 + 100/70 + 100/80
   = 1.429 + 1.25 + 1.429 + 1.25 = 5.357

df = (2-1)×(2-1) = 1
Critical value at α=0.05: χ²(1) = 3.84

5.357 > 3.84 → Reject H₀ → Gender and preference ARE related.
```

---

## 4.4.4 — ANOVA (Analysis of Variance)

### 💡 Intuition

T-test compares **2 means**. ANOVA compares **3 or more means** simultaneously.

**Why not just run many t-tests?**
With 3 groups: need 3 t-tests. Each at α=0.05 → P(at least one false positive) = 1-0.95³ = 14.3%. Much worse than 5%!

ANOVA controls the **family-wise error rate** by testing all groups at once.

### H₀ and H₁ for ANOVA

H₀: μ₁ = μ₂ = μ₃ = ... = μₖ (all group means equal)
H₁: At least one mean differs from the others

### 💡 The Logic of ANOVA

ANOVA compares two sources of variation:
1. **Between-group variance:** How different are the group means from each other?
2. **Within-group variance:** How much do individual values vary within each group?

```
F = Between-group variance / Within-group variance

LARGE F → groups are different from each other relative to noise
SMALL F → groups are similar relative to noise
```

### 📖 F-Statistic

$$F = \frac{MS_{between}}{MS_{within}} = \frac{SS_{between}/df_{between}}{SS_{within}/df_{within}}$$

Where:
- SS = sum of squares
- MS = mean square (SS/df)
- df_between = k-1 (k = number of groups)
- df_within = N-k (N = total observations)

### 📊 Worked Example

Comparing average scores of 3 teaching methods (n=5 per group):

```
Method A: [70, 75, 72, 68, 80]  → Mean = 73
Method B: [85, 88, 90, 82, 87]  → Mean = 86.4
Method C: [65, 70, 68, 72, 69]  → Mean = 68.8

Grand mean = (73+86.4+68.8)/3 = 76.07

SS_between = 5×[(73-76.07)² + (86.4-76.07)² + (68.8-76.07)²]
           = 5×[9.42 + 106.5 + 52.9] = 5×168.8 = 844

SS_within = Σ(each value - its group mean)²
           ≈ 94 (compute for each group)

F = (844/2) / (94/12) = 422 / 7.83 = 53.9

df1=2, df2=12, α=0.05: Critical F ≈ 3.89
53.9 >> 3.89 → Reject H₀ → Teaching methods have different effects.
```

**After ANOVA:** If significant, run **post-hoc tests** (Tukey's HSD, Bonferroni) to determine WHICH pairs differ.

---

## 4.4.5 — Test Selection Guide

```
What type of variable?
│
├── CONTINUOUS (Numerical)
│   │
│   ├── One group vs known value → One-sample t-test
│   │
│   ├── Two independent groups → Two-sample t-test (Welch's)
│   │
│   ├── Two related groups (paired) → Paired t-test
│   │
│   └── Three+ groups → ANOVA (then post-hoc if significant)
│
└── CATEGORICAL
    │
    ├── One group vs expected dist → Chi-square GoF
    │
    └── Two categorical variables → Chi-square test of independence

Non-parametric alternatives (when normality violated):
  Mann-Whitney U (instead of 2-sample t)
  Wilcoxon signed-rank (instead of paired t)
  Kruskal-Wallis (instead of ANOVA)
```

---

# Chapter 4.5 — A/B Testing

> **The industry-standard method for making data-driven product decisions.**

---

## 💡 What Is A/B Testing?

> A/B testing is a controlled experiment where you randomly split users into two groups and expose each to a different version of something, then measure which performs better.

**Real-world A/B tests:**
- Netflix: New homepage layout vs old
- Amazon: "Buy Now" button blue vs orange
- Swiggy: Free delivery threshold ₹200 vs ₹300
- Gmail: New email composer vs old
- Hotstar: Push notification at 7pm vs 8pm

---

## 4.5.1 — The A/B Testing Framework

```
STEP 1: DEFINE HYPOTHESIS
  H₀: Conversion rate_B = Conversion rate_A
  H₁: Conversion rate_B > Conversion rate_A (one-tailed)
  OR  Conversion rate_B ≠ Conversion rate_A (two-tailed)

STEP 2: DECIDE METRIC
  Primary metric: Conversion rate
  Guardrail metrics: Page load time, bounce rate (must not get worse)

STEP 3: SAMPLE SIZE CALCULATION
  How many users needed to detect a meaningful difference?
  (Power analysis — covered in Ch 4.6)

STEP 4: RANDOM ASSIGNMENT
  Each user randomly gets Version A or Version B
  (50/50 or other split)

STEP 5: RUN EXPERIMENT
  Collect data until sample size reached (NOT until p<0.05!)

STEP 6: STATISTICAL TEST
  Use two-proportion z-test or chi-square test

STEP 7: DECISION
  Statistical significance + Practical significance + Business judgment
```

---

## 4.5.2 — Two-Proportion Z-Test for A/B Testing

### 📖 Formula

$$z = \frac{\hat{p}_A - \hat{p}_B}{\sqrt{\hat{p}(1-\hat{p})\left(\frac{1}{n_A} + \frac{1}{n_B}\right)}}$$

Where:
$$\hat{p} = \frac{x_A + x_B}{n_A + n_B}$$

(the pooled proportion)

| Symbol | Meaning |
|--------|---------|
| p̂_A, p̂_B | Observed conversion rates |
| p̂ (pooled) | Combined conversion rate |
| n_A, n_B | Sample sizes |

### 📊 Worked Example

Old page (A): 500 visitors, 45 converted (p̂_A = 0.090)
New page (B): 500 visitors, 60 converted (p̂_B = 0.120)

```
p̂_pooled = (45+60)/(500+500) = 105/1000 = 0.105

SE = √(0.105×0.895×(1/500+1/500))
   = √(0.094×0.004)
   = √0.000376 = 0.0194

z = (0.09 - 0.12) / 0.0194 = -0.03/0.0194 = -1.546

p-value (two-tailed) = 2×P(Z<-1.546) = 2×0.061 = 0.122

p = 0.122 > α = 0.05 → Fail to reject H₀

NOT statistically significant. We can't conclude B is better.
(Even though B had 30% more conversions, the sample is too small!)
```

---

## 4.5.3 — Critical A/B Testing Concepts

### Novelty Effect
Users interact with new version just because it's new, not because it's better. Give experiment time to stabilize.

### Network Effects
If users interact with each other, random assignment violates independence (e.g., social media).

### Holdout Sets
Keep a control group permanently to measure long-term cumulative impact of all changes.

### Stopping Rules (Peeking Problem)

> **NEVER stop an A/B test early because you see p<0.05!**

```
Imagine: You check results every day for 20 days.
Each day, p-value fluctuates.
Eventually (by chance), p may dip below 0.05 for one day.
If you stop then, you've done a Type I error.

True α per experiment (with 20 peeks) >> 0.05
Could be up to 30-40%!

SOLUTION: Pre-register your stopping rule.
  - Stop at pre-determined n, NOT when p<0.05
  - OR use sequential testing methods (SPRT, Always Valid Inference)
```

### Practical vs Statistical Significance

```
Statistical significance: p < 0.05 (yes, something is going on)
Practical significance: effect size is large enough to matter

Example:
A/B test on Amazon checkout: +0.01% conversion
Sample: 10 million users
Result: p < 0.0001 (highly significant!)
Practical: 0.01% × $50 average order × 10M visits/month = $50,000/month
Is it worth the engineering cost to implement? MAYBE.

Never make business decisions from p-value alone!
```

---

## 4.5.4 — Multi-Armed Bandit Alternative

Traditional A/B test: Assign 50/50 to A and B regardless of performance.

**Problem:** You lose revenue during the test by sending half users to the worse version.

**Bandit approach:** Continuously update assignment probabilities based on observed performance. Better versions get more traffic dynamically.

```
Epsilon-greedy strategy:
- With probability ε (e.g., 10%): explore randomly (try all versions)
- With probability 1-ε: exploit (send to current best version)

Thompson Sampling: Bayesian approach — sample from posterior distributions of each variant's success rate.
```

🚀 Used in: Google Ads, Bing ranking, Netflix content display.

---

# Chapter 4.6 — Statistical Power

> **Before you run a study, ensure you can actually detect the effect you're looking for.**

---

## 💡 Intuition

> **Power = your ability to detect a real effect.**

Imagine trying to hear a whisper in a noisy room.
- If the whisper is loud enough (large effect)
- And the room is quiet enough (low variance)
- And your hearing is sharp enough (large sample size)
→ You'll hear it (high power).

Low power = you might miss a real effect (Type II error).

```
Power = P(Reject H₀ | H₁ is true) = 1 - β

Desired power: typically 0.80 (80%)
This means: 80% chance of detecting a true effect
            20% chance of missing it (β = 0.20)
```

---

## 4.6.1 — Factors That Determine Power

```
Power increases when:
1. Effect size is LARGER (easier to detect big differences)
2. Sample size is LARGER (more data = less noise)
3. Significance level α is HIGHER (easier to reject H₀)
4. Population variance is SMALLER (less noise)
5. One-tailed test instead of two-tailed
```

---

## 4.6.2 — Effect Size

> **Effect size measures how large a real difference is, independent of sample size.**

### Cohen's d (for comparing means)

$$d = \frac{\mu_1 - \mu_2}{\sigma_{pooled}}$$

| Cohen's d | Interpretation |
|----------|---------------|
| 0.2 | Small effect |
| 0.5 | Medium effect |
| 0.8 | Large effect |

**Example:** Drug reduces blood pressure by 5 mmHg, σ=10 mmHg.
d = 5/10 = 0.5 → Medium effect size.

### Cohen's h (for proportions)

$$h = 2\arcsin(\sqrt{p_1}) - 2\arcsin(\sqrt{p_2})$$

| h | Interpretation |
|---|---------------|
| 0.2 | Small |
| 0.5 | Medium |
| 0.8 | Large |

---

## 4.6.3 — Sample Size Calculation

### Formula (one-sample, two-tailed z-test)

$$n = \frac{(z_{\alpha/2} + z_\beta)^2 \sigma^2}{\delta^2}$$

Where:
- z_α/2: critical z for significance (1.96 for α=0.05)
- z_β: critical z for power (0.84 for 80% power)
- σ: population SD (estimated from pilot data)
- δ: minimum detectable effect (smallest difference worth detecting)

### 📊 Example — A/B Test Sample Size

Current conversion rate = 5%. Want to detect ≥1.5% improvement (to 6.5%).
α=0.05, power=80%.

```
Using the two-proportion formula:
p₁ = 0.05, p₂ = 0.065
p_bar = (0.05+0.065)/2 = 0.0575

n per group ≈ (z_α/2 + z_β)² × 2×p_bar×(1-p_bar) / (p₂-p₁)²
            = (1.96+0.84)² × 2×0.0575×0.9425 / (0.015)²
            = 7.84 × 0.1084 / 0.000225
            = 0.8499 / 0.000225
            ≈ 3,778 per group (7,556 total)

Need 7,556 users to have 80% power of detecting a 1.5% improvement.
```

### The Power-Sample Size Trade-off

```
                     POWER CURVE

Power
1.0 |                            ─────────────
0.9 |                       ────
0.8 |─────────────────────→  (80% target)
0.7 |              ────
0.6 |       ────
0.5 |  ───
0.4 |
    └──────────────────────────────────────
       0     500  1000  1500  2000  2500
                     Sample Size (n)

Adding more n gives diminishing returns in power.
First 500 → big power gain.
Going from 2000 to 2500 → tiny power gain.
```

---

## 4.6.4 — Power Analysis in Python

```python
from statsmodels.stats.power import TTestPower, zt_ind_solve_power
import numpy as np

# Sample size for two-sample t-test
analysis = TTestPower()
n = analysis.solve_power(
    effect_size=0.5,      # Cohen's d = medium effect
    alpha=0.05,           # significance level
    power=0.80,           # desired power
    alternative='two-sided'
)
print(f"Required n per group: {n:.0f}")  # ≈ 64

# For a proportion test (A/B test)
n = zt_ind_solve_power(
    effect_size=0.2,      # Cohen's h
    alpha=0.05,
    power=0.80
)
print(f"Required n per group: {n:.0f}")
```

---

# 📝 Practice Questions — Chapters 4.4–4.6

## 🟢 Beginner (8 Questions)

**Q1.** When would you use a t-test instead of a z-test?
**Q2.** Chi-square test: What does a large χ² value indicate?
**Q3.** A/B test: Variant A gets 20 conversions from 200 visitors. Variant B gets 30 from 200. Which performed better? Is this necessarily significant?
**Q4.** What is the primary advantage of ANOVA over running multiple t-tests?
**Q5.** If power = 0.80, what is the Type II error rate (β)?
**Q6.** Cohen's d = 0.8. Is this a small, medium, or large effect?
**Q7.** What happens to required sample size as desired power increases from 80% to 90%?
**Q8.** What is the "peeking problem" in A/B testing?

## 🟡 Intermediate (7 Questions)

**Q9.** Three groups: n_1=30 (x̄=50, s=8), n_2=30 (x̄=55, s=9), n_3=30 (x̄=52, s=7). Should you use t-tests or ANOVA? Why?
**Q10.** Chi-square independence test: Survey of 300 people. Build expected frequency table for gender × preference.
**Q11.** Calculate Cohen's d: Group A (μ=70, σ=10), Group B (μ=80, σ=10). Is this practically significant?
**Q12.** You run an A/B test and get p=0.04 with n=100 per group. Your colleague says to collect 200 more per group before deciding. Who's right?
**Q13.** Paired vs independent t-test: Same employees tested before and after training. Which test and why? What's the advantage?
**Q14.** A/B test shows +2% conversion with p=0.01. Implementing the change costs ₹50 lakhs. The site gets 500,000 visits/month with ₹100 average order. Should you implement?
**Q15.** What is post-hoc testing in ANOVA? Why is it needed?

## 🔴 Advanced (5 Questions)

**Q16.** Derive the F-statistic: Show that F = between-group MS / within-group MS follows an F-distribution under H₀.
**Q17.** Multiple comparison problem: With 10 groups in ANOVA (if significant), how many pairwise comparisons are possible? Apply Bonferroni to control FWER at 5%.
**Q18.** Power as a function of effect size: Draw the power curve for d=0.2, 0.5, 0.8 as n varies from 10 to 200.
**Q19.** Sequential analysis (SPRT — Sequential Probability Ratio Test): What is the key idea and how does it solve the peeking problem?
**Q20.** Bayesian A/B testing: How does reporting P(B>A) differ from p-value? Which is more interpretable for business stakeholders?

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** Three ML models evaluated on 1000 test examples each, with accuracy: Model A=87%, B=90%, C=88%. Is there a significant difference? Formulate the test.

**DS2.** Feature engineering: You create 50 new features and test each for correlation with the target. How many would you expect to be "significant" at α=0.05 by chance alone? What correction should you apply?

**DS3.** Product team wants to detect 0.5% conversion improvement (from 3% to 3.5%) in an A/B test with 80% power. Calculate approximate required sample size per group. Is this feasible if the site gets 5,000 visitors per day and the test must finish in 2 weeks?

**DS4.** Your model achieves AUC=0.85 on Dataset A and AUC=0.82 on Dataset B. Is there a statistically significant difference? Describe how you would set up this test.

**DS5.** Experimentation at scale: You run 500 A/B tests per year at your company. You use α=0.05. How many tests will have false significant results? Design an organizational policy to manage this.

---

# ✅ Key Solutions

**A1.** When σ is unknown (almost always) AND for any n; also when n<30. T-test uses s (sample SD) and t-distribution; z-test uses σ (population SD) and normal distribution.

**A3.** Rates: A=10%, B=15%. B performed better. Not necessarily significant — need hypothesis test. With small n, 5% difference can easily be random chance.

**A4.** Controls family-wise error rate. Multiple t-tests inflate Type I error exponentially: k=5 groups → 10 pairs → P(≥1 FP) = 1-(0.95)^10 = 40%.

**A5.** β = 1-0.80 = 0.20 = 20%.

**A11.** d = (80-70)/10 = 1.0. Large effect size. Practically significant — a 10-point gap when typical SD is 10 is meaningful.

**A14.** Monthly uplift = 2% × 500,000 × ₹100 = ₹10,00,000/month. Payback period = ₹50 lakhs / ₹10 lakhs = 5 months. Statistically significant AND practically significant → implement.

**DS2.** Expected false positives = 50 × 0.05 = 2.5 features appear significant by chance. Apply Bonferroni correction: use α = 0.05/50 = 0.001 per feature. Or use FDR control (Benjamini-Hochberg).

**DS3.** p̄ = (0.03+0.035)/2 = 0.0325. Using formula: n ≈ (1.96+0.84)² × 2×0.0325×0.9675 / (0.005)² ≈ 7.84 × 0.0629/0.000025 ≈ 19,750 per group = 39,500 total. 5,000/day × 14 days = 70,000 visitors. Split 50/50 = 35,000 per group < 39,500 needed. Not feasible in 2 weeks. Options: extend test or accept lower power.

**DS5.** Expected false significant results = 500 × 0.05 = 25 tests per year. Policy: (1) Register experiments with primary metric before running. (2) Apply Bonferroni or FDR correction across tests. (3) Require replication of surprising results. (4) Use sequential testing. (5) Report effect sizes alongside p-values.

---

# 💼 Interview Questions

**IQ1.** "How would you decide between parametric and non-parametric tests?"
Parametric (t-test, ANOVA) when: data approximately normal OR n≥30, continuous data, variance assumptions met. Non-parametric (Mann-Whitney, Kruskal-Wallis) when: non-normal with small n, ordinal data, severe outliers, or robust analysis needed. Parametric tests have more power when assumptions hold; non-parametric are safer when they don't.

**IQ2.** "How do you determine sample size for an A/B test?"
Need: (1) Baseline conversion rate, (2) Minimum detectable effect (MDE) — smallest change worth implementing, (3) α (significance level, usually 0.05), (4) Power (usually 0.80). Use power analysis formula or tools like Evan Miller's A/B calculator. Larger sample needed for: smaller MDE, lower α, higher power, lower baseline rate.

**IQ3.** "What is the difference between statistical and practical significance?"
Statistical significance (p<α): the effect is unlikely to be zero. Practical significance: the effect is large enough to matter for business/science. A tiny effect can be statistically significant with a large enough sample but not worth acting on. Always report effect size, not just p-value.

---

# 📌 Part 4 Complete Summary

## ⚡ Quick Revision

**Statistical Tests:**
- z-test: known σ or large n, comparing means
- t-test: unknown σ, one/two samples, paired
- Chi-square: categorical data, independence or GoF
- ANOVA: comparing 3+ group means, controls Type I error

**A/B Testing:**
- Random assignment, one primary metric, pre-determined stopping rule
- Two-proportion z-test for conversion rates
- Statistical + practical significance both matter
- Never peek at results and stop early

**Statistical Power:**
- Power = 1-β = P(detect real effect)
- Increases with: larger n, larger effect, higher α
- Effect size (Cohen's d or h) measures practical magnitude
- Always calculate required n BEFORE running the study

## 📐 Formula Sheet

| Formula | Name |
|---------|------|
| z = (x̄-μ₀)/(σ/√n) | One-sample z-test |
| t = (x̄-μ₀)/(s/√n), df=n-1 | One-sample t-test |
| t = d̄/(s_d/√n) | Paired t-test |
| χ² = Σ(O-E)²/E | Chi-square test |
| F = MS_between/MS_within | ANOVA F-statistic |
| d = (μ₁-μ₂)/σ_pooled | Cohen's d effect size |
| n = (z_α/2+z_β)²σ²/δ² | Sample size formula |

---
*Part 4 Complete | Next: Part 5 — Advanced Probability & Statistics*
