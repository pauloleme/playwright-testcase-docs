---
name: api-testing
description: "Domain knowledge for REST API testing with Chai-HTTP: request patterns, status code validation, schema validation, authentication, and error handling."
---

# Universal REST API Testing — Domain Knowledge

## HTTP Client Pattern (Generic)
```javascript
// Example using axios (can use fetch, Chai-HTTP, etc.)
const axios = require('axios');
const { expect } = require('chai');

const baseUrl = process.env.BASE_URL || 'https://your-api-url.com';
```

## Request Patterns

### POST — Send Data
```javascript
async function sendData(endpoint, body, token) {
    const response = await axios.post(`${baseUrl}${endpoint}`, body, {
        headers: {
            Authorization: `Bearer ${token}`,
            'Content-Type': 'application/json',
        },
    });
    return response;
}
```

### GET — Query Resource
```javascript
async function getResource(endpoint, token) {
    const response = await axios.get(`${baseUrl}${endpoint}`, {
        headers: { Authorization: `Bearer ${token}` },
    });
    return response;
}
```

### Bulk Send (Performance Testing)
```javascript
async function sendBulk(endpoint, payloads, token) {
    const promises = payloads.map(body =>
        axios.post(`${baseUrl}${endpoint}`, body, {
            headers: {
                Authorization: `Bearer ${token}`,
                'Content-Type': 'application/json',
            },
        })
    );
    return await Promise.all(promises);
}
```

## Status Code Validation
| Code | Meaning | When to Expect |
|------|---------|----------------|
| `200` | OK | Successful GET, PUT, POST |
| `201` | Created | Successful POST |
| `204` | No Content | Successful DELETE |
| `400` | Bad Request | Invalid payload |
| `401` | Unauthorized | Missing/expired token |
| `403` | Forbidden | Insufficient permissions |
| `404` | Not Found | Entity doesn't exist |
| `409` | Conflict | Duplicate entity |
| `429` | Too Many Requests | Rate limited |
| `500` | Server Error | Backend failure |

## Assertion Patterns
```javascript
// Status code
expect(response).to.have.status(200);

// Response body structure
expect(response.body).to.have.property('id');
expect(response.body).to.have.property('status').equal('Processed');

// Array response
expect(response.body.items).to.be.an('array').with.lengthOf.at.least(1);

// Schema validation with Joi
const Joi = require('joi');
const schema = Joi.object({
    id: Joi.string().uuid().required(),
    name: Joi.string().max(200).required(),
    status: Joi.string().valid('Active', 'Inactive').required()
});
const { error } = schema.validate(response.body);
expect(error).to.be.undefined;
```

## Authentication
```javascript
// OAuth2 token acquisition
async function getAuthToken() {
    const tenantId = await getSecret('tenant-id');
    const clientId = await getSecret('client-id');
    const clientSecret = await getSecret('client-secret');
    const scope = await getSecret('api-scope');

    const response = await chai.request(`https://login.microsoftonline.com/${tenantId}`)
        .post('/oauth2/v2.0/token')
        .type('form')
        .send({
            grant_type: 'client_credentials',
            client_id: clientId,
            client_secret: clientSecret,
            scope: scope
        });

    return response.body.access_token;
}
```

## Allure Attachments
```javascript
// Always attach request and response for debugging
this.attach(JSON.stringify({ url, method, body }, null, 2), 'application/json');
this.attach(JSON.stringify(response.body, null, 2), 'application/json');
```

## Best Practices
1. **Never hardcode** URLs, tokens, or credentials — use Key Vault / config
2. **Deep clone payloads** before modifying — avoid cross-scenario contamination
3. **Generate unique IDs** per request — use `uuidv4()` or `generateIqid()`
4. **Set explicit timeouts** for long-running async operations
5. **Validate schema**, not just status code — a 200 with wrong body is still a bug
6. **Log correlation IDs** — attach IQID to every request for traceability
