# Launch Checklist: [Feature Name]

| Field | Value |
|---|---|
| Feature | [Feature name] |
| Target Launch Date | [Date] |
| PRD Link | [Link or N/A] |
| Owner | [Name] |
| Rollout Strategy | [Big-bang / Phased / Beta] |
| Last Updated | [Timestamp] |

---

## 1. Release Criteria

Prerequisites that must be met before the feature goes live.

- [ ] All P0 and P1 tickets are closed
- [ ] Feature flag is configured and tested in staging
- [ ] Performance benchmarks pass (p50 < Xms, p99 < Xms)
- [ ] Load testing completed at expected traffic levels
- [ ] Security review completed (if applicable)
- [ ] Data migration scripts tested and verified (if applicable)
- [ ] API versioning and backwards compatibility confirmed
- [ ] Legal/compliance review completed (if applicable)

## 2. QA Sign-off

- [ ] Core user flows tested end-to-end
- [ ] Edge cases documented and tested:
  - [ ] [Edge case 1]
  - [ ] [Edge case 2]
- [ ] Regression testing on adjacent features
- [ ] Cross-browser / cross-device testing
- [ ] Accessibility audit (WCAG 2.1 AA minimum)
- [ ] Error state handling verified
- [ ] Offline/degraded network behavior tested (if applicable)
- [ ] QA lead has signed off

## 3. Communications: Internal

- [ ] Engineering team briefed on rollout plan and on-call expectations
- [ ] Support team briefed with FAQ and known limitations
- [ ] Leadership notified of launch date and expected impact
- [ ] Internal announcement posted (Slack / email / wiki)
- [ ] Sales team briefed on positioning and customer impact (if applicable)

## 4. Communications: External

- [ ] Changelog entry drafted and reviewed
- [ ] Blog post or announcement drafted (if applicable)
- [ ] In-app notification or onboarding flow configured
- [ ] Social media posts scheduled (if applicable)
- [ ] Email to affected users drafted (if applicable)
- [ ] Partner/integration notifications sent (if applicable)

## 5. Rollback Plan

What to do if things go wrong.

- [ ] Rollback steps documented:
  - [ ] Feature flag kill switch tested
  - [ ] Database rollback script tested (if applicable)
  - [ ] Cache invalidation steps documented
- [ ] Rollback decision criteria defined (e.g., error rate > X%, latency > Xms)
- [ ] Rollback owner identified and available during launch window
- [ ] Rollback communication template prepared

## 6. Metrics to Watch

### First 24 hours
- [ ] Error rate (threshold: < X%)
- [ ] Latency p50 and p99
- [ ] Feature adoption rate
- [ ] Support ticket volume

### First 72 hours
- [ ] User retention for feature users vs. control
- [ ] Conversion funnel impact
- [ ] Infrastructure cost changes

### First 7 days
- [ ] Business KPI movement (revenue, engagement, etc.)
- [ ] User feedback themes
- [ ] Bug report volume and severity distribution

## 7. Stakeholder Notifications

| Stakeholder | Channel | Timing | Owner |
|---|---|---|---|
| Engineering | Slack | Day of launch | [Name] |
| Leadership | Email | 24h before | [Name] |
| Support | Meeting + doc | 48h before | [Name] |
| Sales | Slack + deck | 48h before | [Name] |
| Customers | Email / in-app | At launch | [Name] |

## 8. Support Team Prep

- [ ] FAQ document created and shared with support
- [ ] Known limitations documented
- [ ] Escalation path defined:
  - L1: Support team handles with FAQ
  - L2: Engineering on-call
  - L3: Feature team lead
- [ ] Expected ticket volume estimated
- [ ] Canned responses drafted for common questions
- [ ] Support team has access to feature flag controls (if needed)

## 9. Documentation Updates

- [ ] User-facing documentation updated
- [ ] API documentation updated (if applicable)
- [ ] Internal runbooks updated
- [ ] Architecture diagrams updated (if applicable)
- [ ] Onboarding/tutorial content updated (if applicable)

---

## Launch Day Run-of-Show

| Time | Action | Owner | Status |
|---|---|---|---|
| T-2h | Final go/no-go check | [PM] | [ ] |
| T-1h | Notify stakeholders launch is proceeding | [PM] | [ ] |
| T-0 | Enable feature flag / deploy | [Eng lead] | [ ] |
| T+15m | Smoke test core flows | [QA] | [ ] |
| T+1h | First metrics check | [PM + Eng] | [ ] |
| T+4h | Expanded metrics review | [PM] | [ ] |
| T+24h | Day-1 retrospective | [PM + Eng] | [ ] |

## Post-Launch Notes

_Space for real-time observations during and after launch._
