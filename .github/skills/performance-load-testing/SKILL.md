---
name: performance-load-testing
description: "Domain knowledge for performance and load testing patterns: burst vs wave strategies, metrics collection, throughput calculation, and success thresholds."
---

# Universal Performance & Load Testing — Domain Knowledge

## Two Test Patterns

### 1. Performance (Burst) — `@Performance`
Send all requests simultaneously to measure peak handling capacity for any API or service.

```
Requests ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  ← all at once (concurrent execution)
         |________________________|
         0s                     response time
```

```javascript
// Burst pattern — all requests fire at once
async function burstSend(payloads, sendFn) {
    const startTime = Date.now();
    const results = await Promise.all(
        payloads.map(p => sendFn(p).then(r => ({
            success: r.status === 200,
            duration: Date.now() - startTime
        })))
    );
    return {
        total: results.length,
        successful: results.filter(r => r.success).length,
        avgDuration: results.reduce((a, r) => a + r.duration, 0) / results.length
    };
}
```

### 2. Load (Waves) — `@Load`
Send requests in sustained waves with sleep between them to simulate real traffic for any system.

```
Wave 1  ▓▓▓▓▓  sleep  ▓▓▓▓▓  sleep  ▓▓▓▓▓  sleep  ▓▓▓▓▓
Wave 2         ▓▓▓▓▓  sleep  ▓▓▓▓▓  sleep  ▓▓▓▓▓
                |______|______|______|______|______|
                0s    10s    20s    30s    40s    50s
```

```javascript
// Wave pattern — controlled bursts with rest
async function waveSend(payloads, waveSize, sleepMs, sendFn) {
    const waves = chunkArray(payloads, waveSize);
    const metrics = [];

    for (const [i, wave] of waves.entries()) {
        const waveStart = Date.now();
        const results = await Promise.all(wave.map(p => sendFn(p)));
        
        metrics.push({
            wave: i + 1,
            sent: wave.length,
            success: results.filter(r => r.status === 200).length,
            duration: Date.now() - waveStart
        });

        if (i < waves.length - 1) {
            await new Promise(r => setTimeout(r, sleepMs));
        }
    }
    return metrics;
}

function chunkArray(arr, size) {
    const chunks = [];
    for (let i = 0; i < arr.length; i += size) {
        chunks.push(arr.slice(i, i + size));
    }
    return chunks;
}
```

## Metrics to Collect
| Metric | Formula | Target |
|--------|---------|--------|
| **Success Rate** | (successful / total) × 100 | ≥ 95% |
| **Throughput** | total / totalTimeMinutes | docs/min |
| **Avg Response Time** | sum(durations) / count | < 2s |
| **P95 Response Time** | 95th percentile of durations | < 5s |
| **Error Rate** | (failed / total) × 100 | < 5% |

## Allure Reporting for Perf Tests
```javascript
// Attach timing summary as CSV
const csv = 'Wave,Sent,Success,Failed,Duration(ms)\n' +
    metrics.map(m => `${m.wave},${m.sent},${m.success},${m.sent - m.success},${m.duration}`).join('\n');
this.attach(csv, 'text/csv');

// Attach JSON summary
this.attach(JSON.stringify({ totalDocs, successRate, throughput }, null, 2), 'application/json');
```

## Gherkin Pattern for Performance
```gherkin
Scenario Outline: Burst send <count> documents
    Given <count> unique payloads are prepared
    When all payloads are sent simultaneously via SDS
    Then at least 95% should receive HTTP 200
    And all should appear in CosmosDB within <timeout> minutes

    Examples:
        | count | timeout |
        | 10    | 30      |
        | 50    | 30      |
```

## Gherkin Pattern for Load
```gherkin
Scenario Outline: Sustained load of <total> documents in waves of <wave>
    Given <total> unique payloads are prepared
    When payloads are sent in waves of <wave> with <sleep> second intervals
    Then at least 95% should succeed
    And throughput should be at least <minThroughput> documents per minute

    Examples:
        | total | wave | sleep | minThroughput |
        | 100   | 10   | 5     | 20            |
```
