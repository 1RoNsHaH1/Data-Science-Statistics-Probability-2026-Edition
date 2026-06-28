# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 1 — Probability Foundations
# Chapter 1.2 — Conditional Probability

> **Chapter Goal:** Learn how new information changes probabilities — the idea behind spam filters, medical diagnosis, recommendation systems, and almost every ML algorithm.

---

## 🗺️ Chapter Roadmap
```
1.2.1  What Is Conditional Probability?
1.2.2  The Formula — P(A|B)
1.2.3  Independence
1.2.4  Multiplication Rule
1.2.5  The Law of Total Probability
```

---

# 1.2.1 — What Is Conditional Probability?

## 💡 Intuition

> **Conditional probability asks: "How does knowing B happened change the probability of A?"**

**Story — The Medical Test:**

Imagine you go to a hospital for a routine checkup. The doctor says you tested positive for a rare disease.

Your immediate question: **"What is the probability I actually HAVE the disease?"**

The answer is NOT simply "the test is 99% accurate → 99% probability." Why? Because whether you have the disease is **conditional** on multiple factors — your age, whether you were in a high-risk group, whether you had symptoms, etc.

**This is conditional probability:** The probability of event A (you have the disease), **given that** event B occurred (the test was positive).

---

**Story — Cricket:**

If you ask: "What is the probability Virat Kohli scores a century?"
→ P(century) ≈ 0.20 (career average)

But now add the condition: "It's a high-pressure World Cup match against Pakistan."
→ P(century | high-pressure Pakistan match) might be much higher based on historical data

**The new information (condition) changes the probability.**

## 🌍 Real-Life Examples

| Question | Event A | Condition B | Why B Changes Things |
|----------|---------|-------------|---------------------|
| Will user click ad? | Clicks ad | Already bought similar product | Prior purchase signals interest |
| Will email be spam? | Is spam | Contains "FREE MONEY NOW" | These words indicate spam |
| Will stock rise? | Stock rises | Positive earnings report just released | Context changes likelihood |
| Will patient recover? | Recovers | Patient is under 40 and healthy | Age and health affect outcome |

---

# 1.2.2 — The Formula P(A | B)

## 📖 Definition

$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$

Read as: "The probability of A **given** B"

| Symbol | Meaning |
|--------|---------|
| P(A\|B) | Probability of A occurring, given that B has occurred |
| P(A ∩ B) | Probability that both A and B occur |
| P(B) | Probability that B occurs (must be > 0) |
| \| (pipe symbol) | "Given that" or "conditional on" |

## 💡 Why Does This Formula Make Sense?

When we learn B happened, the **sample space shrinks** to only the outcomes where B occurred.

```
BEFORE knowing B:
Full sample space S:
╔══════════════════════════════╗
║   1  2  3  4  5  6           ║
╚══════════════════════════════╝

AFTER learning B = {even number} = {2, 4, 6} occurred:
New (reduced) sample space = {2, 4, 6}
╔═══════════════╗
║   2    4    6  ║
╚═══════════════╝

Now: P(rolling > 4 | rolling even) = P({6}) / P({2,4,6}) = (1/6)/(3/6) = 1/3
(Within the even numbers, only 6 is > 4)
```

## 📊 Worked Examples

**Example 1 — Cards:**
A card is drawn. Given it's a red card, what is P(it's a King)?

```
B = "red card" → P(B) = 26/52 = 1/2
A = "King"     → Red Kings = King of Hearts + King of Diamonds = 2
A ∩ B = "red King" → P(A∩B) = 2/52

P(King | red) = P(King ∩ red) / P(red)
              = (2/52) / (26/52)
              = 2/26 = 1/13
```

Intuitive check: There are 26 red cards. 2 are Kings. So P(King | red) = 2/26 = 1/13 ✅

---

**Example 2 — Disease Screening:**
```
Population: 10,000 people
Disease prevalence: 100 people have disease (P(D) = 0.01)
Test: 90 of the 100 sick people test positive (P(+|D) = 0.90)
       500 of 9,900 healthy people also test positive (false positives)

Calculations:
P(D ∩ +) = 90/10000 = 0.009
P(+) = 590/10000 = 0.059

P(D | +) = 0.009 / 0.059 ≈ 0.153 = 15.3%

SHOCKING RESULT: Even with a 90% accurate test,
if you test positive, there's only a ~15% chance you actually have the disease!
Why? Because the disease is rare — most positives are FALSE positives.
```

> 🧠 **This is the base rate fallacy** — ignoring how rare the disease is. We'll formalize this with Bayes' Theorem in Ch 1.3.

---

**Example 3 — Two Dice:**
Two dice rolled. Given the sum is 8, what is P(one die shows 3)?

```
All outcomes summing to 8:
(2,6), (3,5), (4,4), (5,3), (6,2) = 5 outcomes

Outcomes with a 3 AND sum=8: (3,5) and (5,3) = 2 outcomes

P(one die = 3 | sum = 8) = 2/5 = 0.4
```

---

# 1.2.3 — Independence

## 💡 Intuition

> **Two events are independent if knowing one occurred gives zero information about the other.**

**Dependent (knowing helps):**
- P(rain | dark clouds) > P(rain). Dark clouds are informative.
- P(spam | contains "winner") > P(spam). The word is informative.

**Independent (knowing is useless):**
- P(coin flip heads | previous flip was heads). The coin has no memory.
- P(rolling 6 on a die | the previous roll was 1). Each roll is independent.

## 📖 Mathematical Definition

Events A and B are **independent** if and only if:

$$P(A \cap B) = P(A) \times P(B)$$

Equivalently:
$$P(A \mid B) = P(A) \quad \text{(knowing B doesn't change A's probability)}$$

## 📊 Example

Are "rolling a 6" and "the next roll being odd" independent?

```
P(roll 6) = 1/6
P(roll odd) = 3/6 = 1/2
P(both) = 0 (they're separate rolls, so P(first=6 AND second=odd) = (1/6)(1/2) = 1/12)

Check: P(A)×P(B) = (1/6)×(1/2) = 1/12 = P(A∩B) ✅ Independent
```

## 🚀 Data Science: Independence Assumption

**Naïve Bayes classifier** assumes all features are independent given the class:

P(feature1 ∩ feature2 | class) = P(feature1 | class) × P(feature2 | class)

This simplifies calculation enormously. Even though features are often NOT truly independent in reality, Naïve Bayes works surprisingly well.

---

# 1.2.4 — Multiplication Rule

## 📖 Derivation

From the definition P(A|B) = P(A∩B)/P(B), we can rearrange:

$$P(A \cap B) = P(B) \times P(A \mid B)$$

Or equivalently:

$$P(A \cap B) = P(A) \times P(B \mid A)$$

**For independent events:** P(A∩B) = P(A) × P(B)

## 📊 Worked Examples

**Example 1 — Drawing cards WITHOUT replacement:**
Draw 2 cards. P(both aces)?

```
P(first ace) = 4/52
P(second ace | first was ace) = 3/51  ← now 3 aces left, 51 cards left

P(both aces) = (4/52) × (3/51) = 12/2652 = 1/221 ≈ 0.00452
```

**Example 2 — Manufacturing:**
Factory line has 3 independent checks. Each catches defects with P = 0.95.
P(ALL THREE catch a defect)?

```
P(all three catch it) = 0.95 × 0.95 × 0.95 = 0.95³ ≈ 0.857
P(at least one misses) = 1 - 0.857 = 0.143
```

**Example 3 — Chain of Events:**
P(user opens email) = 0.30
P(user clicks link | opened email) = 0.25
P(user purchases | clicked link) = 0.10

P(user opens email AND clicks AND purchases)?
```
= 0.30 × 0.25 × 0.10 = 0.0075 = 0.75%
```
(This is exactly how email marketing conversion funnels are calculated.)

---

# 1.2.5 — Law of Total Probability

## 💡 Intuition

> **If the paths to an event are mutually exclusive and exhaustive, the total probability is the weighted sum across all paths.**

**Story — Two Factories:**
Company has two factories producing light bulbs:
- Factory A produces 60% of bulbs, 2% are defective
- Factory B produces 40% of bulbs, 5% are defective

What is the overall P(defective)?

```
Path 1: Bulb from A AND defective: 0.60 × 0.02 = 0.012
Path 2: Bulb from B AND defective: 0.40 × 0.05 = 0.020

P(defective) = 0.012 + 0.020 = 0.032 = 3.2%
```

## 📖 Formula

If B₁, B₂, ..., Bₙ are mutually exclusive and exhaustive (they partition S):

$$P(A) = \sum_{i=1}^{n} P(A \mid B_i) \times P(B_i)$$

This is called the **Law of Total Probability**.

## 📊 Worked Example

Email classification:
- 70% of emails are legitimate (L), of which 1% contain the word "free"
- 30% of emails are spam (S), of which 60% contain the word "free"

P(email contains "free")?

```
P(free) = P(free|L)×P(L) + P(free|S)×P(S)
        = 0.01 × 0.70 + 0.60 × 0.30
        = 0.007 + 0.180
        = 0.187 = 18.7%
```

> 🚀 This is the denominator in Bayes' Theorem (which we tackle in Ch 1.3)!

---

# 📝 Practice Questions — Chapter 1.2

## 🟢 Beginner (10 Questions)

**Q1.** P(A∩B) = 0.12, P(B) = 0.4. Find P(A|B).

**Q2.** A bag has 3 red and 7 blue balls. A ball is drawn. Given it's not blue, what's P(red)?

**Q3.** Two dice rolled. Given sum > 9, find P(sum = 11).

**Q4.** P(A) = 0.5, P(B) = 0.4. If independent, find P(A∩B).

**Q5.** A card is drawn. Find P(Queen | face card).

**Q6.** Are two coin flips independent? Justify.

**Q7.** P(user buys | user visits product page) = 0.08. P(user visits product page) = 0.30. Find P(user visits AND buys).

**Q8.** In a class, 40% are girls. 25% of girls wear glasses. What % of class are girls with glasses?

**Q9.** P(A|B) = 0.6, P(B) = 0.5. Find P(A∩B).

**Q10.** P(A) = 0.3, P(B) = 0.4, P(A∩B) = 0.12. Are A and B independent?

## 🟡 Intermediate (10 Questions)

**Q11.** Factory A makes 70% of product, 3% defective. Factory B makes 30%, 7% defective. Find P(defective).

**Q12.** From Q11: Given a product is defective, what is P(it came from Factory A)?

**Q13.** P(A|B) = 0.7, P(B|A) = 0.5, P(A) = 0.4. Find P(B).

**Q14.** In a hospital: 60% patients have fever, 30% have cough, 20% have both. Find P(fever | cough).

**Q15.** Three machines each independently work correctly with P = 0.95. System works only if all three work. Find P(system works).

**Q16.** P(rain) = 0.4. P(traffic jam | rain) = 0.8. P(traffic jam | no rain) = 0.3. Find P(traffic jam).

**Q17.** In a survey: 55% own a car, 35% own a bike, 15% own both. Find P(bike | car owner).

**Q18.** Emails: 60% legitimate, 40% spam. P(contains "lottery"|spam) = 0.7, P(contains "lottery"|legit) = 0.01. Find P(contains "lottery").

**Q19.** If P(A|B) = P(A), show this implies P(B|A) = P(B). (Independence is symmetric.)

**Q20.** P(A∩B∩C) = ? Express using conditional probability for three events.

## 🔴 Advanced (10 Questions)

**Q21.** Prove: If A and B are independent, then A and Bᶜ are also independent.

**Q22.** A sequence of 5 independent components each fails with P=0.1. System fails if ANY component fails. Find P(system fails).

**Q23.** Three cards drawn without replacement from a 52-card deck. Find P(all three are hearts).

**Q24.** Show that for any events A and B: P(A) = P(A|B)P(B) + P(A|Bᶜ)P(Bᶜ).

**Q25.** Events A, B, C pairwise independent. Does this imply mutual independence? Give a counterexample or proof.

**Q26.** Monty Hall Problem: 3 doors, 1 has a car, 2 have goats. You pick door 1. Host opens door 3 (goat). Should you switch to door 2? Calculate P(car|switch) and P(car|stay).

**Q27.** Given P(A|B) = 0.6 and P(A|Bᶜ) = 0.3 and P(B) = 0.5, find P(A).

**Q28.** Gambler's ruin: A player starts with ₹50 and bets ₹1 each round. P(win each round) = 0.5. What's P(reaching ₹100 before going broke)?

**Q29.** Simpson's Paradox: A drug trial shows: Overall, Drug A is better. But in each sub-group (mild and severe patients), Drug B is better. Explain how this is possible.

**Q30.** Prove: P(A∩B) ≤ min(P(A), P(B)).

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** Naïve Bayes assumes independence. Given: P(spam)=0.3, P("free"|spam)=0.6, P("money"|spam)=0.7, P("free"|ham)=0.05, P("money"|ham)=0.02. An email has both words. Calculate P(spam|"free" and "money") using Naïve Bayes (assume independence of features given class).

**DS2.** Click-through rate: P(click|shown ad) = 0.04. P(shown ad) = 0.30. P(purchase|click) = 0.12. Build the full funnel: P(purchase from random user)?

**DS3.** Feature importance: You suspect feature X and feature Y are NOT independent (they're correlated). Why is this a problem for a Naïve Bayes model? What does correlation between features violate?

**DS4.** An ML pipeline: P(correct preprocessing) = 0.95, P(good model | correct preprocessing) = 0.85, P(good model | bad preprocessing) = 0.20. Find P(good model). 

**DS5.** Covariate shift: Model trained when P(A|B=young user) is high. Now deployed when user population is older. Explain how conditional probability shift affects model performance.

---

# ✅ Key Solutions

**A1.** P(A|B) = 0.12/0.4 = 0.3

**A3.** Sums >9: {10,11,12} — outcomes: (4,6),(5,5),(6,4),(5,6),(6,5),(6,6) = 6. Sum=11: (5,6),(6,5) = 2. P(sum=11|sum>9) = 2/6 = 1/3.

**A5.** Face cards: J, Q, K of each suit = 12. Queens: 4. P(Queen|face card) = 4/12 = 1/3.

**A10.** P(A)×P(B) = 0.3×0.4 = 0.12 = P(A∩B). ✅ Yes, independent.

**A11.** P(defective) = 0.70×0.03 + 0.30×0.07 = 0.021+0.021 = 0.042 = 4.2%.

**A12.** P(A|defective) = P(defective|A)×P(A)/P(defective) = (0.03×0.70)/0.042 = 0.021/0.042 = 0.5 = 50%.

**A13.** P(A|B)×P(B) = P(B|A)×P(A) → 0.7×P(B)=0.5×0.4=0.2 → P(B) = 2/7 ≈ 0.286.

**A15.** P(all work) = 0.95³ ≈ 0.857.

**A16.** P(jam) = 0.8×0.4 + 0.3×0.6 = 0.32+0.18 = 0.50.

**A26.** Monty Hall: P(car|stay) = 1/3. P(car|switch) = 2/3. Always switch! Intuition: your initial pick has 1/3 chance. Host reveals goat, concentrating the remaining 2/3 probability into the other door.

**A27.** P(A) = P(A|B)P(B) + P(A|Bᶜ)P(Bᶜ) = 0.6×0.5 + 0.3×0.5 = 0.3+0.15 = 0.45.

**DS1.** P(spam|free,money) ∝ P(free|spam)×P(money|spam)×P(spam) = 0.6×0.7×0.3 = 0.126. P(ham|free,money) ∝ 0.05×0.02×0.7 = 0.0007. P(spam|free,money) = 0.126/(0.126+0.0007) ≈ 0.994 = 99.4%.

**DS2.** P(purchase) = P(shown)×P(click|shown)×P(purchase|click) = 0.30×0.04×0.12 = 0.00144 = 0.144%.

**DS4.** P(good) = 0.95×0.85 + 0.05×0.20 = 0.8075+0.01 = 0.8175 ≈ 81.75%.

---

# 💼 Interview Questions

**IQ1.** "Explain conditional probability in the context of a recommendation engine."
P(user likes movie B | user liked movie A) — the recommendation is based on conditional probability learned from user behavior.

**IQ2.** "What does it mean for two features to be statistically independent in a dataset?"
Their joint distribution equals the product of their marginals: P(X=x, Y=y) = P(X=x)×P(Y=y) for all x,y. In practice: knowing the value of one feature tells you nothing about the other.

**IQ3.** "Why does the Naïve Bayes classifier perform well despite the independence assumption often being violated?"
Even if features are correlated, the ranking of class probabilities often remains correct. The absolute probabilities may be poorly calibrated, but the argmax (the predicted class) is often right.

---

# 📌 Chapter Summary

## ⚡ Quick Revision
- P(A|B) = P(A∩B)/P(B) — "probability of A given B happened"
- Conditional probability **shrinks** the sample space to only outcomes where B occurred
- Independence: P(A|B) = P(A) ↔ P(A∩B) = P(A)×P(B)
- Multiplication rule: P(A∩B) = P(B)×P(A|B)
- Total probability: P(A) = Σ P(A|Bᵢ)×P(Bᵢ)

## ⚠️ Common Mistakes
| Mistake | Correction |
|---------|-----------|
| P(A|B) = P(B|A) | WRONG — switching is the base rate fallacy |
| Assuming independent because unrelated | Must verify: P(A∩B) = P(A)×P(B) |
| P(A|B) + P(A|Bᶜ) = 1 | WRONG. P(A|B) + P(Aᶜ|B) = 1 |
| Ignoring base rates | Disease prevalence changes interpretation of test results |

---
*Chapter 1.2 Complete | Next: Chapter 1.3 — Bayes' Theorem*
