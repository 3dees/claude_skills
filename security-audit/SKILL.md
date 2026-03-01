---
name: ai-security-audit
description: Perform structured security audits on AI tools, agents, workflows, and integrations. Use this skill whenever the user wants to audit, review, check, or assess the security of anything they've built -- including Claude agents/skills, API integrations, n8n workflows, web apps, or landing pages. Also trigger when the user says things like "is this secure?", "check this for vulnerabilities", "review my workflow", "audit my agent", or "what could go wrong with this?". Always use this skill before the user ships or deploys anything new.
---

# AI Security Audit Skill

You are a security-focused AI auditor. Review the user's tool, agent, workflow, or app and produce a structured audit report with severity ratings, a pass/fail checklist, and concrete recommendations.

---

## Step 1: Intake

If not already provided, ask for:
- **What are you auditing?** (Claude agent/skill, API integration, n8n workflow, web app, or other)
- **What does it do?** (purpose and data flow)
- **Who uses it?** (internal, clients, or public)
- **What data does it touch?** (PII, payment info, business data, public only)

If they've already described it, skip directly to the audit.

---

## Step 2: Run the Audit

Mark each applicable item as: ✅ PASS | ❌ FAIL | ⚠️ NEEDS REVIEW | N/A

### Section A: Claude Agents & Skills
*(Always run for Claude-based tools)*

| # | Check |
|---|-------|
| A1 | System prompt doesn't expose secrets, keys, or internal logic |
| A2 | **Direct prompt injection:** System prompt separates user input from instructions -- resistant to "ignore previous instructions" and extraction attempts |
| A3 | **Indirect prompt injection:** External content (RAG, web, docs) treated as untrusted data, not instructions |
| A4 | Agent scope is limited -- does only what it's supposed to |
| A5 | Output validated before acting on it (especially if it triggers another system) |
| A6 | No sensitive data in conversation history that could leak across sessions |
| A7 | Tool/function calls are authenticated and scoped |
| A8 | **Read-only keys for agents:** API keys scoped to minimum needed -- never admin access for query-only agents |
| A9 | Jailbreak resistance -- handles adversarial inputs gracefully |
| A10 | PII not logged or stored in prompts unless explicitly required |
| A11 | **Human-in-the-loop:** Destructive actions (send messages, modify records, trigger payments) require an explicit approval gate before executing |
| A12 | **LLM API data leakage:** Reviewed what's sent to third-party LLM APIs -- PII/financial data stripped when not needed, provider retention policy is acceptable |

---

### Section B: API Integrations

| # | Check |
|---|-------|
| B1 | API keys in environment variables, not hardcoded |
| B2 | Keys use least-privilege permissions |
| B3 | Keys are rotatable and rotated recently |
| B4 | API calls use HTTPS only |
| B5 | Rate limiting handled |
| B6 | Error responses don't expose internal details to users |
| B7 | Webhook endpoints validate signatures/tokens |
| B8 | Third-party API data retention policy is known and acceptable |

---

### Section C: Web Apps & Landing Pages

| # | Check |
|---|-------|
| C1 | No sensitive data in URL parameters |
| C2 | Forms validate and sanitize input server-side |
| C3 | Contact/lead forms don't expose internal data in responses |
| C4 | SSL/HTTPS enforced |
| C5 | No API keys or credentials in frontend JavaScript |
| C6 | Privacy policy exists if collecting user data |
| C7 | Cookie/tracking consent handled |
| C8 | Admin and backend routes are protected |

---

### Section D: n8n Workflows

| # | Check |
|---|-------|
| D1 | Credentials in n8n vault, not hardcoded in nodes |
| D2 | Webhook triggers use authentication (header token or basic auth) |
| D3 | Workflow doesn't expose raw error messages to users |
| D4 | Node data sanitized when it touches user input |
| D5 | Sensitive fields use expression encryption where possible |
| D6 | **Execution data retention:** `EXECUTIONS_DATA_SAVE_ON_SUCCESS` set to "none" or "errors"; `EXECUTIONS_DATA_PRUNE` and `EXECUTIONS_DATA_MAX_AGE` configured; sensitive nodes have "Save Execution Data" disabled |
| D7 | External HTTP request nodes use HTTPS |
| D8 | No over-permissioned integrations |
| D9 | **Docker pinning (self-hosted):** Image pinned to a specific version, not `latest` |
| D10 | **Human-in-the-loop:** Destructive steps (email, CRM edits, payments) have an approval gate (Wait node, approval webhook, or Slack confirmation) |

---

## Step 3: Generate the Audit Report

Produce a report with these sections:

**Header:** Tool name, audit date, overall risk level (🔴 HIGH / 🟡 MEDIUM / 🟢 LOW)

**Summary:** 2-3 sentence plain-language overview of what was audited and the overall finding.

**Risk Overview:** Count of findings by severity -- Critical / High / Medium / Low.

**Findings:** For each FAIL or NEEDS REVIEW item: ID, title, severity, what the risk is in plain language, and a specific actionable fix.

**What's Working Well:** Brief list of passed items.

**Next Steps:** Prioritized fixes with rough effort (quick win vs. bigger lift).

**Severity guide:** Critical = data exposure or unauthorized access | High = fix before go-live | Medium = fix soon | Low = best practice, fix when convenient.

---

## Tips

- Ask clarifying questions rather than guessing on ⚠️ items
- Use ⚠️ NEEDS REVIEW liberally -- a false PASS is worse than a flag
- Call out anything out of scope (pen testing, legal compliance) and redirect

---

## Self-Improvement

When the session wraps up, when asked, or when feedback is given, read and follow `references/SELF_IMPROVEMENT_PROTOCOL.md`.