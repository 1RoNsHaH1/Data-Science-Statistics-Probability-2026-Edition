# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 10 — Modern 2026 Topics
# Chapters 10.1–10.5

---

# Chapter 10.1 — Statistics for Generative AI

> **Understanding the probabilistic foundations behind LLMs, diffusion models, and GenAI systems.**

---

## 💡 Why Statistics Underpins GenAI

Every output from GPT-4, Claude, Gemini, Midjourney, and Sora is fundamentally a **probability distribution**. Understanding GenAI requires understanding the statistics beneath it.

---

## 10.1.1 — Language Models as Probability Distributions

A language model defines a probability distribution over sequences of tokens:

$$P(w_1, w_2, \ldots, w_T) = \prod_{t=1}^{T} P(w_t \mid w_1, w_2, \ldots, w_{t-1})$$

**In plain English:** A language model assigns a probability to every possible sequence of words. It answers: "Given all previous words, how likely is each possible next word?"

```
Prompt: "The cricket match ended in a"

Next token probabilities:
"win"       → 0.35
"draw"      → 0.28
"loss"      → 0.22
"tie"       → 0.09
"victory"   → 0.04
...others   → 0.02

The model SAMPLES from this distribution to generate the next word!
```

---

## 10.1.2 — Softmax Function

The **softmax** converts raw model outputs (logits) into a valid probability distribution:

$$P(w_i) = \text{softmax}(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}$$

| Symbol | Meaning |
|--------|---------|
| zᵢ | Raw logit (score) for token i from the model |
| eˣ | Exponential — makes all values positive |
| Denominator | Normalizes so all probabilities sum to 1 |
| K | Vocabulary size (e.g., 100,000 tokens) |

**Properties:**
- All outputs ∈ (0, 1)
- Sum of all outputs = 1
- Preserves relative ordering of logits
- Amplifies differences (high logit → disproportionately high probability)

```
Logits:      z = [2.0,  0.5,  -1.0,  3.0]
Exponentials:   [7.39, 1.65,  0.37, 20.09]  (sum = 29.5)
Softmax:        [0.25, 0.056, 0.013, 0.681]  (sum ≈ 1.0)

Highest logit (3.0) gets probability 0.681
Smallest logit (-1.0) gets only 0.013
```

---

## 10.1.3 — Temperature Scaling

> **Temperature controls how "confident" or "creative" the model's sampling is.**

$$P_T(w_i) = \frac{e^{z_i / T}}{\sum_j e^{z_j / T}}$$

| Temperature | Effect | Use Case |
|-------------|--------|---------|
| T → 0 | **Greedy** — always pick highest probability | Factual Q&A, code generation |
| T = 1.0 | **Default** — sample from unmodified distribution | Balanced generation |
| T > 1.0 | **Flat** — more uniform, more random/creative | Creative writing, brainstorming |
| T >> 1 | **Nearly uniform** — incoherent output | (Avoid) |

```
T = 0.1 (sharp):          T = 1.0 (normal):         T = 2.0 (flat):
"win" → 0.95              "win" → 0.35               "win" → 0.28
"draw" → 0.04             "draw" → 0.28              "draw" → 0.25
"loss" → 0.01             "loss" → 0.22              "loss" → 0.22
                                                      
Almost always picks "win"  Natural sampling           Much more random
```

---

## 10.1.4 — Sampling Strategies

### Greedy Decoding
Always pick the highest-probability next token.
- Fast, deterministic
- Often produces repetitive, boring text

### Top-k Sampling
Sample only from the k most likely tokens (e.g., k=50).

```
If k=3:
Only sample from: "win" (0.35), "draw" (0.28), "loss" (0.22)
Renormalize: "win"=0.412, "draw"=0.329, "loss"=0.259
```

### Nucleus Sampling (Top-p)
Sample from the smallest set of tokens whose cumulative probability ≥ p.

```
If p=0.9:
"win" cumsum=0.35
"draw" cumsum=0.63
"loss" cumsum=0.85
"tie" cumsum=0.94 ← stop here (cumsum > 0.9)

Sample only from: {"win", "draw", "loss", "tie"}
```

Top-p adapts to the distribution shape — uses fewer tokens when distribution is sharp, more when flat. Superior to top-k in practice.

---

## 10.1.5 — Perplexity

> **The standard metric for evaluating language models — lower is better.**

$$\text{Perplexity} = 2^{H(P,Q)} = 2^{-\frac{1}{T}\sum_{t=1}^T \log_2 P(w_t \mid w_{<t})}$$

**Intuition:** If perplexity = 100, the model is "as confused as if it were choosing uniformly among 100 options at each step." Lower perplexity = better model.

```
Human text perplexity for GPT-2:       ~20-30  (quite good)
GPT-4 perplexity on benchmarks:        ~5-15   (excellent)
Random word model perplexity:          ~50,000 (terrible)
```

---

## 10.1.6 — Diffusion Models — Statistical Perspective

Diffusion models (Stable Diffusion, DALL·E 3, Sora) learn by:

1. **Forward process:** Gradually add Gaussian noise to training images until pure noise
2. **Reverse process:** Learn to denoise — predict the added noise at each step

$$q(x_t \mid x_{t-1}) = \mathcal{N}(x_t; \sqrt{1-\beta_t} x_{t-1}, \beta_t I)$$

The model learns $p_\theta(x_{t-1} \mid x_t)$ — the reverse denoising process.

**Generation:** Start from pure noise x_T ~ N(0, I), repeatedly apply denoising to get x₀ (the image).

---

# Chapter 10.2 — Causal Inference

> **Moving beyond correlation to actual cause-and-effect — the hardest problem in data science.**

---

## 💡 Correlation vs Causation — Why It Matters

```
Correlation: Two things happen together.
Causation:   One thing MAKES the other happen.

Correlation says: "Ice cream sales and drowning deaths both increase in summer."
Causation says:   "Hot weather causes people to swim more → more drownings,
                   AND hot weather causes people to eat more ice cream."
                   Ice cream does NOT cause drowning.

Business example:
Correlation: Users who see our ad are 3× more likely to buy.
Causation:   The ad CAUSED the purchase, OR users who were already 
              going to buy happened to see the ad (selection bias).
```

**In data science: correlation without causation leads to wrong interventions.**

---

## 10.2.1 — The Potential Outcomes Framework (Rubin Causal Model)

For each unit i, define:
- Y(1): Outcome if treated (T=1)
- Y(0): Outcome if not treated (T=0)

**Individual Treatment Effect:** τᵢ = Y_i(1) - Y_i(0)

**The Fundamental Problem of Causal Inference:**
We can only observe ONE of Y(1) or Y(0) for each person — never both.

```
Person   Treated?   Outcome_if_treated   Outcome_if_not_treated   Effect
Alice    Yes (T=1)  Recovery (observed)  ? (counterfactual)        ?
Bob      No  (T=0)  ? (counterfactual)   No recovery (observed)    ?
```

The "?" values are **counterfactuals** — what WOULD have happened.

---

## 10.2.2 — Average Treatment Effect (ATE)

$$\text{ATE} = E[Y(1) - Y(0)] = E[Y(1)] - E[Y(0)]$$

**Naïve estimate (wrong):**
ATE ≈ E[Y|T=1] - E[Y|T=0]
This is only correct if T was randomly assigned!

---

## 10.2.3 — Confounders

> **A confounder is a variable that affects BOTH the treatment assignment AND the outcome.**

```
CONFOUNDED:
             Confounder (Age)
            ↙               ↘
    Exercise              Health
    (Treatment)           (Outcome)

Older people exercise less AND are less healthy.
Naïve comparison: "exercising people are healthier" 
Reality: Younger people happen to both exercise more AND be healthier.

If we just compare exercisers vs non-exercisers,
we attribute the age effect to exercise!
```

**Solution:** Control for confounders by stratifying, matching, or using regression.

---

## 10.2.4 — Directed Acyclic Graphs (DAGs)

A **DAG** is a visual tool for representing causal assumptions.

```
Nodes = Variables
Arrows = Causal effects
No cycles allowed (A→B→A would be a cycle)

Example: Ad Exposure → Purchase
         Age → Ad Exposure (older users shown different ads)
         Age → Purchase (older users buy more)

DAG:
Age ──→ Ad Exposure ──→ Purchase
 └────────────────────→ Purchase

"Age" is a confounder of the Ad→Purchase relationship.
We must adjust for Age to estimate the causal effect of the Ad.
```

---

## 10.2.5 — Key Causal Methods

| Method | When to Use | Key Idea |
|--------|-------------|---------|
| **Randomized Controlled Trial (RCT)** | Gold standard | Random assignment eliminates confounding |
| **Propensity Score Matching** | Observational data | Match treated/control on P(T=1\|X) |
| **Difference-in-Differences** | Before/after with control group | DiD = (Post_treated - Pre_treated) - (Post_control - Pre_control) |
| **Instrumental Variables** | Unobserved confounders | Use an instrument Z that affects T but not Y directly |
| **Regression Discontinuity** | Threshold-based treatment | Compare just above and below threshold |
| **Synthetic Control** | Few units, long time series | Construct weighted control from donor units |

---

## 10.2.6 — Data Science Applications

| Business Question | Causal Method |
|------------------|---------------|
| "Did our ad CAUSE purchases?" | Propensity Score, RCT |
| "Did the policy CAUSE lower crime?" | DiD or Synthetic Control |
| "Does education CAUSE higher earnings?" | Instrumental Variables (use distance to college as IV) |
| "Does our new feature CAUSE higher engagement?" | A/B Test (RCT) |
| "Did COVID CAUSE revenue drop?" | Synthetic Control, DiD |

---

# Chapter 10.3 — Experimentation in AI Systems

> **Running rigorous experiments at scale — online A/B testing, sequential methods, bandit algorithms.**

---

## 10.3.1 — Online Experimentation at Scale

Tech companies run thousands of experiments simultaneously:
- Google: 10,000+ experiments per year
- Netflix: Continuous experiments on all features
- Amazon: Dozens of simultaneous experiments on product pages

**Key challenges not present in offline experiments:**
1. **Interference:** User A's treatment affects User B's outcome (social networks)
2. **Novelty effects:** Short-term spikes due to curiosity, not real improvement
3. **Long-term effects:** Initial lift may decay over time
4. **Multiple metrics:** Optimizing CTR may hurt time-on-site
5. **Multiple testing:** Thousands of simultaneous tests → many false positives

---

## 10.3.2 — Sequential Testing (Always Valid Inference)

**Problem with fixed-horizon testing:** You must decide sample size in advance. Can't "peek" at results.

**Sequential testing** allows valid inference at any time:

**Sequential Probability Ratio Test (SPRT):**

$$\Lambda_t = \frac{P(\text{data} | H_1)}{P(\text{data} | H_0)}$$

- If Λₜ ≥ B: Reject H₀ (significant improvement)
- If Λₜ ≤ A: Accept H₀ (no improvement)
- If A < Λₜ < B: Continue collecting data

**Always Valid p-values:** E-processes and e-values provide valid inference regardless of when you stop — used by modern experimentation platforms.

---

## 10.3.3 — Multi-Armed Bandit Algorithms

**Problem:** Traditional A/B test wastes traffic sending 50% to the worse variant.
**Bandit solution:** Dynamically allocate more traffic to better-performing variants.

### Epsilon-Greedy

```python
if random() < epsilon:
    action = random_choice(arms)    # EXPLORE
else:
    action = argmax(estimated_rewards)  # EXPLOIT
```

### Thompson Sampling (Bayesian Bandit)

For each arm k:
1. Maintain Beta(α_k, β_k) posterior on success rate pₖ
2. Sample p̃_k from Beta(α_k, β_k) for each arm
3. Choose arm with highest p̃_k
4. Observe outcome, update Beta

```python
import numpy as np

class ThompsonSampling:
    def __init__(self, n_arms):
        self.alpha = np.ones(n_arms)  # successes + 1
        self.beta = np.ones(n_arms)   # failures + 1
    
    def choose_arm(self):
        samples = np.random.beta(self.alpha, self.beta)
        return np.argmax(samples)
    
    def update(self, arm, reward):
        self.alpha[arm] += reward
        self.beta[arm] += (1 - reward)
```

### UCB (Upper Confidence Bound)

$$\text{UCB}_k = \hat{\mu}_k + c\sqrt{\frac{\ln t}{N_k}}$$

Choose arm with highest UCB — balances exploitation (high μ̂) with exploration (high uncertainty).

---

## 10.3.4 — Causal ML Experimentation

**Heterogeneous Treatment Effects (HTE):**
Not every user responds equally to a treatment. Find WHICH users benefit most.

**CATE = Conditional Average Treatment Effect:**
$$\tau(x) = E[Y(1) - Y(0) | X = x]$$

Methods: Causal Forests, X-Learner, Meta-Learners (T, S, X, R learners).

```
Traditional A/B: Did the treatment work on AVERAGE?
HTE: For WHOM did it work? How much?

Example: Same discount offer might:
  → Convert casual shoppers at 15% lift
  → Have zero effect on heavy shoppers (would buy anyway)
  → Cannibalize premium customers (take discount instead of paying full)
```

---

# Chapter 10.4 — Responsible AI Statistics

> **Statistical methods for detecting and measuring bias, fairness, and distribution drift.**

---

## 10.4.1 — Bias Detection

**Statistical Bias in Models:**

Types of bias:
- **Historical bias:** Training data reflects past discrimination
- **Representation bias:** Some groups under-represented in training data
- **Measurement bias:** Same label means different things across groups
- **Aggregation bias:** One model for diverse groups hides subgroup differences

---

## 10.4.2 — Fairness Metrics

Multiple (often conflicting) mathematical definitions of fairness:

| Fairness Criterion | Definition | Formula |
|-------------------|------------|---------|
| **Demographic Parity** | Equal positive prediction rates across groups | P(Ŷ=1\|A=0) = P(Ŷ=1\|A=1) |
| **Equal Opportunity** | Equal TPR across groups | P(Ŷ=1\|Y=1,A=0) = P(Ŷ=1\|Y=1,A=1) |
| **Equalized Odds** | Equal TPR AND FPR across groups | Above + P(Ŷ=1\|Y=0,A=0) = P(Ŷ=1\|Y=0,A=1) |
| **Calibration** | Predicted probabilities accurate within each group | P(Y=1\|Ŷ=p,A=a) = p for all a |
| **Individual Fairness** | Similar individuals get similar predictions | d(x,x')≈0 → d(f(x),f(x'))≈0 |

**Impossibility result (Kleinberg-Mullainathan):** When base rates differ between groups, you CANNOT simultaneously achieve calibration, equal TPR, and equal FPR. Choose which to optimize based on context.

---

## 10.4.3 — Disparate Impact Analysis

$$\text{Disparate Impact Ratio} = \frac{P(\hat{Y}=1 | A=\text{minority})}{P(\hat{Y}=1 | A=\text{majority})}$$

**80% rule (EEOC guideline):** A model has disparate impact if the ratio < 0.8.

```python
from aif360.metrics import BinaryLabelDatasetMetric

# Compute disparate impact
metric = BinaryLabelDatasetMetric(dataset, 
                                   privileged_groups=[{'race': 1}],
                                   unprivileged_groups=[{'race': 0}])
print(f"Disparate Impact: {metric.disparate_impact():.3f}")
# < 0.8 → problematic
```

---

## 10.4.4 — Distribution Shift (Data Drift)

> **When the statistical properties of production data differ from training data, model performance degrades.**

Types of shift:

| Type | Definition | Example |
|------|------------|---------|
| **Covariate shift** | P(X) changes, P(Y\|X) stable | User demographics change |
| **Label shift** | P(Y) changes, P(X\|Y) stable | Fraud rate changes with economy |
| **Concept drift** | P(Y\|X) changes | User behavior changes (COVID) |
| **Dataset shift** | General term for above | Any distributional change |

**Detection methods:**
- **KS test (Kolmogorov-Smirnov):** Compare distributions of each feature
- **Population Stability Index (PSI):** Widely used in finance
- **MMD (Maximum Mean Discrepancy):** Distance between distributions in kernel space
- **Adversarial validation:** Train classifier to distinguish train vs prod → AUC measures drift

**PSI:**
$$\text{PSI} = \sum_{i} (\text{Actual}_i - \text{Expected}_i) \times \ln\left(\frac{\text{Actual}_i}{\text{Expected}_i}\right)$$

| PSI | Interpretation |
|-----|---------------|
| < 0.1 | No significant shift |
| 0.1 – 0.2 | Moderate shift — monitor |
| > 0.2 | Significant shift — retrain |

---

## 10.4.5 — Model Monitoring Statistics

```
STATISTICAL PROCESS CONTROL for ML:

Performance metric (e.g., AUC) over time:

AUC
0.90 │─────────────────────
0.85 │                  ↘ drift detected
0.80 │                     ↘
0.75 │─────────────────────── control limits
0.70 │
     └──────────────────────────── Time
     
Alert when metric falls below control limit (μ - 3σ of historical performance)
```

---

# Chapter 10.5 — Data-Centric AI

> **The shift from model-centric to data-centric AI — where improving data quality beats improving models.**

---

## 💡 The Data-Centric AI Insight

**Model-centric AI:** Fix the data, improve the model architecture.
**Data-centric AI:** Fix the model, systematically improve the data quality.

Andrew Ng's observation: For most practical problems, improving data quality gives bigger gains than trying new architectures.

---

## 10.5.1 — Data Quality Metrics

| Dimension | Metric | How to Measure |
|-----------|--------|---------------|
| **Completeness** | % non-missing | df.isnull().sum() / len(df) |
| **Accuracy** | % correct labels | Human expert audit sample |
| **Consistency** | % without conflicts | Cross-field validation rules |
| **Timeliness** | Data freshness | Max(timestamp) - now() |
| **Uniqueness** | % non-duplicate | 1 - df.duplicated().sum()/len(df) |
| **Validity** | % within expected range | % outside domain constraints |

---

## 10.5.2 — Label Noise

**Types of label noise:**

```
SYMMETRIC NOISE:                ASYMMETRIC NOISE:
Labels randomly flipped          Specific classes mislabeled
True: {A, B, C}                 True A → often labeled B (class-dependent)
Flipped: {A→B, B→C, C→A}       Example: "Normal" X-ray mislabeled as "Pneumonia"
                                 (radiologist error pattern is not random)
```

**Statistical impact of label noise:**
- Symmetric noise rate ρ: Optimal classifier accuracy ≈ 1 - 2ρ(1-ρ)
- Asymmetric noise: depends on which classes are confused

**Detecting label noise:**
- Loss distribution: high-loss samples on clean data often mislabeled
- Consensus labeling: multiple annotators, flag disagreements
- Confident Learning (Northcutt et al.): estimate noise transition matrix

```python
from cleanlab.classification import CleanLearning
from sklearn.linear_model import LogisticRegression

cl = CleanLearning(clf=LogisticRegression())
cl.fit(X_train, y_train_noisy)
label_issues = cl.find_label_issues(X_train, y_train_noisy)
# label_issues gives probability each sample is mislabeled
```

---

## 10.5.3 — Data Validation

Statistical tests for data validation in production ML pipelines:

```python
# Great Expectations — data validation framework
import great_expectations as ge

df_ge = ge.from_pandas(df)

# Statistical validation expectations
df_ge.expect_column_mean_to_be_between('age', 25, 45)
df_ge.expect_column_stdev_to_be_between('age', 5, 20)
df_ge.expect_column_values_to_be_between('price', 0, 100000)
df_ge.expect_column_value_z_scores_to_be_less_than('income', 4)
df_ge.expect_column_kl_divergence_to_be_less_than('category', 
                                                    reference_dist, 0.1)

# Run validation
results = df_ge.validate()
```

---

## 10.5.4 — Curriculum Learning

**Statistical motivation:** Train on easy examples first, then harder ones — mimics human learning.

```
Difficulty score (e.g., prediction variance across models):
Easy samples:   Model agrees confidently → low loss variance
Hard samples:   Model disagrees → high loss variance

Training schedule:
Epoch 1-10:    Easy samples only
Epoch 10-20:   Medium difficulty
Epoch 20+:     All samples including hard ones

Result: Better generalization, especially when hard samples are noisy
```

---

## 10.5.5 — Statistical Tests in Data Pipelines

| Test | Purpose | When to Use |
|------|---------|-------------|
| KS test | Check if feature distribution shifted | Daily feature validation |
| Chi-square | Check categorical distribution | Category shift detection |
| Levene's test | Check variance stability | Variance drift detection |
| Jensen-Shannon divergence | Symmetric distribution distance | General distribution monitoring |
| Wasserstein distance | Earth mover's distance | Robust distribution comparison |

```python
from scipy import stats

# KS test for distribution shift
ks_stat, p_value = stats.ks_2samp(train_feature, prod_feature)
if p_value < 0.05:
    print(f"Significant distribution shift detected! KS={ks_stat:.3f}")

# Jensen-Shannon divergence
from scipy.spatial.distance import jensenshannon
js_div = jensenshannon(train_hist, prod_hist)
```

---

# 📝 Practice Questions — Part 10

## 🟢 Beginner (8 Questions)

**Q1.** What is the softmax function and what does it output?
**Q2.** Temperature = 0.1 vs Temperature = 2.0. Which produces more creative text?
**Q3.** What is a confounder? Give a simple example.
**Q4.** What is the "fundamental problem of causal inference"?
**Q5.** Thompson Sampling: You have 2 arms. Arm A: Beta(10,5), Arm B: Beta(8,8). Which arm would you likely choose? Why?
**Q6.** What is PSI and what value indicates significant data drift?
**Q7.** What is label noise? Give an example in image classification.
**Q8.** What is "demographic parity" in fair ML?

## 🟡 Intermediate (8 Questions)

**Q9.** Compute softmax for logits [1.0, 2.0, 0.5]. What is the probability for each class?
**Q10.** Top-p (nucleus) sampling with p=0.9 for tokens: A=0.50, B=0.25, C=0.15, D=0.07, E=0.03. Which tokens are included in the nucleus?
**Q11.** A hiring algorithm has TPR=0.80 for males and TPR=0.60 for females (for identifying qualified candidates). Which fairness criterion is violated?
**Q12.** Difference-in-differences: Pre-treatment: Group A=100, Group B=80. Post-treatment: Group A=120, Group B=95. What is the DiD estimate of treatment effect?
**Q13.** PSI calculation: Expected proportions [0.3, 0.5, 0.2], Actual [0.25, 0.55, 0.20]. Compute PSI.
**Q14.** Perplexity of a language model is 50. What does this mean intuitively?
**Q15.** In a bandit problem, Arm A has been pulled 100 times with 30 successes. Arm B pulled 10 times with 4 successes. UCB formula: choose arm with max (μ̂ + √(ln(t)/2N)). Which arm does UCB choose at t=110?
**Q16.** Explain how propensity score matching works to estimate causal effects from observational data.

## 🔴 Advanced (4 Questions)

**Q17.** Prove that you cannot simultaneously achieve equalized odds AND calibration when base rates differ between groups.
**Q18.** Derive the ELBO (Evidence Lower Bound) for variational inference — how does it relate to KL divergence?
**Q19.** Doubly robust estimator: Show that if either the propensity model OR the outcome model is correctly specified, the ATE estimate is consistent.
**Q20.** Explain why Wasserstein distance (Earth Mover's Distance) is preferred over KL divergence for comparing distributions in data drift detection.

## 🚀 Data Science Scenarios (4 Questions)

**DS1.** You deploy a loan approval model. After 6 months, you discover the approval rate for minority applicants is 45% vs 72% for majority applicants. Disparate Impact Ratio = 45/72 = 0.625 < 0.8. Describe: (a) statistical tests to run, (b) model adjustments to consider, (c) business and legal implications.

**DS2.** You're building a GenAI application. Users complain responses are too repetitive (low temperature) or incoherent (high temperature). Design a temperature scheduling strategy: how would you choose temperature dynamically based on the task type and conversation context?

**DS3.** Data drift monitoring: Your e-commerce recommendation model was trained on pre-2024 data. In 2026, you notice PSI > 0.25 on "price_sensitivity" feature. Describe your full investigation and response protocol.

**DS4.** Label noise in medical imaging: 15% of your chest X-ray labels are suspected to be noisy (radiologist disagreement). Describe a statistical strategy to (a) identify likely mislabeled samples, (b) clean the dataset, (c) train a robust model despite remaining noise.

---

# ✅ Key Solutions

**A1.** Softmax converts K raw logit scores into a probability distribution: P(i) = exp(zᵢ)/Σexp(zⱼ). All outputs ∈ (0,1), sum to 1.

**A2.** Temperature = 2.0 produces more creative (random) text. Flattens the distribution, giving more chance to less likely tokens.

**A5.** Arm A: Beta(10,5) has mean 10/15=0.667. Arm B: Beta(8,8) has mean 8/16=0.5. Arm A has higher expected reward AND its distribution is tighter (more confident). Thomson Sampling would likely sample Arm A more often, but Arm B still gets explored.

**A9.** e¹=2.718, e²=7.389, e^0.5=1.649. Sum=11.756. P(class 1)=2.718/11.756=0.231. P(class 2)=7.389/11.756=0.629. P(class 3)=1.649/11.756=0.140.

**A10.** Cumulative: A=0.50, A+B=0.75, A+B+C=0.90 (stop). Nucleus = {A, B, C}.

**A11.** Equal opportunity (equal TPR across groups) is violated — TPR differs by gender.

**A12.** DiD = (120-100) - (95-80) = 20 - 15 = **5 units** causal effect of treatment.

**A13.** PSI = (0.25-0.3)×ln(0.25/0.3) + (0.55-0.5)×ln(0.55/0.5) + (0.20-0.20)×ln(1) = (-0.05)×(-0.182) + (0.05)×(0.095) + 0 = 0.0091+0.00475 ≈ **0.014** (no significant shift).

**A15.** t=110. Arm A: μ̂=0.30, N=100. UCB_A = 0.30+√(ln110/200) = 0.30+√(0.0235) = 0.30+0.153 = 0.453. Arm B: μ̂=0.40, N=10. UCB_B = 0.40+√(ln110/20) = 0.40+√(0.235) = 0.40+0.485 = 0.885. **Choose Arm B** (higher UCB despite fewer pulls — exploration bonus).

---

# 💼 Interview Questions — Part 10

**IQ1.** "Explain how sampling temperature works in LLMs."
Temperature T scales logits before softmax: z/T. Low T (near 0) makes distribution sharp — model almost always picks top token (deterministic/greedy). High T flattens distribution — more randomness, more creative but potentially incoherent. T=1 is unmodified. In practice: use low T for factual tasks (0.1-0.3), higher T for creative tasks (0.7-1.2).

**IQ2.** "What is distribution shift and how do you detect it?"
Distribution shift = statistical properties of production data differ from training data. Types: covariate shift (X distribution changes), concept drift (P(Y|X) changes), label shift (Y distribution changes). Detection: PSI for tabular features, KS test for continuous distributions, chi-square for categorical, adversarial validation (train classifier to separate train vs prod data — AUC > 0.7 indicates shift).

**IQ3.** "Why is A/B testing considered the gold standard for causal inference?"
Random assignment to treatment/control eliminates confounding by design — at scale, all confounders (observed and unobserved) are balanced between groups. This is the only method that doesn't require assuming you've measured all confounders. Observational methods always require untestable assumptions about what you've controlled for.

---

# 📌 Part 10 Summary

## ⚡ Quick Revision

- **Softmax:** converts logits to probabilities. Temperature controls sharpness.
- **Sampling:** Greedy (deterministic), Top-k, Nucleus (top-p), Temperature
- **Perplexity:** lower = better LM. Measures average branching factor.
- **Causal inference:** correlation ≠ causation. Need RCT or causal methods.
- **Confounders:** affect both treatment and outcome. Must control for them.
- **ATE:** Expected Y(1) - Y(0). Estimated by RCT or propensity scoring.
- **Bandit algorithms:** Thompson Sampling (Bayesian), UCB, ε-greedy
- **Fairness metrics:** Demographic parity, Equal Opportunity, Equalized Odds — often conflicting
- **Data drift:** PSI, KS test, adversarial validation. Retrain when PSI > 0.2
- **Data-centric AI:** data quality often more impactful than model selection

---
*Part 10 Complete | Next: Part 11 — Practical Data Science Statistics*
