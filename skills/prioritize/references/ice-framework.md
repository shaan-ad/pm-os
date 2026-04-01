# ICE Scoring Framework

ICE is a lightweight prioritization framework popularized by Sean Ellis (GrowthHackers). It trades RICE's precision for speed, making it ideal for early-stage prioritization, growth experiments, or situations where detailed data is not available.

## Formula

```
ICE Score = Impact x Confidence x Ease
```

All three dimensions use a 1-10 scale. Maximum possible score: 1,000.

## Dimensions

### Impact

**Definition**: If this works, how much will it move the target metric?

**Scoring guide (1-10)**:

| Score | Meaning |
|-------|---------|
| 10 | Game-changing. Would significantly shift a core business metric. |
| 7-9 | Strong impact. Clear, measurable improvement to a key metric. |
| 4-6 | Moderate impact. Helpful but not transformative. |
| 1-3 | Marginal impact. Small or indirect effect. |

Tips:
- Anchor to a specific metric before scoring. "Impact on what?" must be answered first.
- Compare features against each other, not in isolation. If Feature A is a 7, is Feature B really an 8?

### Confidence

**Definition**: How sure are you that this feature will deliver the expected impact?

**Scoring guide (1-10)**:

| Score | Meaning |
|-------|---------|
| 10 | Near certain. A/B test data, strong precedent, or trivial implementation. |
| 7-9 | High confidence. Strong qualitative evidence: multiple user interviews, competitor validation, clear user demand. |
| 4-6 | Moderate confidence. Some supporting signals but significant assumptions. |
| 1-3 | Low confidence. Mostly intuition. Unvalidated hypothesis. |

Tips:
- Be honest. Overconfidence is the most common scoring error.
- If the idea hasn't been validated with any user feedback, it rarely deserves above a 5.
- Novel ideas start at low confidence by definition. That's fine; just be transparent about it.

### Ease

**Definition**: How easy is this to implement? Consider engineering effort, design work, dependencies, and organizational complexity.

**Scoring guide (1-10)**:

| Score | Meaning |
|-------|---------|
| 10 | Trivial. A config change or copy update. No engineering needed. |
| 7-9 | Easy. One engineer, a few days. Uses existing patterns and infrastructure. |
| 4-6 | Moderate. One to two weeks. Some new code, but no architectural changes. |
| 2-3 | Hard. Multi-week effort. New systems, cross-team dependencies, or complex logic. |
| 1 | Extremely hard. Major infrastructure work, regulatory hurdles, or multi-month timeline. |

Tips:
- Ease is the inverse of Effort. High Ease = Low Effort.
- Include non-engineering work: legal review, vendor negotiations, data migrations.
- When in doubt, score lower. Underestimating difficulty is the default failure mode.

## Example

**Feature**: Add "Share to Slack" button on reports

| Dimension | Score | Rationale |
|-----------|-------|-----------|
| Impact | 6 | Moderate. Increases report visibility but doesn't change core workflow. |
| Confidence | 8 | High. Top-3 feature request. Competitors have it. Users specifically asked for Slack. |
| Ease | 7 | Straightforward. Slack API is well-documented. Similar share button exists for email. |

**ICE Score** = 6 x 8 x 7 = **336**

## Scoring at Scale

When scoring many features at once, use calibration anchors:

1. Pick 2-3 features the team already understands well
2. Score those first as reference points
3. Score remaining features relative to the anchors
4. Review the full list for consistency (does the ranking feel right?)

## ICE vs. RICE: When to Use Each

| Factor | Use ICE | Use RICE |
|--------|---------|----------|
| Data availability | Limited data, early stage | Good analytics and usage data |
| Number of features | Many items to triage quickly | Fewer items needing precise comparison |
| Team stage | Startup, growth team, exploratory | Established product with mature metrics |
| Speed needed | Quick prioritization session | Thorough planning exercise |
| Precision needed | Directional ranking is sufficient | Need defensible, data-backed ordering |

## Common Pitfalls

1. **Anchoring bias**: The first feature scored sets the scale for everything else. Start with a well-understood feature to calibrate.
2. **Confidence inflation**: Teams consistently overrate their confidence. If most features score 7+ on confidence, recalibrate.
3. **Ease optimism**: Engineering estimates skew optimistic. When a feature touches multiple systems, it's rarely above a 5 on Ease.
4. **Score clustering**: If everything lands between 200-400, the framework isn't differentiating. Widen your scoring range or add more granular criteria.
5. **Ignoring strategic fit**: ICE is purely tactical. Always overlay strategic alignment separately (the prioritize skill handles this with OKR multipliers).
