# 🎯 DS Statistics Interview Q&A — Batch 6
# Topic: A/B Testing & Experimentation
# Level: Basic → Advanced | Cross-questions included

---

## 🗂️ Legend
- 🔥 = Frequently asked | ⭐ = Very important | 🔥⭐ = Both

---

# SECTION 1 — BASIC LEVEL

---

## Q1. 🔥⭐ What is A/B testing? Walk through the complete process.

**A:**
**A/B testing** is a randomized controlled experiment comparing two (or more) versions of something to determine which performs better on a defined metric.

**Complete process:**

**1. Define the hypothesis:**
H₀: No difference between variants (p_B = p_A)
H₁: Variant B performs differently/better (p_B ≠ p_A or p_B > p_A)

**2. Choose metrics:**
- Primary metric: the ONE metric that determines success (e.g., conversion rate)
- Guardrail metrics: must not get worse (e.g., page load time, bounce rate)
- Secondary metrics: informative but not decision-driving

**3. Calculate required sample size (power analysis):**
- Baseline rate, Minimum Detectable Effect (MDE), α=0.05, power=0.80
- Determines how many users/days needed

**4. Random assignment:**
- Randomly split users into control (A) and treatment (B)
- Ensure no contamination (same user always sees same variant)

**5. Run the experiment:**
- Run for pre-determined duration/sample size
- Do NOT peek and stop early based on significance
- Account for novelty effects, day-of-week effects (run ≥1-2 full weeks)

**6. Statistical analysis:**
- Two-proportion z-test (or t-test for continuous metrics)
- Compute p-value, confidence interval, effect size

**7. Decision:**
- Combine statistical significance + practical significance + business judgment
- Consider: implementation cost, risk, secondary metric impacts

↳ **Cross Q:** What is the "unit of randomization" and why does it matter?
↳ **Cross A:** The unit of randomization is the entity that gets randomly assigned to a variant — typically user_id, but could be session_id, device_id, or even geographic region depending on the experiment. It matters because: (1) Consistency: a user should see the SAME variant across all interactions during the experiment (using session-level randomization would show a flickering experience). (2) Interference/contamination: if randomizing at session level but users have multiple sessions, the same user might see both variants — diluting the measured effect (called "contamination bias"). (3) Network effects: for social features (e.g., messaging, sharing), user-level randomization can still leak effects between connected users — may need cluster-level (e.g., friend-group level) randomization. (4) Statistical unit: the analysis must match the randomization unit — if randomizing by user but analyzing by session without accounting for repeated measures, you'll underestimate variance (pseudo-replication) and get falsely significant results.

---

## Q2. 🔥⭐ How do you calculate the required sample size for an A/B test?

**A:**
**Formula for comparing two proportions:**

$$n = \frac{(z_{\alpha/2} + z_\beta)^2 \times [p_1(1-p_1) + p_2(1-p_2)]}{(p_2-p_1)^2}$$

Where:
- z_α/2 = 1.96 for α=0.05 (two-tailed)
- z_β = 0.84 for 80% power (or 1.28 for 90% power)
- p₁ = baseline conversion rate
- p₂ = expected conversion rate with treatment (p₁ + MDE)

**Worked example:**
Current conversion rate p₁ = 5%. You want to detect a minimum improvement of 1 percentage point (MDE), so p₂ = 6%.

```
n = (1.96+0.84)² × [0.05×0.95 + 0.06×0.94] / (0.01)²
  = 7.84 × [0.0475 + 0.0564] / 0.0001
  = 7.84 × 0.1039 / 0.0001
  = 0.8146 / 0.0001
  ≈ 8,146 per group (16,292 total)
```

**Key drivers of sample size:**

| Factor | Effect on required n |
|--------|----------------------|
| Smaller MDE (want to detect tiny changes) | n increases (quadratically!) |
| Higher desired power (90% vs 80%) | n increases |
| Lower α (0.01 vs 0.05) | n increases |
| Baseline rate closer to 50% | n increases (max variance at p=0.5) |

```python
from statsmodels.stats.power import zt_ind_solve_power
from statsmodels.stats.proportion import proportion_effectsize

effect_size = proportion_effectsize(0.05, 0.06)  # Cohen's h
n = zt_ind_solve_power(effect_size=effect_size, alpha=0.05, power=0.8)
print(f"Required n per group: {n:.0f}")
```

↳ **Cross Q:** Your team wants to detect a 0.1% lift but your site only gets 1000 visitors/day. What do you tell them?
↳ **Cross A:** I'd calculate the required sample size explicitly and show the timeline implications. With baseline say 5% and wanting to detect just 0.1pp lift (to 5.1%): n ≈ (1.96+0.84)² × 2×0.05×0.95/(0.001)² ≈ 7.84×0.095/0.000001 ≈ 744,800 per group ≈ 1.5M total. At 1000 visitors/day, that's 1,500 DAYS (over 4 years!) — completely impractical. I'd present alternatives: (1) Increase MDE to something detectable in reasonable time (e.g., 1pp lift needs ~16K total, ~16 days) — ask if a smaller, more realistic effect is actually the threshold for business action. (2) Use a more sensitive metric or variance reduction technique (CUPED) to reduce required sample size. (3) Consider if this change is even worth testing via formal A/B — for tiny expected effects, sometimes it's more efficient to just ship the change with monitoring (if low-risk) rather than wait years for statistical proof. (4) Pool across multiple similar changes (a "platform" test of a class of changes) rather than testing one tiny tweak.

---

## Q3. 🔥⭐ What is the "peeking problem" in A/B testing? How do you solve it?

**A:**
**The peeking problem:** Repeatedly checking p-values during an ongoing experiment and stopping as soon as p<0.05 dramatically inflates the actual Type I error rate beyond the nominal α=0.05.

**Why it happens:**
Each time you check the data, you get another "chance" to see a p-value cross below 0.05 purely due to random fluctuation — even if there's truly no effect (H₀ is true). With enough peeks, you're almost guaranteed to see p<0.05 at SOME point, even with zero true effect.

**Simulation result:** With continuous monitoring and stopping at first p<0.05, actual Type I error rate can reach 20-30%+ instead of the intended 5% — even with infinite peeking, P(eventually crossing p<0.05) → 1 (by the law of the iterated logarithm).

**Visual intuition:**
```
p-value over time (H₀ true, no real effect):
0.10 │      ╱╲    ╱╲
0.05 │─────╱──╲──╱──╲──────  ← α threshold
0.02 │    ╱    ╲╱    ╲  ╱╲
     │   ╱            ╲╱  ╲
     └──────────────────────── Day
     
The p-value randomly fluctuates. If you stop the FIRST time
it dips below 0.05, you're cherry-picking a random fluctuation,
not detecting a real effect.
```

**Solutions:**

**1. Fixed-horizon testing (simplest):**
Pre-determine sample size/duration BEFORE starting. Don't look at results until reaching that point. Standard frequentist approach.

**2. Sequential testing with alpha-spending:**
- **O'Brien-Fleming boundaries:** Very strict early thresholds, relaxing toward the planned end — allows legitimate early stopping for very strong effects while controlling overall α
- **Pocock boundaries:** Constant (but more stringent than 0.05) threshold throughout

**3. Always-valid p-values (mSPRT, e-values):**
Modern methods (used by Optimizely, Statsig) that allow continuous monitoring with valid inference at ANY stopping time — using e-processes or confidence sequences.

**4. Bayesian methods:**
Bayesian A/B testing doesn't have the same "peeking" problem in the same sense — you can look at the posterior anytime, though early stopping based on posterior thresholds still needs careful interpretation for decision-making (expected loss framework is more principled).

↳ **Cross Q:** If a teammate says "we checked the dashboard and it's already significant after 3 days, let's ship it" — what do you say?
↳ **Cross A:** I'd explain the peeking problem concretely: "Checking early and stopping as soon as we see significance inflates our false positive rate well beyond 5% — we could be shipping a change with no real effect, or worse, a change that's actually neutral/harmful that just got lucky in early data." I'd ask: (1) Was the test designed for sequential monitoring (alpha-spending boundaries) or fixed-horizon? If fixed horizon was planned, we must wait for the full pre-determined sample size. (2) Were there confounds in the first 3 days — day-of-week effects, novelty effects from users seeing something new, or weekend vs weekday traffic differences? Early data is often unrepresentative. (3) If the platform uses always-valid inference (sequential testing built into the tool), then early stopping might genuinely be valid — I'd verify which methodology is being used. Bottom line: "exciting early results" are a classic trap — I'd push to either wait for the planned duration or confirm we're using a sequential testing framework that explicitly permits early stopping.

---

## Q4. 🔥⭐ What is a guardrail metric? Why are they necessary in A/B testing?

**A:**
**Guardrail metrics** are metrics that must NOT regress (get worse) during an experiment, even if the primary metric improves. They protect against unintended negative consequences.

**Why necessary:**
Optimizing for ONE metric (e.g., clicks) can inadvertently harm other important outcomes (e.g., user trust, site speed, long-term retention) that aren't captured by the primary metric alone.

**Common guardrail metrics:**

| Category | Examples |
|----------|---------|
| Performance | Page load time, API latency, crash rate |
| User Experience | Bounce rate, session abandonment, error rate |
| Business Health | Revenue per user (shouldn't drop even if conversion count rises) |
| Long-term | Unsubscribe rate, complaint rate, churn rate |
| Trust/Safety | Spam reports, content flags |

**Real-world example:**
Testing "more aggressive notification frequency" — Primary metric (clicks) might increase, but guardrail metrics (unsubscribe rate, app uninstalls) might also increase. If guardrails breach acceptable thresholds, DON'T ship even if primary metric improved.

**Statistical approach to guardrails:**
- Often use ONE-SIDED tests: "did the guardrail metric get WORSE" (not just "did it change")
- Sometimes use non-inferiority testing: prove the new variant ISN'T meaningfully worse, rather than requiring statistical proof it's the same
- Multiple guardrails → multiple testing correction may be needed, OR set a high bar (any breach = stop) rather than requiring statistical significance for each

↳ **Cross Q:** Your primary metric (CTR) improved significantly (p<0.01), but a guardrail metric (page load time) also got significantly worse. What do you recommend?
↳ **Cross A:** This requires careful judgment, not just statistical mechanics: (1) Quantify the trade-off precisely: how much did CTR improve (e.g., +2pp) vs how much did page load time degrade (e.g., +200ms)? (2) Investigate causation: did the new feature itself cause slower loading (e.g., an extra image/script), or is this a measurement artifact? (3) Check downstream impact: does the slower load time correlate with reduced conversion or increased bounce in this experiment, or only in the isolated guardrail metric? Sometimes a guardrail regresses without actually harming the ultimate business goal. (4) Consider segment analysis: is the load time regression uniform, or concentrated in certain user segments (e.g., low-bandwidth mobile users) where it might be more harmful? (5) Recommend: if the guardrail breach is small and doesn't show downstream harm, consider shipping with monitoring + a follow-up optimization sprint to address load time. If the breach is severe or shows correlated harm to other metrics, don't ship — first optimize the implementation (e.g., lazy-load the new element) and re-test.

---

## Q5. 🔥 What is the difference between a one-tailed and two-tailed test in A/B testing context?

**A:**
**Two-tailed test:**
H₀: p_B = p_A
H₁: p_B ≠ p_A (could be better OR worse)

Tests for ANY difference, in either direction. The rejection region is split across both tails (2.5% each side for α=0.05).

**One-tailed test:**
H₀: p_B ≤ p_A
H₁: p_B > p_A (only care if B is BETTER)

Tests for difference in ONE specific direction. The entire 5% rejection region is in one tail — making it EASIER to reach significance for that direction (more statistical power for detecting improvement specifically).

**Critical values comparison (α=0.05):**
- Two-tailed: z* = 1.96
- One-tailed: z* = 1.645 (easier to exceed!)

**When is one-tailed appropriate?**
- ONLY when you have NO interest whatsoever in detecting the opposite direction
- Must be decided and stated BEFORE running the experiment (not after seeing results favor one direction)
- Example: testing a change you're CONFIDENT can only help or do nothing (rare in practice)

**Why most practitioners default to two-tailed:**
- In reality, almost ANY change could backfire — even well-intentioned UX improvements sometimes hurt conversion
- Using one-tailed just to get a "cheaper" p-value (after seeing the direction of the effect) is a form of p-hacking
- Two-tailed is the conservative, defensible default for business decisions where negative surprises matter

⚠️ **Major interview red flag:** If you say "I'd always use one-tailed to get more power" without acknowledging the ethical/statistical risk, that's a bad answer. The correct answer acknowledges the trade-off and defaults to two-tailed unless there's a strong, PRE-REGISTERED reason for one-tailed.

↳ **Cross Q:** "Using a one-tailed test, we get p=0.04 (significant). Using two-tailed, p=0.08 (not significant). Which do we report?"
↳ **Cross A:** This scenario is a major red flag for p-hacking if the choice of test type was made AFTER seeing the data. The correct approach: (1) The test type (one vs two-tailed) must be decided and documented BEFORE running the experiment, as part of the pre-registered analysis plan — not chosen after the fact to get a "significant" result. (2) If two-tailed was the pre-specified test (the standard, defensible default), report p=0.08 — NOT significant at the conventional 0.05 threshold. Switching to one-tailed retroactively because it gives a better p-value is a classic p-hacking move that undermines the validity of the inference. (3) If there was a genuine, documented PRE-EXISTING reason for one-tailed (e.g., this is a pure technical optimization that mathematically cannot make things worse, like a caching improvement), then one-tailed might have been legitimately pre-specified — but this must be verifiable from documentation written BEFORE the experiment, not retrofitted. In an interview, I'd flag this scenario explicitly as a methodological warning sign and insist on checking the pre-registered protocol.

---

# SECTION 2 — INTERMEDIATE LEVEL

---

## Q6. 🔥⭐ What is CUPED and why is it used to increase A/B test sensitivity?

**A:**
**CUPED (Controlled-experiment Using Pre-Experiment Data)**, developed at Microsoft, is a variance reduction technique that uses pre-experiment data to increase statistical power without increasing sample size.

**The core idea:**
Adjust the outcome metric using a covariate (typically the SAME metric measured BEFORE the experiment) that's correlated with the outcome but unaffected by the treatment.

**Formula:**
$$Y_{CUPED} = Y - \theta(X - \bar{X})$$

Where:
- Y = outcome metric during experiment
- X = same metric measured in PRE-experiment period (covariate)
- X̄ = mean of X across all users
- θ = Cov(Y,X)/Var(X) (optimal coefficient, estimated via regression)

**Why it works:**
$$Var(Y_{CUPED}) = Var(Y)(1-\rho^2)$$

Where ρ = correlation between pre-period and during-period metrics.

If pre-experiment spending correlates with during-experiment spending at ρ=0.7, variance reduction = 1-0.49 = 51% — you can detect the SAME effect with about HALF the sample size, or detect a SMALLER effect with the same sample size!

**Intuition:** Some users are naturally high-spenders, others low-spenders — this variation exists regardless of treatment. CUPED "subtracts out" this pre-existing variation, leaving a cleaner signal of the TREATMENT EFFECT specifically.

**Requirements:**
- Need pre-experiment data for the same users (requires users to exist before the test starts)
- Covariate must NOT be affected by treatment (use PRE-experiment period only)
- Works best when pre/post correlation is strong (ρ > 0.3 typically worthwhile)

```python
import numpy as np

# Y = outcome during experiment, X = same metric pre-experiment
theta = np.cov(Y, X)[0,1] / np.var(X)
Y_cuped = Y - theta * (X - np.mean(X))
# Run standard t-test on Y_cuped instead of Y — same unbiased estimate, lower variance
```

↳ **Cross Q:** Why doesn't CUPED introduce bias even though it adjusts the outcome metric?
↳ **Cross A:** CUPED preserves unbiasedness because: (1) The adjustment term θ(X-X̄) has EXPECTED VALUE ZERO across the population — since X̄ is the population mean of X, E[X-X̄]=0, so E[Y_CUPED] = E[Y] - θ×0 = E[Y]. The adjustment shifts individual data points but doesn't shift the population-level mean. (2) X (pre-experiment covariate) is, by construction, measured BEFORE the experiment started — so it cannot be affected by which treatment arm a user is randomly assigned to (since assignment happens after X is measured). This means X is balanced between treatment and control by RANDOMIZATION (not just hoped to be) — adjusting for a perfectly balanced covariate cannot introduce bias, only reduce noise. (3) The treatment effect estimate (difference in means of Y_CUPED between groups) equals the treatment effect estimate from raw Y, but with smaller variance — proven via the algebra: E[Y_CUPED|treatment] - E[Y_CUPED|control] = E[Y|treatment] - E[Y|control] exactly, because the θ(X-X̄) term has the same expectation in both groups (X is balanced by randomization).

---

## Q7. 🔥⭐ What is the network effect / interference problem in A/B testing? How do you handle it?

**A:**
**Network effects (interference / SUTVA violation):** When one user's treatment assignment affects ANOTHER user's outcome, violating the core assumption of independence between experimental units (SUTVA: Stable Unit Treatment Value Assumption).

**Why standard A/B testing assumes no interference:**
Standard analysis assumes Y_i (outcome for user i) depends ONLY on user i's OWN treatment assignment — not on anyone else's. This is violated when users interact (social networks, marketplaces, chat apps).

**Examples of interference:**

1. **Social/messaging platforms:** If User A (treatment: new sharing feature) shares more content, and User A's friends (potentially in control) see and engage with that content — control group's behavior is affected by treatment group's behavior.

2. **Marketplaces (two-sided):** Testing a discount for buyers in treatment group increases their purchasing → reduces inventory available for buyers in control group → control group's outcomes are artificially suppressed.

3. **Recommendation systems:** If treatment improves recommendations for some users, freed-up "attention" or engagement might shift away from content that control-group users would have seen, affecting content creators' metrics.

4. **Server/infrastructure sharing:** Treatment causing higher server load can slow down responses for control group users sharing the same infrastructure.

**Why this matters statistically:**
When interference exists, the standard A/B test estimate is BIASED — it doesn't measure the "what if we rolled this out to everyone" effect, because in the experiment, only PART of the network is treated, creating spillover effects that wouldn't exist (or would be different) at full rollout.

**Solutions:**

**1. Cluster-level randomization:**
Instead of randomizing individual users, randomize CLUSTERS (e.g., friend groups, geographic regions, entire markets) — minimizes interference WITHIN a cluster being split.

**2. Graph cluster randomization:**
For social networks, use graph-clustering algorithms to identify densely-connected communities and randomize at that level.

**3. Switchback / time-based randomization:**
For marketplace/infrastructure-level interference, randomize by TIME PERIOD rather than user (e.g., odd days = treatment, even days = control) — useful for testing pricing algorithms, dispatch systems (e.g., Uber's switchback tests).

**4. Ego-network / exposure-based analysis:**
Measure each user's "exposure" to treatment (e.g., what fraction of their network is treated) and model the dose-response relationship explicitly.

↳ **Cross Q:** Uber wants to test a new driver dispatch algorithm. Why can't they simply A/B test it by randomly assigning drivers to treatment/control?
↳ **Cross A:** Driver dispatch is a textbook interference scenario because drivers and riders SHARE a common marketplace within each city/region. If Driver A (treatment: new algorithm) gets dispatched more efficiently and takes more rides, that DIRECTLY reduces the rides available for Driver B (control) in the same area — they're competing for the same finite pool of ride requests. This creates negative interference: control group's performance is artificially suppressed BECAUSE of the treatment group's improved performance, making the treatment look BETTER than it would if rolled out to 100% of drivers (since at 100% rollout, there's no "stealing" from a control group — everyone benefits from the improved algorithm simultaneously, and the comparison disappears). Uber's actual solution: SWITCHBACK testing — alternate the ENTIRE city/region between algorithm A and algorithm B across different time periods (e.g., hour-by-hour or day-by-day), rather than splitting drivers. This way, during "treatment periods," ALL drivers and riders in that region experience the new algorithm together, avoiding the within-marketplace competition/interference problem. The analysis then compares aggregate marketplace metrics (total rides, average wait time) across treatment vs control TIME PERIODS rather than individual users.

---

## Q8. 🔥⭐ What is Simpson's Paradox in the context of A/B testing? How would you detect it?

**A:**
**Simpson's Paradox in A/B testing:** The overall experiment result reverses or disappears when the data is segmented by a confounding variable — Variant B might appear better overall but actually be worse (or equal) within EVERY meaningful subgroup.

**Classic A/B testing example:**

```
                    Mobile Users          Desktop Users         OVERALL
Variant A:    200/1000 (20% conv.)  900/9000 (10% conv.)  1100/10000 (11.0%)
Variant B:    900/9000 (10% conv.)  100/1000 (10% conv.)  1000/10000 (10.0%)
```

A wins on BOTH mobile (20%>10%) and desktop (10%=10%)... but wait, let's reconsider with a clearer reversal:

```
                    New Users             Returning Users        OVERALL
Variant A:    50/1000 (5% conv.)    900/9000 (10% conv.)   950/10000 (9.5%)
Variant B:    180/9000 (2% conv.)   95/1000 (9.5% conv.)   275/10000 (2.75%)
```

This requires careful construction, but the key insight: traffic allocation IMBALANCE across segments between variants (e.g., due to a bug in randomization, or non-uniform rollout timing) can make the AGGREGATE result misleading even when underlying per-segment effects are consistent or reversed.

**Most common REAL cause in A/B testing:** Unequal sample sizes across segments combined with the experiment running at different RATES across segments (e.g., mobile traffic ramped up faster for one variant due to a phased rollout bug) — NOT typically because of true effect heterogeneity alone, but because randomization wasn't perfectly balanced across an important segment over time.

**Detection methods:**

1. **Always segment results by key dimensions:** device type, new vs returning users, geography, traffic source — check if the OVERALL conclusion holds within each segment.

2. **Sample Ratio Mismatch (SRM) check:** Verify the actual traffic split matches the intended split (e.g., 50/50) WITHIN each segment, not just overall. A classic SRM bug (e.g., 49.5/50.5 overall hiding 30/70 within mobile) can cause Simpson's Paradox artifacts.

3. **Check randomization timing:** If one variant was rolled out gradually (ramped from 10%→50% over days) while traffic composition changed over those same days (e.g., weekday vs weekend mix), this creates spurious segment-overall divergence.

↳ **Cross Q:** Your overall A/B test shows Variant B winning, but when you check by device type, Variant A wins on both mobile AND desktop. What's your first hypothesis and how do you investigate?
↳ **Cross A:** First hypothesis: Sample Ratio Mismatch (SRM) or unequal segment representation between variants — likely a bug in the randomization or experiment rollout process, not a genuine effect. Investigation steps: (1) Check the ACTUAL traffic split for each segment (mobile/desktop) within each variant — compare to the INTENDED 50/50 split. If Variant A got disproportionately MORE of the high-converting segment (e.g., desktop) and Variant B got disproportionately more of the low-converting segment (mobile), that alone can flip the aggregate. (2) Run a formal SRM test: chi-square goodness-of-fit test comparing observed vs expected (50/50) sample sizes — even small deviations (p<0.001 threshold often used, very strict because SRM bugs are usually severe when real) signal a randomization bug. (3) Check experiment START and END times for each segment — was the experiment running for the same duration/days-of-week for mobile vs desktop in each variant? (4) Check for bot traffic or filtering bugs that might disproportionately affect one variant's segment composition. (5) Once root cause is found (almost always a randomization/logging bug, not true Simpson's effect), FIX the underlying issue and re-run, OR if it's a measurement-only issue, recompute results properly weighted by segment.

---

## Q9. 🔥 What is novelty effect and primacy effect? How do you account for them?

**A:**
**Novelty Effect:** Users react positively to a CHANGE simply because it's new/different, NOT because it's genuinely better. This effect typically DECAYS over time as users get used to the new experience.

**Primacy Effect (also called "change aversion" when negative):** Users initially perform WORSE with a new design because they're unfamiliar with it (even if the new design is objectively better long-term) — habitual users need time to adapt.

**Why both are problematic for A/B testing:**
A short experiment (e.g., 3 days) might show inflated POSITIVE results (novelty) or unfairly negative results (primacy/change aversion) that DON'T reflect the true long-term steady-state effect.

**Visual pattern:**
```
Effect size over time:

Novelty Effect:                    Primacy Effect:
Lift                                Lift
│╲                                  │
│ ╲___                              │     ___────
│     ╲___                          │   ╱
│         ╲___ (decaying to        │  ╱  (improving as users
│             true effect)          │ ╱    adapt to new feature)
└──────────────── Time              └──────────────── Time
```

**Solutions:**

**1. Run experiments long enough:**
At minimum 1-2 full weeks (to average out day-of-week effects AND give some time for novelty to fade). For significant UX changes, consider 3-4 weeks.

**2. Segment by user tenure in experiment:**
Compare NEW users entering the experiment (who have no "old way" reference point — no novelty/primacy bias) vs LONG-TENURE users (who are comparing to their prior experience). New-user-only analysis often gives the cleanest signal for the TRUE steady-state effect.

**3. Use a holdback/long-term holdout group:**
Keep a small % of users on the OLD experience indefinitely (separate from the main experiment) to track the TRUE long-term incremental effect of ALL accumulated changes over months.

**4. Trend analysis within the experiment:**
Plot the daily/weekly effect size over the course of the experiment — if it's still trending (increasing or decreasing) at the end, the experiment hasn't reached steady-state and needs to run longer.

**5. Cohort-based analysis:**
Track specific user cohorts (e.g., users who joined the experiment in week 1) over multiple weeks to see how THEIR specific response evolves, separate from new cohorts entering later.

↳ **Cross Q:** How would you specifically distinguish a true novelty effect from a genuine, sustained improvement using only experiment data (without extending the test indefinitely)?
↳ **Cross A:** Key diagnostic: plot the DAILY (or weekly) treatment effect over the course of the experiment, segmented by user cohort tenure. Specifically: (1) For users who ENTERED the experiment on day 1, track their effect size on day 1, day 7, day 14, day 21 — if it's decaying toward zero (or toward a smaller stable value) as days progress, that's strong evidence of novelty effect specifically for that cohort. (2) Simultaneously, look at NEW users entering the experiment in week 2, week 3 — if THEIR initial effect size (their "day 1" experience) matches the OLD cohort's STEADY-STATE (decayed) effect rather than the original spike, this confirms the early spike was novelty-driven, not a true initial difference. (3) If instead the effect is STABLE across time for the original cohort, AND new cohorts show the same stable effect, that's evidence of a genuine, sustained improvement — not novelty. (4) Statistically: fit a model with Effect ~ f(days_since_first_exposure) and test if the coefficient on time is significantly different from zero (decaying) — tools like difference-in-differences with time interactions formalize this. This cohort-time decomposition is the standard rigorous approach used at companies like LinkedIn and Netflix to separate novelty/primacy from true effects without needing indefinite test extension.

---

# SECTION 3 — ADVANCED LEVEL

---

## Q10. 🔥⭐ Explain multi-armed bandit algorithms. How do they differ from traditional A/B testing?

**A:**
**Traditional A/B testing:** Fixed allocation (e.g., 50/50) throughout the ENTIRE experiment duration, regardless of which variant is performing better. Optimizes for STATISTICAL CERTAINTY about which variant is best (exploration-heavy, doesn't exploit findings until the test concludes).

**Multi-armed bandit (MAB):** Dynamically adjusts traffic allocation DURING the experiment based on observed performance — sending MORE traffic to better-performing variants as evidence accumulates. Optimizes for CUMULATIVE REWARD during the experiment itself (balances exploration and exploitation continuously).

**Key trade-off — Regret:**
$$\text{Regret} = \sum_{t=1}^T (\mu^* - \mu_{a_t})$$

Where μ* = true best arm's mean reward, μ_{a_t} = reward of arm chosen at time t. Regret measures the cumulative "cost" of not always playing the optimal arm.

**Traditional A/B testing has HIGH cumulative regret** (continues sending 50% traffic to inferior variant for the entire test duration, even once it's clear which is better) — but gives a clean, unbiased estimate of the TRUE effect size with valid statistical inference at the end.

**Bandits have LOWER cumulative regret** (adapt quickly, minimize traffic to bad variants) — but the ADAPTIVE sampling makes standard statistical inference (p-values, CIs) INVALID without special corrections, because the sample sizes per arm are no longer fixed/independent of the outcomes.

**Common bandit algorithms:**

**1. Epsilon-Greedy:**
With probability ε: explore (random arm). With probability 1-ε: exploit (best-known arm). Simple but suboptimal (doesn't adapt ε over time).

**2. Thompson Sampling (Bayesian):**
Maintain a posterior distribution (e.g., Beta) for each arm's success rate. At each round, SAMPLE from each posterior, play the arm with the highest sample. Naturally balances exploration (wide posteriors get sampled with more variance) and exploitation (narrow, high-mean posteriors usually win).

**3. UCB (Upper Confidence Bound):**
Play the arm maximizing μ̂ₐ + c√(ln(t)/Nₐ) — balances estimated mean with an "optimism bonus" for arms with less data (high uncertainty).

**When to use which:**

| Scenario | Recommendation |
|----------|----------------|
| Need rigorous statistical proof for stakeholders/legal/medical | Traditional A/B test |
| Want to minimize "cost" of testing (e.g., ad revenue during test) | Multi-armed bandit |
| Many variants (10+) to test simultaneously | Bandit (more efficient than pairwise A/B) |
| Need to understand WHY/magnitude of effect for future decisions | Traditional A/B test (cleaner causal estimate) |
| Continuously evolving content (news headlines, ad creative) | Bandit (perpetual optimization) |
| One-time, high-stakes decision (pricing change, major UX redesign) | Traditional A/B test |

↳ **Cross Q:** Why can't you just use standard statistical tests (z-test, t-test) directly on the data collected from a multi-armed bandit experiment?
↳ **Cross A:** Standard tests assume FIXED, pre-determined sample sizes per group, with treatment assignment INDEPENDENT of the outcomes observed. In a bandit, the sample size for each arm is DETERMINED BY the observed outcomes (better-performing arms get more samples, dynamically, in response to data) — this creates a fundamental dependency between the "stopping rule" / allocation rule and the data itself, violating the i.i.d. sampling assumptions underlying standard p-values and confidence intervals. Specifically: (1) The "peeking problem" is BUILT INTO bandits by design — they continuously adapt based on interim results, which is exactly the behavior that invalidates naive sequential p-value computation. (2) Arms that perform poorly EARLY (by random chance) get LESS traffic, so even if they're actually the best arm, they may never accumulate enough data to prove it — creating a form of selection bias in the final dataset. (3) The variance estimates from a bandit's adaptively-collected data are generally NOT valid for standard CI formulas because the sample isn't a simple random sample of fixed size. Solutions: use specialized "adaptive experiment" statistical methods (e.g., doubly robust estimators for contextual bandits, or BatchedBandits with valid post-hoc inference), or simply treat the bandit phase as PURE OPTIMIZATION (no statistical claims) and run a SEPARATE, clean, fixed-allocation A/B test afterward if you need formal statistical proof of the winning arm's superiority.

---

## Q11. 🔥 What is variance reduction in experimentation? Name 3 techniques beyond CUPED.

**A:**
**Variance reduction techniques** reduce the noise in the outcome metric WITHOUT changing the underlying treatment effect, allowing you to detect smaller effects with the same sample size (or the same effect with smaller sample size/shorter duration).

**1. Stratified Sampling / Randomization:**
Instead of pure random assignment, stratify by a known confounder (e.g., country, device type, user tenure) and randomize WITHIN each stratum to ensure perfect balance. Reduces variance attributable to between-stratum differences.

```python
# Example: stratify by device type before randomizing
for device in ['mobile', 'desktop', 'tablet']:
    device_users = users[users.device == device]
    treatment_assignment = randomize(device_users, ratio=0.5)
```

**2. Variance-Stabilizing Transformations:**
For metrics with variance that depends on the mean (e.g., Poisson-like count data where Var≈Mean), apply a transformation (e.g., √x or log(x+1)) BEFORE running the t-test, stabilizing variance across different baseline levels and improving test validity/power.

**3. Regression Adjustment (generalized CUPED):**
Beyond using just ONE pre-period covariate (basic CUPED), use MULTIPLE covariates (pre-period metrics, demographic features) in a regression model to predict the outcome, then analyze the RESIDUALS (unexplained variance) for the treatment effect. This is essentially ANCOVA (Analysis of Covariance) and can achieve even greater variance reduction than single-covariate CUPED.

$$Y_{adjusted} = Y - \hat{Y}(X_1, X_2, ..., X_k) + \bar{Y}$$

**4. Trigger/Triggered Analysis (Dilution reduction):**
Only analyze users who were actually EXPOSED to the feature being tested (not all users in the experiment bucket, some of whom may never have encountered the changed element). This reduces "dilution" from users whose behavior couldn't possibly be affected, increasing the SIGNAL-TO-NOISE ratio without losing unbiasedness (as long as triggering is determined independent of treatment).

**5. Paired/Matched Designs:**
When possible, pair similar units (e.g., same user across two time periods, or matched user pairs) to remove between-unit variance, isolating only the WITHIN-pair treatment effect — analogous to paired t-test vs independent t-test.

↳ **Cross Q:** Your team implements "triggered analysis" (only counting users who actually saw the new feature) but conversion seems to improve dramatically. A skeptical stakeholder asks if this introduces bias. How do you respond?
↳ **Cross A:** I'd clarify the critical distinction: triggered analysis is UNBIASED as long as the TRIGGERING EVENT itself (whether a user encounters the changed element) is determined by something INDEPENDENT of the treatment assignment — typically by the user's natural navigation behavior that would be identical regardless of variant (e.g., "did the user visit the checkout page," which both variants' users do at similar rates absent the treatment effect on EARLIER steps). The key validity check: verify that the TRIGGER RATE itself (% of users in each arm who encountered the trigger condition) is statistically similar between treatment and control. If treatment users are triggering (reaching the changed element) at a SIGNIFICANTLY different rate than control — for example, if the new feature itself changes upstream behavior in a way that affects who reaches the triggering point — then triggered analysis WOULD introduce selection bias, because you're no longer comparing equivalent populations. In that case, I'd recommend: (1) First check Sample Ratio Mismatch on the trigger condition itself. (2) If trigger rates differ significantly, triggered analysis isn't valid in the simple form — need more sophisticated methods (e.g., intent-to-treat analysis on the FULL population, or instrumental variable approaches). (3) If trigger rates ARE balanced, triggered analysis is valid and actually MORE rigorous, not less — it precisely isolates the effect among users who could possibly be affected, removing pure noise from users who never saw any difference.

---

## Q12. ⭐ What is a switchback test? When is it preferred over standard randomization?

**A:**
**Switchback testing** randomizes by TIME PERIOD rather than by individual user/unit — the entire system (e.g., a city for a ride-sharing app) alternates between treatment and control across sequential time windows.

**Structure:**
```
Time:        Mon    Tue    Wed    Thu    Fri    Sat    Sun
City A:       A      B      A      B      A      B      A
City B:       B      A      B      A      B      A      B   (or staggered differently)

Each time block, the ENTIRE market experiences one variant.
```

**Why use switchback instead of user-level randomization:**

1. **Network/interference problems:** As discussed in Q7, when units interact (marketplace dynamics, shared infrastructure, social effects), individual-level randomization creates contamination. Switchback ensures EVERYONE in the system experiences the SAME variant simultaneously, eliminating within-period interference.

2. **System-level effects:** Testing algorithm changes that affect aggregate system behavior (e.g., pricing algorithms, dispatch systems, server-side caching) often can't be meaningfully isolated to individual users — the effect is inherently at the SYSTEM level.

**Key statistical challenges with switchback:**

1. **Autocorrelation:** Consecutive time periods aren't independent (today's market state depends on yesterday's) — standard SE formulas assuming independence are WRONG, underestimating variance.

2. **Carryover effects:** Effects from one period may "spill over" into the next (e.g., drivers who relocated due to treatment-period pricing don't instantly reset when control period begins) — need washout/buffer periods between switches.

3. **Limited effective sample size:** With only, say, 14 days of data, you really only have ~14 independent "data points" (time blocks), not thousands of individual data points — drastically reducing statistical power compared to user-level randomization with the same total traffic.

**Analysis approach:**
Use cluster-robust standard errors (clustering by time block) or specialized switchback estimators that account for the autocorrelated, panel-like structure (e.g., the methods developed by Bojinov & Shephard for switchback experiments, or simple time-block-level aggregation followed by a paired t-test on block-level means).

↳ **Cross Q:** Why is a switchback test's effective sample size so much smaller than its raw data volume would suggest?
↳ **Cross A:** While a switchback test might collect millions of individual ride/transaction records, the actual RANDOMIZATION happened only at the TIME-BLOCK level (e.g., 28 daily blocks over 4 weeks) — not at the individual transaction level. This means individual observations WITHIN the same time block are highly correlated (they all experienced the identical treatment AND were subject to the same market conditions, weather, day-of-week effects, etc. on that day) — they don't provide independent information about the treatment effect in the way that truly independent samples would. Statistically, the EFFECTIVE sample size for inference is closer to the NUMBER OF TIME BLOCKS (e.g., 28), not the number of individual transactions (e.g., 2 million). This is analogous to "pseudo-replication" in ecology/biology — measuring 1000 leaves from just 2 trees doesn't give you 1000 independent data points about tree health; you really have an n=2 experiment with extra noise. Practical implication: switchback experiments need MANY independent time blocks (ideally with RANDOM, not just alternating, assignment of treatment/control to blocks, and sufficient blocks for statistical power) — a switchback test with only 4-6 blocks has very low statistical power despite massive raw data volume, and analysts who naively use the individual-transaction-level sample size in their power calculations will dramatically UNDERESTIMATE their margin of error, leading to false confidence in results.

---

# 📌 QUICK REFERENCE — A/B Testing Cheatsheet

```
SAMPLE SIZE FORMULA (two proportions)
n = (z_α/2+z_β)² × [p₁(1-p₁)+p₂(1-p₂)] / (p₂-p₁)²
z_α/2=1.96 (α=0.05)   z_β=0.84 (80% power) or 1.28 (90% power)

KEY PRINCIPLES
✅ Pre-register: hypothesis, metric, sample size, duration BEFORE starting
✅ Run for full pre-planned duration — NEVER stop early on significance
✅ Always check guardrail metrics, not just primary metric
✅ Run minimum 1-2 weeks (day-of-week effects, novelty effects)
✅ Check Sample Ratio Mismatch (SRM) before trusting any results
✅ Segment results to check for Simpson's Paradox / heterogeneous effects

PEEKING PROBLEM SOLUTIONS
Fixed-horizon: decide n upfront, don't look until done
Sequential testing: O'Brien-Fleming or Pocock alpha-spending boundaries
Always-valid inference: e-values, mSPRT (allows continuous monitoring)
Bayesian: posterior + expected loss framework

VARIANCE REDUCTION TECHNIQUES
CUPED: Y_adj = Y - θ(X_pre - X̄_pre)  → reduces variance by ρ²
Stratified randomization: balance known confounders at assignment
Triggered analysis: only analyze users who saw the feature
Regression adjustment (ANCOVA): multiple covariates, residualize

WHEN STANDARD A/B TESTING FAILS
Network effects/interference → cluster randomization or switchback
Marketplace dynamics → switchback testing (time-based)
Many variants → multi-armed bandit (Thompson Sampling, UCB)
Novelty/primacy effects → run longer, segment by user tenure

KEY FORMULAS
Two-proportion z-test: z = (p̂₁-p̂₂)/√[p̂(1-p̂)(1/n₁+1/n₂)]
Cohen's h (effect size): h = 2arcsin(√p₁) - 2arcsin(√p₂)
CUPED variance reduction: Var(Y_CUPED) = Var(Y)(1-ρ²)
```

---

*Batch 6 Complete: 12 Questions | Basic (5) → Intermediate (4) → Advanced (3) + Quick Reference*
*Next: Batch 7 — Bayesian & Advanced Topics*
