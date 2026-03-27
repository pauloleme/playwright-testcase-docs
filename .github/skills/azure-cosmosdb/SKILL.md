---
name: azure-cosmosdb
description: "Domain knowledge for Azure CosmosDB integration testing: connection setup, query patterns, polling strategies, and partition key design. Used by automation and database agents."
---

# Universal NoSQL Database Integration Testing Knowledge

## Connection Setup (Generic)
```javascript
const { NoSQLClient } = require('your-nosql-sdk'); // Replace with your DB SDK

// Credentials from environment or secrets manager — never hardcode
const endpoint = process.env.DB_ENDPOINT;
const key = process.env.DB_KEY;

const client = new NoSQLClient({ endpoint, key });
const database = client.database('your-database');
const container = database.container('your-container');
```

## Query Patterns

### Query by Unique Identifier
```javascript
async function queryById(id) {
    const query = {
        query: 'SELECT * FROM c WHERE c.id = @id ORDER BY c._ts DESC',
        parameters: [{ name: '@id', value: id }]
    };
    const { resources } = await container.items.query(query).fetchAll();
    return resources;
}
```

### Query by Time Range
```javascript
async function queryByTimeRange(type, startTime, endTime) {
    const query = {
        query: `SELECT * FROM c 
                WHERE c.type = @type 
                AND c.timestamp >= @start 
                AND c.timestamp <= @end`,
        parameters: [
            { name: '@type', value: type },
            { name: '@start', value: startTime },
            { name: '@end', value: endTime }
        ]
    };
    const { resources } = await container.items.query(query).fetchAll();
    return resources;
}
```

### Always Use Parameterized Queries
```javascript
// CORRECT — parameterized (safe from injection)
query: 'SELECT * FROM c WHERE c.id = @id'
parameters: [{ name: '@id', value: userInput }]

// WRONG — string concatenation (SQL injection risk!)
query: `SELECT * FROM c WHERE c.IQID = '${userInput}'`
```

## Polling Strategy for Integration Tests
```javascript
async function pollCosmosDB(iqid, maxMinutes = 30, intervalSeconds = 30) {
    const maxAttempts = (maxMinutes * 60) / intervalSeconds;
    
    for (let attempt = 1; attempt <= maxAttempts; attempt++) {
        const results = await queryByIqid(iqid);
        
        if (results.length > 0) {
            console.log(`Found after ${attempt} attempts (${attempt * intervalSeconds}s)`);
            return results[0];
        }
        
        console.log(`Attempt ${attempt}/${maxAttempts} — not found yet, waiting ${intervalSeconds}s...`);
        await new Promise(resolve => setTimeout(resolve, intervalSeconds * 1000));
    }
    
    throw new Error(`IQID ${iqid} not found in CosmosDB after ${maxMinutes} minutes`);
}
```

## Partition Key Design
- **Current partition key**: `/EntityType` on LighthouseIntegrationLog
- Cross-partition queries are expensive — always filter by partition key when possible
- For high-volume entities, consider composite keys

## Important Fields
| Field | Type | Description |
|-------|------|-------------|
| `IQID` | string | Unique integration queue ID |
| `EntityType` | string | Company, Contact, Fund, etc. |
| `Status` | string | Processed, Failed, Pending |
| `UTCModifiedOn` | string (ISO) | When the entity was modified |
| `_ts` | number | CosmosDB internal timestamp (epoch) |
| `ErrorMessage` | string | Error details if Status = Failed |

## Best Practices for Tests
1. **Generate unique IQIDs** per test — never reuse IDs across scenarios
2. **Set reasonable polling timeouts** — 30 min for batch, 5 min for real-time
3. **Attach query results** to Allure for debugging failed tests
4. **Handle transient errors** — CosmosDB may return 429 (throttled)
5. **Clean up test data** only in dedicated test environments
