# Generate Test Cases

Analyze the provided input and produce comprehensive, professional test cases.

## What I Accept
- User story / acceptance criteria
- API endpoint description or Swagger spec
- Screen / UI description or mockup
- Business rule in any format
- Bug report (to create regression tests)
- Even vague/incomplete descriptions

## What I Deliver

### Output Format (choose one, default = Table)
1. **Table** — ID, Category, Priority, Title, Precondition, Steps, Expected Result
2. **Gherkin** — Given/When/Then with @tags
3. **Mind Map** — Tree structure for exploratory testing

### Categories I Cover
| Tag | Meaning | Minimum |
|-----|---------|---------|
| ✅ POSITIVE | Happy path | 3+ |
| ❌ NEGATIVE | Invalid/unauthorized | 5+ |
| 📏 BOUNDARY | Min/max/off-by-one | 4+ |
| 🔲 EDGE | Null/empty/special chars | 3+ |
| 🔒 SECURITY | Injection/auth | 2+ |
| ⚡ PERFORMANCE | Response time/load | 1+ |
| ♿ ACCESSIBILITY | Keyboard/screen reader | 1+ (if UI) |

### Priorities
- 🔴 P1 Critical — Core functionality, data integrity
- 🟠 P2 High — Important business flows
- 🟡 P3 Medium — Secondary features
- 🟢 P4 Low — Cosmetic, extreme edge

## Rules
- Minimum **19 test cases** per feature
- **Concrete test data** — never "valid input", always "test@email.com"
- **Specific expected results** — never "works correctly"
- Flag ambiguities as ⚠️ ASSUMPTION
- Deliver draft FIRST, ask clarifications AFTER
