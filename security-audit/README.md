# AI Security Audit Skill

Practical security audits for small and mid-sized businesses.

## What It Does

Reviews AI tools, agents, workflows, APIs, and automations for security vulnerabilities. Produces actionable findings with severity ratings and effort estimates.

## Target Audience

- No dedicated security team
- Heavy SaaS usage (M365, Google Workspace, etc.)
- Limited custom infrastructure
- Need sustainable, not enterprise-grade, security

## Trigger Phrases

- "Is this secure?"
- "Check for vulnerabilities"
- "Audit this"
- "Review my workflow"
- "What could go wrong?"

## Audit Sections

| Section | What It Covers | Checks |
|---------|----------------|--------|
| A | Claude Agents & Skills | 12 |
| B | API Integrations | 8 |
| C | Web Apps & Landing Pages | 8 |
| D | n8n Workflows | 10 |

**Total: 38 checks**

## Key Checks by Section

### Section A: Claude Agents & Skills
- Secrets not exposed in prompts
- Direct & indirect prompt injection resistance
- Human-in-the-loop for destructive actions
- LLM API data leakage review
- Read-only API keys for query-only agents

### Section B: API Integrations
- Keys in env vars, not hardcoded
- Least-privilege permissions
- HTTPS only, rate limiting
- Webhook signature validation

### Section C: Web Apps & Landing Pages
- No secrets in frontend JS or URLs
- Server-side input validation
- SSL/HTTPS enforced
- Privacy policy & consent handling

### Section D: n8n Workflows
- Credentials in vault
- Authenticated webhooks
- Execution data retention configured
- Docker image pinning (self-hosted)
- Human-in-the-loop for destructive steps

## Files

```
security-audit/
├── SKILL.md                       # Main audit instructions
├── README.md                      # This file
└── references/
    ├── remediation-guide.md       # Quick fixes for findings
    └── SELF_IMPROVEMENT_PROTOCOL.md  # Skill learning protocol
```

## Output Format

Each finding includes:
- **Severity:** Critical / High / Medium / Low
- **Risk:** Plain-language explanation
- **Fix:** Specific actionable recommendation
- **Effort:** Quick win vs. bigger lift

## Severity Guide

| Level | Meaning |
|-------|---------|
| Critical | Data exposure or unauthorized access |
| High | Fix before go-live |
| Medium | Fix soon |
| Low | Best practice, fix when convenient |
