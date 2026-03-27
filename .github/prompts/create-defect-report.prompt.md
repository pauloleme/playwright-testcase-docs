# Create Defect Report

Generate a professional, actionable defect report.

## What I Need From You
- **What happened** (actual behavior)
- **What should happen** (expected behavior)
- **Where** (URL, page, endpoint, module)
- **Evidence** (logs, screenshots, API response — if available)

## What I Produce

### Report Structure
1. **Header** — ID, title, severity, priority, environment, date
2. **Steps to Reproduce** — numbered, specific, reproducible
3. **Expected Result** — what should have happened
4. **Actual Result** — what actually happened (exact error messages)
5. **Evidence** — screenshots, logs, API responses
6. **Root Cause Analysis** — probable cause (if identifiable)
7. **Impact Assessment** — who/what is affected
8. **Regression Test** — Gherkin scenario to prevent recurrence
9. **Related Items** — linked test case, user story, or other defects

### Severity Classification
| Severity | Definition | Example |
|----------|-----------|---------|
| 🔴 Critical | System crash, data loss, security breach | Database corruption on save |
| 🟠 High | Major feature broken, no workaround | Cannot complete checkout |
| 🟡 Medium | Feature partially broken, workaround exists | Filter shows wrong count but list is correct |
| 🟢 Low | Cosmetic, minor UX | Button misaligned by 2px |

## Rules
- Steps must be **specific enough** for anyone to reproduce
- Expected result must cite the **requirement or acceptance criteria**
- Always suggest a **regression test** in Gherkin
- Never include **secrets or PII** in evidence
