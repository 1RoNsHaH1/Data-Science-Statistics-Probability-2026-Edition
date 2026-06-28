# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 2 — Probability Distributions
# Chapters 2.1–2.4 — Bernoulli, Binomial, Geometric & Poisson

---

# Chapter 2.1 — Bernoulli Distribution

> **The simplest of all distributions — one trial, two outcomes.**

---

## 💡 Intuition

> Every yes/no, success/failure, 0/1 outcome in the universe follows a Bernoulli distribution.

**Real-life Bernoulli trials:**
- Will this email be opened? (Yes/No)
- Will this customer churn? (Yes/No)
- Does this image contain a cat? (Yes/No)
- Was a coin heads? (Yes/No)
- Was this transaction fraudulent? (Yes/No)

**One trial. Two outcomes. That's all.**

---

## 📖 Definition & Formula

A random variable X follows Bernoulli(p) if:

$$P(X = x) = p^x (1-p)^{1-x}, \quad x \in \{0, 1\}$$

Which simply means:
```
P(X = 1) = p        ← probability of success
P(X = 0) = 1 - p    ← probability of failure (also written as q)
```

| Parameter | Symbol | Meaning |
|-----------|--------|---------|
| Success probability | p | P(outcome = 1) |
| Failure probability | q = 1-p | P(outcome = 0) |
| Mean | E[X] = p | Average outcome |
| Variance | Var(X) = p(1-p) | Spread |

## 📊 Key Properties

```
Mean:     E[X] = p
Variance: Var[X] = p(1-p) = pq

Maximum variance when p = 0.5:
Var[X] = 0.5 × 0.5 = 0.25 (most uncertain!)

Variance decreases as p → 0 or p → 1
(When outcome is almost certain, less uncertainty)
```

## 📊 Worked Example

A logistic regression predicts P(customer buys) = 0.72.

```
X ~ Bernoulli(p = 0.72)
E[X] = 0.72 (72% chance of purchase)
Var[X] = 0.72 × 0.28 = 0.2016
```

## 🚀 Data Science Connection

Every binary classification output is a Bernoulli RV:
- Logistic regression output = p for Bernoulli(p)
- Neural network sigmoid output = Bernoulli probability
- Binary cross-entropy loss derived from Bernoulli likelihood

---

# Chapter 2.2 — Binomial Distribution

> **Multiple independent Bernoulli trials — how many successes in n attempts?**

---

## 💡 Intuition

You flip a fair coin 10 times. How many heads do you expect? What's the probability of exactly 6 heads?

This is a **Binomial** situation:
- Fixed number of trials: n = 10
- Each trial is independent
- Each trial has same P(success) = p = 0.5
- Count total successes

```
10 trials → each is Bernoulli → count total → Binomial
```

## 🌍 Real-Life Binomial Scenarios

| Scenario | n | p | X |
|----------|---|---|---|
| 100 emails sent, P(opened)=0.3 | 100 | 0.30 | # opened |
| 20 products, P(defective)=0.05 | 20 | 0.05 | # defective |
| 15 cancer patients, P(remission)=0.7 | 15 | 0.70 | # in remission |
| 1000 website visitors, P(convert)=0.02 | 1000 | 0.02 | # converts |

---

## 📖 Formula

$$P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}$$

| Symbol | Meaning |
|--------|---------|
| n | Number of trials |
| k | Number of successes we're asking about |
| p | Probability of success on one trial |
| (1-p) = q | Probability of failure |
| C(n,k) | Ways to choose which k trials succeed |
| p^k | Probability all k successes happen |
| (1-p)^(n-k) | Probability all remaining n-k trials fail |

## 📊 Parameters

```
Mean:     E[X] = np
Variance: Var[X] = np(1-p) = npq
Std Dev:  σ = √(npq)
```

## 📊 Worked Examples

**Example 1 — Quality Control:**
A batch of 20 products. Each has P(defective) = 0.1.
Find P(exactly 2 defective).

```
n=20, k=2, p=0.1

P(X=2) = C(20,2) × (0.1)² × (0.9)¹⁸
        = 190 × 0.01 × 0.1501
        = 190 × 0.001501
        ≈ 0.2852 = 28.52%
```

**Example 2 — Expected Value:**
You send 500 marketing emails. P(open) = 0.25.
E[opens] = 500 × 0.25 = 125 emails expected to open.
SD = √(500 × 0.25 × 0.75) = √93.75 ≈ 9.68

**Example 3 — Cricket:**
Batsman hits a boundary on 30% of balls. In 10 balls, P(at least 3 boundaries)?

```
P(X ≥ 3) = 1 - P(X < 3) = 1 - [P(0) + P(1) + P(2)]

P(X=0) = C(10,0)×0.3⁰×0.7¹⁰ = 0.0282
P(X=1) = C(10,1)×0.3¹×0.7⁹  = 10×0.3×0.0404 = 0.1211
P(X=2) = C(10,2)×0.3²×0.7⁸  = 45×0.09×0.0576 = 0.2335

P(X ≥ 3) = 1 - (0.0282 + 0.1211 + 0.2335) = 1 - 0.3828 = 0.6172
```

## 🎨 Visualizing the Binomial Distribution

```
n=10, p=0.3: Probability of each number of successes

k:  0    1    2    3    4    5    6    7    8    9   10
P: .028 .121 .233 .267 .200 .103 .037 .009 .001 .000 .000
           ↑    ↑
           Peak around k=np=3

Shape: Bell-shaped, centered at np, slightly right-skewed for small p
```

## 🔗 Connection to Bernoulli

X ~ Binomial(n, p) is the sum of n independent Bernoulli(p) variables:

```
X = X₁ + X₂ + ... + Xₙ

where each Xᵢ ~ Bernoulli(p)
```

## 🚀 Data Science Connection

| Application | Binomial Use |
|-------------|-------------|
| A/B test: # conversions | X ~ Binomial(n_visitors, p_conversion) |
| Quality check: # defects | X ~ Binomial(n_items, p_defect) |
| Email campaign: # opens | X ~ Binomial(n_sent, p_open) |
| Ensemble: # models correct | X ~ Binomial(n_models, p_accuracy) |

---

# Chapter 2.3 — Geometric Distribution

> **How many trials until the FIRST success?**

---

## 💡 Intuition

You keep flipping a coin until you get heads. How many flips will it take?

Or: You keep calling customers until one picks up. How many calls until the first answer?

**Geometric = "Waiting for the first success"**

```
Trial 1: Fail
Trial 2: Fail
Trial 3: Fail
Trial 4: SUCCESS ← X = 4 (took 4 trials)
```

## 📖 Formula

$$P(X = k) = (1-p)^{k-1} \times p, \quad k = 1, 2, 3, \ldots$$

| Symbol | Meaning |
|--------|---------|
| k | The trial number on which first success occurs |
| (1-p)^(k-1) | Probability of k-1 failures first |
| p | Probability of success on the k-th trial |

## 📊 Parameters

```
Mean:     E[X] = 1/p
Variance: Var[X] = (1-p)/p²

If P(success) = 0.2:
E[X] = 1/0.2 = 5 trials on average until first success
```

## 📊 Worked Examples

**Example 1 — Customer Service:**
P(customer answers call) = 0.3.
P(first answer on 4th call)?

```
P(X=4) = (0.7)³ × 0.3 = 0.343 × 0.3 = 0.1029
```

E[calls until answer] = 1/0.3 ≈ 3.33 calls on average.

**Example 2 — Model Training:**
Each training epoch has P(converge) = 0.15.
E[epochs until convergence] = 1/0.15 ≈ 6.67 epochs.

## 🧠 Memoryless Property

> **The geometric distribution has no memory.**

If you've already flipped 5 tails in a row, the probability of heads on the next flip is still p — the same as at the start.

```
P(X > k+m | X > k) = P(X > m)

"Having already failed k times doesn't make success any more or less likely"
```

This is a unique property — no other discrete distribution has it.

## 🚀 Data Science Connection

- Time until first user engagement after seeing an ad
- Number of experiments until a statistically significant result
- Number of hyperparameter searches until good performance
- Modeling waiting times in queues (customer service, packet routing)

---

# Chapter 2.4 — Poisson Distribution

> **How many events happen in a fixed interval of time or space?**

---

## 💡 Intuition

> **Poisson models rare, random events happening at a constant average rate.**

**Real-life Poisson scenarios:**
- Emails arriving per hour (average: 5/hour)
- Customers entering a shop per minute (average: 3/minute)
- Defects per square meter of fabric
- Goals scored per match (average: 2.5/match)
- Taxi requests per 10 minutes in Mumbai
- Server errors per day
- Typos per 1000 words

**All share these features:**
1. Events happen randomly and independently
2. Average rate (λ) is constant
3. Two events can't happen simultaneously
4. Events are rare relative to possible opportunities

---

## 📖 Formula

$$P(X = k) = \frac{e^{-\lambda} \lambda^k}{k!}, \quad k = 0, 1, 2, \ldots$$

| Symbol | Meaning |
|--------|---------|
| k | Number of events |
| λ (lambda) | Average number of events in the interval |
| e | Euler's number ≈ 2.71828 |
| k! | k factorial |

## 📊 Parameters

```
Mean:     E[X] = λ
Variance: Var[X] = λ

Special property: Mean = Variance = λ
This is unique to Poisson and can be used to CHECK if Poisson is appropriate!
```

## 📊 Worked Examples

**Example 1 — Restaurant:**
On average, 3 customers arrive per minute. What's P(exactly 5 arrive in a minute)?

```
λ = 3, k = 5, e ≈ 2.71828

P(X=5) = e⁻³ × 3⁵ / 5!
        = 0.0498 × 243 / 120
        = 12.10 / 120
        ≈ 0.1008 = 10.08%
```

**Example 2 — Server Crashes:**
Average 2 server crashes per month (λ=2).
P(no crashes in a month)?

```
P(X=0) = e⁻² × 2⁰ / 0!
        = e⁻²
        = 1/e²
        ≈ 0.1353 = 13.5%
```

P(≥1 crash) = 1 - 0.1353 = 0.8647 = 86.5% chance of at least one crash!

**Example 3 — IPL Goals:**
Average 2.8 sixes per team per innings. P(exactly 4 sixes)?

```
λ = 2.8, k = 4
P(X=4) = e⁻²·⁸ × 2.8⁴ / 4!
        = 0.0608 × 61.47 / 24
        ≈ 0.1557 = 15.57%
```

## 🎨 Poisson Visualization

```
λ=2:                λ=5:
P |                 P |
  | *                 |       *  *
  | * *               |     *       *
  | * * *             |   *           *
  |_* * * *___        |_*_______________
  0 1 2 3 4 5         0 1 2 3 4 5 6 7 8

Small λ: Skewed right    Larger λ: More symmetric (approaches Normal!)
```

## 🔗 Relationship to Binomial

When n is large, p is small, and np = λ is moderate:

**Binomial(n,p) ≈ Poisson(λ = np)**

```
100 emails sent, P(error)=0.03, λ=3
Binomial: C(100,k)×0.03^k×0.97^(100-k)  ← tedious
Poisson approximation: e⁻³ × 3^k / k!   ← much simpler, nearly identical result
```

## 🚀 Data Science Connection

| Application | Poisson Use |
|-------------|------------|
| Website traffic modeling | Requests per second |
| Fraud detection | Fraudulent transactions per day |
| NLP: word frequency | Words per document |
| Queueing models | Customer service systems |
| Network traffic | Packets per second |
| Recommendation systems | User sessions per day |

---

# 📝 Practice Questions — Chapters 2.1–2.4

## 🟢 Beginner (10 Questions)

**Q1.** X ~ Bernoulli(0.4). Find P(X=1), P(X=0), E[X], Var(X).
**Q2.** 10 coin flips. Using Binomial, find P(exactly 5 heads).
**Q3.** X ~ Geometric(0.25). Find P(first success on trial 3).
**Q4.** λ=4 (Poisson). Find P(X=0).
**Q5.** X ~ Binomial(20, 0.3). Find E[X] and Var[X].
**Q6.** Is "number of goals in a football match" better modeled by Binomial or Poisson? Why?
**Q7.** A coin has P(heads)=0.6. Flip it 5 times. What is the expected number of heads?
**Q8.** λ=2 (Poisson). P(X ≤ 1)?
**Q9.** X ~ Geometric(0.5). E[X]?
**Q10.** P(defective)=0.02. Lot of 50 items. E[defectives] and Var[defectives]?

## 🟡 Intermediate (10 Questions)

**Q11.** X ~ Binomial(100, 0.05). Find P(X=0), P(X=1), P(X≥2).
**Q12.** A call center: P(answer)=0.4. P(first answer on 5th call)?
**Q13.** Poisson with λ=3: Find P(2 ≤ X ≤ 4).
**Q14.** X ~ Binomial(n, p) with mean=6 and variance=4.8. Find n and p.
**Q15.** Emails arrive at rate 10/hour. P(≥3 emails in 30 minutes)?
**Q16.** Why does Poisson have equal mean and variance? When does this help identify the distribution?
**Q17.** Sum of independent Poisson: If X~Poisson(3) and Y~Poisson(5) independently, then X+Y~Poisson(?).
**Q18.** Approximate Binomial(200, 0.01) with Poisson. Find P(X=2).
**Q19.** A geometric RV X has P(X>5) = P(X>3)×P(X>2). Show this demonstrates the memoryless property.
**Q20.** In a Bernoulli process: P(X=1)=0.7. The model outputs "1" for 100 customers. E[correct predictions]?

## 🔴 Advanced (5 Questions)

**Q21.** Derive the mean of Binomial: E[X] = np using the PMF formula.
**Q22.** Prove the memoryless property: For Geometric(p), P(X>m+n|X>m) = P(X>n).
**Q23.** X ~ Binomial(n,p). Show that as n→∞, p→0 with np=λ constant, the PMF approaches Poisson(λ).
**Q24.** Negative Binomial: P(k-th success on trial n). How does this generalize Geometric?
**Q25.** Compound Poisson: Events arrive Poisson(λ), each has random size Y. E[total size]?

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** Binary classifier on 1000 test samples with P(correct)=0.92. Find E[correct], Var[correct], and P(≥950 correct).

**DS2.** Web server: average 100 requests/second. P(>150 requests in a second) — qualitative analysis: what does this mean for capacity planning?

**DS3.** Email campaign: 10,000 sent, 25% open rate, 8% CTR given open. Model total clicks as cascaded Binomials. Find E[clicks] and Var[clicks].

**DS4.** A/B test: Old version converts at 5%, new version at 7%. 500 visitors each. Model as Binomials. Find E and Var for both. What's the expected difference?

**DS5.** Anomaly detection: Usual server errors follow Poisson(0.5) per hour. Today you observe 5 errors in one hour. Is this unusual? Calculate P(X≥5) under the Poisson(0.5) model.

---

# ✅ Key Solutions

**A1.** P(X=1)=0.4, P(X=0)=0.6, E[X]=0.4, Var=0.4×0.6=0.24.
**A2.** C(10,5)×0.5^5×0.5^5 = 252×0.03125×0.03125 = 252/1024 ≈ 0.246.
**A3.** P(X=3)=(0.75)²×0.25=0.5625×0.25=0.1406.
**A4.** P(X=0)=e⁻⁴=0.0183.
**A5.** E[X]=20×0.3=6. Var=20×0.3×0.7=4.2.
**A8.** P(X≤1)=P(0)+P(1)=e⁻²+2e⁻²=3e⁻²=3×0.1353=0.4060.
**A9.** E[X]=1/0.5=2.
**A10.** E=50×0.02=1. Var=50×0.02×0.98=0.98.
**A11.** P(X=0)=0.95^100≈0.0059. P(X=1)=100×0.05×0.95^99≈0.0312. P(X≥2)=1-0.0371=0.9629.
**A12.** P(X=5)=(0.6)^4×0.4=0.1296×0.4=0.0518.
**A13.** P(2)+P(3)+P(4)=e⁻³(9/2+27/6+81/24)=e⁻³×(4.5+4.5+3.375)=0.0498×12.375≈0.616.
**A14.** E=np=6, Var=npq=4.8. q=4.8/6=0.8, p=0.2, n=6/0.2=30.
**A15.** λ for 30 min = 5. P(X≥3)=1-P(0)-P(1)-P(2)=1-e⁻⁵(1+5+12.5)=1-0.0067×18.5≈1-0.1247=0.875.
**A17.** X+Y ~ Poisson(3+5) = Poisson(8). Sum of independent Poissons = Poisson with summed rates.
**A18.** λ=200×0.01=2. P(X=2)=e⁻²×4/2=0.1353×2=0.2707.
**DS1.** E[correct]=920. Var=1000×0.92×0.08=73.6. SD≈8.58. P(≥950): z=(950-920)/8.58≈3.5 → very unlikely (p≈0.0002).
**DS5.** λ=0.5. P(X≥5)=1-P(X≤4)=1-Σe⁻⁰·⁵(0.5^k/k!) for k=0..4. ≈1-0.9982=0.0018. Yes, extremely unusual!

---

# 💼 Interview Questions

**IQ1.** "When would you use Poisson vs Binomial?"
Poisson when: n is large, p is small, you're counting rare events in a continuous interval. Binomial when: n is fixed and small/medium, each trial is clearly identifiable.

**IQ2.** "What does the memoryless property of the Geometric distribution mean for ML?"
Each trial is independent — past failures give no information about future success. Useful for modeling retry logic in ML pipelines or number of trials in random search.

**IQ3.** "How is Bernoulli related to Binomial?"
Bernoulli(p) = Binomial(n=1, p). Binomial is the sum of n independent Bernoulli(p) trials.

---

# 📌 Chapter Summary (2.1–2.4)

## 📐 Formula Sheet

| Distribution | PMF | Mean | Variance |
|--------------|-----|------|----------|
| Bernoulli(p) | p^x(1-p)^(1-x) | p | p(1-p) |
| Binomial(n,p) | C(n,k)p^k(1-p)^(n-k) | np | np(1-p) |
| Geometric(p) | (1-p)^(k-1)p | 1/p | (1-p)/p² |
| Poisson(λ) | e^(-λ)λ^k/k! | λ | λ |

## ⚠️ Common Mistakes

| Mistake | Correction |
|---------|-----------|
| Using Binomial when trials aren't independent | Check independence |
| Using Poisson when λ varies over time | Poisson assumes constant rate |
| Forgetting Poisson Mean=Variance | Check this first as a diagnostic |
| P(X≥1) calculated directly | Use complement: 1-P(X=0) |

---
*Chapters 2.1–2.4 Complete | Next: Chapters 2.5–2.8 — Uniform, Normal, Exponential, CLT*
