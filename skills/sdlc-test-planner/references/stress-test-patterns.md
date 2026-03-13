# Stress Test Patterns

Stress testing validates that your system survives real-world traffic patterns — not just individual requests. This reference provides patterns for load, spike, soak, and capacity testing.

Stress testing is mandatory before any production launch. The question isn't whether your system will face unexpected load — it's whether you'll discover the breaking points before or after your users do.

---

## When to stress test

| Feature Risk Level | Stress Test Depth |
|-------------------|-------------------|
| **High** (payments, auth, data pipelines, public APIs) | Full suite: load + spike + soak + capacity |
| **Medium** (CRUD features, internal tools, integrations) | Load + spike + basic soak |
| **Low** (read-only views, static pages, admin tools) | Basic load test to confirm no regressions |

---

## Test Types

### 1. Load Test (sustained traffic)

Simulates expected production traffic over a sustained period. This is your baseline — if this fails, nothing else matters.

**What it reveals:** Baseline latency, throughput ceiling, resource utilization under normal conditions.

**Pattern:**
```
Virtual Users
     ^
     |     +---------------------------+
     |    /                             \
     |   /    Sustained at target load   \
     |  /                                 \
     | /                                   \
     +--+---+---------------------------+--+--> Time
       Ramp    Hold (15-30 min)          Ramp
       Up                                Down
```

**Key metrics to capture:**
- Response time: p50, p95, p99
- Throughput: requests/second
- Error rate: should be < 0.1%
- CPU utilization: should be < 70% at normal load
- Memory utilization: should be stable (no upward trend)
- DB connection pool: should have headroom
- Queue depth: should be draining, not growing

**Example k6 script pattern:**
```javascript
export const options = {
  stages: [
    { duration: '2m', target: 100 },   // ramp up
    { duration: '15m', target: 100 },  // hold at expected load
    { duration: '2m', target: 0 },     // ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<500'],  // 95% of requests < 500ms
    http_req_failed: ['rate<0.001'],   // error rate < 0.1%
  },
};
```

---

### 2. Spike Test (sudden bursts)

Simulates sudden traffic surges — flash sales, viral events, notification pushes, batch job completions.

**What it reveals:** Auto-scaling behavior, queue overflow handling, connection pool behavior under sudden pressure, recovery time.

**Pattern:**
```
Virtual Users
     ^
     |          +--+
     |         /    \
     |        /      \     +--+
     |       /        \   /    \
     |  +---+          +-+      +---+
     |  |                            |
     +--+---+----+----+----+----+---+--> Time
       Normal  Spike  Recovery  Spike  Normal
```

**What to verify:**
- System doesn't crash during spike
- Requests during spike get served (possibly slower) or get proper 503 with backpressure
- System recovers to normal latency within target time after spike subsides
- No data loss or corruption during spike
- No cascading failures (one service overwhelmed shouldn't take down others)
- Auto-scaling kicks in within expected timeframe (if applicable)

**Recovery time is critical.** A system that handles a spike but takes 30 minutes to recover is effectively down for 30 minutes.

---

### 3. Soak Test (endurance)

Runs moderate load for an extended period (4-12+ hours). This catches slow-burn issues that short tests miss.

**What it reveals:** Memory leaks, connection pool leaks, file descriptor leaks, log disk filling, gradual performance degradation, GC pressure buildup.

**Pattern:**
```
Virtual Users
     ^
     |  +-----------------------------------------------+
     |  |                                               |
     |  |        Moderate load for 4-12 hours           |
     |  |                                               |
     +--+---+---------------------------------------+---+--> Time
       Ramp                Hold                      Ramp
       Up                                            Down
```

**What to monitor (trend over time, not just snapshots):**

| Metric | Healthy | Unhealthy (leak signal) |
|--------|---------|------------------------|
| Memory usage | Flat or sawtooth (GC cycles) | Steadily climbing |
| DB connections (active) | Stable | Growing |
| File descriptors | Stable | Growing |
| Thread / goroutine count | Stable | Growing |
| Response time p95 | Stable | Gradually increasing |
| Disk usage | Slow growth (logs) | Rapid growth |
| GC pause time | Consistent | Increasing duration or frequency |

**Common soak test findings:**
- Memory leak: object references not released, caches without eviction
- Connection leak: DB/HTTP connections opened but not closed in error paths
- File descriptor leak: files or sockets opened but not closed
- Log volume: verbose logging fills disk over hours
- Temp file accumulation: temp files created but not cleaned up

---

### 4. Capacity Test (find the ceiling)

Incrementally increases load until the system breaks. The goal is to know your actual limit, not just hope it's enough.

**What it reveals:** Maximum throughput, the resource that bottlenecks first (CPU, memory, DB connections, disk I/O, network), degradation curve shape.

**Pattern:**
```
Virtual Users
     ^
     |                                    X  (break point)
     |                               +---/
     |                          +---/
     |                     +---/
     |                +---/
     |           +---/
     |      +---/
     | +---/
     +--+---+---+---+---+---+---+---+---+--> Time
       Step increases every 5 minutes
```

**Step protocol:**
1. Start at expected normal load
2. Increase by 25-50% every 5 minutes
3. At each step, record: latency (p50/p95/p99), error rate, CPU, memory, DB connections
4. Continue until error rate > 5% or latency > 10x baseline
5. Document: breaking point, first bottleneck resource, degradation curve

**Capacity report format:**
```
Capacity Test Report: [Feature Name]
Date: [Date]
Environment: [Staging/Pre-prod]

Baseline (normal load):
  VUs: 100 | RPS: 500 | p95: 120ms | Errors: 0% | CPU: 35% | Mem: 2.1GB

Breaking point:
  VUs: 450 | RPS: 1800 | p95: 4500ms | Errors: 8.2% | CPU: 98% | Mem: 3.8GB

First bottleneck: CPU on API server pods
Safety margin: 4.5x normal load (target: >3x)

Recommendation: Current capacity is sufficient for expected peak (2x normal).
Consider auto-scaling trigger at 70% CPU.
```

---

## Stress Test Infrastructure

### Environment requirements

- Stress tests should run against a **production-like environment** (same topology, similar hardware specs, same database engine)
- Never stress test against production (unless using shadow traffic)
- Test data should be realistic in volume and distribution
- External dependencies should be stubbed or have sufficient capacity

### Load generation

- Run load generators **outside** the system under test (separate machines/containers)
- Ensure load generators themselves are not the bottleneck
- Distribute load generators if needed for high-volume tests
- Use realistic request patterns (not just one endpoint repeatedly)

### Common tools

| Tool | Language | Best For |
|------|----------|----------|
| k6 | JavaScript | Developer-friendly, CI/CD integration, cloud option |
| Artillery | JavaScript/YAML | Quick setup, good for APIs |
| Locust | Python | Custom load patterns, distributed testing |
| Gatling | Scala | High-throughput, detailed reporting |
| JMeter | Java | GUI-based, enterprise adoption |

---

## Stress Test Report Template

Every stress test should produce a report that answers:

1. **What was tested?** — Endpoints, flows, data volume
2. **What was the load pattern?** — VUs, duration, ramp pattern
3. **What were the results?** — Latency percentiles, error rates, throughput
4. **What is the capacity ceiling?** — Breaking point and first bottleneck
5. **Are there leaks?** — Memory, connections, file descriptors over time
6. **What is the recommendation?** — Ship / don't ship / fix X first

---

## Mapping stress tests to SDLC phases

| SDLC Phase | Stress Test Activity |
|------------|---------------------|
| Phase 3 (Architecture) | Define performance targets and capacity expectations |
| Phase 4-5 (Documentation + Implementation Planning) | Plan stress test environment and tooling |
| Phase 6 (Test Strategy) | Define stress test scripts, execute full suite, produce report |
| Phase 7 (Release Readiness) | Stress test results in release checklist, capacity documented |

---

## Anti-patterns to avoid

- **Testing only happy paths under load** — Include error scenarios, auth flows, and edge cases in your load scripts
- **Testing a single endpoint** — Real traffic hits multiple endpoints; simulate realistic user journeys
- **Ignoring warm-up** — JIT compilation, connection pool initialization, and cache population affect early measurements
- **Testing against unrealistic data** — 10 rows in a table behaves very differently than 10 million
- **Declaring success without soak testing** — Short load tests miss leaks that cause 3am pages
- **Not documenting the breaking point** — If you don't know your ceiling, you can't capacity plan
