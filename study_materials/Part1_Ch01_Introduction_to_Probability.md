# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 1 — Probability Foundations
# Chapter 1.1 — Introduction to Probability

> **Chapter Goal:** Build an unshakeable intuition for probability from scratch — what it means, how to calculate it, and why it's the mathematical language of uncertainty used everywhere in Data Science.

---

## 🗺️ Chapter Roadmap
```
1.1.1  Randomness & Uncertainty
1.1.2  Experiments, Outcomes & Sample Spaces
1.1.3  Events
1.1.4  Probability — Definition & Rules
1.1.5  Complement Rule
1.1.6  Addition Rule
```

---

# 1.1.1 — Randomness & Uncertainty

## 💡 Intuition

> **Probability is the science of measuring uncertainty.**

Every day you face uncertain situations:
- "Will it rain today?"
- "Will my team win this match?"
- "Will this customer click the ad?"
- "Is this email spam?"
- "Will this patient's test come back positive?"

You can't know the answer with 100% certainty before it happens. But you're not completely in the dark either — you can **quantify your uncertainty**.

**Probability is that quantification.** It's a number between 0 and 1 that tells you: "How likely is this to happen?"

```
0 ──────────────────────────────────── 1
│                    │                 │
Impossible        50-50          Certain
(Never happens)  (Coin flip)  (Always happens)

P = 0.0    P = 0.25    P = 0.50    P = 0.75    P = 1.0
  Never      Unlikely     50-50      Likely      Always
```

## 🌍 Real-Life Examples

| Situation | Probability | Meaning |
|-----------|------------|---------|
| Sun rises tomorrow | P ≈ 1.0 | Virtually certain |
| Rain in Mumbai in July | P ≈ 0.85 | Very likely |
| Coin lands heads | P = 0.50 | Equal chance |
| Rolling a 6 on a die | P = 1/6 ≈ 0.167 | Unlikely |
| Drawing a heart from a deck | P = 13/52 = 0.25 | Unlikely |
| Rolling a 7 on a single die | P = 0 | Impossible |

## 🚀 Why Probability Matters in Data Science

Every ML prediction is a probability:
- "This email is spam with 94% probability" → P(spam) = 0.94
- "This customer will churn with 23% probability" → P(churn) = 0.23
- "This image contains a cat with 99.8% probability" → P(cat) = 0.998

> 📌 **Core Insight:** Machine Learning models don't produce "yes/no" answers. They produce **probability scores**. Understanding probability IS understanding machine learning output.

---

# 1.1.2 — Experiments, Outcomes & Sample Spaces

## 📖 Definitions

### Experiment (or Trial)
> Any process that produces a result we can't predict with certainty.

| Experiment | Uncertain because... |
|-----------|---------------------|
| Tossing a coin | Don't know if heads or tails |
| Rolling a die | Don't know which number |
| Drawing a card | Don't know which card |
| A user visits website | Don't know if they buy |
| Weather tomorrow | Don't know if it rains |

### Outcome (or Sample Point)
> One single specific result of one experiment.

- Coin flip outcomes: **H** (heads) or **T** (tails)
- Die roll outcomes: **1, 2, 3, 4, 5, or 6**
- Card draw outcome: **7 of Hearts**, **King of Spades**, etc.

### Sample Space (S or Ω)
> The **complete list of ALL possible outcomes** of an experiment.

```
Sample space for a coin flip:
S = {H, T}

Sample space for a single die roll:
S = {1, 2, 3, 4, 5, 6}

Sample space for flipping 2 coins:
S = {HH, HT, TH, TT}

Sample space for a card drawn from full deck:
S = {A♠, 2♠, 3♠, ..., K♠, A♥, ..., K♥, A♦, ..., K♦, A♣, ..., K♣}
    (52 outcomes total)
```

> 🎯 **Key Rule:** Every possible outcome MUST be in the sample space. Nothing can be left out.

## 📊 Visual: Two-Coin Sample Space

```
First Coin → H          First Coin → T
              │                        │
    ┌─────────┴─────────┐    ┌─────────┴─────────┐
    │                   │    │                   │
Second→H          Second→T  Second→H          Second→T
  (HH)             (HT)      (TH)              (TT)

S = {HH, HT, TH, TT}   ← 4 equally likely outcomes
```

---

# 1.1.3 — Events

## 📖 Definition

> An **event** is any subset of the sample space — a collection of one or more outcomes that we care about.

```
Sample space: S = {1, 2, 3, 4, 5, 6}  (die roll)

Event A = "Rolling an even number" = {2, 4, 6}
Event B = "Rolling greater than 4"  = {5, 6}
Event C = "Rolling a 3"             = {3}       ← simple event (one outcome)
Event D = "Rolling a 7"             = {}        ← impossible event (empty set)
Event E = "Rolling any number"      = {1,2,3,4,5,6} ← certain event (= S)
```

## 🎨 Venn Diagram (ASCII)

```
Sample Space S = {1, 2, 3, 4, 5, 6}
╔══════════════════════════════╗
║   S                         ║
║   ┌──────────┐               ║
║   │  A       │  5            ║
║   │ 2  4  6  │               ║
║   │      ┌───┼──────┐       ║
║   └──────┼───┘      │       ║
║      1 3 │   B      │       ║
║          │  {5, 6}  │       ║
║          └──────────┘       ║
╚══════════════════════════════╝

A = {2,4,6} (even numbers)
B = {5,6}   (greater than 4)
A ∩ B = {6} (even AND greater than 4)
A ∪ B = {2,4,5,6} (even OR greater than 4)
```

### Types of Events

| Type | Description | Example |
|------|-------------|---------|
| Simple event | Exactly one outcome | P(rolling exactly 3) |
| Compound event | Two or more outcomes | P(rolling even) |
| Complementary | Everything NOT in event | P(not rolling even) |
| Mutually exclusive | Events that can't both occur | P(rolling 2 AND rolling 5 simultaneously) |
| Independent | One doesn't affect the other | Two separate coin flips |

---

# 1.1.4 — Probability: Definition & Calculation

## 📖 Classical Probability (Equally Likely Outcomes)

> When all outcomes in the sample space are equally likely:

$$P(A) = \frac{\text{Number of outcomes in event A}}{\text{Total number of outcomes in sample space}}$$

| Symbol | Meaning |
|--------|---------|
| P(A) | Probability of event A (a number between 0 and 1) |
| Numerator | Count of outcomes where A happens |
| Denominator | Total possible outcomes |

## 📊 Worked Examples

**Example 1 — Rolling a Die:**
What is the probability of rolling an even number?

```
Event A = {2, 4, 6}   → 3 outcomes in A
Sample space S = {1, 2, 3, 4, 5, 6} → 6 total outcomes

P(A) = 3/6 = 1/2 = 0.5 = 50%
```

**Example 2 — Drawing a Card:**
What is the probability of drawing a King from a standard 52-card deck?

```
Event = {K♠, K♥, K♦, K♣}  → 4 outcomes
Total outcomes = 52

P(King) = 4/52 = 1/13 ≈ 0.077 = 7.7%
```

**Example 3 — Two Dice:**
Rolling two dice, what's the probability the sum = 7?

```
Total outcomes = 6 × 6 = 36

Outcomes that sum to 7:
(1,6), (2,5), (3,4), (4,3), (5,2), (6,1) → 6 outcomes

P(sum=7) = 6/36 = 1/6 ≈ 0.167
```

## 📐 Empirical Probability (Frequency-Based)

When you can't list all equally-likely outcomes, use **observed data**:

$$P(A) \approx \frac{\text{Number of times A occurred}}{\text{Total number of trials}}$$

**Example:** A batsman hit 42 sixes in 180 matches.
P(hitting a six in a match) ≈ 42/180 ≈ 0.233

> 📌 **Law of Large Numbers:** As you run more and more trials, the empirical probability converges to the true probability. With only 10 flips, you might get 7 heads (70%). With 10,000 flips, you'll be very close to 50%.

## 📋 The Three Fundamental Rules of Probability

| Rule | Statement | Formula |
|------|-----------|---------|
| Non-negativity | Probability is never negative | P(A) ≥ 0 |
| Certainty | The probability of the entire sample space = 1 | P(S) = 1 |
| Additivity | If A and B can't both happen at once, P(A or B) = P(A) + P(B) | P(A∪B) = P(A)+P(B) if A∩B=∅ |

---

# 1.1.5 — Complement Rule

## 💡 Intuition

> **If you know the probability of something happening, you automatically know the probability of it NOT happening.**

```
P(event happens) + P(event does NOT happen) = 1

Because: Either it happens or it doesn't. Those two cover ALL possibilities.
```

## 📖 Formula

$$P(A^c) = 1 - P(A)$$

| Symbol | Meaning |
|--------|---------|
| A^c (or Ā or A') | The complement of A — "A does NOT happen" |
| P(A^c) | Probability A does NOT happen |
| 1 - P(A) | Since A and not-A together = certainty (= 1) |

## 📊 Worked Examples

**Example 1:**
P(raining tomorrow) = 0.3. What is P(NOT raining)?
```
P(not rain) = 1 - 0.3 = 0.7
```

**Example 2:**
P(rolling a 6) = 1/6. What is P(NOT rolling a 6)?
```
P(not 6) = 1 - 1/6 = 5/6 ≈ 0.833
```

**Example 3 — The Complement Trick:**
What is P(at least one Head in 3 coin flips)?

Direct calculation: count HHH, HHT, HTH, HTT, THH, THT, TTH (7 outcomes)
P(at least one head) = 7/8

**Easier via complement:**
P(NO heads at all) = P(TTT) = 1/8
P(at least one head) = 1 - 1/8 = 7/8 ✅

> 🎯 **Exam Tip:** When a problem says "at least one," use the complement. P(at least one X) = 1 - P(zero X).

## 🚀 Data Science Application

In classification models:
- P(user churns) = 0.23
- P(user does NOT churn) = 1 - 0.23 = 0.77
- The model always outputs both implicitly

In anomaly detection:
- P(transaction is normal) = 0.999
- P(transaction is fraudulent) = 1 - 0.999 = 0.001

---

# 1.1.6 — Addition Rule

## 💡 Intuition

> **The probability that event A OR event B occurs.**

There are two cases:
1. A and B **cannot** both happen at the same time (mutually exclusive)
2. A and B **can** both happen at the same time

## 📖 Case 1 — Mutually Exclusive Events (No Overlap)

> Events A and B are **mutually exclusive** if they cannot both occur simultaneously.

```
Die roll: A = {1, 2} and B = {5, 6}
Can A and B both happen in one roll? NO → mutually exclusive

P(A or B) = P(A) + P(B) = 2/6 + 2/6 = 4/6 = 2/3
```

**Formula:**
$$P(A \cup B) = P(A) + P(B) \quad \text{[only if mutually exclusive]}$$

## 📖 Case 2 — Non-Mutually Exclusive (With Overlap)

> If A and B CAN both occur, simply adding P(A) + P(B) **double-counts** the overlap.

```
Die roll:
A = "even number" = {2, 4, 6}
B = "greater than 4" = {5, 6}
A ∩ B = {6} ← both even AND > 4

If we add: P(A) + P(B) = 3/6 + 2/6 = 5/6
But we counted {6} TWICE! So subtract the overlap once.

P(A ∪ B) = P(A) + P(B) - P(A ∩ B)
          = 3/6 + 2/6 - 1/6 = 4/6 = 2/3
```

**Formula (General Addition Rule):**
$$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$

| Symbol | Meaning |
|--------|---------|
| A ∪ B | "A OR B" (union) — at least one occurs |
| A ∩ B | "A AND B" (intersection) — both occur |
| P(A ∩ B) | Subtracted to remove double-counting |

## 📊 Visual: Why We Subtract

```
  ┌─────────────────────────────────┐
  │           S                    │
  │   ┌──────────────┐             │
  │   │  A    │  A∩B │  B   │     │
  │   │ (3,5) │  (6) │(5,6) │     │
  │   └──────────────┘             │
  │                                │
  └─────────────────────────────────┘

Counting A ∪ B:
  All of A + All of B - the overlap (counted twice)
  = P(A) + P(B) - P(A∩B)
```

## 📊 Worked Examples

**Example 1 — Cards:**
Drawing one card: P(Heart OR King)?

```
P(Heart) = 13/52
P(King)  = 4/52
P(King of Hearts) = 1/52  ← overlap

P(Heart OR King) = 13/52 + 4/52 - 1/52 = 16/52 = 4/13 ≈ 0.308
```

**Example 2 — Marketing:**
60% of customers are female. 40% are under 30. 25% are both female AND under 30.
P(female OR under 30)?

```
P(Female) = 0.60
P(Under 30) = 0.40
P(Both) = 0.25

P(Female OR Under 30) = 0.60 + 0.40 - 0.25 = 0.75
```

---

# 📝 Practice Questions — Chapter 1.1

## 🟢 Beginner (10 Questions)

**Q1.** A bag contains 4 red and 6 blue balls. What is the probability of drawing a red ball?

**Q2.** A die is rolled. Find P(odd number).

**Q3.** Write the sample space for flipping 3 coins.

**Q4.** P(rain) = 0.45. What is P(no rain)?

**Q5.** A deck of cards is shuffled. Find P(drawing an Ace).

**Q6.** Are "rolling a 2" and "rolling a 5" mutually exclusive? Why?

**Q7.** A die is rolled. Find P(number > 4).

**Q8.** A class has 12 girls and 18 boys. P(selecting a girl)?

**Q9.** If P(A) = 0.3 and A and B are mutually exclusive with P(B) = 0.4, find P(A or B).

**Q10.** In a bag of 20 balls: 5 red, 7 green, 8 yellow. Find P(green OR yellow).

## 🟡 Intermediate (10 Questions)

**Q11.** Two dice are rolled. Find P(sum = 9).

**Q12.** From a deck of 52 cards, find P(drawing a face card OR a diamond).

**Q13.** P(A) = 0.6, P(B) = 0.5, P(A ∩ B) = 0.3. Find P(A ∪ B).

**Q14.** A coin is flipped 3 times. Find P(at least 2 heads).

**Q15.** In an IPL match, P(team wins batting first) = 0.55, P(team wins when chasing) = 0.45. Are these complementary? Explain.

**Q16.** A traffic light is red for 45 seconds, green for 35 seconds, and yellow for 5 seconds per 85-second cycle. What is P(arriving at a green light)?

**Q17.** P(at least one defective item in a batch) = 0.15. What is P(no defective items)?

**Q18.** A card is drawn. Find P(not a King AND not a Queen).

**Q19.** From {1, 2, 3, ..., 20}, a number is chosen randomly. Find P(multiple of 3 OR multiple of 5).

**Q20.** Events A and B: P(A) = 0.4, P(B) = 0.3. If mutually exclusive, find P(A∪B). If P(A∩B) = 0.12, find P(A∪B).

## 🔴 Advanced (10 Questions)

**Q21.** Prove that for any event A: 0 ≤ P(A) ≤ 1.

**Q22.** Events A, B, C are mutually exclusive and exhaustive with P(A) = 0.2, P(B) = 0.35. Find P(C).

**Q23.** A and B are events where P(A) = 0.5, P(B) = 0.7, P(A∪B) = 0.85. Are A and B mutually exclusive? Are they independent? Find P(A∩B).

**Q24.** Roll three dice. Find P(at least one 6).

**Q25.** From a class of 30 students: 18 like cricket, 15 like football, 8 like both. A student is selected randomly. Find P(likes cricket but NOT football).

**Q26.** A bag has 5 red, 3 blue, 2 green balls. Two balls are drawn with replacement. Find P(both red).

**Q27.** If P(A∩B) = 0 and P(A) = 0.4, P(B) = 0.3, prove that A and B are mutually exclusive.

**Q28.** Extend the addition rule to three events: write P(A∪B∪C).

**Q29.** If P(A) = 0.6 and P(B|A) = 0.4 (probability of B given A), find P(A∩B).

**Q30.** From a 52-card deck, 3 cards are drawn. Find P(at least one ace).

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** A spam classifier flags 8% of all emails as spam. Find P(email is NOT spam). If 500 emails arrive, approximately how many are spam?

**DS2.** A recommendation system shows P(user clicks ad A) = 0.12 and P(user clicks ad B) = 0.08. If the ads can both be clicked and P(both clicked) = 0.02, find P(at least one ad is clicked).

**DS3.** A quality control model predicts "defective" with P = 0.03. Use the complement to find P(passing 10 items with no defects). [Hint: multiply probabilities since each item is independent: 0.97^10]

**DS4.** Write the sample space for a binary classifier's possible outcomes when predicting on a single instance (True Positive, False Positive, True Negative, False Negative). What must these probabilities sum to?

**DS5.** A/B testing: Version A of a webpage converts 5% of visitors. Version B converts 7%. P(a visitor, shown one version randomly, converts)?

---

# ✅ Solutions — Chapter 1.1

**A1.** P(red) = 4/10 = 2/5 = 0.4

**A2.** Odd = {1,3,5}. P(odd) = 3/6 = 1/2.

**A3.** S = {HHH, HHT, HTH, HTT, THH, THT, TTH, TTT} — 8 outcomes.

**A4.** P(no rain) = 1 - 0.45 = 0.55.

**A5.** 4 aces in 52 cards. P(Ace) = 4/52 = 1/13 ≈ 0.077.

**A6.** Yes, mutually exclusive. A single die shows one number only — it cannot show both 2 and 5 simultaneously.

**A7.** {5, 6} → P = 2/6 = 1/3.

**A8.** P(girl) = 12/30 = 2/5 = 0.4.

**A9.** Mutually exclusive: P(A or B) = 0.3 + 0.4 = 0.7.

**A10.** P(green) = 7/20, P(yellow) = 8/20. Green and yellow are mutually exclusive (a ball can't be both). P(green OR yellow) = 7/20 + 8/20 = 15/20 = 3/4.

**A11.** Sums to 9: (3,6),(4,5),(5,4),(6,3) = 4 outcomes. P = 4/36 = 1/9.

**A12.** P(Face card) = 12/52, P(Diamond) = 13/52, P(Face AND Diamond) = 3/52. P(Face OR Diamond) = 12/52 + 13/52 - 3/52 = 22/52 = 11/26.

**A13.** P(A∪B) = 0.6 + 0.5 - 0.3 = 0.8.

**A14.** At least 2 heads: {HHT, HTH, THH, HHH} = 4/8 = 1/2.

**A15.** Yes — they are complementary if we assume the team always either bats first or chases (0.55 + 0.45 = 1.0). In practice P(win) depends on many conditions so these aren't truly complementary across all conditions.

**A16.** P(green) = 35/85 = 7/17 ≈ 0.412.

**A17.** P(no defective) = 1 - 0.15 = 0.85.

**A18.** P(King) = 4/52, P(Queen) = 4/52. P(King OR Queen) = 8/52 (mutually exclusive). P(neither) = 1 - 8/52 = 44/52 = 11/13.

**A19.** Multiples of 3 in 1-20: {3,6,9,12,15,18} = 6. Multiples of 5: {5,10,15,20} = 4. Both: {15} = 1. P = (6+4-1)/20 = 9/20.

**A20.** Mutually exclusive: 0.4+0.3=0.7. With overlap: 0.4+0.3-0.12=0.58.

**A21.** P(A) = |A|/|S| ≥ 0 since counts are non-negative. |A| ≤ |S| so |A|/|S| ≤ 1. Therefore 0 ≤ P(A) ≤ 1.

**A22.** Mutually exclusive and exhaustive means they cover all of S: P(A)+P(B)+P(C)=1. P(C)=1-0.2-0.35=0.45.

**A23.** P(A∩B) = P(A)+P(B)-P(A∪B) = 0.5+0.7-0.85 = 0.35. Not mutually exclusive (P(A∩B)≠0). Not independent either: P(A)×P(B) = 0.5×0.7 = 0.35 = P(A∩B). Actually they ARE independent!

**A24.** P(no 6 on one die) = 5/6. P(no 6 on all three) = (5/6)³ = 125/216. P(at least one 6) = 1 - 125/216 = 91/216 ≈ 0.421.

**A25.** P(cricket only) = P(cricket) - P(both) = 18/30 - 8/30 = 10/30 = 1/3.

**A26.** P(red) = 5/10 = 1/2. With replacement both draws independent. P(both red) = (1/2)×(1/2) = 1/4.

**A27.** P(A∩B) = 0 is the definition of mutually exclusive. ✅

**A28.** P(A∪B∪C) = P(A)+P(B)+P(C)-P(A∩B)-P(A∩C)-P(B∩C)+P(A∩B∩C).

**A29.** P(A∩B) = P(A) × P(B|A) = 0.6 × 0.4 = 0.24 (this is the multiplication rule, previewed here).

**A30.** P(no ace) = (48/52)×(47/51)×(46/50) = 103776/132600 ≈ 0.7826. P(at least one ace) = 1 - 0.7826 ≈ 0.2174.

**DS1.** P(not spam) = 0.92. Spam emails ≈ 500 × 0.08 = 40.

**DS2.** P(A∪B) = 0.12 + 0.08 - 0.02 = 0.18. 18% chance at least one is clicked.

**DS3.** P(all 10 pass) = 0.97^10 ≈ 0.7374. About 73.7% chance a batch of 10 has zero defects.

**DS4.** S = {TP, FP, TN, FN}. P(TP)+P(FP)+P(TN)+P(FN) = 1.

**DS5.** If visitor is randomly assigned (50/50): P(convert) = 0.5×0.05 + 0.5×0.07 = 0.025+0.035 = 0.06 = 6%.

---

# 💼 Interview Questions

**IQ1.** "What is the difference between mutually exclusive and independent events?"
Mutually exclusive = cannot both happen (P(A∩B)=0). Independent = knowing one occurred tells you nothing about the other (P(A∩B)=P(A)×P(B)). Mutually exclusive events with non-zero probability are actually DEPENDENT (knowing A happened tells you B didn't).

**IQ2.** "A model predicts P(fraud)=0.001 for a transaction. Is this the same as saying 1 in 1000 transactions is fraud?" Yes — empirically. But individual probability is an expression of uncertainty about one transaction, not a frequency guarantee.

**IQ3.** "How do you interpret a probability of 0.73 in a logistic regression output?" The model estimates a 73% chance of belonging to the positive class. The decision threshold (usually 0.5) determines the final class label.

---

# 📌 Chapter Summary

## ⚡ Quick Revision
- Probability = number between 0 (impossible) and 1 (certain)
- Experiment → Outcomes → Sample Space S
- Event = subset of sample space
- Classical: P(A) = favorable/total
- Empirical: P(A) = observed frequency/total trials
- Complement: P(Aᶜ) = 1 - P(A)
- Addition (mutually exclusive): P(A∪B) = P(A) + P(B)
- Addition (general): P(A∪B) = P(A) + P(B) - P(A∩B)

## 📐 Formula Sheet
| Formula | Name |
|---------|------|
| P(A) = |A|/|S| | Classical probability |
| 0 ≤ P(A) ≤ 1 | Non-negativity |
| P(S) = 1 | Certainty |
| P(Aᶜ) = 1 - P(A) | Complement rule |
| P(A∪B) = P(A)+P(B) if A∩B=∅ | Addition (ME) |
| P(A∪B) = P(A)+P(B)-P(A∩B) | Addition (general) |

## ⚠️ Common Mistakes
| Mistake | Correction |
|---------|-----------|
| P(A)+P(B) when events overlap | Must subtract P(A∩B) |
| Confusing mutually exclusive with independent | ME = can't coexist; Independent = don't influence each other |
| Probability > 1 | Impossible. Always re-check your calculation |
| "At least one" computed directly | Use complement: 1 - P(none) |

---
*Chapter 1.1 Complete | Next: Chapter 1.2 — Conditional Probability*
