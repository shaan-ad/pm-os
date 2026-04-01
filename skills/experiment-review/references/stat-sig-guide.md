# Statistical Significance Guide for PMs

A practical reference for understanding and applying statistical rigor to A/B tests and experiments.

---

## Core Concepts

### P-value

The probability of observing results at least as extreme as the actual results, assuming the null hypothesis is true (i.e., assuming there is no real difference between variants).

- **p < 0.05**: Conventionally considered "statistically significant." There is less than a 5% chance the observed difference is due to random noise.
- **p = 0.05 is not magic**: It is an arbitrary threshold. p = 0.049 and p = 0.051 are practically identical.
- **A low p-value does not mean the effect is large.** It means the effect is unlikely to be zero. A tiny, meaningless effect can have a low p-value with enough data.

### Confidence Interval (CI)

A range of values that likely contains the true effect size.

- A **95% CI** means: if we ran this experiment 100 times, roughly 95 of those intervals would contain the true effect.
- **If the CI excludes zero**, the result is statistically significant at the 5% level.
- **The width of the CI tells you about precision.** A narrow CI means you have a good estimate. A wide CI means uncertainty remains.
- **Read the CI, not just the point estimate.** A +5% lift with a CI of [+0.1%, +9.9%] means the true effect could be anywhere from barely positive to strongly positive.

### Effect Size

The magnitude of the difference between variants.

- **Statistical significance is not practical significance.** A 0.01% improvement can be statistically significant with millions of users, but may not be worth the engineering cost to maintain.
- Always ask: "Is this effect large enough to matter for the business?"

### Statistical Power

The probability that your experiment will detect a real effect if one exists.

- **Convention: aim for 80% power.** This means a 20% chance of a false negative (missing a real effect).
- **Underpowered experiments are dangerous.** A non-significant result from an underpowered test does not mean "no effect." It means "we could not tell."
- Power depends on three things: sample size, effect size, and variance.

---

## Sample Size Requirements

### How to Calculate

Before running an experiment, determine the required sample size using:

1. **Baseline conversion rate** (current metric value)
2. **Minimum Detectable Effect (MDE)**: the smallest effect you care about detecting
3. **Significance level** (usually 0.05)
4. **Power** (usually 0.80)

### Rule of Thumb Formula

For a two-proportion z-test (e.g., comparing conversion rates):

```
n per variant = 16 * (p * (1-p)) / (MDE)^2
```

Where `p` is the baseline conversion rate and `MDE` is the absolute minimum detectable effect.

**Examples:**

| Baseline Rate | MDE (absolute) | MDE (relative) | N per Variant |
|---|---|---|---|
| 5% | 0.5% | 10% | ~4,864 |
| 5% | 1.0% | 20% | ~1,216 |
| 10% | 1.0% | 10% | ~1,440 |
| 10% | 2.0% | 20% | ~360 |
| 50% | 2.5% | 5% | ~640 |

These are approximations. Use a proper calculator for exact values.

### Duration Planning

- Calculate daily eligible traffic
- Divide required sample size by daily traffic to get minimum duration
- **Always run for at least one full week** to capture day-of-week effects
- **Two weeks is safer** for most experiments

---

## Common Pitfalls

### 1. Peeking (Repeated Significance Testing)

**The problem:** Checking results daily and stopping when you see p < 0.05 inflates your false positive rate dramatically. With daily checks over two weeks, your actual false positive rate can be 20-30% instead of 5%.

**The fix:**
- Pre-commit to a fixed sample size or runtime
- If you must peek, use **sequential testing** methods (e.g., group sequential design, always-valid p-values)
- Or use a **Bayesian approach** that does not penalize for peeking

### 2. Underpowered Tests

**The problem:** Running an experiment without enough sample size. Non-significant results are uninterpretable because you cannot distinguish "no effect" from "effect exists but we could not detect it."

**The fix:**
- Calculate required sample size before starting
- If you cannot reach the required sample size, either accept a larger MDE or do not run the experiment
- Communicate power limitations clearly in the analysis

### 3. Multiple Comparisons

**The problem:** Testing 20 metrics increases the chance that at least one shows p < 0.05 by random chance. With 20 independent tests, there is a 64% chance of at least one false positive.

**The fix:**
- Designate one primary metric before the experiment starts
- Apply Bonferroni correction for secondary metrics: divide alpha by the number of tests (e.g., 0.05 / 10 = 0.005 per metric)
- Or use False Discovery Rate (FDR) correction, which is less conservative

### 4. Simpson's Paradox

**The problem:** A trend that appears in overall data reverses when the data is split by segments. Example: Treatment wins overall, but loses in every individual segment because segment proportions differ between variants.

**The fix:**
- Always check for Sample Ratio Mismatch (SRM) first
- Analyze key segments independently
- If you see paradoxical results, investigate the traffic allocation

### 5. Novelty and Primacy Effects

**The problem:** Users react to change itself, not the change's value. A new button color gets more clicks initially just because it is new. Conversely, some users resist any change initially.

**The fix:**
- Run experiments for at least 2 weeks
- Segment by new vs. returning users
- Plot the treatment effect over time. A decaying effect suggests novelty. A growing effect suggests primacy wearing off.

### 6. Survivorship Bias in Metric Selection

**The problem:** Only looking at users who completed an action, ignoring those who dropped off. "Average revenue per purchasing user" can increase while total revenue decreases if you are driving away buyers.

**The fix:**
- Include intent-to-treat metrics (all users in the experiment, not just those who engaged)
- Track both rate metrics (conversion) and volume metrics (total events)

---

## When to Call a Test Early vs. Let It Run

### Stop Early If:
- **SRM detected**: The traffic split is wrong. Results are invalid. Fix the bug first.
- **Severe negative impact**: If the treatment is causing significant harm (errors, crashes, revenue loss), stop for ethical/business reasons. This is a guardrail metric, not a significance decision.
- **Using a valid sequential testing method**: If you pre-planned sequential analysis with proper alpha spending, you can stop at a pre-defined interim analysis.

### Let It Run If:
- Results look promising but the CI is wide. More data will narrow it.
- You have not reached the pre-committed sample size or runtime.
- The result is borderline significant (p near 0.05). Stopping here is classic peeking bias.
- The experiment has not run for at least one full business cycle.

### Never Do:
- Stop early because p < 0.05 on day 3 of a planned 14-day test.
- Extend a test because you "almost" reached significance and want more time. This inflates false positives. If you extend, pre-commit to a new endpoint.
- Run a test with no predetermined stopping criteria.

---

## Quick Decision Framework

```
Is the result statistically significant (p < 0.05)?
  |
  +-- YES: Is the effect practically significant?
  |     |
  |     +-- YES: Are secondary metrics healthy?
  |     |     |
  |     |     +-- YES --> SHIP
  |     |     +-- NO  --> INVESTIGATE secondary impact, possibly ITERATE
  |     |
  |     +-- NO: Effect is real but too small to matter --> KILL (not worth the complexity)
  |
  +-- NO: Was the experiment sufficiently powered?
        |
        +-- YES: The effect likely does not exist or is smaller than your MDE --> KILL
        +-- NO: You cannot conclude anything --> EXTEND with proper sample size
```

---

## Glossary

| Term | Definition |
|---|---|
| Alpha (significance level) | The false positive rate you accept, typically 0.05 |
| Beta | The false negative rate, typically 0.20 (power = 1 - beta) |
| Bonferroni correction | Divides alpha by the number of tests to control family-wise error rate |
| Confidence interval | Range likely containing the true effect size |
| Effect size | The magnitude of difference between variants |
| FDR (False Discovery Rate) | Expected proportion of false positives among all significant results |
| MDE (Minimum Detectable Effect) | Smallest effect size the experiment is designed to detect |
| Null hypothesis | The assumption that there is no difference between variants |
| P-value | Probability of seeing results this extreme if the null hypothesis is true |
| Power | Probability of detecting a real effect (1 - beta) |
| SRM (Sample Ratio Mismatch) | Actual traffic split differs from expected split, indicating a bug |
| Type I error | False positive, concluding an effect exists when it does not |
| Type II error | False negative, failing to detect a real effect |
