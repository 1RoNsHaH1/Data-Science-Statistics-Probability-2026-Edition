# 📚 Data Science — Statistics & Probability (2026 Edition)
# PART 11 — Practical Data Science Statistics
# Chapters 11.1–11.4

---

# Chapter 11.1 — Python for Statistics

> **Every statistical concept from this course, translated into Python code.**

---

## 11.1.1 — NumPy: The Statistical Foundation

```python
import numpy as np

data = np.array([23, 45, 12, 67, 34, 89, 56, 44, 78, 32])

# === DESCRIPTIVE STATISTICS ===
print(f"Mean:     {np.mean(data):.2f}")        # 48.0
print(f"Median:   {np.median(data):.2f}")      # 44.5
print(f"Std Dev:  {np.std(data):.2f}")         # population (n)
print(f"Std Dev:  {np.std(data, ddof=1):.2f}") # sample (n-1)
print(f"Variance: {np.var(data, ddof=1):.2f}") # sample variance
print(f"Min:      {np.min(data)}")
print(f"Max:      {np.max(data)}")
print(f"Range:    {np.ptp(data)}")             # peak-to-peak = max - min

# === PERCENTILES ===
q1 = np.percentile(data, 25)    # Q1
q3 = np.percentile(data, 75)    # Q3
iqr = q3 - q1
print(f"Q1={q1}, Q3={q3}, IQR={iqr}")

# === CORRELATION ===
x = np.array([1,2,3,4,5])
y = np.array([2,4,5,4,5])
r = np.corrcoef(x, y)[0, 1]   # Pearson correlation
print(f"Correlation: {r:.3f}")

# === PROBABILITY DISTRIBUTIONS ===
# Normal distribution
from numpy.random import default_rng
rng = default_rng(42)
samples = rng.normal(loc=70, scale=10, size=1000)   # N(70,100)
binom = rng.binomial(n=10, p=0.3, size=500)         # Binomial(10,0.3)
poisson = rng.poisson(lam=3, size=500)              # Poisson(3)
```

---

## 11.1.2 — SciPy: Statistical Tests

```python
from scipy import stats

# === Z-TEST (using norm) ===
# H0: mu=70, sample: n=50, x_bar=74, sigma=12
z_stat = (74 - 70) / (12 / np.sqrt(50))
p_value = 2 * (1 - stats.norm.cdf(abs(z_stat)))   # two-tailed
print(f"Z = {z_stat:.3f}, p = {p_value:.4f}")

# === ONE-SAMPLE T-TEST ===
sample = [70, 75, 72, 68, 80, 73, 77, 71, 69, 74]
t_stat, p_val = stats.ttest_1samp(sample, popmean=70)
print(f"T = {t_stat:.3f}, p = {p_val:.4f}")

# === TWO-SAMPLE T-TEST (Welch's) ===
group_a = [72, 68, 75, 80, 71, 73]
group_b = [85, 88, 82, 90, 87, 84]
t_stat, p_val = stats.ttest_ind(group_a, group_b, equal_var=False)
print(f"Welch T = {t_stat:.3f}, p = {p_val:.4f}")

# === PAIRED T-TEST ===
before = [65, 72, 58, 80, 75, 68, 82, 70]
after  = [70, 78, 65, 82, 80, 75, 85, 78]
t_stat, p_val = stats.ttest_rel(before, after)
print(f"Paired T = {t_stat:.3f}, p = {p_val:.4f}")

# === CHI-SQUARE TEST ===
observed = np.array([[80, 70], [60, 90]])   # 2x2 contingency table
chi2, p_val, dof, expected = stats.chi2_contingency(observed)
print(f"Chi2 = {chi2:.3f}, p = {p_val:.4f}, df = {dof}")

# === ONE-WAY ANOVA ===
group1 = [70, 75, 72, 68, 80]
group2 = [85, 88, 90, 82, 87]
group3 = [65, 70, 68, 72, 69]
f_stat, p_val = stats.f_oneway(group1, group2, group3)
print(f"F = {f_stat:.3f}, p = {p_val:.4f}")

# === CONFIDENCE INTERVAL ===
data = np.array([23, 45, 12, 67, 34, 89, 56, 44, 78, 32])
n = len(data)
mean = np.mean(data)
se = stats.sem(data)                                # standard error
ci = stats.t.interval(0.95, df=n-1, loc=mean, scale=se)
print(f"95% CI: ({ci[0]:.2f}, {ci[1]:.2f})")

# === NORMALITY TESTS ===
stat, p = stats.shapiro(data)   # Shapiro-Wilk (best for n<50)
print(f"Shapiro-Wilk: W={stat:.4f}, p={p:.4f}")
# p > 0.05 → cannot reject normality

stat, p = stats.normaltest(data)  # D'Agostino-Pearson (skewness + kurtosis)
print(f"Normal test: stat={stat:.4f}, p={p:.4f}")

# === CORRELATION TESTS ===
r_pearson, p_pearson = stats.pearsonr(x, y)
r_spearman, p_spearman = stats.spearmanr(x, y)
print(f"Pearson: r={r_pearson:.3f}, p={p_pearson:.4f}")
print(f"Spearman: r={r_spearman:.3f}, p={p_spearman:.4f}")

# === DISTRIBUTION FITTING ===
params = stats.norm.fit(data)  # fit normal distribution
mu, sigma = params
print(f"Fitted Normal: mu={mu:.2f}, sigma={sigma:.2f}")

# === KS TEST (distribution comparison) ===
dist1 = np.random.normal(0, 1, 500)
dist2 = np.random.normal(0.5, 1, 500)
ks_stat, p_val = stats.ks_2samp(dist1, dist2)
print(f"KS = {ks_stat:.3f}, p = {p_val:.4f}")
```

---

## 11.1.3 — Pandas: Statistical Data Analysis

```python
import pandas as pd

# Load data
df = pd.read_csv('dataset.csv')

# === SUMMARY STATISTICS ===
print(df.describe())                    # full summary
print(df['salary'].describe())          # single column
print(df.skew())                        # skewness per column
print(df.kurtosis())                    # kurtosis per column

# === MISSING VALUES ===
print(df.isnull().sum())                # count missing per column
print(df.isnull().mean() * 100)        # % missing per column
df['salary'].fillna(df['salary'].median(), inplace=True)  # impute

# === GROUPBY STATISTICS ===
print(df.groupby('department')['salary'].agg(['mean','median','std','count']))

# === CORRELATION MATRIX ===
corr_matrix = df.corr(method='pearson')   # or 'spearman', 'kendall'
print(corr_matrix)

# === OUTLIER DETECTION (IQR) ===
Q1 = df['salary'].quantile(0.25)
Q3 = df['salary'].quantile(0.75)
IQR = Q3 - Q1
lower, upper = Q1 - 1.5*IQR, Q3 + 1.5*IQR
outliers = df[(df['salary'] < lower) | (df['salary'] > upper)]
print(f"Found {len(outliers)} outliers")

# === FEATURE ENGINEERING ===
from sklearn.preprocessing import StandardScaler, MinMaxScaler, RobustScaler

scaler = StandardScaler()
df['salary_scaled'] = scaler.fit_transform(df[['salary']])

# One-hot encoding
df_encoded = pd.get_dummies(df, columns=['department'], drop_first=True)

# Log transformation
df['salary_log'] = np.log1p(df['salary'])   # log(1+x) handles zeros

# Binning
df['age_group'] = pd.cut(df['age'], bins=[0,25,35,50,100], 
                          labels=['Young','Mid','Senior','Elder'])

# Cross-tabulation
print(pd.crosstab(df['gender'], df['department'], normalize='index'))
```

---

## 11.1.4 — Statsmodels: Advanced Statistics

```python
import statsmodels.api as sm
import statsmodels.formula.api as smf

# === ORDINARY LEAST SQUARES (OLS) REGRESSION ===
model = smf.ols('salary ~ age + experience + C(department)', data=df)
result = model.fit()
print(result.summary())
# Key outputs: coefficients, p-values, R², F-statistic, AIC

# === LOGISTIC REGRESSION ===
model = smf.logit('churned ~ age + income + tenure', data=df)
result = model.fit()
print(result.summary())
print(f"Odds ratios: {np.exp(result.params)}")

# === TIME SERIES (ARIMA) ===
from statsmodels.tsa.arima.model import ARIMA

model = ARIMA(ts_data, order=(2,1,1))
result = model.fit()
forecast = result.forecast(steps=12)   # 12-period forecast
conf_int = result.get_forecast(steps=12).conf_int()

# === VARIANCE INFLATION FACTOR (multicollinearity) ===
from statsmodels.stats.outliers_influence import variance_inflation_factor
vif_data = pd.DataFrame()
vif_data["feature"] = X.columns
vif_data["VIF"] = [variance_inflation_factor(X.values, i) for i in range(X.shape[1])]
print(vif_data)  # VIF > 10 → multicollinearity problem

# === POWER ANALYSIS ===
from statsmodels.stats.power import TTestPower
analysis = TTestPower()
n = analysis.solve_power(effect_size=0.5, alpha=0.05, power=0.8)
print(f"Required sample size: {n:.0f}")
```

---

# Chapter 11.2 — Visualization Tools

> **Seeing the statistics — from exploration to communication.**

---

## 11.2.1 — Matplotlib: Full Control

```python
import matplotlib.pyplot as plt
import numpy as np

fig, axes = plt.subplots(2, 3, figsize=(15, 10))

# --- HISTOGRAM ---
ax = axes[0, 0]
data = np.random.normal(70, 10, 500)
ax.hist(data, bins=30, edgecolor='black', color='steelblue', alpha=0.7)
ax.axvline(np.mean(data), color='red', linestyle='--', label=f'Mean={np.mean(data):.1f}')
ax.axvline(np.median(data), color='green', linestyle='--', label=f'Median={np.median(data):.1f}')
ax.set_xlabel('Score'); ax.set_ylabel('Frequency')
ax.set_title('Score Distribution'); ax.legend()

# --- NORMAL DISTRIBUTION VISUALIZATION ---
ax = axes[0, 1]
x = np.linspace(40, 100, 300)
mu, sigma = 70, 10
y = (1/(sigma*np.sqrt(2*np.pi))) * np.exp(-0.5*((x-mu)/sigma)**2)
ax.plot(x, y, 'b-', linewidth=2)
ax.fill_between(x, y, where=(x >= mu-sigma) & (x <= mu+sigma), 
                alpha=0.3, color='blue', label='68%')
ax.fill_between(x, y, where=(x >= mu-2*sigma) & (x <= mu+2*sigma), 
                alpha=0.2, color='green', label='95%')
ax.set_title('Normal Distribution'); ax.legend()

# --- SCATTER PLOT WITH REGRESSION LINE ---
ax = axes[0, 2]
x = np.random.normal(5, 2, 100)
y = 2*x + np.random.normal(0, 2, 100)
ax.scatter(x, y, alpha=0.6, color='navy')
m, b = np.polyfit(x, y, 1)
ax.plot(x, m*x + b, 'r-', linewidth=2, label=f'y={m:.1f}x+{b:.1f}')
ax.set_xlabel('X'); ax.set_ylabel('Y')
ax.set_title('Scatter with Regression'); ax.legend()

# --- BOX PLOT ---
ax = axes[1, 0]
groups = [np.random.normal(70, 10, 100), 
          np.random.normal(80, 15, 100), 
          np.random.normal(65, 8, 100)]
ax.boxplot(groups, labels=['Group A', 'Group B', 'Group C'])
ax.set_title('Score Comparison by Group')

# --- CONFIDENCE INTERVAL PLOT ---
ax = axes[1, 1]
categories = ['Method A', 'Method B', 'Method C', 'Method D']
means = [72, 85, 68, 79]
errors = [3, 4, 2.5, 3.5]  # 95% CI half-widths
ax.errorbar(categories, means, yerr=errors, fmt='o', capsize=5, 
            markersize=8, linewidth=2)
ax.axhline(y=75, color='red', linestyle='--', label='Baseline')
ax.set_title('Means with 95% CI'); ax.legend()

# --- POWER CURVE ---
ax = axes[1, 2]
from scipy import stats
n_values = np.arange(10, 200, 5)
effect_sizes = [0.2, 0.5, 0.8]
for d in effect_sizes:
    power = [stats.norm.cdf(abs(d)*np.sqrt(n/2) - stats.norm.ppf(0.975)) + 
             stats.norm.cdf(-abs(d)*np.sqrt(n/2) - stats.norm.ppf(0.975)) 
             for n in n_values]
    ax.plot(n_values, power, label=f'd={d}')
ax.axhline(0.8, color='black', linestyle='--', label='80% power')
ax.set_xlabel('Sample Size (per group)'); ax.set_ylabel('Power')
ax.set_title('Power Curves'); ax.legend()

plt.tight_layout()
plt.savefig('statistical_plots.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## 11.2.2 — Seaborn: Statistical Visualization

```python
import seaborn as sns
import pandas as pd

# Set theme
sns.set_theme(style='whitegrid', palette='husl')

# --- KDE PLOT (comparing distributions) ---
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

# Load built-in dataset
tips = sns.load_dataset('tips')
iris = sns.load_dataset('iris')
titanic = sns.load_dataset('titanic')

# KDE by group
sns.kdeplot(data=tips, x='total_bill', hue='time', fill=True, ax=axes[0])
axes[0].set_title('Bill Distribution by Meal Time')

# Violin plot
sns.violinplot(data=tips, x='day', y='total_bill', hue='sex', 
               split=True, ax=axes[1])
axes[1].set_title('Bill by Day and Gender')

# Regression plot
sns.regplot(data=tips, x='total_bill', y='tip', 
            scatter_kws={'alpha':0.5}, ax=axes[2])
axes[2].set_title('Tip vs Bill with Regression')

plt.tight_layout(); plt.show()

# --- PAIR PLOT (all pairwise relationships) ---
g = sns.pairplot(iris, hue='species', diag_kind='kde')
g.fig.suptitle('Iris Dataset Pair Plot', y=1.02)
plt.show()

# --- HEATMAP (correlation matrix) ---
plt.figure(figsize=(10, 8))
corr = tips.select_dtypes(include='number').corr()
mask = np.triu(np.ones_like(corr, dtype=bool))  # hide upper triangle
sns.heatmap(corr, mask=mask, annot=True, fmt='.2f', 
            cmap='RdYlGn', center=0, vmin=-1, vmax=1,
            linewidths=0.5)
plt.title('Correlation Matrix'); plt.show()

# --- FACET GRID (multiple subplots by category) ---
g = sns.FacetGrid(tips, col='time', row='sex', margin_titles=True)
g.map_dataframe(sns.histplot, x='total_bill', bins=15)
g.set_axis_labels('Total Bill', 'Count')
g.set_titles(col_template='{col_name}', row_template='{row_name}')
plt.show()

# --- JOINT PLOT (scatter + marginal distributions) ---
sns.jointplot(data=tips, x='total_bill', y='tip', 
              kind='reg',           # 'scatter', 'hex', 'kde', 'reg', 'resid'
              marginal_kws={'bins':20})
plt.show()
```

---

## 11.2.3 — Plotly: Interactive Visualizations

```python
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots

# --- INTERACTIVE SCATTER ---
fig = px.scatter(tips, x='total_bill', y='tip', 
                 color='time', size='size',
                 hover_data=['day', 'sex'],
                 trendline='ols',
                 title='Tips vs Total Bill')
fig.show()

# --- ANIMATED TIME SERIES ---
gapminder = px.data.gapminder()
fig = px.scatter(gapminder, x='gdpPercap', y='lifeExp', 
                 animation_frame='year', animation_group='country',
                 size='pop', color='continent', hover_name='country',
                 log_x=True, size_max=55,
                 range_x=[100,100000], range_y=[25,90])
fig.show()

# --- INTERACTIVE HISTOGRAM WITH STATS ---
data = np.random.normal(70, 10, 1000)
fig = go.Figure()
fig.add_trace(go.Histogram(x=data, nbinsx=30, name='Scores',
                            histnorm='probability density'))

# Overlay normal PDF
x_range = np.linspace(data.min(), data.max(), 100)
from scipy.stats import norm
fig.add_trace(go.Scatter(x=x_range, 
                          y=norm.pdf(x_range, data.mean(), data.std()),
                          mode='lines', name='Normal PDF', line=dict(color='red', width=3)))
fig.update_layout(title='Score Distribution with Fitted Normal')
fig.show()

# --- CONFIDENCE INTERVAL PLOT ---
methods = ['Logistic', 'RF', 'XGBoost', 'Neural Net', 'Ensemble']
accuracies = [0.845, 0.872, 0.889, 0.876, 0.901]
lower = [0.830, 0.855, 0.874, 0.860, 0.888]
upper = [0.860, 0.889, 0.904, 0.892, 0.914]

fig = go.Figure()
fig.add_trace(go.Scatter(x=methods, y=accuracies, mode='markers',
                          marker=dict(size=10),
                          error_y=dict(type='data', 
                                       array=[u-a for u,a in zip(upper,accuracies)],
                                       arrayminus=[a-l for a,l in zip(accuracies,lower)])))
fig.update_layout(title='Model Accuracy with 95% Confidence Intervals',
                  yaxis_title='Accuracy', yaxis_range=[0.8, 0.95])
fig.show()
```

---

# Chapter 11.3 — Real-World Case Studies

---

## Case Study 1 — Healthcare: ICU Readmission Prediction

**Problem:** Hospital wants to predict which patients will be readmitted to ICU within 30 days.

**Statistical Challenges:**
- Class imbalance: only 12% readmitted (88% not)
- Missing data: 23% of lab values missing (often because tests weren't ordered)
- Time series features: vitals measured hourly

**Statistical Workflow:**

```python
import pandas as pd
import numpy as np
from scipy import stats
from sklearn.model_selection import StratifiedKFold
from sklearn.metrics import roc_auc_score, brier_score_loss
from sklearn.calibration import CalibrationDisplay

# 1. EDA — Understand distributions
print("Readmission rate:", df['readmitted'].mean())  # 0.12

# Statistical comparison: readmitted vs not
for col in numeric_features:
    group_1 = df[df['readmitted']==1][col].dropna()
    group_0 = df[df['readmitted']==0][col].dropna()
    
    # Normality test
    _, p_norm = stats.shapiro(group_1[:50])
    
    if p_norm > 0.05:  # normal → t-test
        t_stat, p_val = stats.ttest_ind(group_1, group_0, equal_var=False)
    else:             # non-normal → Mann-Whitney
        t_stat, p_val = stats.mannwhitneyu(group_1, group_0)
    
    print(f"{col}: stat={t_stat:.3f}, p={p_val:.4f} {'***' if p_val<0.001 else '**' if p_val<0.01 else '*' if p_val<0.05 else ''}")

# 2. Handle class imbalance
from imblearn.over_sampling import SMOTE
sm = SMOTE(random_state=42)
X_res, y_res = sm.fit_resample(X_train, y_train)

# 3. Missing value analysis
missingness = df.isnull().mean()
# MCAR test: is missingness random?
# If high-severity patients have more missing → MNAR (not missing at random)

# 4. Model evaluation with calibration
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
aucs, briers = [], []
for train_idx, val_idx in cv.split(X, y):
    model.fit(X[train_idx], y[train_idx])
    probs = model.predict_proba(X[val_idx])[:,1]
    aucs.append(roc_auc_score(y[val_idx], probs))
    briers.append(brier_score_loss(y[val_idx], probs))

print(f"AUC: {np.mean(aucs):.3f} ± {np.std(aucs):.3f}")
print(f"Brier Score: {np.mean(briers):.3f} ± {np.std(briers):.3f}")

# 5. Statistical significance of model comparison
# McNemar's test for comparing classifiers
from statsmodels.stats.contingency_tables import mcnemar
table = mcnemar([[both_correct, model_a_only], [model_b_only, both_wrong]])
print(f"McNemar p-value: {table.pvalue:.4f}")
```

**Key Statistical Insights:**
- Use AUC-ROC, not accuracy (class imbalance)
- Calibration matters in medicine (P=0.7 should mean 70% true risk)
- Report confidence intervals for all metrics
- Use stratified k-fold to preserve class balance in validation

---

## Case Study 2 — Finance: Retail Bank Customer Churn

**Problem:** Predict which customers will close their account in next 3 months.

```python
# Statistical feature analysis for churn
churn_stats = df.groupby('churned').agg({
    'account_age': ['mean', 'median', 'std'],
    'monthly_balance': ['mean', 'median', 'std'],
    'transaction_count': ['mean', 'median'],
    'num_products': ['mean']
}).round(2)

# Key statistical question: What features CAUSE churn?
# (not just correlate with it)

# Chi-square for categorical predictors
for col in categorical_cols:
    contingency = pd.crosstab(df[col], df['churned'])
    chi2, p, dof, expected = stats.chi2_contingency(contingency)
    cramers_v = np.sqrt(chi2 / (len(df) * (min(contingency.shape)-1)))
    print(f"{col}: chi2={chi2:.1f}, p={p:.4f}, Cramér's V={cramers_v:.3f}")

# Logistic regression for interpretable churn model
import statsmodels.formula.api as smf
formula = 'churned ~ account_age + C(segment) + monthly_balance + num_products + tenure'
model = smf.logit(formula, data=df).fit()

# Odds ratios with 95% CI
odds_ratios = pd.DataFrame({
    'OR': np.exp(model.params),
    'CI_lower': np.exp(model.conf_int()[0]),
    'CI_upper': np.exp(model.conf_int()[1]),
    'p-value': model.pvalues
}).round(3)
print(odds_ratios)

# Interpretation:
# OR(num_products) = 0.45 → Each additional product reduces churn odds by 55%
# OR(account_age) = 0.98 → Each additional year reduces churn odds by 2%
```

---

## Case Study 3 — Marketing: Campaign Response Uplift

**Problem:** Which customers should receive a promotional offer? (Not everyone benefits from promotion)

```python
# Uplift modeling — estimate ITE (Individual Treatment Effect)
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier

# T-Learner approach
# Train separate model for treatment and control
model_t = RandomForestClassifier(n_estimators=100, random_state=42)
model_c = RandomForestClassifier(n_estimators=100, random_state=42)

# Train on treatment group
model_t.fit(X_train[T_train==1], y_train[T_train==1])
# Train on control group  
model_c.fit(X_train[T_train==0], y_train[T_train==0])

# Predict uplift for each customer
uplift = model_t.predict_proba(X_test)[:,1] - model_c.predict_proba(X_test)[:,1]

# AUUC (Area Under Uplift Curve) for evaluation
# Qini coefficient for uplift model comparison

# Statistical testing of uplift
# For top-20% uplift customers vs random assignment:
top_20pct = np.percentile(uplift, 80)
high_uplift = uplift >= top_20pct

# Run targeted campaign on high-uplift customers
# Measure: Response rate in high-uplift vs control (should be higher)
from scipy.stats import chi2_contingency
obs = np.array([[converted_treated, not_converted_treated],
                [converted_control, not_converted_control]])
chi2, p, dof, expected = chi2_contingency(obs)
```

---

# Chapter 11.4 — Interview Preparation

> **Complete statistical interview question bank with expert answers.**

---

## 11.4.1 — Probability Interview Questions

**P1. You flip a fair coin 10 times and get 10 heads. What is P(next flip is heads)?**

**Answer:** 0.5. Each flip is independent — the coin has no memory. This is the **gambler's fallacy** if you say "tails is overdue." The coin's history is irrelevant to future flips.

---

**P2. In a class of 30 students, what is the probability that at least 2 students share a birthday?**

**Answer:** Birthday paradox. P(at least 2 share) = 1 - P(all different).
P(all different) = (365/365)(364/365)(363/365)...(336/365)
≈ 0.294
P(at least 2 share) ≈ 1 - 0.294 = **0.706 = 70.6%**

Counterintuitive: 70% chance in a class of only 30!

---

**P3. Three boxes: Box A (2 gold, 1 silver), Box B (1 gold, 2 silver), Box C (1 gold, 1 silver). You pick a box randomly and draw a gold coin. What is P(you picked Box A)?**

**Answer:** Apply Bayes' Theorem.
P(A) = P(B) = P(C) = 1/3
P(gold|A) = 2/3, P(gold|B) = 1/3, P(gold|C) = 1/2
P(gold) = (2/3)(1/3) + (1/3)(1/3) + (1/2)(1/3) = 2/9 + 1/9 + 1/6 = 4/18+2/18+3/18 = 9/18 = 1/2
P(A|gold) = P(gold|A)P(A)/P(gold) = (2/3)(1/3)/(1/2) = (2/9)/(1/2) = **4/9 ≈ 0.444**

---

## 11.4.2 — Statistics Interview Questions

**S1. What is the difference between Type I and Type II errors? Which is worse?**

**Answer:** Type I = false positive (reject H₀ when true). Type II = false negative (fail to reject H₀ when false). Neither is universally worse — depends on context:
- Drug safety trial: Type I worse (approve harmful drug)
- Cancer screening: Type II worse (miss real cancer)
- Spam filter: Type I annoying (lose legit email), Type II bad UX (spam gets through)
Always state your context when answering.

---

**S2. You have two versions of a feature. How would you determine if they have different performance?**

**Answer:** Structured A/B testing approach:
1. State hypotheses: H₀: no difference, H₁: V2 > V1
2. Choose α=0.05, power=0.80
3. Calculate required sample size (power analysis)
4. Randomly assign users to V1 and V2
5. Collect data until sample size reached (don't peek)
6. Apply two-proportion z-test or t-test
7. Report: test statistic, p-value, effect size, CI, practical significance

---

**S3. Explain p-value to a non-technical business stakeholder.**

**Answer:** "The p-value tells us: if our new feature had absolutely no real effect, how often would we see results as impressive as these just by luck? A p-value of 0.03 means there's only a 3% chance we'd see this improvement by random chance — so we're fairly confident the improvement is real. But this doesn't tell us HOW LARGE the improvement is, or if it's worth implementing. For that, look at the actual conversion rate difference and the business impact."

---

**S4. What is the Central Limit Theorem and why does it matter in practice?**

**Answer:** CLT: The distribution of sample means approaches Normal as sample size grows, regardless of the underlying distribution's shape. Practical importance:
1. Justifies z-tests and t-tests even when data isn't normally distributed (need n≥30 typically)
2. Enables confidence intervals for any statistic
3. Explains why Normal distribution is ubiquitous (many processes are sums of many effects)
4. Foundation of A/B testing statistical inference
5. Why bootstrapping works (bootstrap distribution of statistics is normal)

---

## 11.4.3 — Machine Learning Statistics Interview Questions

**ML1. Explain the bias-variance trade-off and how you manage it.**

**Answer:** Every model has: Total Error = Bias² + Variance + Irreducible Noise. High bias (underfitting): model too simple, misses true pattern. High variance (overfitting): model too complex, learns noise. Trade-off: increasing complexity decreases bias but increases variance.

Management strategies:
- Diagnose via learning curves (train vs validation error)
- Cross-validation to find optimal complexity
- Regularization (L1/L2) reduces variance by constraining weights
- More data reduces variance without increasing bias
- Feature selection to reduce variance
- Ensemble methods: bagging (reduces variance), boosting (reduces bias)

---

**ML2. What is regularization? Explain L1 vs L2.**

**Answer:**
Regularization adds a penalty to the loss function to prevent overfitting:

L2 (Ridge): Loss + λΣwᵢ² → Shrinks all weights toward zero, never exactly zero. Handles multicollinearity. MAP with Gaussian prior.

L1 (LASSO): Loss + λΣ|wᵢ| → Drives some weights to exactly zero = automatic feature selection. Produces sparse models. MAP with Laplace prior.

Elastic Net: L1 + L2. Best of both.

λ controls strength: increase → more regularization → more bias, less variance.

---

**ML3. How do you evaluate a classification model beyond accuracy?**

**Answer:** When to look beyond accuracy: class imbalance (always predicting majority class = high accuracy but useless), asymmetric error costs.

Key metrics:
- **Precision** = TP/(TP+FP): Of all predicted positives, how many are real? (minimize false alarms)
- **Recall/TPR** = TP/(TP+FN): Of all actual positives, how many did we catch? (minimize missed cases)
- **F1** = 2(P×R)/(P+R): Harmonic mean — balanced metric for imbalanced data
- **AUC-ROC**: Discrimination ability across all thresholds; 0.5=random, 1.0=perfect
- **PR-AUC**: Better for highly imbalanced data (focuses on positive class)
- **Calibration/Brier Score**: Are probabilities accurate?
- **Log Loss**: Penalizes confident wrong predictions heavily

For business: translate into ₹ impact — precision/recall matter based on cost of FP vs FN.

---

**ML4. What is cross-validation and why is it used?**

**Answer:** CV estimates how well a model generalizes to unseen data by repeatedly training on subsets and testing on the held-out portion.

k-fold CV: Split data into k equal folds. Train on k-1, test on 1. Repeat k times. Average test metrics.

Why better than single train-test split:
- Uses all data for both training and testing
- Reduces variance of performance estimate
- Detects overfitting more reliably

Special cases:
- Stratified k-fold: preserves class proportions (use for classification)
- Time series CV (walk-forward): preserves temporal order
- Leave-one-out (LOOCV): k=n, computationally expensive, low bias
- Nested CV: outer loop for evaluation, inner for hyperparameter tuning

---

## 11.4.4 — Data Science Case Study Questions

**CS1. "Estimate the number of piano tuners in Chicago."**

**Answer (Fermi Estimation):**
- Chicago population: ~2.7 million
- Average household size: 2.7 → ~1 million households
- % with pianos: ~5% → 50,000 pianos
- Commercial pianos (schools, churches, bars): add 10,000 → 60,000 pianos
- Tuning frequency: 1/year → 60,000 tunings per year
- Tuner productivity: 4 pianos/day × 250 days = 1,000/year per tuner
- Piano tuners = 60,000/1,000 = **~60 tuners**

(Actual answer: roughly 100-150. Fermi estimates within 2-3× = great)

---

**CS2. "How would you set up an experiment to test if a new recommendation algorithm increases user engagement?"**

**Answer:**
1. Define metric: primary = 7-day return rate; secondary = session duration, CTR
2. Unit of randomization: user-level (not page/session — avoid contamination)
3. Sample size: power analysis. Current return rate=45%, detect +2%, α=0.05, power=0.80 → n≈5000/group
4. Duration: at least 2 weeks (capture weekly cycles, avoid novelty effect)
5. Randomization: hash(user_id) mod 100 → [0,50)=control, [50,100)=treatment
6. Guard rails: page load time must not increase >10%, crash rate must not increase
7. Analysis: z-test for proportions, also check for heterogeneous effects by user segment
8. Decision framework: statistical significance + practical significance + engineering cost + risk assessment

---

# 📌 Part 11 Summary

## Python Statistics Cheatsheet

```python
# QUICK REFERENCE

import numpy as np
from scipy import stats
import pandas as pd

# Descriptive
np.mean(x), np.median(x), np.std(x, ddof=1), np.percentile(x, [25,75])

# Tests
stats.ttest_1samp(x, popmean)         # one-sample t
stats.ttest_ind(x1, x2)              # two-sample t (Welch's default)
stats.ttest_rel(before, after)        # paired t
stats.chi2_contingency(table)         # chi-square independence
stats.f_oneway(g1, g2, g3)           # one-way ANOVA
stats.pearsonr(x, y)                 # Pearson correlation + p-value
stats.spearmanr(x, y)                # Spearman correlation + p-value
stats.shapiro(x)                     # normality test
stats.ks_2samp(x, y)                 # two-sample KS (distribution comparison)

# CIs
ci = stats.t.interval(0.95, df=n-1, loc=mean, scale=stats.sem(x))

# Distributions
stats.norm.pdf(x, mu, sigma)         # Normal PDF
stats.norm.cdf(x, mu, sigma)         # Normal CDF
stats.norm.ppf(0.975)                # inverse CDF (z-score for 97.5%)
stats.binom.pmf(k, n, p)            # Binomial PMF
stats.poisson.pmf(k, lam)           # Poisson PMF
```

---
*Part 11 Complete | Final Chapter: Roadmaps, Plans, and Learning Paths*
