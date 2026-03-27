---
name: cucumber-bdd
description: "Domain knowledge for Cucumber.js BDD test automation: Gherkin syntax, step definitions, hooks, tags, parallel execution, and Allure reporting. Used by QA and automation agents."
---

# Universal Cucumber.js BDD — Domain Knowledge

## Gherkin Syntax Rules

### Feature Files (Generic)
```gherkin
@Resource @TestType
Feature: Resource - Test Type description

    Background:
        Given the API is authenticated

    @Batch @Regression
    Scenario Outline: Action description with <parameter>
        Given there is a valid "<payloadType>" payload
        When the payload is sent to the API
        Then the result should be available in the database within <timeout> minutes
        And the status should be "<expectedStatus>"

        Examples:
            | payloadType | timeout | expectedStatus |
            | typeA       | 30      | Success        |
            | typeB       | 30      | Success        |
```

### Tag Strategy
| Tag | Purpose | Example |
|-----|---------|---------|
| `@Resource` | Filter by resource | `@Resource` |
| `@Batch` | Batch processing tests | `@Batch` |
| `@Regression` | Full regression suite | `@Regression` |
| `@Performance` | Performance burst tests | `@Performance` |
| `@Load` | Sustained load tests | `@Load` |
| `@UI` | Browser/Selenium tests | `@UI` |
| `@Smoke` | Quick sanity checks | `@Smoke` |

### Step Definitions Pattern (Generic)
```javascript
const { Given, When, Then } = require('@cucumber/cucumber');
const chai = require('chai');
const { expect } = chai;

// Load payload as constant (generic path)
const payload = require('../../payloads/generic.payload.json');

// Deep clone in Given — NEVER mutate the original
Given('there is a valid {string} payload', function (type) {
        this.body = JSON.parse(JSON.stringify(payload.body));
        this.body.id = generateUniqueId();
});

// Timestamp before sending
When('the payload is sent to the API', async function () {
    this.triggerTime = utils.getUtcTime();
    this.body.EntityValue.UTCModifiedOn = this.triggerTime;
    this.attach(JSON.stringify(this.body, null, 2), 'application/json');
    this.response = await utils.sendToSDS(this.body);
});

// Polling with timeout
Then('the integration log should be created in CosmosDB within {int} minutes',
    { timeout: 30 * 60 * 1000 },
    async function (minutes) {
        const log = await utils.pollCosmosDB(this.body.EntityValue.IQID, minutes);
        expect(log).to.not.be.null;
        this.attach(JSON.stringify(log, null, 2), 'application/json');
    }
);
```

## Parallel Execution Rules
- Each scenario is **independent** — no shared state between scenarios
- Use `this.` (Cucumber World) for scenario-scoped data, never global variables
- Generate **unique IDs** per scenario: `uuidv4()`, `utils.generateIqid()`
- Never write to shared files during parallel execution
- Run with: `--parallel 4`

## Hooks
```javascript
const { Before, After, BeforeAll, AfterAll } = require('@cucumber/cucumber');

Before(async function () {
    // Runs before each scenario
    this.scenarioStartTime = Date.now();
});

After(async function (scenario) {
    // Attach scenario timing to Allure
    const duration = Date.now() - this.scenarioStartTime;
    this.attach(`Duration: ${duration}ms`, 'text/plain');
});
```

## Allure Reporting
- Attach request payloads: `this.attach(JSON.stringify(body, null, 2), 'application/json')`
- Attach response data: `this.attach(JSON.stringify(response, null, 2), 'application/json')`
- Attach CSV summaries: `this.attach(csvContent, 'text/csv')`
- Allure results go to `allure-results/` directory

## Project Structure
```
features/
├── entity_name/
│   ├── basic.entities.entityName.feature    ← Gherkin scenarios
│   ├── basic.entities.entityName.js         ← Step definitions
│   ├── performance.entities.entityName.feature
│   └── performance.entities.entityName.js
payloads/
├── EntityName/                              ← CASE-SENSITIVE!
│   ├── edit.entityName.payload.json
│   └── edit.entityName-type.payload.json
common/
├── utils.js                                 ← Shared utilities
├── cosmosdb.js                              ← DB polling
└── sds-client.js                            ← HTTP client
```
