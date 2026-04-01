# RICE Scoring Framework

RICE is a prioritization framework developed by Intercom. It scores features across four dimensions to produce a single comparable number.

## Formula

```
RICE Score = (Reach x Impact x Confidence) / Effort
```

## Dimensions

### Reach

**Definition**: How many users or accounts will this feature affect in a defined time period (typically one quarter)?

**How to measure**:
- Use product analytics for existing user flows
- Estimate from total addressable users in the segment
- Count based on support tickets or feature requests

**Scoring guide**:

| Reach | Example |
|-------|---------|
| 10,000+ | Affects all active users |
| 5,000 | Affects a major user segment |
| 1,000 | Affects a specific cohort or use case |
| 500 | Affects a niche group |
| 100 | Affects a small subset |

Use real numbers, not abstract scales. Reach is measured in users (or accounts, transactions, etc.) per quarter.

### Impact

**Definition**: How much will this feature move the needle for each user it reaches?

**Scoring guide** (use these exact values):

| Score | Label | Meaning |
|-------|-------|---------|
| 3 | Massive | Transforms the user experience. Users can do something previously impossible. |
| 2 | High | Significant improvement. Removes major friction or unlocks important capability. |
| 1 | Medium | Noticeable improvement. Makes something easier or better. |
| 0.5 | Low | Minor improvement. Nice to have. |
| 0.25 | Minimal | Barely noticeable. Cosmetic or marginal change. |

When in doubt, score conservatively. Most features are a 1 (medium impact).

### Confidence

**Definition**: How confident are you in your Reach and Impact estimates?

**Scoring guide**:

| Score | Label | When to use |
|-------|-------|-------------|
| 100% | High | Backed by data: analytics, user research, or validated experiments |
| 80% | Medium | Supported by qualitative signals: customer interviews, sales feedback, industry patterns |
| 50% | Low | Based on intuition or loose analogy. Limited supporting evidence. |

Rules of thumb:
- If you're guessing at Reach, your Confidence cannot be "High"
- If Impact is based on one customer conversation, cap Confidence at 80%
- Pet projects and "gut feel" features start at 50%

### Effort

**Definition**: How much work will this take? Measured in person-weeks (one person working for one week).

**Scoring guide**:

| Effort | Scope |
|--------|-------|
| 0.5 | A few hours. Config change or minor tweak. |
| 1 | One person, one week. Small feature or improvement. |
| 2 | One person, two weeks. Medium feature with some complexity. |
| 4 | Two people for two weeks, or one person for a month. |
| 8 | A full team sprint. Significant new feature. |
| 16+ | Multi-sprint effort. Major new capability or system. |

Include all work: design, engineering, QA, documentation, rollout. If cross-team coordination is needed, add overhead.

## Example

**Feature**: In-app notification center

| Dimension | Value | Rationale |
|-----------|-------|-----------|
| Reach | 8,000 | 80% of monthly active users check notifications |
| Impact | 2 | High. Replaces email-only notifications, which users miss. |
| Confidence | 80% | Based on support ticket volume and user interviews |
| Effort | 4 | Two engineers, two weeks. Requires new backend service + UI. |

**RICE Score** = (8,000 x 2 x 0.80) / 4 = **3,200**

## Common Pitfalls

1. **Inflating Impact**: Most features are a 1. Reserve 3 for truly transformative changes.
2. **Ignoring Confidence**: A feature with 50% confidence and a score of 5,000 is effectively a 2,500.
3. **Underestimating Effort**: Include QA, code review, deployment, and monitoring setup. Engineering estimates are typically 1.5-2x the initial guess.
4. **Comparing across very different scales**: RICE works best when comparing features of roughly similar scope. Comparing a button color change to a platform rebuild produces misleading rankings.
5. **Treating the score as gospel**: RICE is a conversation starter, not an oracle. Use it to surface surprising rankings and challenge assumptions.
