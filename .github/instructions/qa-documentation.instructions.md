# QA Documentation Conventions

## Document Structure
- Every document starts with **metadata table** (project, author, date, version, status)
- Use **Markdown** format for portability (renders in Git, wikis, IDEs)
- Use **tables** for structured data — never long paragraphs
- Include **version history** in long-lived documents

## Test Case Rules
- ID format: TC-001, TC-002 (sequential, zero-padded)
- Every test case has: ID, Category, Priority, Title, Precondition, Steps, Expected Result
- Expected results must be **specific** — "Returns HTTP 200 with {id, status: 'Active'}" not "works"
- Steps use **numbered list** — "1. Navigate to... 2. Click... 3. Fill..."
- Include **concrete test data** — "enter test@email.com" not "enter valid email"

## Category Tags
- ✅ POSITIVE, ❌ NEGATIVE, 📏 BOUNDARY, 🔲 EDGE, 🔒 SECURITY, ♿ ACCESSIBILITY, ⚡ PERFORMANCE

## Priority Tags
- 🔴 P1 Critical, 🟠 P2 High, 🟡 P3 Medium, 🟢 P4 Low

## Coverage Requirements
- Minimum 19 test cases per feature
- Must include: positive (3+), negative (5+), boundary (4+), edge (3+), security (2+)

## Naming Conventions
- Test Plan: test-plan-[feature-name].md
- Test Cases: test-cases-[feature-name].md
- Test Report: test-report-[sprint-or-release].md
- Defect Report: defect-[BUG-ID]-[brief-title].md
- RTM: rtm-[feature-name].md

## Language
- Write in the same language as the project team communicates
- Use **active voice** and **present tense**
- Max 20 words per sentence
