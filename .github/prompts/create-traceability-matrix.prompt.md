# Create Traceability Matrix (RTM)

Map requirements to test cases and identify coverage gaps.

## What I Need From You
- **Requirements list** (user stories, acceptance criteria, business rules)
- **Test cases** (existing, or I'll generate them first)

## What I Produce

### RTM Table
| Req ID | Requirement | Test Case IDs | Status | Coverage |
|--------|-------------|---------------|--------|----------|
| REQ-001 | User can login with email/password | TC-001 to TC-012 | ✅ Covered | 100% |
| REQ-002 | Password must have 8+ chars | TC-013 to TC-018 | ✅ Covered | 100% |
| REQ-003 | Admin can export report | — | ❌ No tests | 0% |

### Coverage Summary
- Total requirements: X
- Fully covered: Y (Z%)
- Partially covered: W
- Not covered: V (**risk!**)

### Gap Analysis
List requirements without test cases and recommend test cases to close gaps.

## Rules
- Every requirement MUST have at least one test case
- Flag **0% coverage** as ❌ risk
- Include **forward traceability** (req → TC) AND **backward traceability** (TC → req)
- Orphan test cases (no linked requirement) should be flagged too
