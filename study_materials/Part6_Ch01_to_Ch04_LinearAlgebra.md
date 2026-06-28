# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 6 — Linear Algebra for Statistics & ML
# Chapters 6.1–6.4: Vectors | Matrices | Eigenvalues | Matrix Decomposition

---

# Chapter 6.1 — Vectors

> **Vectors are the language of data. Every data point IS a vector.**

---

## 💡 Intuition

A vector is simply an ordered list of numbers.

A house has: 3 bedrooms, 2 bathrooms, 1500 sq ft, ₹80 lakh price.
This house IS the vector: **x** = [3, 2, 1500, 80]

Every row in your dataset is a vector. Every image pixel array is a vector. Every word embedding is a vector.

```
GEOMETRIC VIEW:                DATA VIEW:
                               Customer record:
  ↑ y                          x = [age=28, income=55000, score=720]
  │    • (3, 4)                     ↑         ↑              ↑
  │   /                           x₁        x₂             x₃
  │  /  vector (3,4)
  │ /   magnitude = 5
  │/    direction = atan(4/3)
  └──────── x
```

---

## 6.1.1 — Vector Operations

### Addition and Subtraction
$$\mathbf{u} + \mathbf{v} = \begin{pmatrix}u_1+v_1 \\ u_2+v_2 \\ \vdots\end{pmatrix}$$

**Intuition:** u + v = "walk along u, then walk along v"

**Example:** Customer A's features + Customer B's features (component-wise)

### Scalar Multiplication
$$c\mathbf{v} = \begin{pmatrix}cv_1 \\ cv_2 \\ \vdots\end{pmatrix}$$

**Intuition:** Scale a vector — stretch or shrink it, possibly flip direction

### Dot Product
$$\mathbf{u} \cdot \mathbf{v} = \sum_{i=1}^n u_i v_i = u_1v_1 + u_2v_2 + \cdots + u_nv_n$$

**Intuition:** Measures how much two vectors "point in the same direction"

```
Geometric: u·v = |u||v|cos(θ)

u·v > 0: vectors point in similar direction (θ < 90°)
u·v = 0: vectors are perpendicular (θ = 90°) → ORTHOGONAL
u·v < 0: vectors point in opposite directions (θ > 90°)
```

**Example:**
u = [1, 2, 3], v = [4, 5, 6]
u·v = 1×4 + 2×5 + 3×6 = 4 + 10 + 18 = 32

### Vector Magnitude (Length / Norm)
$$||\mathbf{v}|| = \sqrt{v_1^2 + v_2^2 + \cdots + v_n^2}$$

This is the **L2 norm** (Euclidean length).

**Example:** v = [3, 4]
||v|| = √(9+16) = √25 = 5

---

## 6.1.2 — Unit Vectors and Normalization

A **unit vector** has magnitude = 1. It indicates direction only.

$$\hat{\mathbf{v}} = \frac{\mathbf{v}}{||\mathbf{v}||}$$

**Why important in ML?**
- Cosine similarity uses normalized vectors
- Gradient descent uses unit gradient direction × learning rate
- Eigenvectors are typically unit vectors

---

## 6.1.3 — Cosine Similarity

$$\cos(\theta) = \frac{\mathbf{u} \cdot \mathbf{v}}{||\mathbf{u}|| \cdot ||\mathbf{v}||}$$

| Value | Meaning |
|-------|---------|
| +1 | Identical direction (documents have same content) |
| 0 | Perpendicular (no similarity) |
| -1 | Opposite direction |

**Data Science use:** Text similarity, recommendation systems, word embeddings.

```python
from sklearn.metrics.pairwise import cosine_similarity
doc1 = [[1, 2, 0, 1]]   # TF-IDF vector
doc2 = [[1, 1, 1, 0]]
print(cosine_similarity(doc1, doc2))  # similarity score
```

---

## 🚀 Vectors in Data Science

| Application | Vector Representation |
|-------------|----------------------|
| Data point | Feature vector x ∈ ℝᵖ |
| Model weights | Weight vector w ∈ ℝᵖ |
| Word embedding | Word vector ∈ ℝ³⁰⁰ |
| Image | Flattened pixel vector ∈ ℝ^(H×W×C) |
| User preferences | Latent factor vector |
| Gradient | Vector of partial derivatives |

---

# Chapter 6.2 — Matrices

> **Matrices are the spreadsheets of mathematics. Your dataset IS a matrix.**

---

## 💡 Intuition

A **matrix** is a rectangular array of numbers — rows and columns.

Your entire dataset: n rows (observations) × p columns (features) = an **n × p matrix X**.

$$\mathbf{X} = \begin{pmatrix} x_{11} & x_{12} & \cdots & x_{1p} \\ x_{21} & x_{22} & \cdots & x_{2p} \\ \vdots & & \ddots & \vdots \\ x_{n1} & x_{n2} & \cdots & x_{np} \end{pmatrix}$$

---

## 6.2.1 — Matrix Operations

### Transpose
Flip rows and columns. If A is m×n, then Aᵀ is n×m.

$$(A^\top)_{ij} = A_{ji}$$

```
A = [1 2 3]    Aᵀ = [1 4]
    [4 5 6]         [2 5]
                    [3 6]
```

**Uses in ML:** XᵀX appears in linear regression: β = (XᵀX)⁻¹Xᵀy

### Matrix Multiplication

A is m×n, B is n×p → AB is m×p

$$(AB)_{ij} = \sum_{k=1}^n A_{ik} B_{kj}$$

**Intuition:** The (i,j) entry = dot product of row i of A with column j of B.

**Important:**  AB ≠ BA (order matters!)

```
Example:
A = [1 2]   B = [5 6]
    [3 4]       [7 8]

AB[1,1] = 1×5 + 2×7 = 19
AB[1,2] = 1×6 + 2×8 = 22
AB[2,1] = 3×5 + 4×7 = 43
AB[2,2] = 3×6 + 4×8 = 50

AB = [19 22]
     [43 50]
```

### Identity Matrix I

A square matrix with 1s on diagonal, 0s elsewhere.
Property: AI = IA = A (like multiplying by 1)

### Matrix Inverse

For square matrix A, the inverse A⁻¹ satisfies: A A⁻¹ = I

**Exists only when A is non-singular (det(A) ≠ 0)**

**ML use:** β = (XᵀX)⁻¹ Xᵀy — OLS regression solution

```python
import numpy as np
A = np.array([[1,2],[3,4]])
A_inv = np.linalg.inv(A)
print(A @ A_inv)  # ≈ identity matrix
```

---

## 6.2.2 — Special Matrix Properties

| Property | Meaning | ML Example |
|----------|---------|------------|
| **Symmetric** | A = Aᵀ | Covariance matrix |
| **Positive definite** | xᵀAx > 0 for all x≠0 | Valid covariance matrix |
| **Orthogonal** | AᵀA = I | Rotation matrices; eigenvectors of symmetric matrices |
| **Diagonal** | Only diagonal entries ≠ 0 | Scaling transform |
| **Rank** | Number of linearly independent rows/columns | Feature independence |
| **Determinant** | Scalar measuring "volume scaling" | det=0 → singular → no inverse |

---

## 6.2.3 — Matrix as Data Transformation

Every matrix multiplication is a **linear transformation** — it stretches, rotates, or projects data.

```
Rotation matrix (2D, angle θ):
R = [cos θ  -sin θ]
    [sin θ   cos θ]

x_new = R × x_old  ← rotates point x_old by angle θ

This is how PCA rotates data to align with principal components!
```

---

# Chapter 6.3 — Eigenvalues & Eigenvectors

> **The most important concept in linear algebra for data science. Foundation of PCA, SVD, and dimensionality reduction.**

---

## 💡 Intuition — Special Directions

> **An eigenvector is a special direction that doesn't rotate when a matrix is applied — it only stretches or shrinks.**

**The Rubber Sheet Analogy:**

Imagine a rubber sheet. You stretch it. Most arrows drawn on the sheet change direction when stretched. But a few special arrows (eigenvectors) only get longer or shorter — they keep their direction.

```
Regular vector:   v → Av (direction changes, magnitude changes)
Eigenvector:      v → Av = λv (direction SAME, magnitude scaled by λ)

      v               Av = 3v
      ↑               ↑↑↑   (same direction, 3× longer)
```

---

## 6.3.1 — Definition

$$A\mathbf{v} = \lambda \mathbf{v}$$

| Symbol | Meaning |
|--------|---------|
| A | Square matrix (p×p) |
| **v** | Eigenvector — non-zero vector that only gets scaled |
| λ (lambda) | Eigenvalue — the scaling factor |

A p×p matrix has p eigenvalues (counting multiplicity).

---

## 6.3.2 — Finding Eigenvalues

Solve the **characteristic equation**:

$$\det(A - \lambda I) = 0$$

**Example:** A = [[4,1],[2,3]]

```
det([[4-λ, 1],[2, 3-λ]]) = 0
(4-λ)(3-λ) - 1×2 = 0
12 - 4λ - 3λ + λ² - 2 = 0
λ² - 7λ + 10 = 0
(λ-5)(λ-2) = 0
λ₁ = 5, λ₂ = 2
```

For each eigenvalue, find the eigenvector from (A - λI)v = 0.

---

## 6.3.3 — Properties of Eigenvalues

For symmetric matrices (like covariance matrices):
1. All eigenvalues are **real** (not complex)
2. Eigenvectors for different eigenvalues are **orthogonal**
3. Eigenvalues are always ≥ 0 for positive semi-definite matrices (covariance)

---

## 6.3.4 — PCA: The Key Application

**Principal Component Analysis (PCA) uses eigendecomposition of the covariance matrix:**

```
STEP 1: Center data: X_centered = X - mean(X)

STEP 2: Compute covariance matrix: Σ = (1/n) XᵀX

STEP 3: Find eigenvalues λ₁ ≥ λ₂ ≥ ... ≥ λₚ
        and eigenvectors v₁, v₂, ..., vₚ

STEP 4: Sort by eigenvalue (largest first)
        → v₁ is the direction of MOST variance (1st principal component)
        → v₂ is the direction of 2nd most variance, perpendicular to v₁
        → etc.

STEP 5: Project data: Z = X × [v₁, v₂, ..., vₖ]
        Reduces from p dimensions to k dimensions
```

### Variance Explained

$$\text{Variance explained by PC}_i = \frac{\lambda_i}{\sum_j \lambda_j}$$

```
Example: Eigenvalues = [8.5, 3.2, 1.8, 0.5]
Total variance = 14.0

PC1 explains: 8.5/14 = 60.7%
PC2 explains: 3.2/14 = 22.9%
PC1+PC2: 83.6% of total variance preserved in 2D!
```

```python
from sklearn.decomposition import PCA
import numpy as np

X = np.random.randn(100, 10)  # 100 samples, 10 features
pca = PCA(n_components=2)
X_reduced = pca.fit_transform(X)  # 100 samples, 2 components

print(pca.explained_variance_ratio_)  # variance per component
print(pca.components_)  # eigenvectors (principal components)
```

---

# Chapter 6.4 — Matrix Decomposition (SVD)

> **Singular Value Decomposition: the Swiss Army knife of linear algebra.**

---

## 💡 Intuition

> **SVD says: any matrix can be decomposed into three simpler matrices that together reveal its structure.**

Think of it like factoring a number: 12 = 4 × 3. SVD "factors" a matrix.

$$\mathbf{A} = \mathbf{U} \mathbf{\Sigma} \mathbf{V}^\top$$

| Matrix | Shape | Meaning |
|--------|-------|---------|
| U | m×m | Left singular vectors (directions in row space) |
| Σ (Sigma) | m×n (diagonal) | Singular values (importance of each direction) |
| Vᵀ | n×n | Right singular vectors (directions in column space) |

**Singular values σ₁ ≥ σ₂ ≥ ... ≥ 0** quantify the importance of each "direction" in the data.

---

## 6.4.1 — SVD Visually

```
Any matrix A (m×n):

A   =     U     ×    Σ    ×    Vᵀ
[m×n]   [m×m]    [m×n]      [n×n]

Think of it as:
- Vᵀ: Rotate the input space
- Σ:  Scale along axes (by singular values)
- U:  Rotate the output space

The k-th column of U, k-th singular value σₖ, k-th row of Vᵀ
together form the k-th "concept" in the data.
```

---

## 6.4.2 — Truncated SVD (Low-Rank Approximation)

The magic: Use only the top-k singular values/vectors to get the best rank-k approximation.

$$\mathbf{A} \approx \mathbf{U}_k \mathbf{\Sigma}_k \mathbf{V}_k^\top$$

This gives the best possible approximation of A using only k "dimensions."

```
Original: A = U × Σ × Vᵀ (full, n × p)

σ₁ ≥ σ₂ ≥ σ₃ ≥ ... ≥ σₙ ≥ 0

Keep only top k:
Ã = σ₁u₁v₁ᵀ + σ₂u₂v₂ᵀ + ... + σₖuₖvₖᵀ

Larger σ₁ → more important direction → keep it
Smaller σₙ → noise/redundancy → drop it
```

---

## 6.4.3 — Recommendation Systems (Netflix/Amazon)

**Matrix factorization for collaborative filtering:**

User-Item rating matrix R (millions of users × thousands of movies):
- 99% of entries are missing (users haven't rated most movies)

```
         Movie1  Movie2  Movie3  ...
User1  [  4      ?      2      ...]
User2  [  ?      5      ?      ...]
User3  [  3      ?      ?      ...]
User4  [  ?      ?      4      ...]
...

R ≈ U × Vᵀ (via truncated SVD or matrix factorization)

U: User latent factors (user preferences in k dimensions)
V: Item latent factors (movie characteristics in k dimensions)

Predicted rating for User i, Movie j:
r̂ᵢⱼ = uᵢ · vⱼ (dot product of user and movie vectors)
```

---

## 6.4.4 — Connection Between SVD and PCA

```
PCA via SVD (standard approach):
1. Center data: X_c = X - mean
2. Compute SVD: X_c = UΣVᵀ
3. Principal components = columns of V
4. PC scores = UΣ
5. Variance explained ∝ σᵢ²

This is numerically more stable than eigendecomposing XᵀX directly!
```

---

# 📝 Practice Questions — Part 6

## 🟢 Beginner (10 Questions)

**Q1.** What is a vector? Give an example from a real dataset.
**Q2.** Compute the dot product: u=[1,3,5], v=[2,4,6].
**Q3.** ||v|| for v=[3,4,5]?
**Q4.** Matrix A is 4×3, matrix B is 3×5. What is the size of AB?
**Q5.** What is the transpose of [[1,2,3],[4,5,6]]?
**Q6.** What is an identity matrix? What is AI for any matrix A?
**Q7.** u·v = 0. What does this mean geometrically?
**Q8.** What does a large singular value (σ) indicate about a direction in the data?
**Q9.** In PCA, what do eigenvalues represent?
**Q10.** What is cosine similarity? When is it 1? When is it 0?

## 🟡 Intermediate (10 Questions)

**Q11.** Compute AB: A=[[1,2],[3,4]], B=[[5,6],[7,8]].
**Q12.** A=[[4,1],[2,3]]. Find eigenvalues and verify Av=λv for one eigenvector.
**Q13.** Dataset X is 100×5. Write the formula for the sample covariance matrix Σ using matrix notation.
**Q14.** SVD: A = UΣVᵀ. What does keeping only the top 2 singular values do to the data?
**Q15.** Why does PCA require centering the data first?
**Q16.** If the top 3 eigenvalues explain 85% of variance, what does this mean for dimensionality reduction?
**Q17.** Cosine similarity between "I love cricket" and "cricket is lovely" — are these similar? Would Euclidean distance give a different answer if document lengths differ?
**Q18.** Prove that for orthogonal matrix Q: QᵀQ = I.
**Q19.** What is the rank of a matrix? What does rank < p mean for a dataset with p features?
**Q20.** How is matrix inversion used in linear regression? Write the OLS formula.

## 🔴 Advanced (5 Questions)

**Q21.** Prove that eigenvectors of a symmetric matrix corresponding to different eigenvalues are orthogonal.
**Q22.** Spectral theorem: A symmetric matrix A = QΛQᵀ where Λ is diagonal (eigenvalues) and Q is orthogonal (eigenvectors). Show this implies A = Σᵢ λᵢ vᵢvᵢᵀ.
**Q23.** Eckart-Young theorem: Prove that the rank-k SVD approximation minimizes ||A - B||_F over all rank-k matrices B.
**Q24.** Moore-Penrose pseudoinverse A⁺: How does it solve Ax=b when A is not square or not invertible?
**Q25.** Kernel PCA: How does the kernel trick allow PCA in high-dimensional feature spaces without computing the features explicitly?

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** Image compression: A 1000×1000 grayscale image is a matrix. Using rank-50 SVD approximation, how many numbers do you need to store vs the original? What compression ratio does this achieve?

**DS2.** You apply PCA to a dataset with 50 features. The cumulative explained variance reaches 95% at k=8 components. What would you do next, and what are the trade-offs of choosing k=5 vs k=15?

**DS3.** Word embeddings: Each word is a 300-dimensional vector. "King" - "Man" + "Woman" ≈ "Queen". Explain this using vector arithmetic. What property of word embeddings makes this possible?

**DS4.** A square matrix XᵀX appears in linear regression. When is this matrix singular (non-invertible)? What ML problem does this correspond to?

**DS5.** Matrix factorization for recommendations: User matrix U is 10M×50 and item matrix V is 500K×50. How much memory does this require vs a full 10M×500K rating matrix? What is the compression ratio?

---

# ✅ Key Solutions

**A2.** u·v = 1×2+3×4+5×6 = 2+12+30 = 44.
**A3.** √(9+16+25) = √50 ≈ 7.07.
**A4.** 4×5.
**A5.** [[1,4],[2,5],[3,6]].
**A7.** Orthogonal (perpendicular) — at 90° to each other. No overlap in direction.
**A9.** Each eigenvalue = variance explained along corresponding eigenvector (principal component direction).
**A11.** AB[1,1]=1×5+2×7=19, AB[1,2]=1×6+2×8=22, AB[2,1]=3×5+4×7=43, AB[2,2]=3×6+4×8=50. AB=[[19,22],[43,50]].
**A12.** Eigenvalues found: λ=5, λ=2. For λ=5: (A-5I)v=0 → [[-1,1],[2,-2]]v=0 → v=[1,1] (or any scalar multiple). Verify: A[1,1]=[4+1,2+3]=[5,5]=5[1,1] ✅.
**A20.** β = (XᵀX)⁻¹Xᵀy. Minimizes ||y-Xβ||².

**DS1.** Original: 1000×1000 = 1,000,000 values. Rank-50 SVD: U(1000×50) + Σ(50) + Vᵀ(50×1000) = 50,000+50+50,000 = 100,050 values. Compression ratio ≈ 10:1.

**DS4.** XᵀX is singular when: columns of X are linearly dependent = perfect multicollinearity. In ML: features are perfectly correlated, redundant, or n < p (more features than samples). Solution: Ridge regression adds λI: (XᵀX + λI) is always invertible.

**DS5.** Full matrix: 10M × 500K × 4 bytes = 20 petabytes. Matrix factors: (10M×50 + 500K×50) × 4 bytes = (500M+25M) × 4 = 2.1 GB. Compression ratio ≈ 10,000:1.

---

# 💼 Interview Questions — Part 6

**IQ1.** "Explain PCA intuitively and when you'd use it."
PCA finds the directions of greatest variance in data. It rotates the coordinate system so the first axis captures the most variance, second captures the next most (perpendicular to first), etc. Use it when: reducing dimensionality before training, removing multicollinearity, visualizing high-dimensional data in 2D/3D, or compressing data.

**IQ2.** "What is the difference between SVD and eigendecomposition?"
Eigendecomposition works only on square matrices (A=QΛQ⁻¹). SVD works on ANY matrix (A=UΣVᵀ). For square symmetric matrices (like covariance), they're equivalent: U=V=Q (eigenvectors), Σ diagonal = eigenvalues. SVD is numerically more stable and general.

**IQ3.** "Why does word2vec work? What enables 'King - Man + Woman ≈ Queen'?"
Word2vec trains neural network to predict context words. The network learns embeddings where semantic relationships are encoded as vector directions. 'Royal' vs 'commoner' is one direction; 'gender' is another. Arithmetic in embedding space = arithmetic on these semantic directions.

---

# 📌 Part 6 Summary

## 📐 Formula Sheet

| Formula | Name |
|---------|------|
| u·v = Σuᵢvᵢ | Dot product |
| ||v|| = √(Σvᵢ²) | L2 norm |
| cos(θ) = u·v/(||u||||v||) | Cosine similarity |
| (AB)ᵢⱼ = Σₖ AᵢₖBₖⱼ | Matrix multiplication |
| Av = λv | Eigenvector equation |
| det(A-λI)=0 | Characteristic equation |
| A = UΣVᵀ | SVD |
| λᵢ/Σλⱼ | Variance explained by PCᵢ |
| β = (XᵀX)⁻¹Xᵀy | OLS regression (matrix form) |

---
*Part 6 Complete | Next: Part 7 — Calculus for Data Science*
