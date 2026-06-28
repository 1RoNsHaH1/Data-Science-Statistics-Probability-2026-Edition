# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 7 — Calculus for Data Science
# Chapters 7.1–7.5: Functions & Limits | Derivatives | Partial Derivatives | Gradient Descent | Integrals

---

# Chapter 7.1 — Functions & Limits

## 💡 Intuition — Why Calculus for ML?

Every ML model is a function: f(x) = prediction.
Training a model = finding the function parameters that minimize error.
**Calculus is the mathematics of optimization** — finding minima and maxima.

---

## 7.1.1 — Functions (Review)

A **function** f maps inputs to outputs: y = f(x).

**In ML:**
- Linear regression: f(x) = wᵀx + b
- Neural network: f(x) = σ(W₂σ(W₁x+b₁)+b₂)
- Loss function: L(w) = (y - f(x,w))² — maps weights to error

---

## 7.1.2 — Limits

### 💡 Intuition

> **A limit asks: what value does f(x) approach as x approaches some point?**

```
lim f(x) = L  means:
x→a

"As x gets closer and closer to a (from either side),
f(x) gets closer and closer to L"
```

**Why limits matter:** Some functions are undefined at a point but still approach a value.

**Example:**
```
f(x) = sin(x)/x

At x=0: 0/0 = undefined!

But as x → 0: sin(x)/x → 1

lim sin(x)/x = 1
x→0
```

**Continuity:** f is continuous at a if:
1. f(a) exists
2. lim f(x) exists (as x→a)
3. lim f(x) = f(a)

**ML relevance:** Discontinuous loss functions create problems for gradient descent.

---

# Chapter 7.2 — Derivatives

> **The rate of change — how fast is the function changing at this point?**

---

## 💡 Intuition

You're driving. Your position changes over time. The **derivative** is your **speedometer** — it tells you how fast your position is changing at any instant.

```
Position function:  s(t) = distance at time t
Derivative:         s'(t) = ds/dt = speed at time t

If s(t) = t²:
  s(1) = 1 meter, s(2) = 4 meters, s(3) = 9 meters
  Speed at t=2: s'(2) = 4 m/s (answer via derivative)
```

---

## 7.2.1 — Geometric Interpretation

The derivative at a point = the **slope of the tangent line** to the curve at that point.

```
f(x)
   |         /
   |        * ← tangent line at x₀
   |       /|
   |      / |
   |     /  |
   |____/   |
   |        x₀
   
Slope of tangent = f'(x₀)
Positive slope: f is increasing at x₀
Negative slope: f is decreasing at x₀
Zero slope: f is at a minimum or maximum at x₀ ← KEY FOR OPTIMIZATION!
```

---

## 7.2.2 — Formal Definition

$$f'(x) = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}$$

This is the slope of increasingly narrow secant lines converging to the tangent.

---

## 7.2.3 — Derivative Rules

| Rule | Formula | Example |
|------|---------|---------|
| Constant | d/dx(c) = 0 | d/dx(5) = 0 |
| Power rule | d/dx(xⁿ) = nxⁿ⁻¹ | d/dx(x³) = 3x² |
| Sum rule | d/dx(f+g) = f'+g' | d/dx(x²+x) = 2x+1 |
| Product rule | d/dx(fg) = f'g + fg' | d/dx(x·eˣ) = eˣ + xeˣ |
| Chain rule | d/dx(f(g(x))) = f'(g(x))·g'(x) | d/dx(sin(x²)) = cos(x²)·2x |
| Exponential | d/dx(eˣ) = eˣ | eˣ is its own derivative! |
| Log | d/dx(ln x) = 1/x | Used in log-likelihood |
| Sigmoid | dσ/dx = σ(x)(1-σ(x)) | Key for backprop |

---

## 7.2.4 — Key Derivatives for ML

```
Sigmoid: σ(x) = 1/(1+e^(-x))
σ'(x) = σ(x)(1-σ(x))

ReLU: f(x) = max(0,x)
f'(x) = 0 if x<0, 1 if x>0  (undefined at x=0)

Loss (MSE): L = (y-ŷ)²
dL/dŷ = -2(y-ŷ)

Log loss: L = -y·log(p) - (1-y)·log(1-p)
dL/dp = -y/p + (1-y)/(1-p)
```

---

## 7.2.5 — Finding Minima and Maxima

At a **critical point**, f'(x) = 0.

- f''(x) > 0 at critical point → **local minimum** (curve is concave up)
- f''(x) < 0 at critical point → **local maximum** (curve is concave down)
- f''(x) = 0 → **inflection point** (need more analysis)

```
         Minimum                 Maximum
         ─────                   ─────

            •  ← f'=0, f''>0        •  ← f'=0, f''<0
           / \                      / \
          /   \                    /   \
         ↙     ↘                  ↙     ↘
      f decreasing then           f increasing then
      increasing                  decreasing
```

---

# Chapter 7.3 — Partial Derivatives

> **When your function has MULTIPLE inputs, you take derivatives one at a time.**

---

## 💡 Intuition

An ML model has thousands of weights. The loss function L depends on ALL weights: L(w₁, w₂, ..., wₙ).

To minimize L, we need to know: "How does L change if I change just w₁ a little bit, keeping everything else fixed?"

That's a **partial derivative**.

---

## 7.3.1 — Definition

$$\frac{\partial f}{\partial x_i} = \lim_{h \to 0} \frac{f(x_1, \ldots, x_i+h, \ldots, x_n) - f(x_1, \ldots, x_n)}{h}$$

The ∂ (curly d) symbol means "partial" — we're differentiating with respect to one variable while holding all others **constant**.

---

## 7.3.2 — Computation

**Example:** f(x,y) = x²y + 3xy²

```
∂f/∂x: treat y as constant, differentiate with respect to x
  ∂f/∂x = 2xy + 3y²

∂f/∂y: treat x as constant, differentiate with respect to y
  ∂f/∂y = x² + 6xy
```

---

## 7.3.3 — The Gradient

> **The gradient is the vector of ALL partial derivatives — it points in the direction of steepest ascent.**

$$\nabla f = \left(\frac{\partial f}{\partial x_1}, \frac{\partial f}{\partial x_2}, \ldots, \frac{\partial f}{\partial x_n}\right)$$

```
For f(x,y) = x²y + 3xy²:
∇f = (2xy + 3y², x² + 6xy)

At point (1,2):
∇f = (2(1)(2)+3(4), 1+6(2)) = (4+12, 1+12) = (16, 13)

This vector (16,13) points in the direction of STEEPEST INCREASE.
To DECREASE f: move in direction -(16,13)
```

---

# Chapter 7.4 — Gradient Descent

> **The most important optimization algorithm in machine learning.**

---

## 💡 Intuition — The Hiking Analogy

You're lost in mountains in thick fog. You want to reach the valley (minimum). You can't see far. What do you do?

**Strategy:** At each step, feel the ground around you and step in the **steepest downhill direction**.

Repeat until you reach the bottom.

This is **gradient descent** — the algorithm that trains virtually every ML model.

```
LOSS SURFACE (2D simplified):

L(w)
│          *  ← starting point (random init)
│        ↙
│      ↙  ← each step goes downhill
│    ↙
│  * ← we converge to minimum
│
└──────────────── w (weight)
```

---

## 7.4.1 — The Algorithm

$$w_{\text{new}} = w_{\text{old}} - \alpha \cdot \nabla L(w_{\text{old}})$$

| Symbol | Meaning |
|--------|---------|
| w | Model weights (parameters) |
| α (alpha) | Learning rate — step size |
| ∇L(w) | Gradient of loss w.r.t. w |
| − | Subtract = go DOWNHILL (negative gradient direction) |

---

## 7.4.2 — Learning Rate

```
TOO SMALL α:                TOO LARGE α:           JUST RIGHT:
L |    .                    L |.    ← overshoot    L |   .
  |   .                       |  .                   |  .
  |  .                        |   .                  | .
  |  .                        |  .                   |.
  |  .   ← slow              |.  ← diverge         |* ← converge fast
  └────                       └────                  └────

Many iterations,             Bounces around or      Good balance
very slow convergence        may explode            of speed and stability
```

**Typical values:** α ∈ {0.1, 0.01, 0.001, 0.0001}

**Adaptive learning rates:** Adam, RMSprop, Adagrad automatically adjust α.

---

## 7.4.3 — Variants of Gradient Descent

| Variant | How It Works | Pros | Cons |
|---------|-------------|------|------|
| **Batch GD** | Use ALL data per step | Stable, accurate gradient | Slow for large datasets |
| **Stochastic GD (SGD)** | Use ONE random sample per step | Fast updates | Noisy, erratic path |
| **Mini-batch GD** | Use k samples (e.g., 32, 64, 128) | Balance of speed and stability | Most common in practice |

---

## 7.4.4 — Challenges

**Local minima:** Gradient descent finds a minimum, but not guaranteed to be the GLOBAL minimum.

```
L(w)
│    ↗ global min      local min ↘  ↗ saddle point
│   *─────────────────────* ───────*
│                          ↓ GD might get stuck here
│
└──────────────────────────────── w

Solution: Random restarts, momentum, Adam optimizer
```

**Saddle points:** f'=0 but not a minimum — very common in high-dimensional spaces (neural networks).

**Vanishing gradients:** In deep networks, gradients become tiny during backprop → weights in early layers barely update. Solution: ReLU, batch normalization, residual connections.

---

## 7.4.5 — Backpropagation (Chain Rule in Action)

Neural network loss L depends on outputs, which depend on hidden layers, which depend on weights.

**Backpropagation** uses the **chain rule** to compute ∂L/∂w for every weight.

```
Forward pass: x → h₁ → h₂ → ŷ → L
              (compute predictions)

Backward pass: ∂L/∂w₂ = ∂L/∂ŷ × ∂ŷ/∂h₂ × ∂h₂/∂w₂
               (chain rule — propagate gradients backwards)

Then update: w₂ ← w₂ - α × ∂L/∂w₂
```

---

# Chapter 7.5 — Integrals

> **The area under the curve — fundamental to probability.**

---

## 💡 Intuition

**Derivative:** The slope at one point (instantaneous rate of change)
**Integral:** The total area under the curve (accumulated change)

They are **inverses** of each other:

```
∫ f'(x) dx = f(x) + C   (Fundamental Theorem of Calculus)
d/dx ∫f(x)dx = f(x)
```

---

## 7.5.1 — Geometric Interpretation

$$\int_a^b f(x)\, dx = \text{Area between curve and x-axis from a to b}$$

```
f(x)
│    ██████████
│   ████████████
│  ██████████████
│ ████████████████
└──────────────────── x
   a               b

Total shaded area = ∫ₐᵇ f(x)dx
```

If f(x) < 0, that area is subtracted (below the x-axis).

---

## 7.5.2 — Basic Integration Rules

| Rule | Formula |
|------|---------|
| Constant | ∫c dx = cx + C |
| Power | ∫xⁿ dx = xⁿ⁺¹/(n+1) + C |
| Exponential | ∫eˣ dx = eˣ + C |
| Natural log | ∫(1/x) dx = ln\|x\| + C |
| Substitution | ∫f(g(x))g'(x)dx = F(g(x)) + C |

---

## 7.5.3 — Integrals and Probability

For continuous random variables, **probability = area under PDF**:

$$P(a \leq X \leq b) = \int_a^b f(x)\, dx$$

**CDF is an integral of the PDF:**
$$F(x) = P(X \leq x) = \int_{-\infty}^x f(t)\, dt$$

**Expected value uses integration:**
$$E[X] = \int_{-\infty}^{\infty} x \cdot f(x)\, dx$$

---

## 7.5.4 — The Normal Distribution and Integration

The normal PDF integrates to 1:

$$\int_{-\infty}^{\infty} \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}} dx = 1$$

There's no closed form for ∫ₐᵇ of the normal PDF — that's why we use **z-tables** or software.

---

## 7.5.5 — Monte Carlo Integration

When integrals are too hard analytically, use **Monte Carlo:**

```
To compute ∫ₐᵇ f(x)dx:

1. Sample x₁, x₂, ..., xₙ uniformly from [a,b]
2. Approximate: ∫ₐᵇ f(x)dx ≈ (b-a) × (1/n)Σf(xᵢ)

The sample average of f(xᵢ) × interval width ≈ integral

This is the Law of Large Numbers applied to integration!
```

Used in: Bayesian inference, pricing financial derivatives, physics simulations.

---

# 📝 Practice Questions — Part 7

## 🟢 Beginner (10 Questions)

**Q1.** What does the derivative of f at point x tell you?
**Q2.** Find d/dx(3x² + 5x - 2).
**Q3.** Find d/dx(eˣ + ln x).
**Q4.** f(x,y) = 3x²y. Find ∂f/∂x and ∂f/∂y.
**Q5.** What is the gradient of f(x,y) = x² + y²?
**Q6.** If f'(x₀) = 0 and f''(x₀) > 0, is x₀ a min or max?
**Q7.** In gradient descent, why do we subtract the gradient?
**Q8.** What is a learning rate? What happens if it's too large?
**Q9.** ∫x² dx = ?
**Q10.** P(0 ≤ X ≤ 1) for Uniform(0,2) using integration.

## 🟡 Intermediate (10 Questions)

**Q11.** Using the chain rule: d/dx[sin(x³)].
**Q12.** Find the minimum of f(x) = x² - 6x + 8 using calculus.
**Q13.** f(x,y) = x²y² + xy. Compute ∇f at point (1,2).
**Q14.** One step of gradient descent: f(x)=x²-4x+4, x₀=5, α=0.1. Find x₁.
**Q15.** Derive the update rule for logistic regression weights using the gradient of log-loss.
**Q16.** What is the difference between batch, mini-batch, and stochastic gradient descent?
**Q17.** E[X] for Exponential(λ) = ∫₀^∞ x·λe^(-λx)dx. Compute this integral.
**Q18.** Vanishing gradients: why does tanh activation lead to this problem in deep networks?
**Q19.** d/dx[log(σ(x))] where σ(x)=1/(1+e^(-x)). [Needed for logistic regression log-likelihood]
**Q20.** The normal PDF integrates to 1. Verify that a standard normal N(0,1) has mean=0 using the definition E[X]=∫xf(x)dx.

## 🔴 Advanced (5 Questions)

**Q21.** Second-order optimization (Newton's method): w_new = w - H⁻¹∇L where H is the Hessian. Why is this faster than GD but rarely used in deep learning?

**Q22.** Derive the backpropagation update for a two-layer neural network with sigmoid activations.

**Q23.** KL divergence: D_KL(P||Q) = ∫P(x)log(P(x)/Q(x))dx. Show this is always ≥ 0 (Gibbs' inequality).

**Q24.** Expectation-Maximization (EM) algorithm: how does the E-step use integration/expectation and M-step use optimization?

**Q25.** Adam optimizer: Explain the role of first moment (momentum) and second moment (adaptive learning rate). Write the update equations.

## 🚀 Data Science Scenarios (5 Questions)

**DS1.** Linear regression loss: L(w,b) = (1/n)Σ(yᵢ-wxᵢ-b)². Compute ∂L/∂w and ∂L/∂b. Show these are the gradient components used in gradient descent.

**DS2.** A neural network training curve shows loss decreasing then plateauing. Plot what the loss landscape might look like and explain what might be happening (saddle point, local minimum, wrong learning rate).

**DS3.** You're training a model with Adam optimizer with α=0.001. After 1000 epochs, loss is still decreasing slowly. What would you try? List 3 specific interventions.

**DS4.** Monte Carlo estimation: You want to compute ∫₀¹ eˣ² dx (no closed form). Describe the Monte Carlo algorithm and estimate the result using 5 samples: x=[0.1,0.3,0.5,0.7,0.9].

**DS5.** Compute the gradient of cross-entropy loss with respect to softmax output. Show why softmax + cross-entropy has a clean gradient: ∂L/∂zᵢ = p̂ᵢ - yᵢ.

---

# ✅ Key Solutions

**A2.** 6x + 5.
**A3.** eˣ + 1/x.
**A4.** ∂f/∂x = 6xy. ∂f/∂y = 3x².
**A5.** ∇f = (2x, 2y). At any point (x,y), gradient points radially outward.
**A6.** Minimum (bowl shape — concave up).
**A7.** Gradient points uphill. Subtract it to go downhill (minimize loss).
**A9.** x³/3 + C.
**A10.** P(0≤X≤1) = ∫₀¹ (1/2)dx = [x/2]₀¹ = 1/2.
**A11.** cos(x³)·3x².
**A12.** f'(x)=2x-6=0 → x=3. f''(3)=2>0 → minimum. f(3)=9-18+8=-1.
**A13.** ∂f/∂x = 2xy²+y = 2(1)(4)+2=10. ∂f/∂y = 2x²y+x=4+1=5. ∇f=(10,5).
**A14.** f'(x)=2x-4. f'(5)=6. x₁=5-0.1×6=4.4.
**A17.** ∫₀^∞ xλe^(-λx)dx. Integration by parts: = 1/λ. E[X]=1/λ ✅.
**DS1.** ∂L/∂w = (-2/n)Σxᵢ(yᵢ-wxᵢ-b). ∂L/∂b = (-2/n)Σ(yᵢ-wxᵢ-b). GD update: w ← w - α·∂L/∂w, b ← b - α·∂L/∂b.
**DS4.** Monte Carlo: sample 5 values, compute average of eˣ², multiply by interval length (1). e^(0.01)+e^(0.09)+e^(0.25)+e^(0.49)+e^(0.81) = 1.010+1.094+1.284+1.632+2.248 = 7.268. Average=1.454. Multiply by 1 → estimate ≈ 1.454. (True ≈ 1.463.)

---

# 💼 Interview Questions — Part 7

**IQ1.** "Explain gradient descent to a non-technical interviewer."
"Imagine you're in hilly terrain and want to find the lowest valley. You're blindfolded. You can only feel the slope of the ground. So you always step in whichever direction goes downhill. Gradient descent does the same thing with model parameters — it repeatedly adjusts them in the direction that reduces prediction error."

**IQ2.** "What is backpropagation?"
The algorithm for computing gradients in a neural network. Uses the chain rule to propagate error signals backward from output to input layers. Each layer's gradient = product of downstream gradient × local derivative. Enables efficient computation of ∂L/∂w for every weight in O(n) time.

**IQ3.** "Why does Adam optimizer often outperform plain SGD?"
Adam tracks: (1) first moment — exponential moving average of gradients (momentum — helps escape local minima and navigate saddle points) and (2) second moment — exponential moving average of squared gradients (per-parameter learning rate scaling — large gradients get smaller steps, small gradients get larger steps). This adaptive per-parameter learning rate handles sparse gradients and different learning dynamics per parameter.

---

# 📌 Part 7 Summary

## 📐 Formula Sheet

| Formula | Name |
|---------|------|
| f'(x) = lim[f(x+h)-f(x)]/h | Derivative definition |
| d/dx(xⁿ) = nxⁿ⁻¹ | Power rule |
| d/dx(f(g(x))) = f'(g(x))g'(x) | Chain rule |
| ∂f/∂xᵢ | Partial derivative |
| ∇f = (∂f/∂x₁,...,∂f/∂xₙ) | Gradient vector |
| w ← w - α∇L(w) | Gradient descent update |
| ∫xⁿdx = xⁿ⁺¹/(n+1)+C | Power integration |
| P(a≤X≤b) = ∫ₐᵇ f(x)dx | Probability via integration |
| E[X] = ∫x·f(x)dx | Expected value (continuous) |

## ⚠️ Common Mistakes

| Mistake | Correction |
|---------|-----------|
| d/dx(x²) = x (forgetting power rule) | d/dx(x²) = 2x |
| ∂/∂x(xy²) = y² (forgetting to treat x as variable) | ∂/∂x(xy²) = y² ✅ (y is constant) |
| Choosing too large α | Model diverges; try 10× smaller |
| Stopping too early | Loss still decreasing — train longer |
| Using batch GD on huge datasets | Use mini-batch instead |

---
*Part 7 Complete | Next: Part 8 — Statistics for Machine Learning*
