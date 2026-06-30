# 🎯 DS Statistics Interview Q&A — Batch 4
# Topic: Inferential Statistics & Hypothesis Testing
# Level: Basic → Advanced | Cross-questions included

---

## 🗂️ Legend
- 🔥 = Frequently asked | ⭐ = Very important | 🔥⭐ = Both

---

# SECTION 1 — BASIC LEVEL

---

## Q1. 🔥⭐ What is a confidence interval? How do you interpret it correctly?

**A:**
A **confidence interval** is a range of plausible values for a population parameter, constructed from sample data with a stated level of confidence.

**Formula (for population mean, known σ):**
$$CI = \bar{x} \pm z^* \times \frac{\sigma}{\sqrt{n}}$$

**Common critical values:**
- 90% CI: z* = 1.645
- 95% CI: z* = 1.960
- 99% CI: z* = 2.576

**Example:**
Sample of 100 users: x̄ = 72 minutes (average session time), σ = 15 minutes.
95% CI: 72 ± 1.96 × (15/√100) = 72 ± 2.94 = **(69.06, 74.94)**

**CORRECT interpretation:**
"If we repeated this study many times with different random samples, approximately 95% of the resulting confidence intervals would contain the true population mean."

**INCORRECT interpretations (very common):**
❌ "There is a 95% probability the true mean is between 69.06 and 74.94."
❌ "95% of users have session times between 69.06 and 74.94."
❌ "This interval contains the true mean 95% of the time."

The interval is FIXED once computed — the true mean either is or isn't in it. The 95% refers to the long-run performance of the METHOD, not this specific interval.

↳ **Cross Q:** What happens to CI width when you increase the sample size?
↳ **Cross A:** Width = 2 × z* × σ/√n. Width decreases proportionally to 1/√n. To halve the width, quadruple the sample size. Intuition: more data → better estimate → tighter bounds. Conversely, increasing confidence level (90% → 99%) widens the interval. These two factors work against each other: you can have a narrow CI OR high confidence, but gaining both requires more data. Practical implication: when planning a study, decide minimum meaningful precision (acceptable CI width) and required confidence level → calculate minimum n. Don't collect less data than needed.

---

## Q2. 🔥⭐ What is a p-value? What are common misinterpretations?

**A:**
**Formal definition:**
The p-value is the probability of observing test results at least as extreme as the observed results, assuming the null hypothesis is true.

p-value = P(data as extreme as observed | H₀ is true)

**Example:**
Testing if a coin is fair. You flip 10 times, get 8 heads.
H₀: p = 0.5 (fair coin)
p-value = P(8 or more heads | p=0.5) = P(X≥8) where X~Bin(10,0.5) = 0.055

Interpretation: If the coin were truly fair, there's a 5.5% chance of getting 8+ heads by luck. Not quite convincing enough to reject H₀ at α=0.05.

**CORRECT interpretation:**
"Assuming H₀ is true, the probability of seeing data this extreme or more extreme by random chance is p."

**Common INCORRECT interpretations:**
❌ "p = probability the null hypothesis is true"
❌ "p = probability we made an error"
❌ "p = probability the result is due to chance" (partially wrong — it IS the probability of this data under H₀, but H₀ is assumed not proven)
❌ "1-p = probability the alternative is true"
❌ "Small p = large effect size" (a huge sample can give tiny p for a trivially small effect)

**What p-value does NOT tell you:**
1. The probability H₀ is true
2. The size of the effect (use Cohen's d, η², etc.)
3. Whether the result is practically important
4. Whether you'll replicate the result

↳ **Cross Q:** What is the replication crisis and how does p-value misuse contribute?
↳ **Cross A:** The replication crisis: many published scientific findings (especially psychology, medicine) fail to replicate when tested again. Contributing factors from p-value misuse: (1) P-hacking: testing many things, reporting only significant results. With 20 tests at α=0.05, one significant result expected by chance. (2) HARKing (Hypothesizing After Results are Known): finding an interesting result by data mining, then framing it as confirmatory. (3) Publication bias: journals publish significant (p<0.05) results; null results are filed away — the published literature is systematically skewed. (4) Low power: many studies are underpowered (β-error risk high), so significant findings may be inflated effect sizes. Solutions: pre-registration, pre-specified analysis plans, larger samples, Bayesian methods, focusing on effect sizes and confidence intervals rather than p<0.05 binary.

---

## Q3. 🔥⭐ What is the difference between Type I and Type II errors?

**A:**

|  | H₀ is TRUE | H₀ is FALSE |
|--|------------|-------------|
| **Reject H₀** | **Type I Error (α)** — False Positive | **Correct** — True Positive |
| **Fail to reject H₀** | **Correct** — True Negative | **Type II Error (β)** — False Negative |

**Type I Error (α — False Positive):**
- Rejecting H₀ when it is actually TRUE
- "Seeing" an effect that doesn't exist
- Probability = α (significance level, usually 0.05)
- Controlled by choice of α BEFORE the test

**Type II Error (β — False Negative):**
- Failing to reject H₀ when it is actually FALSE
- Missing a real effect
- Probability = β
- Statistical **Power = 1 − β** (probability of correctly detecting a real effect)
- Decreases with larger sample size and larger effect size

**Real-world examples:**

| Scenario | Type I Error | Type II Error |
|----------|-------------|---------------|
| Medical drug trial | Approve ineffective drug | Reject effective drug |
| Spam filter | Block legitimate email | Let spam through |
| Fraud detection | Flag legitimate transaction | Miss real fraud |
| Quality control | Reject good batch | Ship defective batch |

**Which is worse?** Context-dependent:
- Cancer screening: Type II worse (miss real cancer → patient goes untreated)
- Drug safety approval: Type I worse (approve dangerous drug)
- Spam filter: Balance needed (missing too many spam = bad, blocking legit = bad)

↳ **Cross Q:** "You set α = 0.01 instead of 0.05. How does this affect Type I and Type II errors?"
↳ **Cross A:** Decreasing α from 0.05 to 0.01: (1) Type I error (α) DECREASES — you're now willing to false alarm only 1% of the time vs 5%. The test requires stronger evidence to reject H₀. (2) Type II error (β) INCREASES — because you've moved the rejection threshold further into the tail, making it harder to reject H₀ even when it's false. Power (1-β) DECREASES. This is the fundamental trade-off: reducing Type I risk increases Type II risk for a fixed sample size. To reduce BOTH: increase sample size. Alternatively, accept that for different decisions, different α/β trade-offs are optimal (clinical trials: α=0.01, exploratory research: α=0.10).

---

## Q4. 🔥⭐ When do you use a z-test vs a t-test?

**A:**

| Criterion | Z-test | T-test |
|-----------|--------|--------|
| Population σ | Known | Unknown (use sample s) |
| Sample size | n ≥ 30 (large) | Any n, but designed for small n |
| Distribution used | Standard Normal N(0,1) | t-distribution with df=n-1 |
| Critical values | 1.645 (90%), 1.96 (95%), 2.576 (99%) | Larger (heavier tails), depend on df |
| When n is large | Both give same result | t → z as n→∞ |

**In practice:**
- σ is almost NEVER known in real problems
- t-test is almost always the correct choice
- With n ≥ 30, t-critical ≈ z-critical (difference negligible)
- With n < 30, t-test gives wider intervals (more uncertainty) — correct!

**Types of t-tests:**

1. **One-sample t-test:** Compare sample mean to known value
   H₀: μ = μ₀, t = (x̄ - μ₀) / (s/√n), df = n-1

2. **Two-sample (independent) t-test:** Compare means of two groups
   H₀: μ₁ = μ₂, t = (x̄₁ - x̄₂) / SE_pooled, df = n₁+n₂-2

3. **Paired t-test:** Compare means when observations are matched
   H₀: μ_diff = 0, t = d̄ / (s_d/√n), df = n-1

↳ **Cross Q:** Why does the t-distribution have heavier tails than the Normal?
↳ **Cross A:** The t-distribution accounts for double uncertainty: we're estimating μ from x̄ (sampling variability) AND using s instead of σ (additional uncertainty from estimating the variance). With small n, our estimate of σ via s is imprecise — s could be much smaller or larger than σ. This creates extra variability in the t-statistic beyond what the Normal captures. As n→∞: (1) s → σ (variance estimate becomes precise), (2) t → z (t-distribution approaches Normal). The heavier tails of the t-distribution mean wider confidence intervals — correctly reflecting our greater uncertainty with small samples. Degrees of freedom df = n-1 determines how heavy the tails are: df=1 (very heavy tails, Cauchy-like), df=∞ (Normal distribution).

---

## Q5. 🔥 What is ANOVA? Why use it instead of multiple t-tests?

**A:**
**ANOVA (Analysis of Variance)** tests whether the means of three or more groups are equal.

H₀: μ₁ = μ₂ = ... = μₖ (all group means equal)
H₁: At least one mean differs

**Why not just run all pairwise t-tests?**
With k groups: C(k,2) = k(k-1)/2 pairwise t-tests needed.
For k=5 groups: 10 t-tests, each at α=0.05.

P(at least one false positive) = 1-(0.95)¹⁰ = 1-0.599 = **40.1%**

This catastrophic inflation of Type I error is called the **familywise error rate (FWER)**.

ANOVA tests all groups simultaneously at the correct α level.

**The F-statistic:**
$$F = \frac{MS_{between}}{MS_{within}} = \frac{\text{Variance between group means}}{\text{Variance within groups (noise)}}$$

Large F → group means differ more than expected from random noise → reject H₀.

**ANOVA assumptions:**
1. Observations are independent
2. Each group is normally distributed (or large n for CLT)
3. Homoscedasticity: equal variances across groups (test with Levene's test)

↳ **Cross Q:** If ANOVA is significant, how do you find WHICH groups differ?
↳ **Cross A:** ANOVA tells you "at least one mean differs" but not which pairs. Post-hoc tests compare specific pairs while controlling for multiple comparisons: (1) Tukey's HSD (Honest Significant Difference): compares all pairs, controls FWER. Most commonly used. (2) Bonferroni correction: divide α by number of comparisons — conservative. (3) Scheffé's method: most conservative, but allows complex contrasts. (4) Games-Howell: when group variances are unequal (heteroscedastic). (5) Dunnett's test: compare only against a control group (fewer comparisons). In practice, for exploratory analysis with a few groups, Tukey's HSD is the default. For highly unequal group sizes or variances, Welch's ANOVA (non-homoscedastic version) + Games-Howell post-hoc is preferred.

---

## Q6. 🔥 What is the Chi-square test? When do you use it?

**A:**
The chi-square test (χ²) is used for **categorical data**. Two main versions:

**1. Chi-square Goodness-of-Fit test:**
Tests whether observed categorical frequencies match expected frequencies.
H₀: Observed frequencies = Expected frequencies

**Example:** Are die rolls fair?
Rolled 60 times: got 1→8, 2→12, 3→9, 4→11, 5→10, 6→10
Expected: 10 each (fair die)
χ² = (8-10)²/10 + (12-10)²/10 + (9-10)²/10 + ... = 0.4+0.4+0.1+0.1+0+0 = 1.0
df = k-1 = 5, critical value at 0.05 = 11.07. 1.0 < 11.07 → fail to reject → die appears fair.

**2. Chi-square Test of Independence:**
Tests whether two categorical variables are related or independent.
H₀: The two variables are independent
$$\chi^2 = \sum_{all cells} \frac{(O_{ij} - E_{ij})^2}{E_{ij}}, \quad E_{ij} = \frac{R_i \times C_j}{N}$$

df = (rows - 1) × (columns - 1)

**Assumptions:**
1. Data are frequencies/counts (not percentages or proportions)
2. Expected frequency in each cell ≥ 5 (otherwise use Fisher's exact test)
3. Observations are independent

↳ **Cross Q:** What is Cramér's V and when do you use it after a significant chi-square test?
↳ **Cross A:** Cramér's V is an effect size measure for chi-square tests: V = √(χ²/(N × min(r-1, c-1))). Always in [0,1]. Interpretation: 0=no association, 1=perfect association. Benchmarks (similar to Cohen's w): 0.1=small, 0.3=medium, 0.5=large. Why needed: chi-square's magnitude depends on N — a large sample can give a highly significant χ² for a trivially small association. Cramér's V normalizes for sample size and table dimensions. Example: In a 2×2 table with N=10,000, χ²=25 gives V=√(25/10000)=0.05 — tiny association despite very significant p-value. Always report both χ² p-value AND Cramér's V (or similar effect size) in research.

---

# SECTION 2 — INTERMEDIATE LEVEL

---

## Q7. 🔥⭐ What is statistical power? How do you ensure adequate power?

**A:**
**Statistical power** is the probability of correctly rejecting the null hypothesis when it is false.

Power = P(reject H₀ | H₁ is true) = 1 − β

**Factors affecting power:**
1. **Sample size n:** Larger n → more power (fundamental)
2. **Effect size δ:** Larger true effect → easier to detect → more power
3. **Significance level α:** Larger α → more power (but more Type I errors)
4. **Population variance σ²:** Smaller variance → easier to detect effects → more power
5. **One-tailed vs two-tailed:** One-tailed test has more power (but use only when justified)

**Sample size formula (two-sided z-test):**
$$n = \frac{(z_{\alpha/2} + z_\beta)^2 \sigma^2}{\delta^2}$$

For α=0.05 and 80% power: (z₀.₀₂₅ + z₀.₂)² = (1.96+0.84)² = 7.84

**Planning power analysis (BEFORE the study):**
1. Choose α (usually 0.05)
2. Choose desired power (usually 0.80, sometimes 0.90 for high-stakes)
3. Estimate effect size from pilot data or domain knowledge
4. Estimate σ from pilot data or literature
5. Calculate required n

**Ensuring adequate power:**
- Calculate n before collecting data — don't "peek" at results
- If n is too expensive, consider: reduce α (allows smaller n for same power if you're willing to accept more Type I error... actually increases needed n), or accept smaller effect size threshold, or get a larger budget
- Use paired designs when possible (paired t-test has more power than two-sample)
- Reduce measurement error (lower σ)

↳ **Cross Q:** "Your A/B test ran for 2 weeks with 5000 users and showed p=0.08. Your manager says 'run it for 2 more weeks to get significance.' Is this a good idea?"
↳ **Cross A:** NO — this is one of the most common statistical mistakes. If you planned for n=5000 and got p=0.08, running longer until p<0.05 (peeking) inflates the Type I error rate far above α=0.05. With 5 looks at the data (weekly), the actual Type I error rate can reach 20% or more. Correct approach: (1) The sample size should have been calculated BEFORE the test based on MDE, α, and desired power. (2) If n was too small (underpowered), acknowledge that and plan a new, properly powered study — don't extend the existing one. (3) If you must do interim analyses, use sequential testing methods (alpha spending functions: O'Brien-Fleming, Pocock boundaries) that maintain overall α=0.05 while allowing stopping. "Running until significant" is p-hacking.

---

## Q8. 🔥⭐ What are parametric vs non-parametric tests? When do you use each?

**A:**
**Parametric tests:** Assume the data follows a specific distribution (usually Normal). Use moments (mean, variance) of the distribution.

**Non-parametric tests:** Make minimal distributional assumptions. Operate on ranks or signs, not raw values.

**Comparison:**

| Aspect | Parametric | Non-parametric |
|--------|-----------|----------------|
| Assumption | Normal distribution, continuous data | Minimal — any distribution |
| Sample size | Need n≥30 (or normality verified) | Work for any n |
| Power | Higher (when assumptions met) | Lower (more conservative) |
| Data type | Continuous, ratio/interval | Any — including ordinal |
| Outliers | Sensitive | Robust |
| Speed | Fast | Fast (rank-based) |

**Common tests and non-parametric equivalents:**

| Parametric | Non-parametric equivalent | When to switch |
|-----------|--------------------------|---------------|
| One-sample t-test | Wilcoxon signed-rank test | Non-normal, small n |
| Two-sample t-test | Mann-Whitney U test | Non-normal, ordinal data |
| Paired t-test | Wilcoxon signed-rank | Non-normal differences |
| One-way ANOVA | Kruskal-Wallis test | Non-normal or ordinal groups |
| Pearson correlation | Spearman correlation | Non-linear monotonic, ordinal |

**When to use non-parametric:**
1. Ordinal data (Likert scales: 1-5 rating)
2. Very small samples (n<15) where normality can't be verified
3. Confirmed non-normal data (after checking with Shapiro-Wilk)
4. Presence of outliers that can't be removed
5. When robustness is more important than power

↳ **Cross Q:** If n is large (n=500), does it matter whether you use parametric or non-parametric?
↳ **Cross A:** For large n (≥100), by CLT: (1) Sample means are approximately normal regardless of the underlying distribution → parametric tests are valid. (2) The two-sample t-test and Mann-Whitney U test give nearly identical results. (3) The difference in power between parametric and non-parametric is negligible. So for large n with continuous data: use parametric (more familiar, more software support, more interpretable effect sizes). The only case where non-parametric is still preferred for large n: ordinal data (Likert scales) where the mean of ordinal labels has no clear interpretation — use Spearman correlation and Mann-Whitney. Also for multi-modal or extremely heavy-tailed distributions even with large n.

---

## Q9. 🔥⭐ What is the multiple testing problem and how do you correct for it?

**A:**
**The problem:** When you run many hypothesis tests simultaneously, the probability of at least one false positive (Type I error) increases dramatically, even if all null hypotheses are true.

**FWER (Familywise Error Rate):** P(at least one false positive)
With m tests at α = 0.05:
FWER = 1 − (1-0.05)^m

| m tests | FWER |
|---------|------|
| 1 | 5.0% |
| 5 | 22.6% |
| 10 | 40.1% |
| 20 | 64.2% |
| 100 | 99.4% |

**Real ML examples:**
- Testing 50 features for correlation with target: ~13 false discoveries at α=0.05
- Testing 100 model variants: 5 appear significant by chance
- Sequential model evaluation: each check inflates false positive rate

**Corrections:**

**1. Bonferroni Correction (FWER control):**
Use α_corrected = α/m for each test
Very conservative — often too stringent for exploratory analysis
Best when: small m, tests are independent, Type I error is very costly

**2. Holm-Bonferroni (sequential, less conservative):**
Sort p-values: p₁ ≤ p₂ ≤ ... ≤ pₘ
Reject pᵢ if pᵢ ≤ α/(m-i+1) for all previous rejections
More powerful than Bonferroni, controls FWER

**3. Benjamini-Hochberg (FDR control):**
FDR = E[false discoveries / total discoveries]
Sort p-values; reject pᵢ if pᵢ ≤ (i/m)×q
Controls expected proportion of false discoveries at q (e.g., 5%)
Most useful for exploratory analysis: you expect some false discoveries and accept q%
Standard approach for genomics, feature selection

↳ **Cross Q:** What is the difference between FWER and FDR control? Which is more appropriate for ML feature selection?
↳ **Cross A:** FWER control (Bonferroni/Holm): guarantees P(ANY false discovery)≤α. Very conservative. Appropriate when even one false positive is costly (e.g., drug approval, legal decisions). FDR control (Benjamini-Hochberg): guarantees E[false discoveries / total discoveries]≤q. Less conservative — allows some false discoveries as long as their proportion is controlled. Appropriate when: (1) Testing many features, (2) Some false positives are acceptable, (3) Missing true features is costly, (4) Results will be validated downstream. For ML feature selection: BH (FDR) is almost always the right choice. If you test 100 features with q=0.05, you might have ~5% of selected features be noise — acceptable when downstream cross-validation will further filter them. Bonferroni would require p<0.0005 per feature, likely dropping many truly useful features.

---

## Q10. 🔥⭐ Explain the concept of effect size. Why is it more important than p-value?

**A:**
**Effect size** measures the MAGNITUDE of the difference or relationship — independent of sample size.

**Common effect size measures:**

**Cohen's d (comparing two means):**
$$d = \frac{\mu_1 - \mu_2}{\sigma_{pooled}}$$
| d | Interpretation |
|---|---------------|
| 0.2 | Small |
| 0.5 | Medium |
| 0.8 | Large |

**Cohen's h (comparing two proportions):**
$$h = 2\arcsin(\sqrt{p_1}) - 2\arcsin(\sqrt{p_2})$$
Similar benchmarks: 0.2/0.5/0.8 for small/medium/large.

**r (for correlation):** Already an effect size. |r|: 0.1=small, 0.3=medium, 0.5=large.

**η² (eta-squared) for ANOVA:** Proportion of variance explained by group membership.

**Why effect size > p-value:**

1. **p-value depends on n:** With n=10M, even d=0.001 (trivial effect) gives p<0.0001.
2. **Effect size is practical:** d=0.8 means the treatment group improved by 0.8 standard deviations — large and meaningful regardless of sample size.
3. **Business decisions need magnitude:** "Conversion rate increased significantly" — how much? +0.001% or +5%?
4. **Replication:** Effect sizes are more stable across studies; p-values are not.

**Practical example:**
Drug trial with n=100,000: lowered blood pressure by 0.5 mmHg, p=0.00001.
Cohen's d ≈ 0.05 (negligible). Despite extreme statistical significance, the drug has no meaningful clinical effect.

↳ **Cross Q:** How do you report results properly in a data science context?
↳ **Cross A:** Complete results reporting for ML/DS should include: (1) Point estimate: "Conversion rate increased by 1.5 percentage points (from 5.0% to 6.5%)." (2) Confidence interval: "95% CI: (0.8%, 2.2%)." (3) Statistical test: "Two-proportion z-test: z=2.94, p=0.003." (4) Effect size: "Relative lift: +30%. Cohen's h=0.07 (small effect)." (5) Sample size and power: "n=5,000 per variant; power=0.85 at MDE=1%." (6) Practical significance statement: "A 1.5pp increase represents ~₹15L additional monthly revenue, exceeding the cost of implementing the change (₹5L)." Never just report "p<0.05, statistically significant" — that's incomplete and potentially misleading.

---

## Q11. 🔥 What is the sampling distribution of the mean? Why is it important?

**A:**
The **sampling distribution of the mean** is the probability distribution of all possible sample means x̄ computed from samples of size n from the same population.

**Key properties:**
1. Center: E[X̄] = μ (sample mean is unbiased for population mean)
2. Spread: SD[X̄] = σ/√n (standard error of the mean)
3. Shape: By CLT, approximately N(μ, σ²/n) for large n

**Visualization:**
```
POPULATION (any shape):
│  ██
│ ████
│ ██████
│████████
└────────────── x

SAMPLING DISTRIBUTION of X̄ (n=30):
│    ████
│   ██████
│  ████████
│ ██████████
└────────────── x̄

→ More symmetric, narrower, approaching Normal!
```

**Why it matters:**
1. **Confidence intervals** use the sampling distribution: x̄ ± z*(σ/√n)
2. **Hypothesis tests** use the sampling distribution: z = (x̄-μ₀)/(σ/√n)
3. **Central Limit Theorem** describes the sampling distribution → enables all parametric inference
4. **Standard Error** = standard deviation of the sampling distribution → measures estimation precision

↳ **Cross Q:** What is the difference between standard deviation and standard error?
↳ **Cross A:** Standard Deviation (σ or s): measures spread/variability in the DATA. How much individual data points differ from the mean. A property of the population/sample. Standard Error (SE = σ/√n or s/√n): measures spread in the SAMPLING DISTRIBUTION of the mean. How much sample means would vary across different random samples. A property of the estimation procedure. SE is always smaller than SD (by factor 1/√n). As n→∞, SE→0 (estimates become infinitely precise), but SD doesn't change (the data is still spread). Common confusion: reporting SE when SD is expected (or vice versa) in plots with error bars — SE bars are smaller and make results look more precise than they are. Convention: in descriptive statistics, report SD; in inference, report SE.

---

# SECTION 3 — ADVANCED LEVEL

---

## Q12. 🔥⭐ Explain the bootstrap method. When is it used?

**A:**
**Bootstrap** is a resampling method for estimating the sampling distribution of a statistic without assuming a specific theoretical distribution.

**Algorithm:**
1. Have original sample of size n
2. Repeat B times (typically B=1000-10000):
   a. Draw n observations WITH REPLACEMENT from original sample → bootstrap sample
   b. Compute statistic of interest (mean, median, correlation, model accuracy...) → θ*_b
3. The B bootstrap statistics {θ*₁, ..., θ*_B} approximate the sampling distribution
4. Use this distribution for CI, SE, hypothesis tests

**Bootstrap confidence interval (percentile method):**
CI_95% = [θ*_{2.5th percentile}, θ*_{97.5th percentile}]

**When to use bootstrap:**

1. **Complex statistics** where analytical SE is unknown (e.g., median, ratio, correlation)
2. **Non-normal data** where CLT-based inference is unreliable
3. **Small samples** where asymptotic results don't apply
4. **Model performance metrics** (bootstrap CV for AUC, F1-score)
5. **Feature importance CI** (are SHAP values significantly different from zero?)

**Bootstrap confidence interval for median:**
```python
import numpy as np

data = np.array([...])  # your sample
n = len(data)
B = 10000

bootstrap_medians = []
for _ in range(B):
    sample = np.random.choice(data, size=n, replace=True)
    bootstrap_medians.append(np.median(sample))

ci_lower = np.percentile(bootstrap_medians, 2.5)
ci_upper = np.percentile(bootstrap_medians, 97.5)
print(f"95% Bootstrap CI for median: ({ci_lower:.2f}, {ci_upper:.2f})")
```

↳ **Cross Q:** What are the limitations of the bootstrap?
↳ **Cross A:** Bootstrap is powerful but has important limitations: (1) Bootstrap assumes the sample represents the population well — doesn't help if original sample is biased. (2) For very small n (n<10), bootstrap has poor coverage because it can't generate enough unique samples. (3) For heavy-tailed distributions (Cauchy, Pareto with α<2), bootstrap doesn't converge properly — the mean might not exist. (4) For time series data, simple bootstrap breaks dependence structure. Use block bootstrap or stationary bootstrap instead (preserves temporal autocorrelation by resampling blocks of consecutive observations). (5) Computationally expensive for large n with B=10000 iterations. Solutions: subsample bootstrap (smaller B), approximate bootstrap (parametric). Despite limitations, bootstrap is one of the most valuable tools in statistics — when in doubt about sampling distribution shape, bootstrap.

---

## Q13. ⭐ What is Bayesian hypothesis testing? How does it differ from frequentist?

**A:**
**Frequentist hypothesis testing:**
- Binary decision: reject or fail to reject H₀
- p-value: P(data | H₀)
- Cannot directly compare P(H₀) to P(H₁)
- Does not give probability that H₁ is true

**Bayesian hypothesis testing:**
- Gives posterior probability of each hypothesis
- P(H₀|data) and P(H₁|data) directly
- Uses Bayes factor to compare hypotheses
- Updates prior beliefs with evidence

**Bayes Factor (BF₁₀):**
$$BF_{10} = \frac{P(data | H_1)}{P(data | H_0)} = \frac{P(\text{data comes from H₁})}{P(\text{data comes from H₀})}$$

| BF₁₀ | Evidence for H₁ |
|-------|----------------|
| 1-3 | Anecdotal |
| 3-10 | Moderate |
| 10-30 | Strong |
| 30-100 | Very strong |
| >100 | Decisive |

**Bayesian test example:**
Comparing two conversion rates pₐ and p_b:
- Prior: Beta(1,1) for each rate (uniform)
- After data: posterior_a = Beta(1+conversions_A, 1+failures_A)
- Compute P(p_b > p_a) via Monte Carlo:

```python
from scipy.stats import beta
import numpy as np

# A: 50 conversions from 1000; B: 70 conversions from 1000
alpha_a, beta_a = 1+50, 1+950
alpha_b, beta_b = 1+70, 1+930

samples_a = beta.rvs(alpha_a, beta_a, size=100000)
samples_b = beta.rvs(alpha_b, beta_b, size=100000)
prob_b_better = np.mean(samples_b > samples_a)
print(f"P(B > A) = {prob_b_better:.3f}")
```

↳ **Cross Q:** What is the Jeffreys-Lindley paradox and what does it reveal?
↳ **Cross A:** The Jeffreys-Lindley paradox: for the same data, frequentist testing can reject H₀ at α=0.05 while Bayesian testing (with appropriate prior) strongly supports H₀. This occurs when n is large: (1) Frequentist: z-test has high power → detects tiny effects → p<0.05. (2) Bayesian: BF may favor H₀ because the observed effect, while statistically significant, is very small compared to what H₁ predicted. The paradox reveals a fundamental difference: frequentist p-values measure "surprising data given H₀" while Bayesian BF measures "how much more likely is the data under H₁ vs H₀." For large n, these can disagree dramatically. Resolution: both are asking different questions. The frequentist gives a calibrated Type I error; the Bayesian gives a probability of H₀ being true. Neither is wrong — they answer different questions.

---

## Q14. 🔥 What is overfitting in the context of statistical inference?

**A:**
**Overfitting in inference** (distinct from ML overfitting) occurs when:
1. **Data snooping / p-hacking:** Testing many hypotheses, selecting only significant ones → actual α >> nominal α
2. **Model specification:** Fitting complex models with many parameters to small datasets → spurious precision
3. **Post-hoc analysis:** Analyzing results to find patterns, then claiming confirmatory significance

**Examples:**

**Data Snooping:**
A researcher tests 20 different outcomes (depression, anxiety, sleep, etc.) in a clinical trial. One is significant at p=0.04. Claimed as "the drug improves depression." In reality: expected ~1 significant at α=0.05 by chance alone.

**Model Overfitting → Overconfident Inference:**
Linear regression with p=50 features, n=60 observations. R²=0.95, many coefficients "significant." But: p/n ratio = 0.83 → catastrophic overfitting. Test set R² ≈ 0 or negative.

**Solutions:**
1. Pre-register hypotheses before data collection
2. Multiple testing corrections (Bonferroni, BH)
3. Train-test split / cross-validation before any inference
4. Regularization (Ridge/LASSO)
5. Adjust degrees of freedom: information criteria (AIC, BIC) penalize complexity
6. Use hold-out set for final evaluation only

↳ **Cross Q:** What are information criteria (AIC, BIC) and when do you use them?
↳ **Cross A:** AIC (Akaike Information Criterion) = -2ℓ(θ̂) + 2k. BIC (Bayesian IC) = -2ℓ(θ̂) + k·log(n). Both balance model fit (log-likelihood) against complexity (k parameters). Lower is better. Differences: (1) BIC penalizes complexity more strongly (k·log(n) vs 2k) → selects simpler models. (2) AIC minimizes expected prediction error on new data; BIC is consistent (selects true model as n→∞ if true model is in candidate set). (3) For prediction: AIC preferred. For model selection/explanation: BIC preferred. Use cases: comparing regression models with different numbers of predictors, time series ARIMA order selection (choose p,d,q minimizing AIC/BIC), comparing GMM with different K. Neither replaces cross-validation for estimating true generalization — use CV for prediction, AIC/BIC for understanding model complexity trade-offs.

---

## Q15. ⭐ What is the Neyman-Pearson framework for hypothesis testing?

**A:**
The **Neyman-Pearson framework** (1933) provides a decision-theoretic foundation for hypothesis testing, distinct from Fisher's p-value approach.

**Key ideas:**
1. **Two hypotheses:** H₀ (null) and H₁ (specific alternative) — not just H₁ = "not H₀"
2. **Two error types:** α (Type I) and β (Type II) — explicitly control both
3. **Decision rule:** Create a rejection region C before seeing data: reject H₀ if test statistic falls in C
4. **Optimality:** Among all tests with Type I error ≤ α, find the one with maximum power against H₁

**Neyman-Pearson Lemma:**
The optimal test (most powerful) for testing H₀: θ=θ₀ vs H₁: θ=θ₁ is the **Likelihood Ratio Test (LRT):**

Reject H₀ if: Λ = L(θ₁|x)/L(θ₀|x) > c

Where c is chosen so that P(Λ > c | H₀) = α.

**Uniformly Most Powerful (UMP) test:**
For one-sided alternatives with exponential family distributions, LRT is UMP — no other test with the same α has higher power against ANY value in H₁.

**Practical implications:**
1. **Pre-specify everything** before looking at data: H₀, H₁, α, sample size, test statistic
2. **The p-value vs decision tension:** Neyman-Pearson gives a binary decision; Fisher gives a continuous measure of evidence (p-value). They're philosophically different. Most applied statistics blends both improperly.
3. **α must be chosen for its consequences** — not always 0.05.

↳ **Cross Q:** What is the uniformly most powerful (UMP) test? Does it always exist?
↳ **Cross A:** A UMP test has maximum power against ALL alternatives in H₁ simultaneously. For simple vs simple hypotheses (both specify exact parameter values), the LRT is UMP. For composite H₁ (e.g., H₁: θ>θ₀ with no specific value): UMP often exists for one-sided tests in exponential families (by monotone likelihood ratio property). For two-sided tests (H₁: θ≠θ₀): UMP generally does NOT exist — a test most powerful against θ>θ₀ is not most powerful against θ<θ₀. Solution: use UMPU (Uniformly Most Powerful Unbiased) tests, which restrict to unbiased tests (those where power≥α). For normal distribution tests, the t-test is UMPU. This theoretical framework justifies why commonly used tests (t-test, z-test) are actually optimal under their assumptions.

---

# 📌 QUICK REFERENCE — Hypothesis Testing Cheatsheet

```
SETTING UP THE TEST
H₀ always contains: =, ≤, ≥ (equality)
H₁ always contains: ≠, <, > (inequality)
α chosen BEFORE data collection (usually 0.05 or 0.01)
Two-tailed: H₁: ≠    |    One-tailed: H₁: > or <

TEST STATISTICS
z = (x̄-μ₀) / (σ/√n)              one-sample z-test
t = (x̄-μ₀) / (s/√n), df=n-1      one-sample t-test
t = d̄ / (s_d/√n), df=n-1          paired t-test
χ² = Σ(O-E)²/E                    chi-square
F = MS_between/MS_within            ANOVA

CRITICAL VALUES (two-tailed)
         90%        95%        99%
z:       1.645      1.960      2.576
t(∞):    1.645      1.960      2.576
t(30):   1.697      2.042      2.750
t(10):   1.812      2.228      3.169

DECISION RULE
p < α → reject H₀ (statistically significant)
p ≥ α → fail to reject H₀ (not significant)
"Fail to reject" ≠ "Accept H₀" — never say "accept"

EFFECT SIZES (report always!)
Cohen's d:    small=0.2  medium=0.5  large=0.8
Cohen's h:    small=0.2  medium=0.5  large=0.8
Pearson r:    small=0.1  medium=0.3  large=0.5
Cramér's V:   small=0.1  medium=0.3  large=0.5

POWER FORMULA
n ≈ (z_α/2 + z_β)² σ²/δ²
80% power: z_β = 0.84
90% power: z_β = 1.28

MULTIPLE TESTING
m tests at α=0.05: FWER = 1-(0.95)^m
FWER control: Bonferroni α/m (conservative)
FDR control: Benjamini-Hochberg (recommended for ML)
```

---

*Batch 4 Complete: 15 Questions | Basic (6) → Intermediate (6) → Advanced (4) + Quick Reference*
*Next: Batch 5 — Machine Learning Statistics*
