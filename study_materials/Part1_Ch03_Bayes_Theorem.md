# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 1 — Probability Foundations
# Chapter 1.3 — Bayes' Theorem

> **Chapter Goal:** Master the most important theorem in Data Science — the mathematical formula for updating beliefs with new evidence. Used in spam filters, medical tests, ML classifiers, and the foundations of AI.

---

# 1.3.1 — The Story Before the Formula

## 💡 Intuition — Updating Beliefs

> **Bayes' Theorem is a formula for updating what you believe when you get new information.**

**The Detective Analogy:**

Imagine you're Sherlock Holmes investigating a crime.

Before seeing any evidence:
- "Could this suspect be guilty?" → You have some initial belief (called the **prior**)

After finding evidence (fingerprints on the weapon):
- Your belief updates → You now have a stronger belief (called the **posterior**)

After more evidence (the suspect has no alibi):
- Your belief updates again → even stronger posterior

**Bayes' Theorem is the mathematical rule for how to update beliefs rationally.**

```
Prior Belief → [New Evidence] → Updated Posterior Belief

"What I believed before" → "What happened" → "What I believe now"
```

---

## 🌍 Three Classic Real-World Examples

### Example 1 — Medical Testing

You test positive for a disease. What is the actual probability you have it?

Before the test: Prior = disease prevalence in population (e.g., 1%)
Evidence: Positive test result
After the test: Posterior = P(disease | positive test)

### Example 2 — Spam Filtering

An email arrives with the word "WINNER!" What is P(spam | contains "WINNER!")?

Before seeing the word: Prior = overall spam rate (e.g., 40%)
Evidence: The word "WINNER!" appears
After: Updated probability this email is spam

### Example 3 — Bayesian ML

Before seeing data: Prior = our initial guess about model parameters
After seeing training data: Posterior = updated belief about model parameters

---

# 1.3.2 — The Formula

## 📖 Bayes' Theorem

$$P(A \mid B) = \frac{P(B \mid A) \times P(A)}{P(B)}$$

### Symbol-by-Symbol Breakdown

| Symbol | Name | Meaning |
|--------|------|---------|
| P(A\|B) | **Posterior** | What we want: probability of A **after** seeing B |
| P(B\|A) | **Likelihood** | How probable is evidence B if A is true? |
| P(A) | **Prior** | What we believed about A **before** seeing B |
| P(B) | **Evidence / Marginal** | Total probability of seeing B (normalizing factor) |

### The Full Expanded Form

Using the Law of Total Probability to expand P(B):

$$P(A \mid B) = \frac{P(B \mid A) \times P(A)}{P(B \mid A) \times P(A) + P(B \mid A^c) \times P(A^c)}$$

### The Core Relationship

$$\text{Posterior} \propto \text{Likelihood} \times \text{Prior}$$

The ∝ means "proportional to" — the posterior is proportional to the product of likelihood and prior (before normalizing to sum to 1).

---

## 🎨 Visual: The Bayes Table (2×2 Table Approach)

This is the most intuitive way to apply Bayes for beginners. Instead of formulas, build a table.

**Setup — Disease Test Example:**

| Known facts | Values |
|-------------|--------|
| Population | 100,000 people |
| Disease prevalence | 1% (1,000 have it) |
| Test sensitivity (True positive rate) | 99% |
| Test specificity (True negative rate) | 95% |

**Build the table:**

```
                   HAS DISEASE    NO DISEASE    TOTAL
Test Positive  |      990      |    4,950    |  5,940
Test Negative  |       10      |   94,050    | 94,060
TOTAL          |    1,000      |   99,000    | 100,000
```

**Calculation:**
```
Of 1,000 sick: 99% test positive → 990 true positives
Of 99,000 healthy: 5% test positive (false positive) → 4,950 false positives

P(disease | positive test) = 990 / (990 + 4950) = 990 / 5940 ≈ 0.1667 = 16.7%
```

> 🔥 **Shocking Insight:** Even with a 99% accurate test, there's only a **16.7%** chance you have the disease if you test positive (because the disease is rare). This is the **base rate fallacy** — and Bayes corrects for it.

---

# 1.3.3 — Medical Testing Deep Dive

## 📊 Step-by-Step with Full Formula

**Problem:** A rare disease affects 0.5% of the population.
A test has 90% sensitivity (P(+|sick) = 0.90) and 95% specificity (P(-|healthy) = 0.95).
You test positive. What is P(you have disease)?

**Define:**
- A = "You have the disease"
- B = "Test is positive"
- P(A) = 0.005 (prior — 0.5% prevalence)
- P(Aᶜ) = 0.995
- P(B|A) = 0.90 (sensitivity = likelihood of + if sick)
- P(B|Aᶜ) = 0.05 (1 - specificity = false positive rate)

**Step 1 — Calculate P(B):**
```
P(B) = P(B|A)×P(A) + P(B|Aᶜ)×P(Aᶜ)
     = 0.90×0.005 + 0.05×0.995
     = 0.0045 + 0.04975
     = 0.05425
```

**Step 2 — Apply Bayes:**
```
P(A|B) = [P(B|A) × P(A)] / P(B)
        = [0.90 × 0.005] / 0.05425
        = 0.0045 / 0.05425
        ≈ 0.083 = 8.3%
```

**Interpretation:** Despite testing positive, there is only an 8.3% chance you actually have the disease. Most of the positives are false alarms because the disease is rare.

## 📊 What Happens When We Test Again?

If you test positive a **second time**, now your prior updates:
- New prior P(A) = 0.083 (posterior from first test)
- P(B|A) = 0.90, P(B|Aᶜ) = 0.05 (same test)

```
P(B) = 0.90×0.083 + 0.05×0.917 = 0.0747 + 0.04585 = 0.12055
P(A|B) = 0.90×0.083 / 0.12055 = 0.0747/0.12055 ≈ 0.620 = 62%
```

Two positive tests → 62% probability. Three tests → even higher. This is **iterative Bayesian updating**.

---

# 1.3.4 — Spam Filter Example

## 📊 Bayesian Spam Classification

**Setup:**
- P(spam) = 0.40 (40% of all emails are spam) — **prior**
- P("FREE" | spam) = 0.70 (70% of spam has the word "FREE")
- P("FREE" | not spam) = 0.05 (5% of legit emails have "FREE")

**Question:** Email contains "FREE". Is it spam?

```
P(B) = P("FREE") = P("FREE"|spam)×P(spam) + P("FREE"|not spam)×P(not spam)
     = 0.70×0.40 + 0.05×0.60
     = 0.28 + 0.03 = 0.31

P(spam | "FREE") = [0.70 × 0.40] / 0.31
                 = 0.28 / 0.31
                 ≈ 0.903 = 90.3%
```

**Adding more evidence (also contains "WINNER"):**
P("WINNER"|spam) = 0.65, P("WINNER"|not spam) = 0.01

```
New prior: P(spam) = 0.903 (from previous update)
P("WINNER"|spam)×0.903   = 0.65×0.903 = 0.587
P("WINNER"|not spam)×0.097 = 0.01×0.097 = 0.00097

P(spam|FREE AND WINNER) = 0.587 / (0.587+0.00097) ≈ 0.998 = 99.8%
```

> 🚀 This is literally how Gmail and other spam filters work — they maintain probabilities for thousands of words and update using Bayes.

---

# 1.3.5 — Prior, Likelihood, Posterior — Deep Understanding

## 🎨 The Three Components Visualized

```
BEFORE EVIDENCE:
Your Belief
│         ← Weak, spread out
│  Prior  (vague — could be anything)
│ ▓▓▓▓▓▓▓▓▓▓
└─────────────────────── Parameter value

EVIDENCE:
New Data Points
│         ← Evidence points to a specific region
│ Likelihood
│     ▓▓▓▓▓▓▓
└─────────────────────── Parameter value

AFTER EVIDENCE:
Updated Belief
│         ← Sharper, more focused
│ Posterior (Prior × Likelihood, normalized)
│   ▓▓▓▓▓▓▓▓▓▓
└─────────────────────── Parameter value

More data → sharper posterior → more certainty
```

## 🌍 Intuitive Mappings

| Concept | Medical Test | Spam Filter | Cricket Player |
|---------|-------------|-------------|----------------|
| **Prior** | Disease prevalence | Overall spam rate | Career batting average |
| **Likelihood** | Test accuracy for sick | Word frequency in spam | Strike rate in similar conditions |
| **Evidence** | Your test result | Words in this email | Today's conditions |
| **Posterior** | P(disease\|test) | P(spam\|this email) | P(century\|conditions) |

---

# 1.3.6 — Bayesian Thinking in AI

## 🚀 Data Science & ML Applications

### 1. Naïve Bayes Classifier
Uses Bayes' Theorem with independence assumption to classify text:
```
P(class | features) ∝ P(class) × ∏ P(featureᵢ | class)
```
Fast, interpretable, great for text classification.

### 2. Bayesian Linear Regression
Instead of finding one "best" line, finds a **distribution** over lines:
- Prior: Our initial belief about the coefficients
- Likelihood: How well each line fits the data
- Posterior: Updated distribution of coefficients after seeing data

Output: Not just "β = 2.3" but "β is probably between 2.1 and 2.5 with 95% confidence"

### 3. Bayesian Optimization (Hyperparameter Tuning)
Uses a probabilistic model to decide which hyperparameter combination to try next, balancing exploration and exploitation.

### 4. Hidden Markov Models
P(next state | current state) — used in speech recognition, NLP.

### 5. Probabilistic Programming (PyMC, Stan)
Define statistical models as probability distributions. Compute posteriors using MCMC sampling.

---

# 📝 Practice Questions — Chapter 1.3

## 🟢 Beginner (10 Questions)

**Q1.** Write Bayes' Theorem and name each component.

**Q2.** P(disease) = 0.02. Test: P(+|disease) = 0.95, P(+|healthy) = 0.10. Find P(disease|+).

**Q3.** What is a prior probability? Give a real-life example.

**Q4.** What happens to the posterior as we collect more data?

**Q5.** P(spam) = 0.35. P("offer"|spam) = 0.60. P("offer"|not spam) = 0.05. Find P(spam|"offer").

**Q6.** If P(A|B) = P(A), what can you say about using Bayes' Theorem here?

**Q7.** Why does having a very rare disease (low prior) pull the posterior down even with a good test?

**Q8.** Label each: prior, likelihood, posterior: "Before testing: 2% disease rate. Test says positive. After testing: 15% probability."

**Q9.** What is "sensitivity" in a medical test? What is "specificity"?

**Q10.** A coin is either fair (P=0.5) or biased (P=0.7 for heads). You flip 3 heads in a row. Update your belief about which coin it is using Bayesian logic (qualitative answer).

## 🟡 Intermediate (10 Questions)

**Q11.** P(A) = 0.3, P(B|A) = 0.8, P(B|Aᶜ) = 0.2. Find P(A|B) and P(Aᶜ|B).

**Q12.** A factory: 60% Machine A (1% defective), 40% Machine B (4% defective). Given item is defective, P(from A)?

**Q13.** Weather model: P(rain) = 0.3. P(dark clouds|rain) = 0.9. P(dark clouds|no rain) = 0.4. You see dark clouds. Find P(rain|dark clouds).

**Q14.** In a Naïve Bayes spam filter, why do we multiply the individual likelihoods? What assumption does this use?

**Q15.** A COVID test has 85% sensitivity and 99% specificity. Population prevalence = 0.1%. Someone tests positive. Find P(COVID|positive).

**Q16.** Show that Bayes' Theorem is symmetric: P(A|B)/P(B|A) = P(A)/P(B).

**Q17.** Posterior becomes prior: You estimated P(spam)=0.9 from prior analysis. A new email arrives with word "meeting." P("meeting"|spam)=0.05, P("meeting"|ham)=0.40. Update P(spam|"meeting").

**Q18.** Three urns: Urn 1 (5R, 5B), Urn 2 (8R, 2B), Urn 3 (2R, 8B). One urn selected randomly, one ball drawn — it's red. Find P(Urn 2 | red).

**Q19.** What is MAP (Maximum a Posteriori) estimation? How does it differ from MLE?

**Q20.** Explain in plain English why Bayes' Theorem is described as "turning the likelihood around."

## 🔴 Advanced (10 Questions)

**Q21.** Derive Bayes' Theorem from the definition of conditional probability.

**Q22.** Bayesian updating: Start with P(fair coin) = 0.5. Flip 10 heads in a row. Calculate the posterior P(fair|10 heads). Assume P(10H|fair) = (1/2)^10, P(10H|biased with P=0.8) = 0.8^10.

**Q23.** Jeffrey's prior: Explain why using a uniform prior (all values equally likely) is not always uninformative.

**Q24.** Beta distribution is the conjugate prior for Bernoulli likelihood. If prior is Beta(α=2, β=2) and we observe 7 successes in 10 trials, what is the posterior?

**Q25.** Explain the difference between frequentist and Bayesian interpretations of probability.

**Q26.** Likelihood ratio: L = P(evidence|hypothesis) / P(evidence|not hypothesis). If L > 1, what does this mean for the posterior vs prior?

**Q27.** Sequential Bayes: You run an A/B test and check after each 100 users. Explain how Bayesian updating differs from frequentist hypothesis testing in this scenario.

**Q28.** KL divergence D(P||Q) measures how different posterior P is from prior Q. When the data is very informative, what happens to this KL divergence?

**Q29.** Empirical Bayes: Explain how the prior can be estimated from the data itself (hierarchical Bayes).

**Q30.** MCMC intuition: Why can't we always compute the posterior analytically, and what does MCMC approximate?

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** You're building a fraud detection model. P(fraud) = 0.001. Your model flags a transaction. P(flag|fraud) = 0.95. P(flag|legitimate) = 0.02. What is P(fraud|flagged)? What does this mean for precision?

**DS2.** Naïve Bayes text classifier: Given classes C1="Sports" and C2="Politics" with equal priors. Doc contains words [goal, match, score]. P(goal|sports)=0.15, P(match|sports)=0.12, P(score|sports)=0.10, P(goal|politics)=0.01, P(match|politics)=0.03, P(score|politics)=0.04. Classify this document.

**DS3.** In Bayesian A/B testing, you start with Beta(1,1) prior (uniform). After observing 50 conversions from 200 visitors on variant A, what is the posterior? What is the posterior mean?

**DS4.** Concept drift: Your model was trained when P(churn|high usage) = 0.05. Now new data suggests this probability has changed. Explain how Bayesian updating with the new data helps adapt the model.

**DS5.** Calibration: A model outputs P(positive) = 0.90 for 100 samples. Only 60 of them are actually positive. Is the model calibrated? How does Bayesian thinking connect to model calibration?

---

# ✅ Key Solutions

**A2.** P(B) = 0.95×0.02 + 0.10×0.98 = 0.019+0.098 = 0.117. P(disease|+) = 0.019/0.117 ≈ 0.162 = 16.2%.

**A5.** P(B) = 0.60×0.35 + 0.05×0.65 = 0.21+0.0325 = 0.2425. P(spam|"offer") = 0.21/0.2425 ≈ 0.866 = 86.6%.

**A11.** P(B) = 0.8×0.3 + 0.2×0.7 = 0.24+0.14 = 0.38. P(A|B) = 0.24/0.38 ≈ 0.632. P(Aᶜ|B) = 1-0.632 = 0.368.

**A12.** P(D) = 0.60×0.01+0.40×0.04=0.006+0.016=0.022. P(A|D) = 0.006/0.022 ≈ 0.273 = 27.3%.

**A13.** P(clouds) = 0.9×0.3+0.4×0.7=0.27+0.28=0.55. P(rain|clouds) = 0.27/0.55 ≈ 0.491 = 49.1%.

**A15.** P(+) = 0.85×0.001+0.01×0.999=0.00085+0.00999=0.01084. P(COVID|+) = 0.00085/0.01084 ≈ 0.0784 = 7.8%. (Only 7.8% even with positive test!)

**A17.** New P(B) = 0.05×0.9+0.40×0.10=0.045+0.04=0.085. P(spam|meeting) = 0.045/0.085 ≈ 0.529. Down from 0.90 — the word "meeting" is strong evidence it's NOT spam.

**A18.** P(R|U1)=0.5, P(R|U2)=0.8, P(R|U3)=0.2. P(R)=(0.5+0.8+0.2)/3=0.5. P(U2|R)=(0.8×1/3)/(0.5)=0.267/0.5=0.533.

**A21.** From conditional prob: P(A|B)=P(A∩B)/P(B) and P(B|A)=P(A∩B)/P(A). So P(A∩B)=P(B|A)×P(A). Substitute: P(A|B)=P(B|A)×P(A)/P(B). ✅

**A22.** P(fair)=0.5, P(biased)=0.5. P(10H|fair)=(1/2)^10=1/1024≈0.000977. P(10H|biased)=0.8^10≈0.1074. P(10H)=0.000977×0.5+0.1074×0.5=0.05418. P(fair|10H)=0.000977×0.5/0.05418≈0.009=0.9%. After 10 heads, P(fair)≈0.9% — strongly suggests biased coin.

**DS1.** P(+) = 0.95×0.001+0.02×0.999=0.00095+0.01998=0.02093. P(fraud|flagged)=0.00095/0.02093≈0.0454=4.5%. Only 4.5% precision. 95.5% of flagged transactions are legitimate — very high false positive rate. Need to improve model or raise threshold.

**DS2.** P(sports) ∝ 0.5×0.15×0.12×0.10 = 0.0009. P(politics) ∝ 0.5×0.01×0.03×0.04 = 0.000006. Sports wins overwhelmingly. Classify as "Sports."

**DS3.** Prior Beta(1,1). After 50 conversions from 200: Posterior = Beta(1+50, 1+150) = Beta(51, 151). Posterior mean = 51/(51+151) = 51/202 ≈ 0.252 = 25.2% conversion rate.

---

# 💼 Interview Questions

**IQ1.** "What is Bayes' Theorem and why is it important in ML?"
It's the mathematical framework for updating probabilities with new evidence. In ML: it underpins Naïve Bayes, Bayesian inference, probabilistic graphical models, and model calibration. It formalizes how we incorporate prior knowledge and update beliefs with data.

**IQ2.** "What is the difference between P(spam|"free") and P("free"|spam)?"
P(spam|"free") = posterior — probability email is spam given it contains "free" (what we want). P("free"|spam) = likelihood — what fraction of spam emails contain "free" (what we can measure from training data). They're related by Bayes but not equal.

**IQ3.** "What is the base rate fallacy?"
Ignoring the prior probability when interpreting conditional probabilities. Example: A test is 99% accurate. If the condition affects 0.1% of people, most positive tests are still false positives. Base rate (prevalence) must always be considered.

---

# 📌 Chapter Summary

## ⚡ Quick Revision
- Bayes = formula for updating beliefs with evidence
- Posterior ∝ Likelihood × Prior
- P(A|B) = P(B|A)×P(A) / P(B)
- Prior = belief before evidence; Posterior = belief after evidence
- Base rate fallacy = ignoring the prior
- Naïve Bayes = applies Bayes with independence assumption for each feature
- More data → posterior becomes sharper → approaches truth

## 📐 Formula Sheet
| Formula | Name |
|---------|------|
| P(A\|B) = P(B\|A)×P(A) / P(B) | Bayes' Theorem |
| P(B) = Σ P(B\|Bᵢ)P(Bᵢ) | Total probability (denominator) |
| Posterior ∝ Likelihood × Prior | Core proportionality |
| P(A\|B) + P(Aᶜ\|B) = 1 | Posterior sums to 1 |

## ⚠️ Common Mistakes
| Mistake | Correction |
|---------|-----------|
| P(A\|B) = P(B\|A) | Never equal except in special cases |
| Ignoring base rate | Always multiply likelihood by prior |
| Higher test accuracy = higher posterior | Not if disease is very rare |
| Forgetting to normalize | Posterior must sum to 1 |

---
*Chapter 1.3 Complete | Next: Chapter 1.4 — Counting Techniques*
