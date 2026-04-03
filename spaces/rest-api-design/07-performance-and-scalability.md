# 7. Performance & Scalability

Performance and scalability ensure your API can handle increasing traffic efficiently, provide fast responses, and remain reliable under load.

## Goals

- Minimize latency (response time)
- Maximize throughput (requests/sec)
- Handle horizontal scaling
- Optimize resource usage
- Ensure resilience under load

---

## Key Techniques

### 1. Pagination

Never return large datasets in a single response.

**Example:**

```http
GET /users?page=1&limit=20
```

**Response:**

```json
{
  "data": [],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 5000
  }
}
```

Why: prevents memory overload, reduces response size, improves latency.

---

### 2. Filtering & Query Optimization

Return only necessary data.

**Example:**

```
GET /orders?status=completed
```

Combine with proper DB indexing and query optimization.

---

### 3. Field Selection (Sparse Responses)

Allow clients to request only needed fields.

**Example:**

```
GET /users?fields=id,name
```

Benefits: smaller payloads, faster serialization/deserialization.

---

### 4. Caching

Leverage HTTP caching and infrastructure caching.

**HTTP headers:**

```
Cache-Control: max-age=3600
ETag: "abc123"
```

**Conditional requests:**

```
If-None-Match: "abc123"
→ 304 Not Modified
```

Benefits: reduces server load, faster client responses, enables CDN usage.

---

### 5. Compression

Enable response compression and honor `Accept-Encoding`.

```
Accept-Encoding: gzip
```

Use `gzip` or `brotli` for better compression. Benefits: reduced bandwidth, faster transfers.

---

### 6. Efficient Serialization

- Use lightweight formats (JSON)
- Avoid deeply nested objects
- Exclude unnecessary fields

Bad example:

```json
{
  "user": {
    "profile": {
      "details": {
        "address": {}
      }
    }
  }
}
```

---

### 7. Asynchronous Processing

Offload long-running tasks.

**Example:**

```
POST /reports
→ 202 Accepted
```

**Response:**

```json
{
  "jobId": "abc123",
  "status": "processing"
}
```

Use for file processing, reports, batch operations.

---

### 8. Rate Limiting

Protect the API from overload.

- Response code: `429 Too Many Requests`

**Headers:**

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 20
Retry-After: 60
```

Prevents abuse and system overload.

---

### 9. Connection Management

- Use HTTP/2 or HTTP/3
- Enable keep-alive connections

Benefits: reduced latency, fewer TCP handshakes.

---

### 10. Database Optimization

- Use indexes
- Avoid N+1 queries
- Use connection pooling

Prefer batch queries over per-item queries.

---

### 11. Horizontal Scaling

Scale by adding more instances. Stateless APIs are easy to scale.

Example architecture:

```
Client → Load Balancer → API Instances → Database
```

---

### 12. Caching Layers

Multi-level caching:

- CDN (edge caching)
- Reverse proxy (e.g., Nginx)
- Application cache (Redis)

---

### 13. Avoid Over-Fetching & Under-Fetching

Balance with field selection and proper endpoint design.

---

### 14. Bulk Operations

Reduce number of requests by supporting batch endpoints.

**Example:**

```
POST /users/batch
[
  { "name": "User1" },
  { "name": "User2" }
]
```

---

### 15. Timeouts & Circuit Breakers

- Set request timeouts
- Use circuit breakers to prevent cascading failures

---

### 16. Monitoring Performance

Track: latency (P50, P95, P99), throughput, error rates.

---

## Performance Anti-Patterns

- Returning entire database tables
- No pagination
- No caching
- Synchronous heavy processing
- Chatty APIs (many small requests)
- N+1 database queries

---

## Performance Checklist

- Pagination implemented
- Filtering supported
- Field selection available
- Caching headers used
- Compression enabled
- Async processing for heavy tasks
- Rate limiting configured
- Database optimized
- Monitoring in place

---

## Real-World Optimization Stack

- CDN (Cloudflare, Akamai)
- Load balancer (NGINX, AWS ALB)
- API service (stateless)
- Cache (Redis)
- Database (optimized queries + indexes)

---

## Example Optimized Request

```
GET /users?page=1&limit=20&fields=id,name
Cache-Control: max-age=60
```

---

## Summary

| Area | Key Practice |
|---|---|
| Data size | Pagination + filtering |
| Speed | Caching + compression |
| Scalability | Stateless + horizontal scaling |
| Efficiency | Async processing |
| Protection | Rate limiting |
| DB | Indexing + query optimization |

---

## AI Agents

Optimize REST API performance:

- Add pagination, filtering, field selection
- Use caching (ETag, Cache-Control)
- Enable compression (gzip/brotli)
- Use async processing for long tasks
- Avoid N+1 queries
- Implement rate limiting
- Design stateless services for horizontal scaling
