---
name: cosmosdb-query
---

# Universal NoSQL Database Query Instructions

1. Use the official SDK/client for your NoSQL database (e.g., CosmosClient, MongoClient).
2. Retrieve credentials securely (e.g., environment variables, secrets manager).
3. Always use parameterized queries to prevent injection.
4. Query by unique identifier or time range as needed.
5. Poll for results if eventual consistency is expected.
