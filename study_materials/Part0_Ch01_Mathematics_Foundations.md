# 📚 Data Science — Statistics & Probability (2026 Edition)

> **A complete, beginner-to-advanced course in Statistics & Probability for Data Science.**
> Written for learners who are afraid of math and want to become confident Data Scientists.

---

# PART 0 — Foundations

> **Why Part 0 exists:**
> Before we can talk about probability, statistics, or machine learning, we need a shared language.
> That language is mathematics.
> This part builds that language from absolute zero — no prior knowledge assumed.

---

# Chapter 0.1 — Mathematics for Data Science

> 📌 **Chapter Goal:**
> By the end of this chapter, you will feel comfortable with the mathematical tools
> that appear constantly throughout Data Science — arithmetic, algebra, fractions,
> powers, roots, logarithms, functions, graphs, sigma notation, and growth patterns.
> You do not need to memorize everything. You need to *understand* it.

---

## 🗺️ Chapter Roadmap

```
0.1.1  Arithmetic & Algebra Refresh
0.1.2  Fractions, Powers, Roots, Logarithms
0.1.3  Functions & Graphs
0.1.4  Sigma Notation
0.1.5  Scientific Notation
0.1.6  Exponents & Logarithmic Growth
0.1.7  Linear vs Exponential Growth
```

---
---

# 0.1.1 — Arithmetic & Algebra Refresh

---

## 💡 Intuition First — What Is Mathematics, Really?

> **Before you read a single formula, understand this one idea:**

Mathematics is just **organized counting and comparing**.

That's it.

Everything else — no matter how scary it looks — is a shortcut notation for counting and comparing things.

When you:
- Split a restaurant bill → **arithmetic**
- Figure out how many days left in the month → **arithmetic**
- Calculate a 20% discount on a shirt → **arithmetic + percentages**
- Predict how much your salary might grow → **algebra + growth patterns**

You are doing mathematics every day without realizing it.

> 🧠 **Think About It:**
> If you can count, add, subtract, and share things fairly — you already understand the core of mathematics.
> We are just going to write those ideas with more precision.

---

## 🌍 Real-Life Context — Why Does a Data Scientist Need Arithmetic?

Imagine you are a Data Scientist at Netflix.

You have a dataset with:
- 5,000 users
- Each user watched an average of 3.7 hours per day
- Netflix charges ₹199/month per user

You need to answer:
- "How many total hours of content were watched today?" → **multiplication**
- "What percentage of users watched more than 5 hours?" → **division + percentages**
- "If we raise the price to ₹249, how much extra revenue do we earn?" → **algebra**
- "At what user count do we break even on server costs?" → **equation solving**

Every single analysis you do as a Data Scientist uses arithmetic and algebra **constantly**.

---

## 🧱 The Four Basic Operations

### ➕ Addition — Combining Things

**Intuition:** You have some things. You get more things. How many total?

**Real Life:** You scored 78 in Math and 85 in English. Total marks = 78 + 85 = 163.

```
78
+85
---
163
```

**Data Science:** Adding up all the values in a column to find the total.

---

### ➖ Subtraction — Removing Things

**Intuition:** You have some things. Some are taken away. How many remain?

**Real Life:** A product costs ₹1,200. There is a ₹150 discount. You pay ₹1,200 − ₹150 = ₹1,050.

```
1200
- 150
-----
1050
```

**Data Science:** Finding the difference between a prediction and the actual value (called **error**).

---

### ✖️ Multiplication — Repeated Addition

**Intuition:** Instead of adding the same number many times, multiply.

Adding 5 three times: 5 + 5 + 5 = 15

Multiplying: 5 × 3 = 15

**Real Life:** A cinema has 24 rows of seats. Each row has 18 seats. Total seats = 24 × 18 = 432.

**Data Science:** Scaling values, computing probabilities, matrix operations.

---

### ➗ Division — Fair Sharing

**Intuition:** Divide a total equally into parts.

**Real Life:** You and 4 friends split a ₹2,400 restaurant bill.
Each person pays: ₹2,400 ÷ 5 = ₹480

**Data Science:** Computing averages (sum ÷ count), ratios, rates.

---

## 📐 Order of Operations — BODMAS / PEMDAS

> ⚠️ **Common Mistake Alert:**
> When a calculation has multiple operations, people often calculate left to right.
> This gives the **wrong answer**.
> There is a universal rule for the order.

### The Rule: BODMAS

| Letter | Meaning         | Example                     |
|--------|-----------------|----------------------------|
| B      | Brackets        | Solve ( ) first             |
| O      | Orders (Powers) | Exponents like 2³           |
| D      | Division        | Left to right after above   |
| M      | Multiplication  | Left to right after above   |
| A      | Addition        | Left to right after above   |
| S      | Subtraction     | Left to right after above   |

> 📌 **Memory Trick:** **"Big Old Dogs Make A Scene"** → B-O-D-M-A-S

### 📊 Example — Order of Operations in Action

**Problem:** Calculate 3 + 4 × 2

❌ **Wrong approach (left to right):** 3 + 4 = 7, then 7 × 2 = **14**

✅ **Correct approach (BODMAS):** Multiplication first → 4 × 2 = 8, then 3 + 8 = **11**

---

**Problem:** Calculate (3 + 4) × 2

Step 1 — Brackets first: (3 + 4) = 7
Step 2 — Multiply: 7 × 2 = **14**

Notice: The brackets changed the answer entirely!

---

**Problem (Data Science style):** Mean Squared Error = (actual − predicted)² ÷ n

Suppose actual = 10, predicted = 7, n = 1:

Step 1 — Brackets: (10 − 7) = 3
Step 2 — Order (power): 3² = 9
Step 3 — Division: 9 ÷ 1 = **9**

> 🚀 **Data Science Connection:**
> BODMAS appears in every formula in statistics: mean, variance, standard deviation, error metrics.
> Understanding it prevents calculation mistakes that corrupt your analysis.

---

## 📦 Variables — The Core of Algebra

### What Is a Variable?

> **Intuition:** A variable is just a **labeled box that holds a number**.

In mathematics, instead of writing the actual number (which we might not know yet), we use a **letter as a placeholder**.

```
┌─────────┐
│    x    │  ← x is a box. We don't know what's inside yet.
└─────────┘
```

When we find out x = 5:

```
┌─────────┐
│    5    │  ← Now we know the box holds the number 5.
└─────────┘
```

### Variables in Data Science

In Data Science, variables represent:
- `n` → the number of data points (how many rows in your dataset)
- `μ` (mu) → the average (mean) of a population
- `σ` (sigma) → the standard deviation (spread of data)
- `x` → an individual data point
- `ŷ` (y-hat) → a predicted value
- `p` → a probability value

> 📌 **Remember:** Greek letters (μ, σ, α, β, θ) are just labeled boxes, exactly like x and y.
> Don't let them intimidate you. They are just names for numbers we care about.

---

## 🔢 Expressions vs Equations

### Expression — A Mathematical Phrase

An expression is a combination of numbers and variables **without an equals sign**.

```
2x + 3        ← an expression
5n − 7        ← an expression
3x² + 2x + 1  ← an expression
```

Think of an expression like a sentence fragment: "two bags of apples plus three oranges"
It describes something but doesn't state a complete fact.

---

### Equation — A Mathematical Statement

An equation has an **equals sign** and states that two things are equal.

```
2x + 3 = 11       ← an equation
5n − 7 = 18       ← an equation
y = 3x + 2        ← an equation (this is also a formula!)
```

Think of an equation like a complete sentence: "two bags of apples plus three oranges equals eleven fruits."

> **Key insight:** An equation can be **solved** — we can find the value of the unknown variable.

---

## 🧮 Solving Simple Equations — Algebra in Action

### The Golden Rule of Algebra

> **Whatever you do to one side of the equation, you MUST do to the other side.**

The equation is a balanced scale. If you add weight to one side, you must add the same weight to the other side to keep it balanced.

```
Before:              After adding 3 to both sides:

  [2x+5] = [11]        [2x+5+3] = [11+3]
     ⚖️                     ⚖️
  (balanced)             (still balanced)
```

### 📊 Step-by-Step Example 1 — Simple Equation

**Problem:** Solve for x: `x + 5 = 12`

**Goal:** We want x alone on the left side.

**Step 1:** x has +5 added to it. To remove it, subtract 5 from both sides.

```
x + 5      = 12
x + 5 - 5  = 12 - 5    ← subtract 5 from both sides
x + 0      = 7
x          = 7
```

**Check:** Put x = 7 back into the original: 7 + 5 = 12 ✅

---

### 📊 Step-by-Step Example 2 — Two-Step Equation

**Problem:** Solve for x: `2x + 3 = 11`

**Step 1:** Remove the +3 first (undo addition, subtract 3 from both sides).

```
2x + 3 - 3 = 11 - 3
2x         = 8
```

**Step 2:** Remove the ×2 (undo multiplication, divide both sides by 2).

```
2x ÷ 2 = 8 ÷ 2
x      = 4
```

**Check:** 2(4) + 3 = 8 + 3 = 11 ✅

---

### 📊 Step-by-Step Example 3 — Data Science Context

**Problem:** You know total revenue = price × users. Revenue = ₹49,750. Price = ₹199. Find the number of users.

```
Revenue = Price × Users
49750   = 199 × Users
49750 ÷ 199 = Users
Users = 250
```

There are 250 users. ✅

> 🚀 **Data Science Connection:**
> Solving equations is the foundation of:
> - **Linear Regression** (finding best-fit line: y = mx + b)
> - **Optimization** (finding weights in neural networks)
> - **Cost functions** (solving for minimum error)

---

## ⚠️ Common Mistakes in Arithmetic & Algebra

| Mistake | Wrong | Correct |
|---------|-------|---------|
| Ignoring BODMAS | 3 + 4 × 2 = 14 | 3 + 4 × 2 = 11 |
| Not doing same thing to both sides | 2x + 3 = 11 → x = 11/2 + 3 | 2x = 8 → x = 4 |
| Confusing expression and equation | "solving" 2x + 3 | Only equations can be solved |
| Treating variable letter as word | x means "unknown", not "times" | x is a placeholder for a number |
| Sign errors when subtracting | 2x + 3 = 11 → 2x = 11 + 3 = 14 | 2x = 11 - 3 = 8 |

---
---

# 0.1.2 — Fractions, Powers, Roots, Logarithms

---

## 🍕 FRACTIONS — Dividing Things Into Parts

### 💡 Intuition

A fraction is just a way to describe **part of a whole**.

Imagine a pizza cut into 8 equal slices. You eat 3 slices.

```
Whole pizza (8 slices):
🍕🍕🍕🍕🍕🍕🍕🍕

You ate (3 slices):
🍕🍕🍕
```

You ate **3 out of 8** slices. We write this as: **3/8**

```
    3      ← Numerator   (how many parts you have)
   ───
    8      ← Denominator (how many equal parts the whole is divided into)
```

> **Memory Trick:** **N**umerator is on top like the **N**orth Pole. **D**enominator is on the **D**own side.

---

### 📊 Types of Fractions

| Type | Description | Example |
|------|-------------|---------|
| Proper fraction | Numerator < Denominator | 3/8 (less than 1 whole) |
| Improper fraction | Numerator > Denominator | 9/4 (more than 1 whole) |
| Mixed number | Whole number + fraction | 2¼ (= 9/4) |
| Decimal fraction | Written with decimal point | 0.375 (= 3/8) |

---

### ➗ Operations with Fractions

#### Adding Fractions — Same Denominator

```
1/5 + 2/5 = 3/5

(Keep the denominator. Add the numerators.)
```

#### Adding Fractions — Different Denominator

**Intuition:** You can't add halves and thirds directly — you need a common unit.

Convert to the same denominator (find the Least Common Denominator):

```
1/2 + 1/3

LCD of 2 and 3 = 6

1/2 = 3/6
1/3 = 2/6

3/6 + 2/6 = 5/6
```

#### Multiplying Fractions

**Intuition:** "Half of a half is a quarter." 1/2 × 1/2 = 1/4

**Rule:** Multiply numerators. Multiply denominators.

```
3/4 × 2/5 = (3×2)/(4×5) = 6/20 = 3/10
```

#### Dividing Fractions

**Rule:** Flip the second fraction and multiply. ("Keep, Change, Flip")

```
3/4 ÷ 2/5 = 3/4 × 5/2 = 15/8
```

---

### Converting Fractions to Decimals and Percentages

```
Fraction → Decimal:    Divide numerator by denominator
3/4 = 3 ÷ 4 = 0.75

Decimal → Percentage:  Multiply by 100
0.75 × 100 = 75%

Fraction → Percentage: Divide then multiply by 100
3/4 → 0.75 → 75%
```

> 🚀 **Data Science Connection:**
> Fractions appear constantly in:
> - **Probability:** P(rain) = 3/7 (it rained 3 of the last 7 days)
> - **Proportions:** 45% of users clicked the button → 45/100 = 0.45
> - **Ratios:** Train-test split = 80/20 = 4:1

---

## 💪 POWERS (EXPONENTS) — Repeated Multiplication

### 💡 Intuition

Just as multiplication is a shortcut for repeated addition, **powers** are a shortcut for repeated multiplication.

Adding 5 three times: 5 + 5 + 5 = 15
Multiplying 5 three times: 5 × 5 × 5 = 125

Instead of writing 5 × 5 × 5, we write: **5³** (read as "5 to the power of 3" or "5 cubed")

```
5³ = 5 × 5 × 5 = 125

    ↑ base
      ↑ exponent (how many times to multiply the base by itself)
```

### Visual Understanding

```
5¹ = 5                        (5, once)
5² = 5 × 5 = 25               (5, twice) — "5 squared"
5³ = 5 × 5 × 5 = 125          (5, three times) — "5 cubed"
5⁴ = 5 × 5 × 5 × 5 = 625     (5, four times)
```

```
GROWTH PATTERN:
                    *
               *    |
          *    |    |
     *    |    |    |
*    |    |    |    |
──────────────────────
5¹  5²   5³  5⁴   5⁵
5   25  125  625  3125
```

Notice how it **grows explosively**. This is why we call it "exponential growth."

---

### 📋 Laws of Exponents (Rules for Working with Powers)

These rules are used constantly in Data Science. Learn them!

| Rule | Formula | Example |
|------|---------|---------|
| Multiplication rule | aᵐ × aⁿ = aᵐ⁺ⁿ | 2³ × 2⁴ = 2⁷ = 128 |
| Division rule | aᵐ ÷ aⁿ = aᵐ⁻ⁿ | 2⁵ ÷ 2² = 2³ = 8 |
| Power of a power | (aᵐ)ⁿ = aᵐˣⁿ | (2³)² = 2⁶ = 64 |
| Zero exponent | a⁰ = 1 | 5⁰ = 1, 100⁰ = 1 |
| Negative exponent | a⁻ⁿ = 1/aⁿ | 2⁻³ = 1/8 = 0.125 |
| Fraction exponent | a^(1/n) = ⁿ√a | 9^(1/2) = √9 = 3 |

> ⚠️ **Common Mistake:** 2³ does NOT mean 2 × 3 = 6. It means 2 × 2 × 2 = 8.
> The exponent tells you HOW MANY TIMES to multiply, not WHAT to multiply by.

---

### 📊 Worked Example — Powers

**Problem:** Your investment grows by doubling every year. You start with ₹10,000. How much do you have after 5 years?

Starting amount × 2⁵ = ₹10,000 × 32 = **₹3,20,000**

```
Year 0: ₹10,000 × 2⁰ = ₹10,000
Year 1: ₹10,000 × 2¹ = ₹20,000
Year 2: ₹10,000 × 2² = ₹40,000
Year 3: ₹10,000 × 2³ = ₹80,000
Year 4: ₹10,000 × 2⁴ = ₹1,60,000
Year 5: ₹10,000 × 2⁵ = ₹3,20,000
```

---

## √ ROOTS — The Reverse of Powers

### 💡 Intuition

If powers are "squaring," then roots are "un-squaring."

**Question:** What number, multiplied by itself, gives 25?

Answer: 5, because 5 × 5 = 25.

So **√25 = 5** (The square root of 25 is 5)

---

### Types of Roots

```
√9   = 3   (square root — what number × itself = 9?)
∛8   = 2   (cube root — what number × itself × itself = 8?)
⁴√16 = 2   (fourth root)
```

> **Memory Trick:** The root symbol (√) looks like a check mark going under the ground —
> it's asking "what's buried underground that, when brought up to the surface and squared, gives this number?"

---

### Calculating Square Roots

| Number | Square Root | Calculation |
|--------|-------------|-------------|
| √1     | 1           | 1 × 1 = 1   |
| √4     | 2           | 2 × 2 = 4   |
| √9     | 3           | 3 × 3 = 9   |
| √16    | 4           | 4 × 4 = 16  |
| √25    | 5           | 5 × 5 = 25  |
| √100   | 10          | 10 × 10 = 100 |
| √2     | ≈ 1.414     | Irrational (goes on forever) |

> 🚀 **Data Science Connection:**
> Square roots appear in:
> - **Standard deviation:** σ = √(variance) — the most used formula in statistics
> - **RMSE:** Root Mean Squared Error — the most used error metric in ML
> - **Euclidean distance:** √((x₁-x₂)² + (y₁-y₂)²) — used in clustering and KNN

---

## 📈 LOGARITHMS — The Inverse of Exponents

### 💡 Intuition — The Most Important Math Concept in Data Science

> **Logarithms are the reverse of exponents.**

If 2³ = 8 asks: "What do I get when I multiply 2 three times?"

Then log₂(8) = 3 asks: "How many times do I need to multiply 2 to get 8?"

```
Exponent:    2³ = 8      → "Start with 2, multiply 3 times, get 8"
Logarithm: log₂(8) = 3  → "Start with 8, how many times was 2 multiplied?"
```

They are **two sides of the same coin**.

---

### Visual Understanding — The Stairs Analogy

Imagine a staircase where each step **multiplies** the previous height by 2:

```
Step 0: 1      (= 2⁰)
Step 1: 2      (= 2¹)
Step 2: 4      (= 2²)
Step 3: 8      (= 2³)
Step 4: 16     (= 2⁴)
Step 5: 32     (= 2⁵)
Step 6: 64     (= 2⁶)
Step 7: 128    (= 2⁷)
Step 8: 256    (= 2⁸)
Step 9: 512    (= 2⁹)
Step 10: 1024  (= 2¹⁰)
```

- **Exponent** tells you: "You're on step 10. The height is 1024."
- **Logarithm** tells you: "The height is 1024. Which step is that?" → Step 10

log₂(1024) = 10

---

### The Three Common Logarithms

| Notation | Base | Meaning | Used In |
|----------|------|---------|---------|
| log₂(x) or lb(x) | 2 | How many times multiply 2 to get x | Information theory, binary |
| log₁₀(x) or log(x) | 10 | How many times multiply 10 to get x | General math, pH scale |
| ln(x) | e ≈ 2.718 | Natural logarithm | Calculus, ML, probability |

> 📌 **Remember:** When you see just "log" with no base written, it usually means log₁₀ in general math, but **ln** (natural log) in Data Science and ML.

---

### Reading Logarithms

**Rule:** log_base(result) = exponent

```
log₁₀(100) = 2    because 10² = 100
log₁₀(1000) = 3   because 10³ = 1000
log₂(16) = 4      because 2⁴ = 16
log₂(32) = 5      because 2⁵ = 32
ln(e) = 1         because e¹ = e
ln(e²) = 2        because e² = e²
```

---

### 📋 Laws of Logarithms

| Rule | Formula | Example |
|------|---------|---------|
| Product rule | log(A × B) = log(A) + log(B) | log(6) = log(2) + log(3) |
| Division rule | log(A/B) = log(A) − log(B) | log(5/2) = log(5) − log(2) |
| Power rule | log(Aⁿ) = n × log(A) | log(8) = 3 × log(2) |
| Log of 1 | log(1) = 0 | Always, any base |
| Log of base | log_b(b) = 1 | log₂(2) = 1 |

> 🧠 **Why log(1) = 0?** Because anything to the power 0 = 1. So b⁰ = 1, meaning log_b(1) = 0.

---

### 🌍 Real-Life Applications of Logarithms

**1. Earthquake Richter Scale:**
Each step on the Richter scale is 10× stronger.
An earthquake of magnitude 6 is 10× stronger than magnitude 5.
Magnitude 7 is 100× stronger than magnitude 5.
The scale is **logarithmic** (base 10).

**2. Decibels (Sound):**
0 dB = silence
30 dB = whisper
60 dB = normal conversation
90 dB = lawnmower
Each 10 dB increase = 10× more intensity.

**3. pH Scale (Chemistry):**
pH 7 = neutral water
pH 6 = 10× more acidic than pH 7
pH 5 = 100× more acidic than pH 7

---

### 🚀 Data Science Applications of Logarithms

Logarithms are everywhere in Data Science:

1. **Log Loss (Cross-Entropy):** The primary loss function for classification models.
   ```
   Loss = −[y × log(p) + (1−y) × log(1−p)]
   ```

2. **Information Theory & Entropy:** Measuring uncertainty in data.
   ```
   H = −Σ p(x) × log₂(p(x))
   ```

3. **Log-normal distributions:** Many real-world quantities (income, stock prices) follow log-normal distributions.

4. **Feature scaling:** Taking the log of highly skewed data compresses extreme values.
   ```
   If salary data has outliers like ₹10 crore,
   log(₹10 crore) brings it closer to normal range.
   ```

5. **Neural networks:** Activation functions and softmax use exponentials (inverse of log).

---
---

# 0.1.3 — Functions & Graphs

---

## 💡 Intuition — What Is a Function?

> **A function is a machine that takes an input and produces exactly one output.**

Imagine a vending machine:

```
┌────────────────────────────────┐
│         FUNCTION MACHINE       │
│                                │
│  Input → [  Process  ] → Output │
│                                │
│  Press A1 → [ gives ] → Pepsi  │
│  Press B2 → [ gives ] → Water  │
│  Press C3 → [ gives ] → Juice  │
└────────────────────────────────┘
```

**Rules of a function:**
1. Every valid input must give **exactly one** output
2. The same input always gives the **same output** (predictable, consistent)

---

### Function Notation

We write functions using the notation: **f(x) = ...**

Read as: "f of x" or "the function f applied to x"

```
f(x) = 2x + 1

This means: "Take any number x, multiply it by 2, then add 1."

f(3) = 2(3) + 1 = 7        Input 3 → Output 7
f(5) = 2(5) + 1 = 11       Input 5 → Output 11
f(0) = 2(0) + 1 = 1        Input 0 → Output 1
f(-2) = 2(-2) + 1 = -3     Input -2 → Output -3
```

---

### Domain and Range

| Term | Meaning | Example |
|------|---------|---------|
| **Domain** | All valid inputs (x values) | For f(x) = √x, domain is x ≥ 0 (can't square root negative numbers in real numbers) |
| **Range** | All possible outputs (y values) | For f(x) = x², range is y ≥ 0 (x² is never negative) |

> 📌 **Think of it as:** Domain = ingredients you can put in the machine. Range = what can come out of the machine.

---

### Plotting a Function

To draw a function, pick several input values (x), compute the output values (y), and plot the points.

**Example:** f(x) = 2x + 1

| x (Input) | f(x) = 2x + 1 (Output) | Point |
|-----------|------------------------|-------|
| -2 | 2(-2) + 1 = -3 | (-2, -3) |
| -1 | 2(-1) + 1 = -1 | (-1, -1) |
| 0  | 2(0) + 1 = 1   | (0, 1)  |
| 1  | 2(1) + 1 = 3   | (1, 3)  |
| 2  | 2(2) + 1 = 5   | (2, 5)  |

**Graph (ASCII):**

```
y
|              * (2,5)
5 |           *
4 |          
3 |       * (1,3)
2 |        
1 | * (0,1)
0 |__________________ x
-1|* (-1,-1)
-2|
-3| * (-2,-3)
```

This creates a **straight line** — because f(x) = 2x + 1 is a **linear function**.

---

### Types of Functions in Data Science

| Function Type | Formula | Graph Shape | Data Science Use |
|--------------|---------|-------------|-----------------|
| Linear | y = mx + b | Straight line | Linear regression |
| Quadratic | y = x² | Parabola (U shape) | Loss functions |
| Exponential | y = aˣ | Rapid growth | Compound interest, ML training curves |
| Logarithmic | y = log(x) | Slow growth | Feature transformation |
| Sigmoid | y = 1/(1+e⁻ˣ) | S-curve | Logistic regression, neural network activation |
| Step | y = 0 or 1 | Staircase | Decision boundaries |

> 🚀 **Data Science Connection:**
> Every Machine Learning model is a **function**.
> Linear Regression: ŷ = β₀ + β₁x₁ + β₂x₂ + ...
> The model takes features (inputs) and produces a prediction (output).
> Training the model = finding the right function that fits the data.

---
---

# 0.1.4 — Sigma Notation (Σ)

---

## 💡 Intuition — What Is Sigma?

> **Sigma notation is just a shortcut for writing long addition problems.**

Imagine you have 100 students in a class, and you want to add all their exam scores.

Without sigma: score₁ + score₂ + score₃ + ... + score₁₀₀

That's tedious to write! Instead we use:

```
  100
  Σ   scoreᵢ
 i=1
```

Read as: "Sum of all scoreᵢ, where i goes from 1 to 100"

---

## 🔤 Anatomy of Sigma Notation

```
   n           ← Upper limit (stop here)
   Σ  xᵢ      ← What we are summing
  i=1          ← Lower limit (start here), i is the index
```

**Breaking it down:**

| Part | Meaning |
|------|---------|
| Σ | "Sum up" (the Greek capital letter Sigma = S for Sum) |
| i = 1 | Start the counter at 1 |
| n at top | Stop the counter at n |
| xᵢ | The i-th value of x |

---

## 📊 Worked Examples

### Example 1 — Sum of a Simple List

Add the numbers 3, 5, 7, 9, 11.

The list: x₁ = 3, x₂ = 5, x₃ = 7, x₄ = 9, x₅ = 11

```
  5
  Σ  xᵢ  =  x₁ + x₂ + x₃ + x₄ + x₅
 i=1
         =  3 + 5 + 7 + 9 + 11
         =  35
```

### Example 2 — Computing a Mean (Average)

Mean formula:

```
        n
  x̄ = (Σ xᵢ) / n
       i=1
```

Read as: "x-bar equals the sum of all x values divided by n"

If scores = [70, 80, 90, 60, 100] and n = 5:

```
Σ xᵢ = 70 + 80 + 90 + 60 + 100 = 400

x̄ = 400 / 5 = 80
```

The average score is 80.

### Example 3 — Variance Formula (Preview)

Variance uses Sigma notation:

```
        n
σ² = (1/n) Σ (xᵢ - x̄)²
           i=1
```

This says: "For each value, find how far it is from the mean, square it, sum all of those, then divide by n."

> 🚀 **Data Science Connection:**
> Sigma (Σ) appears in nearly every formula in statistics:
> - Mean: x̄ = Σxᵢ/n
> - Variance: σ² = Σ(xᵢ - x̄)²/n
> - Linear regression: ŷ = Σβᵢxᵢ
> - Cost functions: Loss = Σ(actual - predicted)²

---
---

# 0.1.5 — Scientific Notation

---

## 💡 Intuition — Dealing With Very Large (and Very Small) Numbers

Data Science often deals with enormous numbers:
- 1 billion internet users = 1,000,000,000
- 1 trillion bytes in a terabyte = 1,000,000,000,000
- 0.00000001 = probability of a rare event

These are hard to read and write. **Scientific notation** is a compact way to express them.

---

## 📐 How Scientific Notation Works

**Format:** a × 10ⁿ, where 1 ≤ a < 10

**Rule:** Move the decimal point until you have a number between 1 and 10. Count how many places you moved.

```
1,000,000,000 = 1.0 × 10⁹
                 ↑
                 Decimal moved 9 places to the left
                 So the exponent is +9

0.00000001    = 1.0 × 10⁻⁸
                 ↑
                 Decimal moved 8 places to the right
                 So the exponent is -8
```

### 📊 Examples

| Standard Notation | Scientific Notation | Explanation |
|-------------------|--------------------|-|
| 6,500,000 | 6.5 × 10⁶ | Moved decimal 6 places left |
| 0.0045 | 4.5 × 10⁻³ | Moved decimal 3 places right |
| 3,890,000,000 | 3.89 × 10⁹ | Moved decimal 9 places left |
| 0.000000072 | 7.2 × 10⁻⁸ | Moved decimal 8 places right |

---

### 🚀 Data Science Connection

Scientific notation matters when:
- Working with huge datasets (billions of rows)
- Computing very small probabilities (P = 1.2 × 10⁻⁷)
- Understanding machine learning learning rates (α = 0.001 = 1 × 10⁻³)
- Storing numbers in computers (floating-point representation)

```python
# Python handles scientific notation naturally
p = 1.2e-7      # means 1.2 × 10⁻⁷
large = 6.5e9   # means 6.5 × 10⁹ = 6,500,000,000
```

---
---

# 0.1.6 — Exponents & Logarithmic Growth

---

## 💡 Intuition — Two Opposite Patterns of Growth

In nature and data, things can grow in fundamentally different ways.

### Pattern 1: Exponential Growth — Explosive

Imagine a rumor spreading in school:
- Day 1: 1 person knows
- Day 2: 2 people know (it doubled)
- Day 3: 4 people know
- Day 4: 8 people know
- Day 5: 16 people know
- Day 10: 512 people know
- Day 20: 524,288 people know

The **rate of increase** itself is increasing. It gets faster and faster.

```
Day:     1    2    3    4    5    6    7
People:  1    2    4    8   16   32   64

Graph:
64 |                               *
32 |                          *
16 |                    *
 8 |              *
 4 |        *
 2 |    *
 1 | *
   |________________________
   1    2    3    4    5    6    7
```

---

### Pattern 2: Logarithmic Growth — Fast Then Slow

Imagine learning a new skill (like Data Science):
- Week 1: Learn a huge amount (basics are fast!)
- Week 2: Learn a lot more
- Week 5: Still learning, but slower
- Week 10: Progress is steady but not explosive
- Week 50: Still improving, but increments are smaller

The growth is **fast at first, then slows down**.

```
Weeks:    1    5   10   20   50  100
Progress: 10   35  50   60   70   75 (hypothetical units)

Graph:
80 |                               * *
70 |                    *
50 |            *
35 |      *
10 | *
   |_________________________________
   1    5   10   20   50  100
```

---

### The Connection

```
Exponential Growth: y = bˣ         (x is the exponent)
Logarithmic Growth: y = log_b(x)  (x is inside the log)
```

**They are inverse functions** — logarithm reverses exponential, and vice versa.

---

### 🌍 Real-World Examples of Each

| Exponential Growth | Logarithmic Growth |
|--------------------|-------------------|
| COVID spread (early stages) | Learning a language (fast start, then plateaus) |
| Compound interest | Sound loudness perception |
| Population growth | Ranking algorithms (more clicks = diminishing rank boost) |
| Neural network weight magnitudes | Compression algorithms |
| Viral social media posts | Weber-Fechner law (human perception) |

> 🚀 **Data Science Connection:**
>
> **Exponential appears in:**
> - Softmax function (neural networks)
> - Exponential distribution (time between events)
> - Exponential moving average (time series)
>
> **Logarithm appears in:**
> - Cross-entropy loss (classification)
> - Log-likelihood (statistical inference)
> - Log scaling of features (data preprocessing)
> - Decision tree information gain (entropy)

---
---

# 0.1.7 — Linear vs Exponential Growth

---

## 💡 Intuition — The Most Important Growth Concept for Data Science

Understanding how things grow is fundamental to every analysis.

### Linear Growth — Adding a Constant Amount

A linear pattern grows by **adding the same amount** each time.

**Example:** You save ₹500 every month.

```
Month 0: ₹0
Month 1: ₹500
Month 2: ₹1,000
Month 3: ₹1,500
Month 4: ₹2,000

Growth: +₹500 each month (constant addition)

Formula: Savings = 500 × months
```

---

### Exponential Growth — Multiplying by a Constant Amount

An exponential pattern grows by **multiplying by the same factor** each time.

**Example:** You invest ₹500 and it doubles every year.

```
Year 0: ₹500
Year 1: ₹1,000  (×2)
Year 2: ₹2,000  (×2)
Year 3: ₹4,000  (×2)
Year 4: ₹8,000  (×2)

Growth: ×2 each year (constant multiplication)

Formula: Investment = 500 × 2^years
```

---

### Visual Comparison

```
         Amount
         (₹)
         |
  8000 --|                              E *
         |
  6000 --|
         |
  4000 --|                          E *
         |
  2000 --|       L *   L *   L * E *
         |  L *
     0 --|_________________________________  Years
             0     1     2     3     4

L = Linear Growth (₹500/year savings)
E = Exponential Growth (₹500 doubling investment)
```

At first they look similar. But over time, exponential **dwarfs** linear.

---

### 📊 Side-by-Side Comparison Table

| Property | Linear | Exponential |
|----------|--------|-------------|
| How it grows | By a fixed addition | By a fixed multiplication |
| Formula | y = mx + b | y = a × bˣ |
| Graph shape | Straight line | Curved, accelerating |
| Real-world examples | Constant salary, distance at constant speed | Compound interest, viral spread, bacteria |
| After 10 steps from 1 | 10 (adding 1 each step) | 1024 (doubling each step) |
| After 20 steps from 1 | 20 | 1,048,576 |

---

### 🚀 Data Science Applications

**Why this matters in Data Science:**

1. **Feature engineering:** If your feature grows exponentially (like website traffic), taking its log makes it linear — easier for models to learn.

2. **Model training:** Neural network loss decreases **exponentially** during training (drops fast first, then slowly).

3. **Complexity:** Algorithm time complexity:
   - Linear: O(n) — good!
   - Exponential: O(2ⁿ) — terrible! Grows too fast!

4. **Data:** Stock prices, user counts, virus spreads follow exponential patterns.

5. **Probability:** Certain probability functions involve exponentials (Poisson, Exponential distribution).

---
---

# 📝 Practice Questions — Chapter 0.1

---

## 🟢 Beginner Practice (10 Questions)

**Q1.** Calculate: 15 + 4 × 3 − 2

**Q2.** Solve for x: x + 7 = 20

**Q3.** Solve for x: 3x = 24

**Q4.** Convert the fraction 3/4 to a decimal and a percentage.

**Q5.** What is 2⁴?

**Q6.** What is √49?

**Q7.** Write 7,500,000 in scientific notation.

**Q8.** Calculate log₁₀(10,000).

**Q9.** Using sigma notation, find Σ from i=1 to i=4 of (2i). In other words, compute: 2(1) + 2(2) + 2(3) + 2(4).

**Q10.** A shop has a 25% discount on a ₹800 item. What is the discounted price?

---

## 🟡 Intermediate Practice (10 Questions)

**Q11.** Calculate using BODMAS: 4 + 3 × (2² − 1) ÷ 3

**Q12.** Solve for x: 5x − 8 = 2x + 7

**Q13.** Simplify: (3/4) + (5/6) (show all steps)

**Q14.** Using laws of exponents, simplify: x⁵ × x⁻² ÷ x³

**Q15.** If f(x) = x² − 3x + 2, find f(0), f(1), f(2), f(5).

**Q16.** A bacteria culture doubles every 3 hours. Starting with 500 bacteria, how many are there after 12 hours?

**Q17.** Convert 0.000045 to scientific notation.

**Q18.** If log₂(x) = 6, what is x?

**Q19.** Compute the mean of [12, 15, 18, 21, 24] using Sigma notation (write the formula and compute).

**Q20.** What is ln(e³)? Explain why.

---

## 🔴 Advanced Practice (10 Questions)

**Q21.** Prove that log(A × B) = log(A) + log(B) using the definition of logarithms.

**Q22.** The formula for compound interest is: A = P(1 + r/n)^(nt). If P = ₹10,000, r = 0.08, n = 12, t = 5, calculate A.

**Q23.** Solve for x: 3^(2x+1) = 27

**Q24.** If f(x) = e^x and g(x) = ln(x), prove that f(g(x)) = x and g(f(x)) = x (showing they are inverses).

**Q25.** The Sigmoid function is σ(x) = 1/(1 + e^(-x)). Calculate σ(0), σ(1), σ(-1), σ(10), σ(-10). What pattern do you notice?

**Q26.** Compute: Σ from i=1 to 5 of i² (that is, 1² + 2² + 3² + 4² + 5²)

**Q27.** If a dataset has values [2, 4, 8, 16, 32, 64], is this linear or exponential growth? What is the common ratio?

**Q28.** The natural log of a number is 5. What is the number (to 3 decimal places)?

**Q29.** Simplify the expression: log₂(64) − log₂(8) + log₂(4)

**Q30.** A machine learning model's loss decreases as: Loss(n) = 100 × (0.9)ⁿ, where n is the training epoch. What is the loss after 10, 20, and 50 epochs?

---

## 🚀 Data Science Scenario Questions (5 Questions)

**DS1.** You have a dataset with 1,000,000 rows. You want to compute the sum of a column called "revenue" which has values x₁, x₂, ..., x₁,₀₀₀,₀₀₀. Write this sum using sigma notation, then write the formula for the mean revenue.

**DS2.** In logistic regression, the sigmoid function is used: σ(z) = 1/(1 + e^(-z)). A model computes z = 2.5 for a particular user. What is σ(2.5)? What does this represent in terms of probability? (Hint: e^(-2.5) ≈ 0.082)

**DS3.** A social media post has 10 views on Day 1. Views double each day for 2 weeks. On which day do views first exceed 1 million? Use logarithms to solve.

**DS4.** In data preprocessing, you take the log of "salary" column to reduce skewness. A salary of ₹1,00,000 becomes log₁₀(100000) = ___. A salary of ₹10,00,000 becomes log₁₀(1000000) = ___. What is the ratio of original values? What is the ratio of log-transformed values? Why does this help?

**DS5.** The mean formula is x̄ = (1/n) × Σxᵢ. A data scientist has 5 measurements: [10.2, 9.8, 10.5, 9.7, 10.3]. Compute the mean using sigma notation. Then determine the deviation of each point from the mean (xᵢ − x̄) and verify that Σ(xᵢ − x̄) = 0 (this is always true).

---
---

# ✅ Solutions — Chapter 0.1

---

### Beginner Solutions

**A1.** 15 + 4 × 3 − 2
- BODMAS: Multiplication first → 4 × 3 = 12
- Then left to right: 15 + 12 − 2 = 27 − 2 = **25**

**A2.** x + 7 = 20
- Subtract 7 from both sides: x = 20 − 7 = **13**

**A3.** 3x = 24
- Divide both sides by 3: x = 24 ÷ 3 = **8**

**A4.** 3/4 to decimal: 3 ÷ 4 = **0.75**. To percentage: 0.75 × 100 = **75%**

**A5.** 2⁴ = 2 × 2 × 2 × 2 = **16**

**A6.** √49 = **7** (because 7 × 7 = 49)

**A7.** 7,500,000 = **7.5 × 10⁶** (decimal moved 6 places left)

**A8.** log₁₀(10,000) = log₁₀(10⁴) = **4** (because 10⁴ = 10,000)

**A9.** 2(1) + 2(2) + 2(3) + 2(4) = 2 + 4 + 6 + 8 = **20**

**A10.** 25% of ₹800 = 0.25 × 800 = ₹200 discount. Discounted price = 800 − 200 = **₹600**

---

### Intermediate Solutions

**A11.** 4 + 3 × (2² − 1) ÷ 3
- Brackets: 2² = 4, then 4 − 1 = 3 → 4 + 3 × (3) ÷ 3
- Multiplication/Division left to right: 3 × 3 = 9, then 9 ÷ 3 = 3
- Addition: 4 + 3 = **7**

**A12.** 5x − 8 = 2x + 7
- Move 2x to left: 5x − 2x − 8 = 7 → 3x − 8 = 7
- Add 8 to both sides: 3x = 15
- Divide by 3: x = **5**
- Check: 5(5) − 8 = 17, and 2(5) + 7 = 17 ✅

**A13.** (3/4) + (5/6)
- LCD of 4 and 6 = 12
- 3/4 = 9/12, 5/6 = 10/12
- 9/12 + 10/12 = **19/12** (or 1 and 7/12 or 1.583...)

**A14.** x⁵ × x⁻² ÷ x³
- Multiply: x⁵ × x⁻² = x^(5-2) = x³
- Divide: x³ ÷ x³ = x^(3-3) = x⁰ = **1**

**A15.** f(x) = x² − 3x + 2
- f(0) = 0 − 0 + 2 = **2**
- f(1) = 1 − 3 + 2 = **0**
- f(2) = 4 − 6 + 2 = **0**
- f(5) = 25 − 15 + 2 = **12**

**A16.** Doubles every 3 hours, so in 12 hours there are 12/3 = 4 doublings.
- 500 × 2⁴ = 500 × 16 = **8,000 bacteria**

**A17.** 0.000045 = **4.5 × 10⁻⁵** (decimal moved 5 places right)

**A18.** log₂(x) = 6 means 2⁶ = x, so x = **64**

**A19.** Values: [12, 15, 18, 21, 24], n = 5
- x̄ = (1/5) × Σxᵢ = (1/5) × (12 + 15 + 18 + 21 + 24)
- Σxᵢ = 90
- x̄ = 90/5 = **18**

**A20.** ln(e³) = **3**, because ln and e are inverses. ln(eˣ) = x always. So ln(e³) = 3.

---

### Advanced Solutions

**A21.** Proof that log(A × B) = log(A) + log(B):
- Let log(A) = m, so A = 10ᵐ
- Let log(B) = n, so B = 10ⁿ
- A × B = 10ᵐ × 10ⁿ = 10^(m+n)
- log(A × B) = log(10^(m+n)) = m + n = log(A) + log(B) ✅

**A22.** A = P(1 + r/n)^(nt) = 10000(1 + 0.08/12)^(12×5)
= 10000(1.006667)^60 ≈ 10000 × 1.4898 ≈ **₹14,898**

**A23.** 3^(2x+1) = 27 = 3³
- Since bases are equal: 2x + 1 = 3
- 2x = 2 → x = **1**
- Check: 3^(2(1)+1) = 3³ = 27 ✅

**A24.** f(g(x)) = e^(ln(x)) = x (property of inverse functions, e and ln cancel)
g(f(x)) = ln(e^x) = x (same property in reverse)

**A25.** σ(x) = 1/(1 + e^(-x)):
- σ(0) = 1/(1+1) = **0.5**
- σ(1) = 1/(1+e⁻¹) ≈ 1/1.368 ≈ **0.731**
- σ(-1) = 1/(1+e¹) ≈ 1/3.718 ≈ **0.269**
- σ(10) ≈ 1/(1+0.0000454) ≈ **0.9999546** (very close to 1)
- σ(-10) ≈ 1/(1+22026) ≈ **0.0000454** (very close to 0)
- **Pattern:** Sigmoid always outputs between 0 and 1. Large positive x → close to 1. Large negative x → close to 0. This is why it's used for probability outputs in classification!

**A26.** Σ i² from i=1 to 5 = 1² + 2² + 3² + 4² + 5² = 1 + 4 + 9 + 16 + 25 = **55**

**A27.** [2, 4, 8, 16, 32, 64] — **Exponential growth**. Each term is 2× the previous. Common ratio = **2**.

**A28.** ln(x) = 5 → x = e⁵ ≈ **148.413**

**A29.** log₂(64) − log₂(8) + log₂(4)
= log₂(64/8) + log₂(4)   [using division rule for logs]
= log₂(8) + log₂(4)
= 3 + 2 = **5**
(Or: log₂(64 × 4/8) = log₂(32) = **5**)

**A30.** Loss(n) = 100 × (0.9)ⁿ
- n=10: 100 × 0.9¹⁰ = 100 × 0.3487 ≈ **34.87**
- n=20: 100 × 0.9²⁰ = 100 × 0.1216 ≈ **12.16**
- n=50: 100 × 0.9⁵⁰ = 100 × 0.00515 ≈ **0.515**

---

### Data Science Scenario Solutions

**DS1.** Sum of revenue column:
```
          1,000,000
Total =     Σ       xᵢ
           i=1
         
Mean = (1/1,000,000) × Total = Total/1,000,000
```
In Python: `total = df['revenue'].sum()` and `mean = df['revenue'].mean()`

**DS2.** σ(2.5) = 1/(1 + e^(-2.5)) = 1/(1 + 0.082) = 1/1.082 ≈ **0.924**
This means the model predicts a **92.4% probability** that this user belongs to the positive class (e.g., will click, will buy, will churn). Sigmoid converts any number to a probability between 0 and 1.

**DS3.** Views(n) = 10 × 2^n > 1,000,000
- 10 × 2^n > 1,000,000
- 2^n > 100,000
- n > log₂(100,000)
- log₂(100,000) = log₁₀(100,000)/log₁₀(2) = 5/0.301 ≈ 16.6
- So on **Day 17**, views first exceed 1 million.
- Verify: 10 × 2¹⁷ = 10 × 131,072 = 1,310,720 > 1,000,000 ✅

**DS4.** log₁₀(100,000) = **5**. log₁₀(1,000,000) = **6**.
- Original ratio: 1,000,000/100,000 = **10** (10× difference)
- Log ratio: 6/5 = **1.2** (tiny difference!)
- This "squeezes" extreme values together, removing the distorting effect of very high salaries on model training.

**DS5.** Values: [10.2, 9.8, 10.5, 9.7, 10.3], n = 5
- Σxᵢ = 10.2 + 9.8 + 10.5 + 9.7 + 10.3 = 50.5
- x̄ = 50.5/5 = **10.1**
- Deviations: (10.2−10.1), (9.8−10.1), (10.5−10.1), (9.7−10.1), (10.3−10.1)
  = 0.1, −0.3, 0.4, −0.4, 0.2
- Sum of deviations: 0.1 − 0.3 + 0.4 − 0.4 + 0.2 = **0** ✅

> 📌 **Key Insight:** The deviations from the mean always sum to zero. This is not a coincidence — it's a mathematical property of the mean. It means the mean is the "center of gravity" of the data.

---
---

# 💼 Interview Questions — Chapter 0.1

---

### Theory Questions

**IQ1.** "What is the difference between an expression and an equation? Give a data science example of each."

**Answer:** An expression (like `2x + 3`) combines variables and numbers but has no equals sign — it just describes a computation. An equation (like `2x + 3 = 11`) states equality and can be solved. In data science: a feature transformation `log(x) + 1` is an expression; the mean formula `x̄ = Σxᵢ/n` is an equation.

---

**IQ2.** "Why do data scientists use logarithms on skewed data?"

**Answer:** Logarithms compress large values and stretch small values. If a salary column has values between ₹20,000 and ₹1 crore, the huge values will dominate models. After log transformation, ₹20,000 → ~4.3 and ₹1 crore → ~8 — the scale is now comparable. This helps linear models and neural networks train more stably and prevents single extreme values from distorting the model.

---

**IQ3.** "What does the sigma symbol (Σ) mean in a formula? Give an example."

**Answer:** Σ means "sum up." It's shorthand for adding a series of values. For example, the mean formula x̄ = (1/n)Σxᵢ means: add up all x values (that's the Σ part), then divide by the count n.

---

### Practical Questions

**IQ4.** "Without a calculator, estimate log₁₀(500)."

**Answer:** log₁₀(100) = 2 and log₁₀(1000) = 3. So log₁₀(500) is between 2 and 3, closer to 3 since 500 is closer to 1000. A good estimate is ≈ **2.7** (actual: 2.699).

---

**IQ5.** "If I have features with values like [1, 10, 100, 1000, 10000], should I use them directly or transform them? Why?"

**Answer:** Transform them. The raw values span 4 orders of magnitude. Machine learning models (especially those using gradient descent) struggle when features have such different scales. After log₁₀ transformation: [0, 1, 2, 3, 4] — evenly spaced, much easier for the model. This is called **log scaling** or **log transformation**.

---

### Data Science Interview Questions

**IQ6.** "Explain the difference between linear and exponential growth in the context of model training."

**Answer:** In model training, loss typically shows **exponential decay** — it drops quickly in early epochs and slows significantly later. This is why learning rate schedules (like exponential decay of learning rate) are used. If we plotted loss on a log scale, exponential decay would appear as a **straight line** — this is how practitioners quickly identify if training is progressing correctly.

---

**IQ7.** "You're computing a weighted average of model predictions from 3 models. The weights are 0.5, 0.3, and 0.2. The predictions are 0.7, 0.6, and 0.8. What is the ensemble prediction?"

**Answer:**
```
Ensemble = 0.5 × 0.7 + 0.3 × 0.6 + 0.2 × 0.8
         = 0.35 + 0.18 + 0.16
         = 0.69
```
Note: The weights must sum to 1 (0.5 + 0.3 + 0.2 = 1) — this is a valid weighted average.

---
---

# 📌 Chapter 0.1 — End of Chapter Summary

---

## ⚡ Quick Revision Notes

- **Arithmetic:** The four operations (+, −, ×, ÷). Always follow BODMAS order.
- **Variables:** Letters (like x, μ, σ) are just boxes holding numbers. Don't be intimidated.
- **Algebra:** Solving equations means isolating the variable. Balance the equation on both sides.
- **Fractions:** Part/Whole. Top = numerator, Bottom = denominator. Convert to decimals by dividing.
- **Powers:** Repeated multiplication. aⁿ means multiply a by itself n times.
- **Roots:** Reverse of powers. √x asks "what squared gives x?"
- **Logarithms:** Reverse of exponents. log_b(x) asks "what power of b gives x?"
- **Sigma (Σ):** Shorthand for "add everything up." Used in every statistical formula.
- **Functions:** A rule that takes input and gives exactly one output. Every ML model is a function.
- **Linear growth:** Adds a constant. Grows in a straight line.
- **Exponential growth:** Multiplies by a constant. Grows explosively.

---

## 📐 Formula Sheet

| Concept | Formula | Notes |
|---------|---------|-------|
| BODMAS order | B→O→D→M→A→S | Brackets, Orders, Division, Multiplication, Addition, Subtraction |
| Algebra golden rule | Do same to both sides | Keeps equation balanced |
| Fraction to decimal | Numerator ÷ Denominator | 3/4 = 0.75 |
| Power definition | aⁿ = a × a × ... × a (n times) | |
| Power rule (multiply) | aᵐ × aⁿ = a^(m+n) | |
| Power rule (divide) | aᵐ ÷ aⁿ = a^(m-n) | |
| Zero power | a⁰ = 1 | Always |
| Negative power | a⁻ⁿ = 1/aⁿ | Flip to denominator |
| Fractional power | a^(1/n) = ⁿ√a | |
| Log definition | log_b(bⁿ) = n | |
| Log product | log(AB) = log(A) + log(B) | |
| Log quotient | log(A/B) = log(A) − log(B) | |
| Log power | log(Aⁿ) = n·log(A) | |
| Sigma notation | Σ from i=1 to n of xᵢ | Sum all x values |
| Mean | x̄ = (1/n)Σxᵢ | |
| Linear function | y = mx + b | m = slope, b = y-intercept |
| Exponential function | y = a × bˣ | b > 1 for growth, 0 < b < 1 for decay |
| Scientific notation | a × 10ⁿ | 1 ≤ a < 10 |

---

## 🎯 Key Takeaways

1. **Mathematics is a language.** It's not scary magic — it's a precise way to describe the world.

2. **Every data science formula is just arithmetic.** Even the most complex ML formula is operations (+, −, ×, ÷) on numbers.

3. **Variables are just labels for numbers.** μ, σ, β — they're just boxes with fancy labels.

4. **Logarithms compress scale.** They turn multiplicative differences into additive differences. This is incredibly useful in data science.

5. **BODMAS is non-negotiable.** Getting this wrong means getting the wrong answer every time.

6. **Sigma notation is everywhere.** Every statistical formula uses it. Make it your friend.

7. **Functions are the foundation of ML.** Every model maps inputs to outputs — that's a function.

8. **Exponential vs linear growth matters.** Data, algorithms, and models all have growth characteristics.

---

## ⚠️ Common Mistakes

| Mistake | Why It Happens | How to Fix |
|---------|---------------|------------|
| 2³ = 6 (wrong) | Confusing power with multiplication | 2³ = 2×2×2 = 8. Power = repeated multiplication. |
| Not following BODMAS | Habitual left-to-right reading | Always check: are there brackets? Exponents? Do those first. |
| log(A + B) = log(A) + log(B) | Mixing up log rules | WRONG! Only log(A×B) = log(A) + log(B). |
| a/0 = 0 (wrong) | Guessing | Division by zero is UNDEFINED. Always check denominators! |
| Adding different denominators | Forgetting to find common denominator | Find LCD first, then convert fractions, then add |
| -x² = (-x)² (wrong) | Order of operations | -x² = -(x²). Only (-x)² equals x² (positive). |

---

## 🎓 Interview Preparation Checklist

✅ Can you explain what a logarithm is to a non-mathematician?
✅ Can you compute log₁₀(1000) without a calculator?
✅ Can you explain why log transformation helps with skewed data?
✅ Can you write and explain the mean formula using sigma notation?
✅ Can you distinguish between linear and exponential growth patterns?
✅ Can you evaluate f(3) if f(x) = 2x² + 3x − 1?
✅ Can you solve a two-step linear equation?
✅ Can you convert between fractions, decimals, and percentages?
✅ Do you understand why a⁰ = 1?
✅ Can you explain how functions relate to Machine Learning models?

---

## 🧩 Mini Quiz (Test Yourself)

**Q1.** What does BODMAS stand for?
**Q2.** True or False: log(A + B) = log(A) + log(B)
**Q3.** What is 3⁻² equal to?
**Q4.** If f(x) = 3x + 5, what is f(4)?
**Q5.** Write 0.0000072 in scientific notation.
**Q6.** What is √144?
**Q7.** What does Σ (sigma) mean in a formula?
**Q8.** Is population growth typically linear or exponential?
**Q9.** What is log₂(64)?
**Q10.** A number raised to the power 0 equals what?

---

## Mini Quiz Solutions

1. Brackets, Orders, Division, Multiplication, Addition, Subtraction
2. **False.** log(A×B) = log(A) + log(B). The product rule, not the sum rule.
3. 3⁻² = 1/3² = 1/9 ≈ 0.111
4. f(4) = 3(4) + 5 = 12 + 5 = **17**
5. 7.2 × 10⁻⁶
6. **12** (12 × 12 = 144)
7. Σ means "sum up" — add all the values that follow
8. **Exponential** (each person can multiply to create more)
9. log₂(64) = **6** (because 2⁶ = 64)
10. **1** (any non-zero number to the power 0 = 1)

---

## 🚀 Real Data Science Applications of Chapter 0.1

| Concept | Where It Appears in Data Science |
|---------|----------------------------------|
| Algebra | Solving for model parameters; regression equations |
| Fractions | Probability values; train/test splits; ratios |
| Powers | Variance computation; square losses; distance metrics |
| Square roots | Standard deviation; RMSE; Euclidean distance |
| Logarithms | Cross-entropy loss; feature transformation; information theory; log-likelihood |
| Sigma notation | Mean, variance, regression; virtually every formula |
| Functions | Every ML model is a function mapping features → output |
| Exponential | Softmax; exponential smoothing; model decay |
| Linear growth | Linear regression; linear time complexity |
| Scientific notation | Representing very small probabilities; very large datasets |

---

> 🎉 **Congratulations on completing Chapter 0.1!**
>
> You now have the mathematical vocabulary needed to understand everything that follows.
> The concepts in this chapter appear in **every single topic** in the rest of this course.
> Keep this chapter as a reference — you'll come back to it often.
>
> **Next Up:** Chapter 0.2 — Data Fundamentals
> (What is data? Types of variables? Structured vs unstructured? Population vs sample?)

---

*— End of Chapter 0.1 —*

---

> **Document Info:**
> Course: Data Science — Statistics & Probability (2026 Edition)
> Chapter: 0.1 — Mathematics for Data Science
> Level: Absolute Beginner → Intermediate
> Topics Covered: 7 major topics
> Practice Questions: 35 total (10 beginner + 10 intermediate + 10 advanced + 5 DS scenarios)
> Interview Questions: 7 (theory + practical + DS-specific)
