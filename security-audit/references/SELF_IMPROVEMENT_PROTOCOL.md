# Self-Improvement Protocol

This protocol guides how to improve the AI Security Audit skill based on feedback and lessons learned.

---

## When to Trigger

Run this protocol when:
- User provides feedback on an audit (positive or negative)
- A finding was missed that should have been caught
- A false positive was flagged
- User requests a new check type
- Session is wrapping up and improvements were identified

---

## Step 1: Capture the Learning

Ask yourself:
1. **What happened?** Describe the specific situation
2. **What should have happened?** The ideal outcome
3. **Why did it fail/succeed?** Root cause
4. **What's the fix?** Concrete change to prevent/replicate

---

## Step 2: Categorize the Improvement

| Type | Where to Update |
|------|-----------------|
| New check needed | Add to SKILL.md checklist |
| Check wording unclear | Clarify in SKILL.md |
| Missing remediation | Add to remediation-guide.md |
| False positive pattern | Add exclusion note to check |
| New attack vector | Add to relevant section |
| Scope creep | Clarify boundaries in SKILL.md |

---

## Step 3: Propose the Change

Format your proposal:

```
## Proposed Improvement

**Type:** [New check / Clarification / Remediation / Exclusion]
**Section:** [A / B / C / D]
**Change:**
[Specific text to add or modify]

**Rationale:**
[Why this improves the skill]

**Example:**
[Real scenario where this would help]
```

---

## Step 4: Validate Before Applying

Before modifying skill files:

1. **Does it fit the scope?** SMB-focused, practical, not enterprise-grade
2. **Is it actionable?** Clear pass/fail criteria
3. **Is it sustainable?** User can maintain the fix long-term
4. **Does it duplicate?** Check existing items first
5. **Is it testable?** Can verify the check works

---

## Step 5: Apply the Change

1. Edit the appropriate file (SKILL.md or remediation-guide.md)
2. Maintain consistent formatting
3. Keep check IDs sequential within sections
4. Update README.md if section counts change

---

## Common Improvement Patterns

### Pattern: Missed Check
**Symptom:** Audit passed but vulnerability existed
**Fix:** Add new check or make existing check more specific

### Pattern: False Positive
**Symptom:** Flagged something that wasn't actually a risk
**Fix:** Add context or exclusion criteria to the check

### Pattern: Unclear Remediation
**Symptom:** User didn't know how to fix the finding
**Fix:** Add specific steps to remediation-guide.md

### Pattern: Scope Confusion
**Symptom:** Audit went too deep or not deep enough
**Fix:** Clarify boundaries in SKILL.md intro or tips

---

## Quality Gates

Changes should NOT be applied if:
- Based on a single edge case (wait for pattern)
- Would make the audit significantly longer
- Requires expertise the target user doesn't have
- Duplicates existing coverage
- Is already handled by another tool (e.g., npm audit)
