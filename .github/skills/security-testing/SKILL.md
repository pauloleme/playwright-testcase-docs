---
name: security-testing
description: "Domain knowledge for application security testing: OWASP Top 10 checklist, API security patterns, authentication testing, and secure coding practices."
---

# Universal Security Testing — Domain Knowledge

## OWASP Top 10 Quick Reference

### 1. Broken Access Control
```javascript
// Test: Unauthorized access to another user's resource (generic)
const response = await request.get(`/api/resource/${otherUserId}`)
    .set('Authorization', `Bearer ${regularUserToken}`);
expect(response).to.have.status(403); // NOT 200
```

### 2. Injection Prevention
```javascript
// Test payloads for injection
const injectionPayloads = [
    "'; DROP TABLE test; --",
    "<script>alert('xss')</script>",
    "{{7*7}}",                          // template injection
    "${7*7}",                           // expression injection
    "../../../etc/passwd",              // path traversal
    "| ls -la",                         // command injection
];

// Server should reject or sanitize ALL of these
for (const payload of injectionPayloads) {
    const response = await sendPayload({ name: payload });
    expect(response).to.have.status(400);
}
```

### 3. Authentication Edge Cases
```javascript
// Expired token
const response1 = await request.get('/api/resource')
    .set('Authorization', `Bearer ${expiredToken}`);
expect(response1).to.have.status(401);

// No token
const response2 = await request.get('/api/resource');
expect(response2).to.have.status(401);

// Malformed token
const response3 = await request.get('/api/resource')
    .set('Authorization', 'Bearer invalid.token.here');
expect(response3).to.have.status(401);
```

### 4. Security Headers Checklist
```javascript
expect(response.headers).to.include.keys([
    'strict-transport-security',     // HSTS
    'x-content-type-options',        // nosniff
    'x-frame-options',               // DENY or SAMEORIGIN
    'content-security-policy',       // CSP
]);
expect(response.headers['x-content-type-options']).to.equal('nosniff');
```

### 5. Sensitive Data Exposure
```javascript
// Verify PII is not in logs
// Verify secrets are not in response bodies
// Verify error messages don't leak stack traces
expect(response.body).to.not.have.property('stackTrace');
expect(response.body).to.not.have.property('internalError');
expect(JSON.stringify(response.body)).to.not.include('password');
```

## Secrets Management Rules
- **NEVER** hardcode secrets in code, env files, or pipeline variables
- **ALWAYS** use Azure Key Vault or equivalent
- **ROTATE** secrets on a regular schedule
- **AUDIT** secret access logs

## Rate Limiting Tests
```javascript
// Send 100 rapid requests — should be throttled
const promises = Array.from({ length: 100 }, () =>
    request.post('/api/auth/token').send(credentials)
);
const responses = await Promise.all(promises);
const throttled = responses.filter(r => r.status === 429);
expect(throttled.length).to.be.greaterThan(0);
```
