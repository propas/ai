# 10. Idempotency & Reliability

Idempotency and reliability ensure that your API behaves predictably under retries, failures, and network issues. These principles are critical for distributed systems, payments, and high-availability services.

## Goals

- Prevent duplicate operations
- Ensure safe retries
- Handle network failures gracefully
- Guarantee data consistency
- Improve fault tolerance

---

## Idempotency

An operation is idempotent if performing it multiple times produces the same result as performing it once.

### Idempotent Methods

| Method | Idempotent | Example |
|---|:---:|---|
| GET | ✅ | Fetch same resource |
| PUT | ✅ | Replace with same data |
| DELETE | ✅ | Delete resource repeatedly |
| POST | ❌ | Creates duplicates by default |

### Why Idempotency Matters

- Requests may be retried automatically
- Network failures may cause duplicate submissions
- Clients may not know if a request succeeded

Without idempotency you risk duplicate payments, duplicate orders, and data corruption.

---

## Idempotency for POST (Critical)

POST is not idempotent by default — handle explicitly using an idempotency key pattern.

**Client:**

```
POST /payments
Idempotency-Key: abc-123
```

**Server behavior:**

- If the key is new → process request and store the result
- If the key exists → return the stored response

**Example response:**

```json
{
  "data": {
    "paymentId": "pay_123",
    "status": "completed"
  }
}
```

### Storage options for idempotency state

- Database table (idempotency key + response)
- Cache (Redis)
- Message store

### Key design

- Unique per operation
- Should expire (TTL)
- May include client ID and operation type

---

## Reliability

Reliability ensures the API works correctly even under failure conditions.

### Retry Strategy

- Safe to retry: `GET`, `PUT`, `DELETE`
- Unsafe (without idempotency): `POST`

**Retry example:**
Client sends `POST /payments` with `Idempotency-Key`. If timeout occurs and client retries with same key, server returns the stored response (no duplicate).

### Timeout Handling

- Set reasonable client and server timeouts
- Client timeout (e.g., 5–10s), server timeout slightly lower

### Circuit Breaker Pattern

- Prevent cascading failures
- Stop calling failing services and retry after cooldown

### Delivery semantics

- At-least-once: may process multiple times (common)
- Exactly-once: difficult; typically achieved via idempotency and deduplication patterns

---

## Duplicate Request Handling

Assume duplicates can happen. Handle via:

- Idempotency keys
- Unique constraints in DB
- Request deduplication middleware/gateway

---

## Data Consistency

Techniques:

- Transactions
- Optimistic locking (versioning)

**Example optimistic locking payload:**

```json
{
  "id": "123",
  "version": 2
}
```

Reject updates if version mismatches.

---

## Graceful Degradation & Eventual Consistency

- Return partial responses or fallback values when parts fail
- Accept temporary inconsistencies in distributed systems and reconcile later

---

## Observability for Reliability

Track:

- Retry rates
- Failure rates
- Duplicate requests

Include `X-Request-ID: abc-123` in logs and responses to correlate retries and failures.

---

## Common Patterns

1. Idempotent Create: use client-generated IDs or PUT `/orders/{id}` to avoid POST duplicates.
2. Safe Updates: use `PATCH` with versioning.
3. Deduplication Layer: middleware or gateway that handles duplicate requests.

---

## Common Mistakes

- No idempotency for payments
- Retrying POST without idempotency protection
- No timeout configuration
- Ignoring duplicate requests
- No retry strategy

---

## Checklist

- Idempotency implemented for critical operations
- Idempotency keys supported and persisted
- Retry strategy defined
- Timeouts configured
- Duplicate requests handled (deduplication)
- Observability in place (request IDs, retry/failure metrics)
- Data consistency strategy defined (transactions/versioning)

---

## Example Flow

1. Client: `POST /payments` (Idempotency-Key: `abc`)
2. Server processes and stores result
3. Client retries due to network issue
4. Server returns stored response → no duplicate payment

---

## Summary

| Area | Key Practice |
|---|---|
| Idempotency | Prevent duplicate effects |
| Reliability | Handle failures gracefully |
| Retries | Use safe retry strategies |
| Consistency | Use transactions/versioning |
| Observability | Track failures & retries |

---

## AI Agents

- Implement idempotency keys for `POST`
- Support safe retries for idempotent operations
- Handle duplicate requests with deduplication or unique constraints
- Use timeouts and circuit breakers
- Apply optimistic locking for concurrent updates
- Track `X-Request-ID` for observability
