# Protocol Governance

Rules for improving the Self-Improvement Protocol itself.

---

## How to Trigger a Protocol Review

Say one of these:
- "Review the self-improvement protocol"
- "Improve the protocol"
- "The protocol isn't working"
- "Check protocol friction"

When triggered, Claude will:
1. Read this file and SELF_IMPROVEMENT_PROTOCOL.md
2. Review any logged Category 7 (Protocol Friction) findings from recent sessions
3. Propose changes using the template below
4. Wait for approval before editing

---

## When Protocol Changes Are Allowed

Protocol changes may be proposed only if:
- User explicitly triggers a review (see above), OR
- Protocol caused clear harm in the current session (blocked a valid fix, created bloat, or incorrect behavior)

Do NOT propose changes from single-session friction unless it caused demonstrable harm.

---

## Category 7: Protocol Friction (Log Only)

During normal skill improvement, log issues where the protocol caused friction:
- Scoring criteria unclear or inconsistent
- Approval flow too heavy for small fixes
- Anti-bloat rules too strict or too loose
- Categories missing for recurring failure types

Action: Log in findings, take no automatic action. Review only when user requests.

---

## Governance Levels

**Level A** (Safe, normal approval flow):
- Clarify wording
- Add examples
- Tighten definitions

**Level B** (Requires explicit "Protocol Change" flag):
- Change thresholds
- Change guardrails
- Add/remove categories

---

## No-Recursion Guardrail

Forbidden:
- Any change that requires more frequent protocol self-review
- Any rule that triggers protocol revision on every session

---

## Protocol Improvement Template

```
### Protocol Improvement Candidate

- What happened:
- Why it matters:
- Proposed tweak:
- Governance level: A / B
- Recursion risk: None / Low / High
```
