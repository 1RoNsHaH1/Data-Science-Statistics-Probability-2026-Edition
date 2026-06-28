# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 5 — Advanced Probability & Statistics
# Chapters 5.1–5.5

---

# Chapter 5.1 — Joint Probability Distributions

> **What happens when we care about TWO random variables simultaneously?**

---

## 💡 Intuition

So far we've studied single random variables. But real data has many variables:
- Height AND weight of a person
- Temperature AND rainfall in a city
- Study hours AND exam score

A **joint distribution** describes the probability behavior of two (or more) random variables together.

---

## 5.1.1 — Joint PMF (Discrete)

### 📖 Definition

For two discrete random variables X and Y:

$$P(X=x, Y=y) = p(x,y)$$

The joint PMF gives the probability that **both** X = x AND Y = y simultaneously.

**Properties:**
- p(x,y) ≥ 0 for all (x,y)
- ΣΣ p(x,y) = 1 (sum over all pairs)

### 📊 Example — Two Dice

X = outcome of die 1, Y = outcome of die 2.

```
Joint PMF table: (each cell = 1/36)

     Y=1   Y=2   Y=3   Y=4   Y=5   Y=6
X=1  1/36  1/36  1/36  1/36  1/36  1/36
X=2  1/36  1/36  1/36  1/36  1/36  1/36
X=3  1/36  1/36  1/36  1/36  1/36  1/36
X=4  1/36  1/36  1/36  1/36  1/36  1/36
X=5  1/36  1/36  1/36  1/36  1/36  1/36
X=6  1/36  1/36  1/36  1/36  1/36  1/36

Total = 36 × (1/36) = 1 ✅
```

Since X and Y are independent (different dice), p(x,y) = P(X=x) × P(Y=y) = (1/6)(1/6) = 1/36.

---

## 5.1.2 — Marginal Distributions

### 💡 Intuition

> The **marginal distribution** of X is what you get when you "ignore" Y and just focus on X alone.

Called "marginal" because in textbooks, these probabilities are written in the margins of the joint table.

### 📖 Formula (Discrete)

$$P(X=x) = \sum_y P(X=x, Y=y) \quad \text{(sum over all y)}$$
$$P(Y=y) = \sum_x P(X=x, Y=y) \quad \text{(sum over all x)}$$

### 📊 Example

Joint distribution of (X = study hours category, Y = pass/fail):

```
              Y=Pass   Y=Fail   Marginal P(X)
X=Low (0-2h)  0.10     0.25         0.35
X=Med (3-5h)  0.20     0.15         0.35
X=High (6+h)  0.25     0.05         0.30
Marginal P(Y) 0.55     0.45         1.00

P(X=Low) = 0.10 + 0.25 = 0.35
P(X=Med) = 0.20 + 0.15 = 0.35
P(X=High) = 0.25 + 0.05 = 0.30
P(Y=Pass) = 0.10 + 0.20 + 0.25 = 0.55
```

---

## 5.1.3 — Conditional Distributions

### 📖 Formula

$$P(Y=y \mid X=x) = \frac{P(X=x, Y=y)}{P(X=x)}$$

Using the example above:

$$P(\text{Pass} \mid \text{High Study}) = \frac{0.25}{0.30} = 0.833$$

Students who study 6+ hours have an 83.3% pass rate — much higher than overall 55%.

---

## 5.1.4 — Joint PDF (Continuous)

For continuous random variables:

$$P(a \leq X \leq b, \; c \leq Y \leq d) = \int_a^b \int_c^d f(x,y)\, dy\, dx$$

Properties:
- f(x,y) ≥ 0
- ∫∫ f(x,y) dy dx = 1

Marginals:
$$f_X(x) = \int_{-\infty}^{\infty} f(x,y)\, dy$$

---

## 5.1.5 — Independence (Revisited)

X and Y are **independent** if and only if:

$$P(X=x, Y=y) = P(X=x) \times P(Y=y) \quad \text{for all x, y}$$

Equivalently: f(x,y) = f_X(x) × f_Y(y)

**Test for independence:** Build the joint table. Multiply marginals. If they match the joint — independent. If not — dependent.

---

## 🚀 Data Science Connection

| Concept | ML Application |
|---------|--------------|
| Joint distribution | Full probabilistic model of data |
| Marginal distribution | Distribution of a single feature |
| Conditional distribution | P(Y\|X) — what models learn! |
| Independence | Naïve Bayes assumption |
| Joint PMF | Confusion matrix normalization |

---

# Chapter 5.2 — Covariance Matrices

> **Extending covariance to multiple variables at once.**

---

## 💡 Intuition

You have a dataset with p features. How does each feature relate to every other feature? The **covariance matrix** captures all these relationships in one structured object.

---

## 5.2.1 — From Scalar to Matrix

Single variable: variance σ² = E[(X-μ)²]

Two variables: covariance Cov(X,Y) = E[(X-μ_X)(Y-μ_Y)]

p variables: **Covariance Matrix** Σ (capital sigma) — a p×p matrix

---

## 5.2.2 — Structure of the Covariance Matrix

For variables X₁, X₂, ..., Xₚ:

$$\boldsymbol{\Sigma} = \begin{pmatrix} \text{Var}(X_1) & \text{Cov}(X_1,X_2) & \cdots & \text{Cov}(X_1,X_p) \\ \text{Cov}(X_2,X_1) & \text{Var}(X_2) & \cdots & \text{Cov}(X_2,X_p) \\ \vdots & \vdots & \ddots & \vdots \\ \text{Cov}(X_p,X_1) & \text{Cov}(X_p,X_2) & \cdots & \text{Var}(X_p) \end{pmatrix}$$

**Key properties:**
1. **Diagonal entries** = variances of each variable (always positive)
2. **Off-diagonal entries** = covariances between pairs (can be positive, negative, or zero)
3. **Symmetric:** Cov(Xᵢ, Xⱼ) = Cov(Xⱼ, Xᵢ)
4. **Positive semi-definite:** All eigenvalues ≥ 0

### 📊 Example

3 variables: Age (X₁), Income (X₂), Spending (X₃)

```
       Age    Income   Spending
Age   [ 25      60       30  ]
Income[ 60     2500     1800  ]
Spending[30    1800     900   ]

Diagonal: Var(Age)=25, Var(Income)=2500, Var(Spending)=900
Off-diag: Cov(Age,Income)=60 (positive — older → higher income)
          Cov(Income,Spending)=1800 (positive — higher income → more spending)
          Cov(Age,Spending)=30 (small positive)
```

---

## 5.2.3 — Correlation Matrix vs Covariance Matrix

The **correlation matrix** normalizes covariances by standard deviations:

$$R_{ij} = \frac{\Sigma_{ij}}{\sigma_i \sigma_j}$$

```
Covariance matrix: entries depend on variable scales
  (income in ₹ millions vs age in years — very different magnitudes)

Correlation matrix: entries always in [-1, +1]
  → Easier to interpret
  → Standard output in most EDA tools
```

---

## 5.2.4 — Computing in Python

```python
import numpy as np
import pandas as pd

# Covariance matrix
df = pd.DataFrame({'age': [25,30,35,40], 
                   'income': [30000,50000,70000,90000],
                   'spending': [15000,25000,35000,42000]})

cov_matrix = df.cov()     # covariance matrix
corr_matrix = df.corr()   # correlation matrix (Pearson)

# NumPy
X = df.values  # n×p matrix
Sigma = np.cov(X.T)  # p×p covariance matrix
```

---

## 🚀 Data Science Applications

| Application | Covariance Matrix Role |
|-------------|----------------------|
| **PCA** | Eigendecompose Σ to find principal components |
| **Linear Discriminant Analysis** | Within-class and between-class covariance |
| **Gaussian Mixture Models** | Each Gaussian has its own Σ |
| **Portfolio optimization** | Minimize variance: w^T Σ w |
| **Multivariate anomaly detection** | Mahalanobis distance uses Σ⁻¹ |
| **Multicollinearity detection** | Near-singular Σ signals high correlation |

---

# Chapter 5.3 — Multivariate Normal Distribution

> **The multi-dimensional bell curve — foundation of countless ML algorithms.**

---

## 💡 Intuition

The Normal distribution N(μ, σ²) describes one variable. The **Multivariate Normal** (MVN) describes **multiple variables jointly**, capturing both their individual distributions and their correlations.

Think of a 2D scatter plot of height and weight. The cloud of points tends to be elliptical — the MVN describes this elliptical cloud precisely.

---

## 5.3.1 — The 2D Case (Bivariate Normal)

### 🎨 Visual

```
Weight (kg)
   │          . .
   │        . . . .
   │      . . . . . .
   │        . . . .
   │          . .
   └──────────────── Height (cm)

The "cloud" is elliptical.
Orientation of ellipse = direction of correlation
Width of ellipse axes = standard deviations
Center of ellipse = (μ_height, μ_weight)
```

---

## 5.3.2 — Formal Definition

A p-dimensional random vector **X** = (X₁, ..., Xₚ)ᵀ follows a **Multivariate Normal**:

$$\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$$

**PDF:**

$$f(\mathbf{x}) = \frac{1}{(2\pi)^{p/2}|\boldsymbol{\Sigma}|^{1/2}} \exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^\top \boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu})\right)$$

| Symbol | Meaning |
|--------|---------|
| **μ** | Mean vector (p×1) — center of the distribution |
| **Σ** | Covariance matrix (p×p) — shape and orientation |
| \|**Σ**\| | Determinant of Σ — controls volume/spread |
| **Σ**⁻¹ | Inverse of covariance matrix |
| p | Number of dimensions |

The exponent (x-μ)ᵀ Σ⁻¹(x-μ) is the **Mahalanobis distance** — a generalization of z-score to multiple dimensions.

---

## 5.3.3 — Properties

1. **Marginals are normal:** Each Xᵢ ~ N(μᵢ, σᵢ²)
2. **Conditionals are normal:** (X₁ | X₂=x₂) ~ N(conditional mean, conditional variance)
3. **Linear combinations are normal:** aᵀX ~ N(aᵀμ, aᵀΣa)
4. **Independent ↔ uncorrelated:** For MVN only, Cov(Xᵢ,Xⱼ)=0 ⟺ independent
5. **Sum:** X + Y ~ N(μ_X + μ_Y, Σ_X + Σ_Y) if independent

---

## 🚀 ML Applications

| Algorithm | MVN Role |
|-----------|---------|
| **Gaussian Naive Bayes** | Assumes features ~ MVN within each class |
| **Linear Discriminant Analysis** | Assumes shared Σ across classes |
| **Gaussian Mixture Models** | Sum of MVNs |
| **Kalman Filter** | State and measurement are MVN |
| **GP Regression** | Prior over functions is MVN |
| **VAE** | Latent space ~ N(0, I) |

```python
from scipy.stats import multivariate_normal
import numpy as np

mu = [0, 0]
sigma = [[1, 0.8], [0.8, 1]]  # correlated
rv = multivariate_normal(mean=mu, cov=sigma)
print(rv.pdf([0, 0]))   # density at origin
samples = rv.rvs(1000)  # generate 1000 samples
```

---

# Chapter 5.4 — Markov Chains

> **Modeling systems that change over time, where the future depends only on the present.**

---

## 💡 Intuition — The Memoryless Walk

> **A Markov Chain is a sequence of states where the next state depends ONLY on the current state, not on the history.**

**The Markov Property:**
$$P(X_{t+1} = s \mid X_t, X_{t-1}, \ldots, X_0) = P(X_{t+1} = s \mid X_t)$$

"The future is independent of the past, given the present."

**Real-Life Markov Chains:**

| System | States | Transition |
|--------|--------|------------|
| Weather | {Sunny, Rainy, Cloudy} | Tomorrow's weather depends on today's |
| Stock market | {Bull, Bear, Sideways} | Tomorrow's trend depends on today's |
| User session | {Home, Product, Cart, Buy, Leave} | Next page depends on current page |
| Text generation | Words | Next word depends on current word (bigram) |
| Board game | Board positions | Next position depends on current + dice |

---

## 5.4.1 — Transition Matrix

The **transition matrix** P captures probabilities of moving between states.

$$P_{ij} = P(X_{t+1} = j \mid X_t = i)$$

Row i shows probabilities of leaving state i. **Each row sums to 1.**

### 📊 Example — Weather

```
States: S (Sunny), R (Rainy), C (Cloudy)

Transition Matrix P:
         To:  S     R     C
From: S  [  0.7   0.2   0.1  ]
      R  [  0.3   0.4   0.3  ]
      C  [  0.4   0.3   0.3  ]

Reading row 1: If sunny today:
  70% chance sunny tomorrow
  20% chance rainy tomorrow
  10% chance cloudy tomorrow
```

### 📊 Multi-Step Transitions

To find probabilities after n steps: **P^n** (matrix to the nth power)

```python
import numpy as np
P = np.array([[0.7, 0.2, 0.1],
              [0.3, 0.4, 0.3],
              [0.4, 0.3, 0.3]])

# 2-step transition
P2 = np.linalg.matrix_power(P, 2)
print(P2)  # P(state after 2 days | current state)
```

---

## 5.4.2 — Stationary Distribution

> **As time goes to infinity, where does the chain spend most of its time?**

The **stationary distribution** π satisfies: **π P = π** and Σπᵢ = 1

```
For the weather example:
Solve: [πS, πR, πC] × P = [πS, πR, πC]

Solution (approximately):
πS ≈ 0.56, πR ≈ 0.27, πC ≈ 0.17

Long-run: 56% sunny, 27% rainy, 17% cloudy
```

**This is independent of where you started** (for ergodic chains).

---

## 5.4.3 — Types of States

| Type | Description |
|------|-------------|
| **Absorbing** | Once entered, never leave (P_ii = 1) |
| **Transient** | Will eventually leave and never return |
| **Recurrent** | Will always return eventually |
| **Ergodic** | Positive recurrent + aperiodic → has unique stationary dist |

---

## 🚀 Data Science Applications

| Application | How Markov Chains Are Used |
|-------------|--------------------------|
| **PageRank** (Google) | Web pages as states, links as transitions; stationary distribution = importance |
| **Recommendation systems** | User navigation paths between items |
| **NLP language models** | Bigram/trigram models: next word given current |
| **Reinforcement learning** | MDP = Markov Decision Process (states + actions + rewards) |
| **MCMC** | Markov Chain Monte Carlo — sampling from complex posteriors |
| **Queueing models** | Server states (busy/idle) |

---

# Chapter 5.5 — Bayesian Statistics

> **A coherent framework for learning from data — updating beliefs as evidence accumulates.**

---

## 💡 Intuition — Bayesian vs Frequentist

Two fundamentally different schools of thought:

```
FREQUENTIST (Classical):
- Probability = long-run frequency of events
- Parameters are FIXED (unknown but not random)
- Data is RANDOM
- Inference: p-values, confidence intervals
- "What is the probability of data given parameter?"

BAYESIAN:
- Probability = degree of belief (can apply to single events)
- Parameters are RANDOM (have distributions!)
- Data is FIXED (observed)
- Inference: posterior distributions, credible intervals
- "What is the probability of parameter given data?"
```

---

## 5.5.1 — Bayesian Inference Framework

**The Bayesian updating cycle:**

```
Step 1: Start with PRIOR π(θ)
  "Before seeing data, what do we believe about θ?"
  
Step 2: Collect data x, compute LIKELIHOOD L(x|θ)
  "How probable is the observed data, given each possible θ?"
  
Step 3: Compute POSTERIOR π(θ|x)
  "After seeing data, what do we now believe about θ?"

Bayes Rule:
π(θ|x) = L(x|θ) × π(θ) / P(x)
       ∝ L(x|θ) × π(θ)
       = Likelihood × Prior
```

---

## 5.5.2 — Choosing Priors

| Prior Type | Description | When to Use |
|------------|-------------|-------------|
| **Uninformative (flat)** | All values equally likely | No prior knowledge |
| **Weakly informative** | Broad but not flat | Soft constraints on scale |
| **Informative** | Based on domain knowledge | Strong prior evidence |
| **Conjugate** | Prior + likelihood → same family posterior | Mathematical convenience |

### Conjugate Prior Examples

| Likelihood | Conjugate Prior | Posterior |
|-----------|-----------------|-----------|
| Bernoulli/Binomial | Beta(α,β) | Beta(α+successes, β+failures) |
| Poisson | Gamma(α,β) | Gamma(α+sum, β+n) |
| Normal (unknown μ) | Normal | Normal |
| Multinomial | Dirichlet | Dirichlet |

### 📊 Beta-Binomial Example

Estimating the probability p of success (e.g., click-through rate).

**Prior:** Beta(α=2, β=2) — weakly informative, slight belief p ≈ 0.5

**Data:** 7 successes in 10 trials

**Posterior:** Beta(α+7, β+3) = Beta(9, 5)

```
Prior mean = α/(α+β) = 2/4 = 0.50
Posterior mean = 9/(9+5) = 9/14 ≈ 0.643

Prior pulled the estimate toward 0.5.
With more data, posterior mean → MLE estimate (7/10 = 0.70)
```

---

## 5.5.3 — Credible Intervals vs Confidence Intervals

| | Frequentist CI | Bayesian Credible Interval |
|--|---------------|--------------------------|
| **Definition** | 95% of such intervals capture true parameter | 95% probability parameter lies in this range |
| **Interpretation** | Repeated sampling guarantee | Direct probability statement |
| **Natural?** | No — counter-intuitive | Yes — what we actually want |
| **Requires prior?** | No | Yes |
| **Width** | Fixed by sample size + α | Depends on prior + data |

For large n, they converge to same result.

---

## 5.5.4 — MAP Estimation

**Maximum A Posteriori (MAP):** Find the mode of the posterior.

$$\hat{\theta}_{MAP} = \arg\max_\theta \; \pi(\theta \mid x) = \arg\max_\theta \; L(x|\theta) \times \pi(\theta)$$

- MAP with flat prior → MLE (maximum likelihood estimate)
- MAP with informative prior → regularized MLE
- L2 regularization in regression = MAP with Gaussian prior on weights
- L1 regularization = MAP with Laplace prior

---

## 5.5.5 — MCMC Intuition

For complex posteriors, we can't compute the posterior analytically.

**Markov Chain Monte Carlo (MCMC):** Create a Markov chain whose stationary distribution IS the posterior. Run the chain → collect samples from posterior.

```
Why? Because:
1. Posterior often has no closed form
2. But we can evaluate π(θ|x) ∝ L(x|θ)π(θ) at any point
3. MCMC explores the space, spending time proportional to posterior probability

Popular algorithms:
- Metropolis-Hastings: Accept/reject proposed moves
- Gibbs sampling: Sample one parameter at a time
- HMC (Hamiltonian MC): Uses gradient information → used by Stan, PyMC
- NUTS: No-U-Turn Sampler → default in PyMC, used in production
```

---

## 🚀 Bayesian Data Science Applications

| Application | Bayesian Method |
|-------------|----------------|
| **Hyperparameter tuning** | Bayesian optimization |
| **A/B testing** | Bayesian A/B: P(B>A) directly computed |
| **Recommendation** | Bayesian matrix factorization |
| **NLP** | LDA (Latent Dirichlet Allocation) |
| **Time series** | Prophet uses Bayesian framework |
| **Anomaly detection** | Posterior update on anomaly probability |
| **Uncertainty quantification** | Bayesian Neural Networks |

---

# 📝 Practice Questions — Part 5

## 🟢 Beginner (10 Questions)

**Q1.** What is a joint probability distribution? How many variables does it describe?
**Q2.** In a joint PMF table, what must all the cell probabilities sum to?
**Q3.** How do you get the marginal distribution of X from a joint distribution of (X,Y)?
**Q4.** What is the covariance matrix of a dataset with 4 features? What size is it?
**Q5.** What values are on the diagonal of a covariance matrix?
**Q6.** State the Markov property in plain English.
**Q7.** In a transition matrix, what must each ROW sum to?
**Q8.** What is a prior distribution? What is a posterior distribution?
**Q9.** What is MAP estimation? How is it related to MLE?
**Q10.** What is MCMC used for in Bayesian statistics?

## 🟡 Intermediate (10 Questions)

**Q11.** Joint PMF: P(X=1,Y=1)=0.1, P(X=1,Y=2)=0.2, P(X=2,Y=1)=0.3, P(X=2,Y=2)=0.4. Find marginal P(X=1), P(Y=2). Are X and Y independent?

**Q12.** 3×3 covariance matrix Σ. How many UNIQUE values does it contain? (Hint: symmetric matrix.)

**Q13.** Weather Markov chain: P(Sunny→Sunny)=0.8, P(Sunny→Rainy)=0.2, P(Rainy→Sunny)=0.4, P(Rainy→Rainy)=0.6. Find the stationary distribution.

**Q14.** Prior: Beta(1,1). Observe 8 heads in 12 flips. Find posterior distribution and posterior mean.

**Q15.** Why is L2 regularization in regression equivalent to MAP estimation with a Gaussian prior?

**Q16.** Bivariate Normal: μ=(0,0), Σ=[[1,0.9],[0.9,1]]. What does ρ=0.9 tell us about X and Y?

**Q17.** What is the Mahalanobis distance? How does it differ from Euclidean distance?

**Q18.** Explain why the stationary distribution of a Markov chain is independent of the starting state (for ergodic chains).

**Q19.** Bayesian credible interval: Prior Beta(2,2), posterior Beta(9,5). Find the 95% credible interval conceptually (where does 95% of Beta(9,5) mass lie?).

**Q20.** Why does the Bayesian approach naturally handle small datasets better than frequentist methods?

## 🔴 Advanced (5 Questions)

**Q21.** Derive the conditional distribution of X₁ given X₂=x₂ for a bivariate normal distribution.

**Q22.** Show that if X ~ N(μ, Σ), then AX ~ N(Aμ, AΣAᵀ) for any matrix A.

**Q23.** Metropolis-Hastings algorithm: Describe the accept/reject step and why it converges to the target distribution.

**Q24.** Variational Inference: Instead of sampling (MCMC), how does VI approximate the posterior? What is ELBO?

**Q25.** Hierarchical Bayes: Model where priors themselves have parameters (hyperpriors). Give a practical ML example.

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** You're building a click-through rate model. You observe 3 clicks from 100 impressions for a new ad. Using Beta-Binomial with prior Beta(5,95) (based on historical 5% CTR), find the posterior and posterior mean CTR. How does this compare to the MLE estimate?

**DS2.** PCA uses eigendecomposition of the covariance matrix. If the top 2 eigenvalues are 8.5 and 3.2 and all eigenvalues sum to 15, what percentage of variance do the first 2 principal components explain?

**DS3.** Bayesian A/B test: After observing 50/500 conversions (A) and 70/500 (B), you compute P(B>A) = 0.94. How would you present this to a business stakeholder vs how you'd present p=0.04 from a frequentist test?

**DS4.** Hidden Markov Model: A user's engagement states are Hidden={Active, Churning} with observed signals={Opens email, Ignores email}. Sketch how the Markov property and emission probabilities model this.

**DS5.** Your recommendation system uses a Gaussian Mixture Model with K=5 components. Each component has its own μₖ (mean user preference vector) and Σₖ (covariance). What does each component represent? How does the model assign a new user to a component?

---

# ✅ Key Solutions

**A11.** P(X=1)=0.1+0.2=0.3. P(Y=2)=0.2+0.4=0.6. For independence: P(X=1)×P(Y=2)=0.3×0.6=0.18 ≠ P(X=1,Y=2)=0.2. NOT independent.

**A12.** Symmetric p×p matrix: diagonal (p entries) + upper triangle [p(p-1)/2 entries]. For 3×3: 3+3=6 unique values.

**A13.** Solve πS×0.2 = πR×0.4 and πS+πR=1. πS×0.2=πR×0.4 → πS=2πR. So 2πR+πR=1 → πR=1/3, πS=2/3. Long-run: 67% sunny, 33% rainy.

**A14.** Posterior = Beta(1+8, 1+4) = Beta(9,5). Posterior mean = 9/14 ≈ 0.643. (Compared to MLE 8/12 ≈ 0.667.)

**A15.** MAP: θ* = argmax[log L(θ|data) + log π(θ)]. With Gaussian prior N(0,λ): log π(θ) = -λ||θ||²/2. Maximizing = minimizing -log L + λ||θ||² = negative log-likelihood + L2 penalty. This is exactly ridge regression.

**A16.** ρ=0.9 means X and Y are strongly positively correlated. Knowing X is high tells us Y is almost certainly also high. The bivariate normal contours are elongated ellipses oriented along the 45° line.

**DS1.** Prior Beta(5,95). After 3/100: Posterior = Beta(5+3, 95+97) = Beta(8,192). Posterior mean = 8/200 = 0.04 = 4%. MLE = 3/100 = 3%. Prior pulled estimate toward 5% (historical rate). With only 3 clicks, the historical rate dominates — sensible behavior.

**DS2.** Variance explained = (8.5+3.2)/15 = 11.7/15 = 78%. The first 2 PCs capture 78% of total variance.

---

# 💼 Interview Questions — Part 5

**IQ1.** "What is PCA and how does the covariance matrix relate to it?"
PCA finds directions of maximum variance in data. It eigendecomposes the covariance matrix Σ. Eigenvectors = principal component directions. Eigenvalues = variance explained in each direction. We keep the top k eigenvectors as our reduced-dimension representation.

**IQ2.** "What is the Markov property and why does it matter for RL?"
The future depends only on the current state, not history. In RL, this allows us to use dynamic programming to find optimal policies efficiently. The MDP formulation provides theoretical guarantees for Q-learning, policy gradient, etc.

**IQ3.** "How would you explain Bayesian inference to a business stakeholder?"
"We start with our best guess based on past data. As we collect new data, we update our guess. The Bayesian approach gives us not just a number but a range of plausible values with probabilities attached. This lets us say 'we're 94% confident Version B is better' — which is much more actionable than 'p=0.04'."

---

# 📌 Part 5 Summary

## ⚡ Quick Revision

- **Joint distribution:** P(X=x, Y=y) — both variables together
- **Marginal:** Σ_y P(X=x, Y=y) — one variable, ignoring the other
- **Conditional:** P(Y|X) = P(X,Y)/P(X) — one given the other
- **Covariance matrix Σ:** p×p symmetric, variances on diagonal, covariances off-diagonal
- **Multivariate Normal:** X ~ N(μ, Σ), generalizes normal to p dimensions
- **Markov chain:** States + transition matrix P; each row sums to 1; stationary distribution π satisfies πP=π
- **Bayesian inference:** Posterior ∝ Likelihood × Prior; MAP = mode of posterior; MCMC = sampling from posterior
- **Conjugate prior:** Prior and posterior in same family (Beta-Binomial, Gamma-Poisson)

---
*Part 5 Complete | Next: Part 6 — Linear Algebra for Statistics & ML*
