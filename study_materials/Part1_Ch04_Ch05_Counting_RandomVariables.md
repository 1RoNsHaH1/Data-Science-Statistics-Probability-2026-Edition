# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 1 — Probability Foundations
# Chapter 1.4 — Counting Techniques

> **Chapter Goal:** Learn to count systematically — factorials, permutations, and combinations. These are the building blocks for calculating probabilities when outcomes are enumerable.

---

# 1.4.1 — Why Counting Matters

## 💡 Intuition

To compute P(A) = favorable / total, we need to **count** both the favorable outcomes and the total outcomes.

For simple problems: easy.
For complex problems: "How many ways can 5 players be arranged in batting order?" → need systematic counting.

---

# 1.4.2 — The Multiplication Principle

## 💡 Intuition

> **If you have m ways to do the first thing, and n ways to do the second thing, you have m × n ways to do both.**

**Example:** A restaurant has 4 appetizers and 6 main courses.
How many different meals can you order?

```
4 appetizers × 6 mains = 24 different meals
```

**Example:** License plates: 3 letters followed by 4 digits.
```
26 × 26 × 26 × 10 × 10 × 10 × 10 = 175,760,000 possible plates
```

---

# 1.4.3 — Factorials

## 💡 Intuition

How many ways can you arrange 5 books on a shelf?

```
Position 1: 5 choices
Position 2: 4 remaining choices
Position 3: 3 remaining choices
Position 4: 2 remaining choices
Position 5: 1 remaining choice

Total = 5 × 4 × 3 × 2 × 1 = 120 ways
```

We write this as **5!** (read "5 factorial").

## 📖 Definition

$$n! = n \times (n-1) \times (n-2) \times \cdots \times 2 \times 1$$

**Special case:** 0! = 1 (by convention — there's exactly one way to arrange nothing)

## 📋 Factorial Table

| n | n! |
|---|-----|
| 0 | 1 |
| 1 | 1 |
| 2 | 2 |
| 3 | 6 |
| 4 | 24 |
| 5 | 120 |
| 6 | 720 |
| 7 | 5,040 |
| 10 | 3,628,800 |
| 20 | 2.43 × 10¹⁸ |

> 🧠 **Factorials grow EXTREMELY fast.** 20! is larger than 2 quintillion.

---

# 1.4.4 — Permutations (Order Matters)

## 💡 Intuition

A **permutation** is an arrangement where **order matters**.

"Selecting 3 runners for Gold, Silver, Bronze from 10 athletes" — order matters! Gold ≠ Silver ≠ Bronze.

```
Choose Gold:   10 options
Choose Silver: 9 options (one taken)
Choose Bronze: 8 options

Total = 10 × 9 × 8 = 720 arrangements
```

## 📖 Formula

$$P(n, r) = \frac{n!}{(n-r)!}$$

Read: "Permutations of n things taken r at a time"

| Symbol | Meaning |
|--------|---------|
| n | Total items to choose from |
| r | How many you're choosing |
| n! | All arrangements of n things |
| (n-r)! | Divides out arrangements we don't use |

## 📊 Examples

**Example 1:** How many 3-letter passwords from 26 letters (no repeat)?
P(26, 3) = 26! / 23! = 26 × 25 × 24 = 15,600

**Example 2:** 8 cricket players, choose top-order (positions 1-3):
P(8, 3) = 8!/5! = 8×7×6 = 336

**Example 3:** A 4-digit PIN from 0-9 (no repeat):
P(10, 4) = 10!/6! = 10×9×8×7 = 5,040

---

# 1.4.5 — Combinations (Order Doesn't Matter)

## 💡 Intuition

A **combination** is a selection where **order does NOT matter**.

"Choosing 3 players for a team from 10" — order doesn't matter! The team is the same regardless of which player was chosen first.

## 📖 Formula

$$C(n, r) = \binom{n}{r} = \frac{n!}{r!(n-r)!}$$

Read: "n choose r" — how many ways to choose r items from n, ignoring order

| Symbol | Meaning |
|--------|---------|
| C(n,r) or ⁿCᵣ or $\binom{n}{r}$ | "n choose r" |
| n | Total items |
| r | Number chosen |
| r! | Divides out the different orderings (since we don't care about order) |

## 🎨 Permutations vs Combinations — Visual

```
3 people: A, B, C
Choose 2.

Permutations (order matters): AB, BA, AC, CA, BC, CB = 6
Combinations (order doesn't matter): {A,B}, {A,C}, {B,C} = 3

C(3,2) = P(3,2) / 2! = 6/2 = 3

Rule: C(n,r) = P(n,r) / r!
(Divide permutations by r! to remove ordering)
```

## 📊 Examples

**Example 1:** Choose 5 cards from 52:
C(52,5) = 52!/(5!×47!) = 2,598,960 possible hands

**Example 2:** Choose 11 cricketers from a squad of 15:
C(15,11) = C(15,4) = 15!/(4!×11!) = (15×14×13×12)/(4×3×2×1) = 32760/24 = 1,365

**Example 3:** Probability all 5 cards are hearts:
Favorable: C(13,5) = 1,287
Total: C(52,5) = 2,598,960
P = 1287/2598960 ≈ 0.000495

---

# 1.4.6 — When to Use Which?

```
Does ORDER matter?
      │
      ├── YES → PERMUTATION: P(n,r) = n!/(n-r)!
      │         Examples: Passwords, Rankings, Schedules
      │
      └── NO  → COMBINATION: C(n,r) = n!/[r!(n-r)!]
                Examples: Teams, Committees, Card Hands, Subsets
```

## 📊 Quick Decision Table

| Scenario | Formula |
|----------|---------|
| Arranging all n items | n! |
| Top-3 from 10 athletes (medals) | P(10,3) |
| Team of 3 from 10 players | C(10,3) |
| 5-card hand from deck | C(52,5) |
| 4-digit PIN (no repeat) | P(10,4) |
| Committee of 5 from 20 people | C(20,5) |

## 🚀 Data Science Connection

| Application | Counting Concept |
|-------------|-----------------|
| Feature selection: choose k from p features | C(p,k) combinations |
| Cross-validation splits | Combinations of train/test |
| Binomial distribution | C(n,k) appears in the formula |
| Ensemble methods: choosing subsets of models | Combinations |
| Random forest: feature subset size | Combinations |
| Hyperparameter grid search: count configurations | Multiplication principle |

---

# 📝 Practice Questions — Chapter 1.4

## 🟢 Beginner (8 Questions)
**Q1.** Calculate 7!
**Q2.** P(6,2) = ?
**Q3.** C(8,3) = ?
**Q4.** How many ways to arrange 4 books on a shelf?
**Q5.** How many 3-player teams from 7 players?
**Q6.** A password: 3 letters + 2 digits, no repeats. How many possible passwords?
**Q7.** What is C(n,0) for any n?
**Q8.** What is C(n,n) for any n?

## 🟡 Intermediate (7 Questions)
**Q9.** A class of 30. Choose: president, VP, secretary. How many ways?
**Q10.** Choose a committee of 4 from 30. How many ways?
**Q11.** A cricket team of 11 from 16 players (order doesn't matter). How many ways?
**Q12.** In a lottery: choose 6 numbers from 1-49. Probability of winning?
**Q13.** P(at least 2 heads in 4 flips) using combinations.
**Q14.** How many 5-card hands contain exactly 2 aces?
**Q15.** Prove that C(n,r) = C(n, n-r).

## 🔴 Advanced (5 Questions)
**Q16.** Prove P(n,r) = C(n,r) × r!
**Q17.** How many ways to arrange the letters of "STATISTICS"?
**Q18.** In a group of 10, P(at least 2 people share a birthday) — birthday problem approach.
**Q19.** C(52,5): How many hands have exactly one pair?
**Q20.** How many strings of length 8 using {0,1} have exactly three 1s?

## ✅ Key Solutions
**A1.** 7! = 5040. **A2.** P(6,2) = 6×5=30. **A3.** C(8,3)=56. **A4.** 4!=24. **A5.** C(7,3)=35. **A7.** C(n,0)=1. **A8.** C(n,n)=1.
**A9.** P(30,3)=30×29×28=24,360. **A10.** C(30,4)=27,405. **A11.** C(16,11)=4368.
**A12.** C(49,6)=13,983,816. P=1/13,983,816≈7.15×10⁻⁸.
**A13.** At least 2 heads: C(4,2)+C(4,3)+C(4,4) = 6+4+1=11 out of 2⁴=16. P=11/16.
**A14.** C(4,2)×C(48,3)=6×17296=103,776.
**A17.** STATISTICS has 10 letters: S×3, T×3, A×1, I×2, C×1. Arrangements = 10!/(3!3!2!1!) = 3,628,800/72 = 50,400.
**A20.** C(8,3) = 56 strings.

---

# 💼 Interview Questions

**IQ1.** "What is the difference between a permutation and a combination?"
Permutation: order matters (ranking). Combination: order doesn't matter (selection). 

**IQ2.** "In feature selection, if you have 20 features and want to try all subsets of 5, how many models do you train?"
C(20,5) = 15,504 models.

---

# 📌 Chapter Summary

## ⚡ Quick Revision
- Multiplication principle: m ways × n ways = m×n ways total
- Factorial: n! = n×(n-1)×...×1, 0!=1
- Permutation P(n,r) = n!/(n-r)! — ORDER MATTERS
- Combination C(n,r) = n!/[r!(n-r)!] — ORDER DOESN'T MATTER
- C(n,r) = C(n, n-r) — symmetry

---

---
---

# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 1 — Probability Foundations
# Chapter 1.5 — Random Variables

> **Chapter Goal:** Understand random variables — the bridge from probability to statistics. Every dataset column is a random variable. Every ML prediction is a random variable output.

---

# 1.5.1 — What Is a Random Variable?

## 💡 Intuition

> **A random variable is a variable whose value is determined by the outcome of a random experiment.**

**It's a function that maps outcomes to numbers.**

```
Experiment: Flip a coin twice
Outcome Space: {HH, HT, TH, TT}

Random Variable X = "number of heads"

X(HH) = 2
X(HT) = 1
X(TH) = 1
X(TT) = 0

So X can take values {0, 1, 2} depending on what happens
```

## 🌍 Real-World Random Variables

| Experiment | Random Variable X | Possible Values |
|-----------|-------------------|----------------|
| Cricket match | Runs scored today | 0, 1, 2, ..., 300+ |
| Customer visit | Will they buy? | 0 (no) or 1 (yes) |
| Weather | Temperature at noon | -10.5°C to 50.0°C |
| Product | Delivery time (days) | 1, 2, 3, 4, 5... |
| Email | Is it spam? | 0 or 1 |
| Stock | Daily price change | -∞ to +∞ |

> 📌 **Key Insight:** Capital letters (X, Y, Z) denote random variables. Lowercase letters (x, y, z) denote specific values they take.

---

# 1.5.2 — Discrete vs Continuous Random Variables

## 🔵 Discrete Random Variable

> Takes **countable** values — either finite or countably infinite.

```
X = number of customers who arrive per hour
X ∈ {0, 1, 2, 3, 4, ...}
```

Examples: Number of defects, number of goals, number of clicks.

## 🔴 Continuous Random Variable

> Can take **any value** in a range (uncountably infinite values).

```
X = height of a randomly selected adult
X ∈ [100cm, 250cm] (any decimal value possible)
```

Examples: Temperature, weight, time, income.

---

# 1.5.3 — Probability Mass Function (PMF) — For Discrete

## 📖 Definition

The **PMF** of a discrete random variable X gives the probability of each specific value.

$$P(X = x) = f(x)$$

Must satisfy:
1. P(X = x) ≥ 0 for all x
2. Σ P(X = x) = 1 (probabilities sum to 1)

## 📊 Example — Coin Flip (2 coins)

X = number of heads:

| x | Outcomes | P(X = x) |
|---|---------|----------|
| 0 | TT | 1/4 = 0.25 |
| 1 | HT, TH | 2/4 = 0.50 |
| 2 | HH | 1/4 = 0.25 |
| **Total** | | **1.00** |

```
PMF Bar Chart:
0.5 ─────── * ──────────
0.25 * ──────────────── *
     x=0    x=1    x=2
```

---

# 1.5.4 — Probability Density Function (PDF) — For Continuous

## 📖 Definition

For a continuous random variable, P(X = exact value) = 0.

Instead, we use the **PDF** f(x), where probability is the **area under the curve** between two values.

$$P(a \leq X \leq b) = \int_a^b f(x)\, dx$$

Must satisfy:
1. f(x) ≥ 0
2. Total area under curve = 1

```
         PDF
          │   /\
          │  /  \
          │ /    \
          │/      \──────
          └──────────────── x
          a         b
          ◄─ P(a≤X≤b) = area here ─►
```

> 📌 **Critical Point:** For continuous variables, P(X = 3.14159...) = 0. Only probabilities of RANGES have meaning.

---

# 1.5.5 — Cumulative Distribution Function (CDF)

## 📖 Definition

The **CDF** F(x) gives the probability that X is **at most** x:

$$F(x) = P(X \leq x)$$

Works for **both discrete and continuous** random variables.

## 📊 Properties of CDF

1. F(-∞) = 0 (nothing is less than -∞)
2. F(+∞) = 1 (everything is less than +∞)
3. F(x) is non-decreasing
4. P(a < X ≤ b) = F(b) - F(a)

## 📊 Example — 2 Coin Flips

X = number of heads:

| x | P(X = x) | F(x) = P(X ≤ x) |
|---|---------|-----------------|
| 0 | 0.25 | 0.25 |
| 1 | 0.50 | 0.75 |
| 2 | 0.25 | 1.00 |

```
CDF:
1.0 ──────────────── *
0.75 ──────── *
0.25 * 
     x=0   x=1   x=2
```

P(X ≤ 1) = 0.75 means: 75% chance of getting 0 or 1 heads.

---

# 1.5.6 — Expected Value (Mean of a Random Variable)

## 💡 Intuition

> **The expected value is the long-run average outcome of the random variable.**

If you ran the experiment millions of times and averaged the results, what would you get?

## 📖 Formula — Discrete

$$E[X] = \mu = \sum_x x \cdot P(X = x)$$

Each value × its probability, summed over all values.

## 📊 Examples

**Example 1 — Coin Flips:**
X = number of heads in 2 flips:

```
E[X] = 0×(0.25) + 1×(0.50) + 2×(0.25)
     = 0 + 0.50 + 0.50
     = 1.0

On average, you get 1 head in 2 flips. Makes intuitive sense!
```

**Example 2 — Die Roll:**
E[X] where X = outcome of a fair die:

```
E[X] = 1×(1/6) + 2×(1/6) + 3×(1/6) + 4×(1/6) + 5×(1/6) + 6×(1/6)
     = (1+2+3+4+5+6)/6 = 21/6 = 3.5

A die never shows 3.5 — but the LONG-RUN average is 3.5.
```

**Example 3 — Business Decision:**
Insurance company: P(claim) = 0.05. Average claim = ₹1,00,000. Premium = ₹5,500.
Expected payout = 0.05 × 1,00,000 = ₹5,000.
Expected profit per customer = ₹5,500 - ₹5,000 = ₹500.

## 📐 Properties of Expected Value

| Property | Formula |
|----------|---------|
| E[c] = c | Expected value of a constant is the constant |
| E[cX] = c·E[X] | Scale the variable, scale the expectation |
| E[X + Y] = E[X] + E[Y] | Linearity (always true, even if X,Y dependent) |
| E[X²] ≠ (E[X])² | Generally — important for variance |

---

# 1.5.7 — Variance and Standard Deviation

## 📖 Variance

$$Var(X) = \sigma^2 = E[(X - \mu)^2] = E[X^2] - (E[X])^2$$

**Intuition:** Average squared distance from the mean.

| Symbol | Meaning |
|--------|---------|
| σ² | Variance |
| μ | E[X], the mean |
| E[(X-μ)²] | Average squared deviation |

## 📊 Example

X = die roll. E[X] = 3.5.

```
E[X²] = 1²×(1/6)+2²×(1/6)+3²×(1/6)+4²×(1/6)+5²×(1/6)+6²×(1/6)
      = (1+4+9+16+25+36)/6 = 91/6 ≈ 15.167

Var(X) = 15.167 - (3.5)² = 15.167 - 12.25 = 2.917

SD(X) = √2.917 ≈ 1.708
```

---

# 📝 Practice Questions — Chapter 1.5

## 🟢 Beginner (8 Questions)
**Q1.** What is a random variable? How does it differ from a regular variable?
**Q2.** X = outcome of one die roll. List all values X can take.
**Q3.** P(X=1)=0.2, P(X=2)=0.5, P(X=3)=0.3. Is this a valid PMF?
**Q4.** Compute E[X] for Q3.
**Q5.** What is the CDF F(2) for Q3?
**Q6.** Why is P(X = 5.000) = 0 for a continuous RV but not for a discrete RV?
**Q7.** X is number of heads in 3 coin flips. List all values and their probabilities.
**Q8.** If E[X] = 4 and E[Y] = 3, find E[X + Y].

## 🟡 Intermediate (7 Questions)
**Q9.** X: P(X=0)=0.4, P(X=1)=0.35, P(X=2)=0.20, P(X=5)=0.05. Find E[X], E[X²], Var(X).
**Q10.** A game: win ₹300 with P=0.3, lose ₹100 with P=0.7. Find E[winnings].
**Q11.** Construct the CDF for number of heads in 3 flips.
**Q12.** If Var(X) = 9, find Var(3X + 2). [Var(aX+b) = a²Var(X)]
**Q13.** Show E[X - μ] = 0 for any random variable.
**Q14.** X is uniform on {1,2,3,4,5}. Find E[X] and Var(X).
**Q15.** Company models Y = 2X + 500 where X = units sold. E[X]=100, Var(X)=25. Find E[Y] and Var(Y).

## 🔴 Advanced (5 Questions)
**Q16.** Chebyshev's inequality: P(|X-μ| ≥ kσ) ≤ 1/k². What does this say when k=2?
**Q17.** Moment generating function (MGF): M(t) = E[e^(tX)]. What is M(0)?
**Q18.** Covariance: Cov(X,Y) = E[(X-μₓ)(Y-μᵧ)]. If X and Y are independent, what is Cov(X,Y)?
**Q19.** Jensen's inequality: For a convex function g, E[g(X)] ≥ g(E[X]). Show an example with g(x)=x².
**Q20.** Var(X+Y) = Var(X) + Var(Y) + 2Cov(X,Y). When does Var(X+Y)=Var(X)+Var(Y)?

## 🚀 Data Science Scenarios (5 Questions)
**DS1.** A model output is P(churn)=0.73. Interpret this as a Bernoulli RV with p=0.73.
**DS2.** Revenue X per customer: P(₹0)=0.6, P(₹500)=0.3, P(₹2000)=0.1. Find E[X] and interpret for business.
**DS3.** A neural network produces a probability distribution over 10 classes. What type of RV is "the predicted class"? What about the probability score for class 3?
**DS4.** Loss function L = (Y - Ŷ)². E[L] is Mean Squared Error. Explain MSE as the expected value of a random variable.
**DS5.** In A/B testing, X_A ~ Binomial(n_A, p_A). E[X_A] = n_A × p_A. If n=1000 and p=0.05, find E[conversions] and Var[conversions].

---

# ✅ Key Solutions

**A3.** 0.2+0.5+0.3=1.0, all ≥0. ✅ Valid PMF.
**A4.** E[X] = 1×0.2+2×0.5+3×0.3 = 0.2+1.0+0.9 = 2.1.
**A5.** F(2) = P(X≤2) = P(X=1)+P(X=2) = 0.2+0.5 = 0.7.
**A7.** X=0: P=1/8; X=1: P=3/8; X=2: P=3/8; X=3: P=1/8.
**A8.** E[X+Y]=E[X]+E[Y]=4+3=7.
**A9.** E[X]=0×0.4+1×0.35+2×0.20+5×0.05=0+0.35+0.40+0.25=1.0. E[X²]=0+0.35+0.80+1.25=2.4. Var=2.4-1²=1.4.
**A10.** E[winnings]=300×0.3+(-100)×0.7=90-70=₹20. Positive expected value — play the game!
**A12.** Var(3X+2)=3²×Var(X)=9×9=81. SD=9.
**A15.** E[Y]=2×100+500=700. Var(Y)=2²×25=100.
**A16.** k=2: P(|X-μ|≥2σ)≤1/4=25%. At most 25% of values lie more than 2 std devs from mean.
**A18.** Cov(X,Y)=0 when independent (no linear relationship).
**DS2.** E[X]=0×0.6+500×0.3+2000×0.1=0+150+200=₹350 per customer. If you have 10,000 customers, expected revenue = ₹35,00,000.
**DS5.** E[conversions]=1000×0.05=50. Var=1000×0.05×0.95=47.5. SD≈6.89.

---

# 💼 Interview Questions

**IQ1.** "What is the expected value, and why does it matter in business?"
Expected value is the probability-weighted average outcome. In business: guides decisions under uncertainty (insurance premiums, investment returns, product pricing).

**IQ2.** "What is the difference between PMF, PDF, and CDF?"
PMF = probability at each exact point (discrete). PDF = probability density (continuous — area gives probability). CDF = cumulative probability up to x — works for both.

**IQ3.** "Why is variance important in ML?"
Low variance = consistent predictions. High variance = unstable, sensitive to training data (overfitting). The bias-variance tradeoff balances underfitting vs overfitting.

---

# 📌 Combined Chapter Summary (1.4 + 1.5)

## ⚡ Quick Revision
- Factorial: n! = arrangements of n items
- Permutation P(n,r) = n!/(n-r)! — order matters
- Combination C(n,r) = n!/[r!(n-r)!] — order doesn't matter
- Random Variable X: maps experiment outcomes to numbers
- PMF: P(X=x) for discrete — must sum to 1
- PDF: f(x) for continuous — area gives probability
- CDF: F(x) = P(X≤x) — works for both
- E[X] = Σ x·P(X=x) — probability-weighted average
- Var(X) = E[X²] - (E[X])²

## 📐 Formula Sheet
| Formula | Name |
|---------|------|
| n! = n×(n-1)×...×1 | Factorial |
| P(n,r) = n!/(n-r)! | Permutation |
| C(n,r) = n!/[r!(n-r)!] | Combination |
| E[X] = Σ x·P(X=x) | Expected value (discrete) |
| Var(X) = E[X²] - μ² | Variance shortcut |
| SD(X) = √Var(X) | Standard deviation |
| Var(aX+b) = a²·Var(X) | Variance of linear transform |

---
*Chapters 1.4 & 1.5 Complete | Next: Part 2 — Probability Distributions*
