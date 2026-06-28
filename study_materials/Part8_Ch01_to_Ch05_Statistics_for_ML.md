# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 8 — Statistics for Machine Learning
# Chapters 8.1–8.5

---

# Chapter 8.1 — Bias vs Variance

> **The fundamental trade-off that governs every ML model's performance.**

---

## 💡 The Dartboard Analogy

Imagine throwing darts at a bullseye. Your "model" is your throwing technique. The bullseye is the true target (true relationship in data).

```
HIGH BIAS,           LOW BIAS,            HIGH BIAS,        LOW BIAS,
LOW VARIANCE         HIGH VARIANCE        HIGH VARIANCE     LOW VARIANCE
(Underfitting)       (Overfitting)        (Worst)           (Ideal!)

  ·  ·                · ·  ·               · · · ·           · ·
  · ·                ·      ·             ·   · ·            ·●·
  ·                    ·  ·                 ·   · ·            ·
Darts cluster       Darts spread         Darts spread       Darts cluster
far from            around center        AND far from       around center
bullseye            randomly             bullseye           (bullseye!)

Systematic wrong    No consistent        Both errors        Perfect!
direction           aim                  at once
```

---

## 8.1.1 — Definitions

**Bias:** The systematic error from wrong assumptions in the model.
- High bias = model is too simple = misses the true pattern
- Low bias = model captures the pattern well

**Variance:** The sensitivity to fluctuations in the training data.
- High variance = model changes a lot with different training sets = overfits to noise
- Low variance = model is stable across different training sets

**Noise:** The irreducible error in the data (measurement error, randomness). Cannot be reduced.

---

## 8.1.2 — The Bias-Variance Decomposition

For expected squared prediction error at point x:

$$E[(y - \hat{f}(x))^2] = \text{Bias}^2[\hat{f}(x)] + \text{Var}[\hat{f}(x)] + \sigma^2$$

| Component | Formula | Reducible? |
|-----------|---------|-----------|
| Bias² | (E[f̂(x)] - f(x))² | Yes — use more complex model |
| Variance | E[(f̂(x) - E[f̂(x)])²] | Yes — use simpler model / more data |
| Noise σ² | Irreducible error | No — inherent randomness |

---

## 8.1.3 — The Trade-off

```
Error
│   Total Error
│  \───────────────────
│   ──                  ← Variance (increases with complexity)
│    ──────────────     ← Bias² (decreases with complexity)
│      ─────────────
│           * ← Sweet spot: minimum total error
└──────────────────────── Model Complexity
Simple                Complex
(Linear)             (Deep NN)
```

**The trade-off:** You can't simultaneously minimize both bias and variance for a fixed dataset.

---

## 8.1.4 — Diagnosing Bias and Variance

| Symptom | Likely Problem | Solution |
|---------|---------------|---------|
| High training error + high test error | High Bias (underfitting) | More complex model, more features |
| Low training error + high test error | High Variance (overfitting) | More data, regularization, simpler model |
| Low training + low test error | Good fit! | Deploy |

```
Diagnostic: Plot learning curves

High Bias:                        High Variance:
Error                             Error
│  ─────────────── (train)        │  ─────── (train)
│  ─────────────── (test)         │
│                                 │           ─────── (test)
│ Both errors high, close together│ Large gap between train and test
└──────────── n (data size)       └──────────── n (data size)

With more data:                   With more data:
Gap doesn't close much            Gap narrows (test → train)
```

---

## 8.1.5 — Solutions

**Reducing High Bias (Underfitting):**
- Increase model complexity (add layers, increase depth)
- Add more features
- Reduce regularization strength
- Train longer

**Reducing High Variance (Overfitting):**
- Get more training data
- Regularization (L1/L2)
- Dropout (neural networks)
- Cross-validation for model selection
- Reduce model complexity
- Feature selection / dimensionality reduction
- Ensemble methods (bagging reduces variance)

---

# Chapter 8.2 — Feature Engineering Statistics

> **The data you feed your model matters more than the model you choose.**

---

## 8.2.1 — Feature Scaling

### Why Scale?

Many ML algorithms are sensitive to feature scale:
- Gradient descent: large features dominate gradient → training instability
- Distance-based (KNN, K-means, SVM): large-scale features dominate distances
- Regularization: penalizes large coefficients — meaningful only when features are comparable scale

### Min-Max Normalization (Scaling to [0,1])

$$x_{\text{scaled}} = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$$

- Output always in [0, 1]
- Sensitive to outliers (outlier becomes 0 or 1, compressing all others)
- Use for: neural networks, image pixel normalization

### Standardization (Z-score Scaling)

$$x_{\text{scaled}} = \frac{x - \mu}{\sigma}$$

- Output: mean=0, std=1 (approximately normal)
- Robust to outliers (they become large z-scores but don't compress others)
- Use for: linear/logistic regression, SVM, PCA, most distance-based methods

### Robust Scaling (Uses IQR)

$$x_{\text{scaled}} = \frac{x - Q_2}{Q_3 - Q_1}$$

- Centers on median, scales by IQR
- Most robust to outliers
- Use for: data with significant outliers

### When NOT to Scale

- Tree-based models (decision trees, random forests, XGBoost) — splits are based on thresholds, scale-invariant
- Models that you want to interpret in original units

---

## 8.2.2 — Encoding Categorical Variables

### Label Encoding

Assign integer to each category: Red=0, Green=1, Blue=2

**Problem:** Implies ordering (Blue=2 > Red=0) and magnitude (Blue is "twice" Green)

**Use ONLY for:** Ordinal variables or tree-based models

### One-Hot Encoding

Create binary column for each category:

```
Color = [Red, Green, Blue]

After one-hot:
Color_Red  Color_Green  Color_Blue
    1           0           0      ← Red
    0           1           0      ← Green
    0           0           1      ← Blue
```

**Problem with dummy trap:** 3 columns are perfectly multicollinear (sum always = 1). Drop one.

**Use for:** Linear models, SVM, neural networks with nominal categorical features.

### Target Encoding

Replace category with mean of target variable for that category:

```
City    Salary(mean)    → encoded
Delhi   75000           → 75000
Mumbai  90000           → 90000
Chennai 65000           → 65000
```

**Risk:** Data leakage — must compute ONLY on training data, apply to test.
**Use for:** High-cardinality categoricals in tree-based models.

---

## 8.2.3 — Handling Missing Values

| Strategy | Formula/Method | When to Use |
|----------|---------------|-------------|
| Mean imputation | Replace with column mean | Normal distribution, low % missing |
| Median imputation | Replace with median | Skewed data, outliers present |
| Mode imputation | Replace with most common value | Categorical features |
| KNN imputation | Use k nearest neighbors | Low-to-moderate % missing |
| Multiple imputation | Sample from conditional distributions | Research/high-stakes |
| Indicator variable | Add "was_missing" binary flag | When missingness itself is informative |
| Drop rows | Remove rows with missing values | Very low % missing |

---

## 8.2.4 — Feature Transformation

| Transformation | Formula | Purpose |
|----------------|---------|---------|
| Log | log(x+1) | Reduce right skew |
| Square root | √x | Mild right skew reduction |
| Square | x² | Capture non-linear relationship |
| Box-Cox | (xλ-1)/λ | Optimal normalization (find λ) |
| Yeo-Johnson | Like Box-Cox but handles negatives | More general |
| Binning | pd.cut() | Convert continuous to categorical |
| Polynomial | x, x², x³ | Capture non-linear relationships in linear model |

---

# Chapter 8.3 — Distance Metrics

> **Defining 'similarity' mathematically — used in clustering, KNN, recommendations.**

---

## 💡 Intuition

How "far apart" are two data points? Different definitions of distance capture different kinds of similarity.

```
Points: A=(1,1), B=(4,5)

Euclidean (as the crow flies):
  d = √((4-1)²+(5-1)²) = √(9+16) = √25 = 5

Manhattan (walking blocks in a city grid):
  d = |4-1| + |5-1| = 3 + 4 = 7

Chebyshev (max of any coordinate difference):
  d = max(3,4) = 4
```

---

## 8.3.1 — Euclidean Distance (L2)

$$d(x,y) = \sqrt{\sum_{i=1}^p (x_i - y_i)^2} = ||x - y||_2$$

- Most natural for geometric data
- Sensitive to scale → always standardize features first!
- Used in: KNN, K-means, PCA distance reconstruction error

---

## 8.3.2 — Manhattan Distance (L1)

$$d(x,y) = \sum_{i=1}^p |x_i - y_i| = ||x - y||_1$$

- Less sensitive to outliers than Euclidean
- Used in: sparse data (text), L1 regularization (LASSO), robust regression

---

## 8.3.3 — Cosine Similarity / Distance

$$\text{similarity} = \frac{x \cdot y}{||x|| \cdot ||y||}$$

$$\text{distance} = 1 - \text{cosine similarity}$$

- Measures angle between vectors, NOT magnitude
- Perfect for high-dimensional sparse vectors (text, embeddings)
- Two documents with same words (regardless of length) → cosine = 1
- Used in: NLP, recommendation systems, face recognition

---

## 8.3.4 — Mahalanobis Distance

$$d_M(x, \mu) = \sqrt{(x-\mu)^T \Sigma^{-1} (x-\mu)}$$

- Accounts for correlations between features (using covariance matrix Σ)
- Scale-invariant (doesn't need standardization)
- Reduces to Euclidean when Σ = I
- Used in: multivariate outlier detection, LDA

```
Why it matters:
Height=180cm, Weight=120kg might be similar Euclidean distance
to Height=180cm, Weight=80kg from some center,
but Mahalanobis knows that 120kg weight is MORE unusual
relative to height 180cm (correlation structure matters)
```

---

## 8.3.5 — Distance Metric Summary

| Metric | Formula | Best For |
|--------|---------|---------|
| Euclidean | √Σ(xᵢ-yᵢ)² | Continuous, geometric data |
| Manhattan | Σ\|xᵢ-yᵢ\| | Sparse data, robust to outliers |
| Cosine | x·y/(||x||||y||) | Text, high-dimensional, embeddings |
| Mahalanobis | √(x-μ)ᵀΣ⁻¹(x-μ) | Correlated features, outlier detection |
| Hamming | Count of differing bits | Binary/categorical data |
| Jaccard | \|A∩B\|/\|A∪B\| | Set similarity, binary data |

---

# Chapter 8.4 — Information Theory

> **Quantifying information, uncertainty, and model quality.**

---

## 8.4.1 — Entropy

### 💡 Intuition

> **Entropy measures the average uncertainty (surprise) in a probability distribution.**

```
HIGH ENTROPY: Coin with P(H)=0.5
  Outcome is maximally uncertain — lots of information in each flip

LOW ENTROPY: Coin with P(H)=0.99
  You know it'll be heads — little uncertainty, little information

ZERO ENTROPY: Coin with P(H)=1.0
  No uncertainty whatsoever — zero information
```

### 📖 Formula (Shannon Entropy)

$$H(X) = -\sum_{x} P(x) \log_2 P(x)$$

Measured in **bits** (when using log₂) or **nats** (when using ln).

**Properties:**
- H ≥ 0 always
- Maximum entropy: all outcomes equally likely
- Minimum entropy = 0: one outcome certain

### 📊 Example

Fair coin: P(H)=0.5, P(T)=0.5
H = -(0.5 log₂(0.5) + 0.5 log₂(0.5)) = -(0.5×(-1) + 0.5×(-1)) = **1 bit**

Biased coin: P(H)=0.9, P(T)=0.1
H = -(0.9 log₂(0.9) + 0.1 log₂(0.1)) = -(0.9×(-0.152) + 0.1×(-3.322)) = -(−0.137 − 0.332) = **0.469 bits**

---

## 8.4.2 — Cross-Entropy

### 💡 Intuition

> **Cross-entropy measures how well distribution Q approximates the true distribution P.**

If P is the true label distribution and Q is your model's predicted distribution:

$$H(P, Q) = -\sum_x P(x) \log Q(x)$$

**Key property:** H(P,Q) ≥ H(P), with equality only when P = Q.

### 📊 Binary Cross-Entropy Loss (Log Loss)

For binary classification (y ∈ {0,1}, model predicts p̂):

$$L = -[y \log(\hat{p}) + (1-y) \log(1-\hat{p})]$$

```
If true label y=1:
  L = -log(p̂)
  p̂=0.99 → L = -log(0.99) ≈ 0.01 (small loss, good prediction)
  p̂=0.01 → L = -log(0.01) = 4.61 (huge loss, terrible prediction)

If true label y=0:
  L = -log(1-p̂)
  p̂=0.01 → L = -log(0.99) ≈ 0.01 (small loss, good prediction)
  p̂=0.99 → L = -log(0.01) = 4.61 (huge loss, terrible prediction)
```

---

## 8.4.3 — KL Divergence

### 💡 Intuition

> **KL divergence measures how much information is LOST when using Q to approximate P.**

$$D_{KL}(P \| Q) = \sum_x P(x) \log \frac{P(x)}{Q(x)}$$

**Properties:**
- D_KL ≥ 0 always
- D_KL = 0 ↔ P = Q exactly
- NOT symmetric: D_KL(P||Q) ≠ D_KL(Q||P)

**Relationship:** Cross-entropy = Entropy + KL divergence
$$H(P, Q) = H(P) + D_{KL}(P \| Q)$$

---

## 8.4.4 — Information Gain (Used in Decision Trees)

$$IG(X, Y) = H(Y) - H(Y|X)$$

Entropy of target BEFORE split minus entropy AFTER split on feature X.
- Higher IG → better feature split → select this feature

```
Root node: 50% spam, 50% ham → H=1 bit

Split on "contains FREE":
  "FREE" = Yes: 90% spam, 10% ham → H=0.47 bits
  "FREE" = No:  30% spam, 70% ham → H=0.88 bits

Expected H after split = 0.3×0.47 + 0.7×0.88 = 0.141+0.616 = 0.757 bits
IG = 1.0 - 0.757 = 0.243 bits

ID3, C4.5 decision tree algorithms use this to select splits.
```

---

## 8.4.5 — Mutual Information

$$I(X;Y) = H(X) - H(X|Y) = H(Y) - H(Y|X) = D_{KL}(P(X,Y) \| P(X)P(Y))$$

- Measures how much knowing X tells us about Y (and vice versa)
- Unlike correlation: captures non-linear dependencies
- MI = 0 ↔ X and Y are independent

---

# Chapter 8.5 — Probabilistic Machine Learning

> **ML models as probability distributions — the principled framework.**

---

## 8.5.1 — Naïve Bayes Classifier

### Model

$$P(Y=c \mid X_1, \ldots, X_p) \propto P(Y=c) \prod_{j=1}^p P(X_j \mid Y=c)$$

**Assumption:** Features are conditionally independent given class.

### Variants

| Variant | Likelihood Assumption | Use Case |
|---------|----------------------|---------|
| Gaussian NB | P(Xⱼ\|Y=c) ~ Normal | Continuous features |
| Multinomial NB | Word count distributions | Document classification |
| Bernoulli NB | Binary features | Binary text features |
| Complement NB | Improved for imbalanced | Imbalanced text data |

### 📊 Example

Email: Contains "FREE" and "WINNER"
Classes: spam vs ham

```
P(spam) = 0.4, P(ham) = 0.6
P("FREE"|spam)=0.7,   P("FREE"|ham)=0.05
P("WINNER"|spam)=0.6, P("WINNER"|ham)=0.01

P(spam|email) ∝ 0.4 × 0.7 × 0.6 = 0.168
P(ham|email)  ∝ 0.6 × 0.05 × 0.01 = 0.0003

Normalize: P(spam|email) = 0.168/(0.168+0.0003) ≈ 0.998 → SPAM
```

---

## 8.5.2 — Gaussian Models

**Gaussian Discriminant Analysis (GDA):**
- Class-conditional densities: P(X|Y=c) ~ N(μc, Σc)
- Linear DA: Σc same for all classes → linear decision boundary
- Quadratic DA: Different Σc per class → quadratic boundary

**Gaussian Mixture Models (GMM):**
$$P(x) = \sum_{k=1}^K \pi_k \mathcal{N}(x; \mu_k, \Sigma_k)$$

- Soft clustering: each point belongs to all clusters with probabilities
- Fit via EM algorithm
- More flexible than K-means (handles elliptical clusters, unequal sizes)

---

## 8.5.3 — Hidden Markov Models (HMM)

**Structure:**
- Hidden states: Sₜ ∈ {s₁, ..., sₖ} (follow Markov chain)
- Observations: Oₜ depend on current hidden state only

$$P(O₁,...,Oₜ, S₁,...,Sₜ) = P(S₁)\prod_{t=2}^T P(Sₜ|Sₜ₋₁) \prod_{t=1}^T P(Oₜ|Sₜ)$$

**Key algorithms:**
- Forward algorithm: P(observations) efficiently
- Viterbi: Most likely hidden state sequence
- Baum-Welch: Learn parameters from observations (EM on HMMs)

**Applications:** Speech recognition, NLP POS tagging, DNA sequence modeling, financial regime detection.

---

# 📝 Practice Questions — Part 8

## 🟢 Beginner (8 Questions)

**Q1.** A model has 95% training accuracy and 65% test accuracy. Does it have high bias or high variance?
**Q2.** When should you use standardization vs min-max normalization?
**Q3.** Why can't you one-hot encode a column with 500 unique values directly?
**Q4.** Euclidean distance between (1,2) and (4,6)?
**Q5.** H(X) for a fair 4-sided die. (Hint: H = -Σ P(x) log₂P(x))
**Q6.** Binary cross-entropy: y=1, p̂=0.8. Compute the loss.
**Q7.** What does entropy = 0 mean?
**Q8.** What is the key assumption of Naïve Bayes?

## 🟡 Intermediate (8 Questions)

**Q9.** A model has 70% train accuracy and 68% test accuracy. Is this underfitting or good fit? What would you try?
**Q10.** Dataset: feature "salary" has mean=50k, std=20k, min=20k, max=1M. Should you use standardization or min-max? Justify.
**Q11.** Compute IG for splitting on feature X that perfectly separates classes (after split, each leaf has 100% one class).
**Q12.** Text document: "dog bites man" vs "man bites dog". Euclidean distance (word count vectors) = 0. Explain and compute cosine similarity.
**Q13.** GMM with K=3 assigns P(cluster1|x)=0.7, P(cluster2|x)=0.2, P(cluster3|x)=0.1 for a point x. What cluster does K-means assign it to? What does GMM do differently?
**Q14.** Regularization and bias-variance: How does increasing L2 penalty λ affect bias and variance?
**Q15.** Cross-entropy loss for multiclass (K=3): true label=[0,1,0], predicted=[0.1,0.7,0.2]. Compute H(P,Q).
**Q16.** Feature "age" has 5% missing values. List 3 imputation strategies with pros/cons for each.

## 🔴 Advanced (4 Questions)

**Q17.** Prove that uniform distribution maximizes entropy for a discrete distribution with K outcomes.
**Q18.** Show that KL divergence D_KL(P||Q) ≥ 0 (Jensen's inequality approach).
**Q19.** EM algorithm for GMM: Describe E-step (compute responsibilities) and M-step (update μ, Σ, π).
**Q20.** Relate log-loss (cross-entropy) to MLE: Show that minimizing log-loss = maximizing log-likelihood under Bernoulli model.

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** Your fraud detection model has 99.9% training accuracy and 99.8% test accuracy, but catches only 30% of actual fraud cases. Explain bias-variance diagnosis and the REAL problem here.

**DS2.** Feature engineering for a house price model: Features include [bedrooms, bathrooms, sqft, year_built, zip_code, neighborhood]. Apply appropriate encodings and scalings for a linear regression model.

**DS3.** You're comparing 3 document clustering algorithms using different distance metrics: Euclidean on TF-IDF, cosine on TF-IDF, Euclidean on word2vec embeddings. What do you expect and which would you choose?

**DS4.** Decision tree splits: Node has 100 samples, 60 positive, 40 negative. Option A: split creates {40 pos, 5 neg} and {20 pos, 35 neg}. Option B: split creates {35 pos, 15 neg} and {25 pos, 25 neg}. Compute IG for both. Which split is better?

**DS5.** A Naïve Bayes spam filter sees a new word "cryptocurrency" never seen in training. What problem occurs? What smoothing technique fixes this?

---

# ✅ Key Solutions

**A1.** High variance (overfitting). 95% train vs 65% test = large gap.

**A4.** √((4-1)²+(6-2)²) = √(9+16) = 5.

**A5.** Each face P=1/4. H = -4×(0.25×log₂(0.25)) = -4×(0.25×(-2)) = 2 bits.

**A6.** L = -[1×log(0.8) + 0×log(0.2)] = -log(0.8) ≈ 0.223.

**A7.** Zero uncertainty — one outcome is certain (probability 1).

**A9.** Good fit (small train-test gap). Try: more data, more complex model, feature engineering.

**A11.** Before split: H(Y) depends on class balance. After split: H(Y|X)=0 (each leaf pure). IG = H(Y) - 0 = H(Y). Maximum possible IG.

**A12.** Both have dog=1, bites=1, man=1 (same word counts regardless of order). Euclidean = 0 is correct (identical vectors). Cosine = 1 (identical directions). Bag-of-words loses word order — both docs have same representation.

**A14.** Increasing λ: increases bias (forces weights toward zero → simpler model → underfits) but reduces variance (less sensitive to training data fluctuations). High λ → underfitting; λ=0 → potential overfitting.

**A15.** H(P,Q) = -Σ P(x)log(Q(x)) = -(0×log(0.1) + 1×log(0.7) + 0×log(0.2)) = -log(0.7) ≈ 0.357.

**A17.** For K outcomes, maximize H = -Σpᵢlogpᵢ subject to Σpᵢ=1. Using Lagrange multipliers or Jensen's inequality: H ≤ log₂K with equality iff all pᵢ=1/K (uniform). ✅

**DS1.** Not a bias-variance problem — it's a **class imbalance problem**. Only ~0.1% fraud in data, so always predicting "not fraud" gives 99.9% accuracy. Metric: use F1-score, precision-recall, or AUC-ROC. Solution: oversample fraud (SMOTE), undersample non-fraud, use class weights, adjust decision threshold.

**DS4.** Current: 60 pos, 40 neg. H = -(0.6log0.6+0.4log0.4) = 0.971 bits.
Option A: Leaf1(45 items: 40/5): H = -(0.889log0.889+0.111log0.111) = 0.5 bits. Leaf2(55 items: 20/35): H = -(0.364log0.364+0.636log0.636) = 0.943 bits. Expected H = (45/100)×0.5+(55/100)×0.943 = 0.225+0.519=0.744. IG_A = 0.971-0.744 = 0.227 bits.
Option B: both leaves mixed → IG_B ≈ 0.05 bits. Option A is clearly better.

**DS5.** Zero-probability problem: P("cryptocurrency"|spam) = 0 → entire product = 0. Fix: **Laplace smoothing** (add-1 smoothing): P(w|class) = (count(w,class)+1)/(total_count+vocabulary_size). Ensures no word has zero probability.

---

# 💼 Interview Questions — Part 8

**IQ1.** "Explain the bias-variance trade-off and how regularization affects it."
Bias = systematic error (model too simple). Variance = sensitivity to training data (model too complex). Total error = Bias² + Variance + Noise. Regularization (L1/L2) penalizes large weights, forcing simpler models. Increases bias but decreases variance. Optimal λ found via cross-validation.

**IQ2.** "When would cosine similarity be better than Euclidean distance?"
For high-dimensional sparse data (text), two documents with many words will naturally have large Euclidean distance simply because of length. Cosine similarity normalizes for length, measuring only directional similarity — i.e., topical similarity regardless of document length. Essential for NLP.

**IQ3.** "What is entropy and how is it used in decision trees?"
Entropy measures impurity/uncertainty in a node. A pure node (all one class) has entropy=0. Maximum entropy when classes are equally mixed. Information Gain = reduction in entropy from a split. Decision trees choose the feature and threshold that maximize information gain (or Gini impurity decrease), greedily building a tree that separates classes.

---

# 📌 Part 8 Summary

## 📐 Formula Sheet

| Formula | Name |
|---------|------|
| Error = Bias² + Variance + σ² | Bias-Variance decomposition |
| z = (x-μ)/σ | Standardization |
| x_norm = (x-min)/(max-min) | Min-max normalization |
| d_E = √Σ(xᵢ-yᵢ)² | Euclidean distance |
| d_M = Σ\|xᵢ-yᵢ\| | Manhattan distance |
| cos(θ) = x·y/(||x||||y||) | Cosine similarity |
| H(X) = -ΣP(x)log₂P(x) | Shannon entropy |
| H(P,Q) = -ΣP(x)log Q(x) | Cross-entropy |
| D_KL(P||Q) = ΣP log(P/Q) | KL divergence |
| IG = H(Y) - H(Y\|X) | Information gain |

---
*Part 8 Complete | Next: Part 9 — Time Series Statistics*
