# AI Security Audit Skill

Practical security audits for small and mid-sized businesses.

## What It Does

Reviews AI tools, agents, workflows, APIs, and automations for security vulnerabilities. Produces actionable findings with effort estimates.

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

## Audit Modes

| Mode | What It Checks | When to Use |
|------|----------------|-------------|
| Quick | Top 10 critical checks only | Pre-deploy sanity check |
| Full | All 10 sections (includes 9b n8n hardening) | New systems, periodic review |

## Top 10 Critical Checks

1. No secrets in prompts, code, repos, or logs
2. Every entry point requires authentication
3. Admin access separated from normal users
4. Input validation on all user data
5. Output validation before triggering tools
6. Rate limiting on public endpoints
7. HTTPS everywhere
8. Dependencies updated, no critical CVEs
9. No sensitive data in logs
10. RAG content treated as untrusted

## Full Audit Sections

1. Identity & Access
2. Agent & Automation Safety
3. Abuse & Cost Protection
4. Webhook & API Security
5. URL Fetching / SSRF Safety
6. Dependencies & SaaS Risk
7. Data Protection
8. Logging & Alerts
9. n8n / Automation Platforms
9b. n8n Hardening (Advanced) - instance security, workflow patterns, network isolation

## Files

```
security-audit/
├── SKILL.md           # Main audit instructions
├── README.md          # This file
└── references/
    └── remediation-guide.md   # Quick fixes for findings
```

## Output Format

Each finding includes:
- **Severity:** HIGH / MEDIUM / LOW
- **Why It Matters:** Plain-language explanation
- **Exploit Scenario:** Brief realistic attack
- **Efficient Fix:** Simple, sustainable solution
- **Effort:** S (under 1 hour) / M (half day) / L (multi-day)
- **Residual Risk:** What remains after the fix
