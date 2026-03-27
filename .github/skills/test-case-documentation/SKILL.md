# Test Case Design & QA Documentation — Domain Knowledge

## TEST DESIGN TECHNIQUES

### 1. Equivalence Partitioning (EP)
Divide input domain into classes where all values in a class should behave identically.

**Example — Age field (valid: 18-65):**
| Partition | Values | Expected |
|-----------|--------|----------|
| Below minimum | 0, 5, 17 | Reject |
| Valid range | 18, 30, 65 | Accept |
| Above maximum | 66, 100, 999 | Reject |
| Non-numeric | "abc", "!@#" | Reject |
| Empty/null | "", null | Reject (if required) |
| Negative | -1, -100 | Reject |

**Rule**: Pick ONE representative value from each partition. Don't test every value — that's the point of EP.

### 2. Boundary Value Analysis (BVA)
Test at the exact edges of partitions — where most bugs live.

**Example — Age field (valid: 18-65):**
| Value | Position | Expected |
|-------|----------|----------|
| 17 | max-1 (below min) | Reject |
| 18 | min (boundary) | Accept |
| 19 | min+1 (just above) | Accept |
| 64 | max-1 (just below) | Accept |
| 65 | max (boundary) | Accept |
| 66 | max+1 (above max) | Reject |
| 0 | zero | Reject |
| -1 | negative | Reject |

**Rule**: Always test min-1, min, min+1, max-1, max, max+1, zero, and empty.

### 3. Decision Table
For features where multiple conditions interact to produce different outcomes.

**Example — Discount Rules:**
| Condition | Rule 1 | Rule 2 | Rule 3 | Rule 4 |
|-----------|--------|--------|--------|--------|
| Customer is VIP? | Yes | Yes | No | No |
| Order > $100? | Yes | No | Yes | No |
| **Action** | 20% off | 10% off | 5% off | 0% off |

Each column = one test case. Cover all combinations.

### 4. State Transition Testing
For entities with lifecycle (status that changes based on events).

**Example — Order lifecycle:**
[Created] --pay--> [Paid] --ship--> [Shipped] --deliver--> [Delivered]
    |                  |                                         |
    +---cancel--> [Cancelled]                           +--return--> [Returned]

**Test cases from state diagram:**
| TC | Current State | Event | Expected New State |
|----|--------------|-------|--------------------|
| TC-001 | Created | Pay | Paid |
| TC-002 | Created | Cancel | Cancelled |
| TC-003 | Paid | Ship | Shipped |
| TC-004 | Paid | Cancel | Cancelled |
| TC-005 | Shipped | Deliver | Delivered |
| TC-006 | Shipped | Cancel | ❌ Should be rejected |
| TC-007 | Delivered | Return | Returned |
| TC-008 | Cancelled | Pay | ❌ Should be rejected |

**Rule**: Test ALL valid transitions AND all INVALID transitions (events that should be rejected in each state).

### 5. Pairwise / Combinatorial Testing
When too many parameter combinations exist to test exhaustively.

**Example — Search filters (3 params, 3 values each = 27 combos):**
Full combinatorial: 27 test cases
Pairwise coverage: ~9 test cases (covers all pairs)

| TC | Status | Category | Sort By |
|----|--------|----------|---------|
| 1 | Active | Finance | Name |
| 2 | Active | HR | Date |
| 3 | Active | IT | Amount |
| 4 | Inactive | Finance | Date |
| 5 | Inactive | HR | Amount |
| 6 | Inactive | IT | Name |
| 7 | All | Finance | Amount |
| 8 | All | HR | Name |
| 9 | All | IT | Date |

### 6. Error Guessing (Experience-Based)
Common defect patterns from 10+ years of QA:

- **Null/undefined** — what if a required field is not sent at all?
- **Empty string** — "" vs null vs " " (whitespace only)
- **Off-by-one** — loop boundaries, pagination limits
- **Timezone** — dates crossing midnight, DST transitions, UTC vs local
- **Concurrency** — same record edited by two users simultaneously
- **Large data** — 10,000 records, 5MB payload, 1000-char string
- **Special characters** — <>&"'/\  Unicode, emoji, RTL text
- **Duplicate submission** — double-click, back button + resubmit
- **Session expiry** — action attempted after token expires
- **Case sensitivity** — "Admin" vs "admin" vs "ADMIN"

### 7. Exploratory Heuristics

**SFDPOT** — Structure, Function, Data, Platform, Operations, Time
**FEW HICCUPS** — Features, Errors, Workflow, History, Images, Configuration, Compatibility, Usability, Performance, Security
**RCRCRC** — Recent, Core, Risky, Configuration, Repaired, Chronic

---

## QA DOCUMENT STANDARDS

### Document Hierarchy
Test Strategy (project-level, written once)
    └── Test Plan (per feature/release)
            ├── Test Cases (per requirement)
            ├── Traceability Matrix (req → TC mapping)
            └── Test Report (per execution cycle)
                    └── Defect Reports (per bug found)

### Document Metadata (EVERY document must have)
| Field | Purpose |
|-------|---------|
| Title | Clear document name |
| Version | Track changes (1.0, 1.1, 2.0) |
| Author | Who wrote it |
| Date | When created/updated |
| Status | Draft → In Review → Approved |
| Reviewers | Who approved |
| Distribution | Who receives this |

### Test Case Quality Criteria
A good test case must be:
- **Atomic** — tests ONE thing
- **Independent** — doesn't depend on other test cases
- **Repeatable** — same result every time
- **Traceable** — linked to a requirement
- **Specific** — concrete data, not "valid input"
- **Complete** — precondition + steps + expected result

### Coverage Metrics
| Metric | Formula | Target |
|--------|---------|--------|
| Requirement Coverage | (reqs with TCs / total reqs) × 100 | ≥ 95% |
| Test Execution Rate | (executed / total TCs) × 100 | 100% |
| Pass Rate | (passed / executed) × 100 | ≥ 90% |
| Defect Detection Rate | defects found / defects total | Track trend |
| Defect Density | defects / KLOC or per feature | Compare |

### Severity vs Priority
|  | Critical | High | Medium | Low |
|---|---|---|---|---|
| **Severity** (impact) | Data loss, crash | Major feature broken | Minor issue | Cosmetic |
| **Priority** (urgency) | Fix NOW | Fix this sprint | Fix next sprint | Backlog |

Note: High severity ≠ high priority. A crash in an unused feature is critical severity but may be low priority.

---

## WRITING GUIDELINES

1. **Tables over paragraphs** — structured data is always clearer
2. **Concrete over abstract** — "enter 'test@email.com'" not "enter a valid email"
3. **Numbers over adjectives** — "95% pass rate" not "mostly passing"
4. **Short sentences** — max 20 words per sentence
5. **Active voice** — "User clicks Submit" not "Submit button is clicked by user"
6. **Present tense** — "System displays error" not "System will display error"
7. **One idea per bullet** — if it has "and", split it
8. **Include the negative** — document what should NOT happen too
