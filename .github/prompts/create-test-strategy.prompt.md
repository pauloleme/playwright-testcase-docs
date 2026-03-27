# Create Test Strategy

Generate a project-level Test Strategy document.

## What I Need From You
- **Project name and description**
- **Technology stack** (frontend, backend, database, CI/CD)
- **Team structure** (who does what — devs, QA, PO)
- **Release cadence** (sprints? releases? continuous delivery?)

## Document Sections I Produce
1. **Testing Levels** — Unit, Integration, System, Acceptance (who owns each)
2. **Test Types Matrix** — Functional, API, UI, Performance, Security, Accessibility (automated? tool? frequency?)
3. **Automation Strategy** — What to automate, what to keep manual, ROI criteria
4. **Defect Management** — Severity definitions, SLAs, workflow, escalation
5. **Test Data Strategy** — How to create, manage, and clean up test data
6. **Environment Strategy** — DEV, QA, Staging, Prod-like requirements
7. **Metrics & Reporting** — What metrics to track, how often to report
8. **Tools** — Selected tools with justification
9. **Risks** — Strategic-level risks and mitigations

## Rules
- This is a **living document** — include version history section
- Strategy is **project-wide** (not per feature — that's the Test Plan)
- Include the **automation decision rule**: "If a test runs 3+ times, automate it"
- Define **clear ownership** for each testing level
