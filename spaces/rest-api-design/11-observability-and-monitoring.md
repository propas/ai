# 11. Observability & Monitoring

Observability and monitoring ensure your API is visible, debuggable, and measurable in production. They allow you to detect issues, diagnose problems, and maintain reliability at scale.

## Goals

- Understand system behavior in real time
- Detect and respond to failures quickly
- Measure performance and usage
- Enable root cause analysis
- Improve system reliability

---

## Observability vs Monitoring

- Monitoring: Tracking known metrics (CPU, errors)
- Observability: Understanding why something happens

Observability = Logs + Metrics + Traces

---

## Core Pillars

### 1. Logging

Logs capture detailed event data about API behavior.

**What to log**
- Incoming requests (method, path)
- Response status codes
- Errors and exceptions
- Request duration
- User ID (if available)
- Correlation / request ID

**Example log (JSON):**

```json
{
  "timestamp": "2026-01-01T12:00:00Z",
  "level": "ERROR",
  "requestId": "abc-123",
  "method": "GET",
  "path": "/users/123",
  "status": 500,
  "message": "Database timeout"
}
```

**Best practices**
- Use structured logs (JSON)
- Avoid logging sensitive data
- Use a consistent log format
- Include correlation IDs

---

### 2. Metrics

Metrics provide quantitative insights into system performance.

**Key metrics**
- Latency: P50, P95, P99
- Throughput: requests per second (RPS)
- Error rate: percentage of failed requests

**Example metrics**
```
requests_total{endpoint="/users"} 1000
request_duration_ms_p95 250
error_rate 0.02
```

**Best practices**
- Use percentiles (P95, P99) not just averages
- Tag metrics (endpoint, method, status)
- Track SLIs/SLOs

---

### 3. Distributed Tracing

Tracing tracks a request across multiple services.

**Trace data includes**
- Request path across services
- Latency per component
- Downstream dependencies

**Tools:** OpenTelemetry, Jaeger, Zipkin

---

## Correlation / Request ID

Every request should have a unique identifier.

- Header: `X-Request-ID: abc-123`
- Propagate the ID across all services
- Include it in logs, metrics, and traces

---

## Health Checks

Provide endpoints to verify system health.

**Example:**
```
GET /health
```
**Response:**
```json
{ "status": "UP" }
```

Types:
- Liveness — is the service running?
- Readiness — can it handle traffic?

---

## Alerting

Set alerts for critical conditions.

**Examples**
- High error rate (> X%)
- High latency (P95 > threshold)
- Service down
- CPU/memory spikes

**Best practices**
- Avoid alert fatigue
- Use meaningful, actionable thresholds
- Prioritize alerts by severity

---

## Dashboards

Visualize system health (request rate, error rate, latency percentiles, top endpoints, resource usage).

---

## Logging Levels

- DEBUG: detailed debugging
- INFO: normal operations
- WARN: potential issues
- ERROR: failures

---

## Sampling

For high-traffic systems:
- Sample logs/traces (e.g., 1%)
- Always log errors

---

## SLA, SLI, SLO

- SLI: an indicator (metric, e.g., latency)
- SLO: objective (e.g., 95% < 200ms)
- SLA: contractual agreement

---

## Error Tracking

Track and group errors (stack traces, frequency, affected endpoints).

**Tools:** Sentry, Datadog, New Relic

---

## Security Observability

Monitor security-relevant events:
- Failed login attempts
- Unauthorized access
- Rate limit violations

---

## Common Mistakes

- No request IDs
- Logging sensitive data
- Only tracking averages (no percentiles)
- No alerting
- Unstructured logs
- No tracing in distributed systems

---

## Checklist

- Structured logging implemented
- Request ID included everywhere
- Metrics collected (latency, errors, throughput)
- Distributed tracing enabled
- Health endpoints available
- Alerts configured
- Dashboards created
- Sensitive data excluded from logs

---

## Example Full Observability Flow

1. Request arrives with `X-Request-ID: abc`
2. Logs created with `requestId`
3. Metrics recorded (latency, status)
4. Trace spans created across services
5. Error occurs → logged and alert triggered

---

## Summary

| Area | Key Practice |
|---|---|
| Logs | Structured + correlated |
| Metrics | Latency, errors, throughput |
| Tracing | End-to-end visibility |
| Alerts | Actionable thresholds |
| Health | `/health` endpoints |

---

## AI Agents

Implement API observability:

- Add structured logging with requestId
- Track metrics (latency, errors, throughput)
- Enable distributed tracing (OpenTelemetry)
- Provide `/health` endpoints
- Configure alerts for failures
- Avoid logging sensitive data
