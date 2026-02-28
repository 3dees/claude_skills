# Self-Improvement Protocol (v3)

Evaluates skill performance, prevents drift, enforces complexity discipline.

---

# PHASE 1 — SESSION ANALYSIS

Review the conversation. Log findings in priority order.

---

## Finding Categories

### 1. Automation Gaps (Highest Priority)
User manually requested something that should have been automatic per the skill's mission.

### 2. Structural Failures
Systemic breakdowns: missing intake logic, conflicting instructions, misalignment with core mission.

### 3. Edge Cases
Repeatable input scenarios handled poorly. Skip one-off anomalies.

### 4. Checklist Gaps
Meaningful elements missing from the workflow. Confirm not implicitly covered.

### 5. Output Format Issues
Formatting that caused confusion. Skip stylistic preferences.

### 6. Context Patterns
Recurring user behaviors or workflow patterns. Default action: memory update (not skill edit).

### 7. Protocol Friction
Issues where this protocol caused friction. Log only, take no action. See [PROTOCOL_GOVERNANCE.md](PROTOCOL_GOVERNANCE.md).

---

## Required Structure for Each Finding

- **Category**
- **What happened**
- **Root cause** (if unknown, do not propose structural change)
- **Impact:** High / Medium / Low
- **Confidence:** High / Medium / Low

---

## Auto-Reject Threshold

Findings with LOW impact AND LOW confidence are logged but not proposed as structural changes.

---

# PHASE 2 — FINDINGS SUMMARY

Present before making changes.

```
## Skill Improvement Summary

Skill: [name]
Session: [YYYY-MM-DD]
Findings: [count]

## Proposed Structural Changes

### [Category]
- What happened:
- Root cause:
- Impact:
- Proposed fix:
- Location in SKILL.md:

## Complexity Check

Before proposing, verify:
- Does this increase cognitive load?
- Can consolidation solve this instead of adding?
- Is this a repeatable pattern?

## Memory Updates (No Approval Needed)

[List items going to memory, not SKILL.md]
```

Then ask: "Should I apply these structural changes? You may approve all, select specific items, or reject."

---

# PHASE 3 — APPLY APPROVED CHANGES

1. Update SKILL.md with approved changes only
2. Prefer consolidation over addition
3. Preserve mission and scope
4. Save memory updates

## Confirmation Output

```
### Skill Updated

Changes applied: [count]
- [Description] → [Section]

Consolidations: [if any]
Skipped: [if any]
```

---

# GUARDRAILS

Never auto-apply changes that:
- Remove coverage without consolidation
- Expand scope beyond original mission
- Change fundamental purpose
- Are triggered by a single unusual session
- Reflect stylistic preference rather than structural flaw

---

# ANTI-BLOAT RULE

Skills must not grow indefinitely.

**Allowed:** Refinement, clarification, consolidation, precision edits

**Discouraged:** Additive checklist expansion

If 3+ additions proposed without consolidation → require Simplification Review first.

---

## Consolidation Defined

Consolidation means:
- Merging two overlapping checklist items into one
- Generalizing specific rules into a broader principle
- Removing redundant instructions that duplicate existing coverage
- Replacing verbose explanations with concise references

Goal: Net-zero or negative line count when adding capability.

---

# DRIFT DETECTION

Before applying changes, verify:
- Does this shift the skill's mission?
- Does it increase unnecessary complexity?
- Is tone changing unintentionally?

If yes → Surface as "Potential Drift" before applying.

---

# DECISION DISCIPLINE

Default action is no structural change unless:
- Structural weakness confirmed
- Repeatable pattern exists
- Misalignment with mission evident

Precision over accumulation.
