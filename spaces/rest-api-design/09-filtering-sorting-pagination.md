# 9. Filtering, Sorting, Pagination

These mechanisms allow clients to efficiently query large datasets, reduce payload size, and improve performance and usability.

## Goals

- Return only relevant data
- Reduce response size
- Improve performance
- Enable flexible querying

---

## Filtering

Filtering allows clients to restrict results based on conditions.

### Basic filtering (query params)

```
GET /users?status=active
GET /orders?type=online
```

### Multiple filters

```
GET /users?status=active&role=admin
```

Combine filters logically (AND by default).

### Range filtering

```
GET /orders?createdFrom=2026-01-01&createdTo=2026-01-31
GET /products?priceMin=10&priceMax=100
```

### Advanced filtering (optional)

```
GET /users?age[gte]=18&age[lte]=30
```

Operators:
- `gte` → greater than or equal
- `lte` → less than or equal

### Boolean filtering

```
GET /users?active=true
```

Best practices
- Use consistent parameter names
- Keep filters simple and predictable
- Document available filters

---

## Sorting

Allow clients to control result order.

### Basic sorting

```
GET /users?sort=name
```

### Descending order

Use `-` prefix:

```
GET /users?sort=-createdAt
```

### Multi-field sorting

```
GET /users?sort=role,-createdAt
```

Meaning: sort by `role` ascending, then by `createdAt` descending.

Best practices
- Allow sorting only on indexed fields
- Validate allowed sort fields
- Provide a default sort order

---

## Pagination

Divide large datasets into manageable chunks.

### Offset-based pagination (common)

```
GET /users?page=2&limit=20
```

Response example:

```json
{
  "data": [],
  "meta": {
    "page": 2,
    "limit": 20,
    "total": 120
  }
}
```

Pros: easy to implement and simple for clients.
Cons: slow for deep pages and inconsistent if data changes frequently.

### Cursor-based pagination (recommended for large scale)

```
GET /users?cursor=abc123&limit=20
```

Response example:

```json
{
  "data": [],
  "meta": {
    "nextCursor": "def456"
  }
}
```

Pros: high performance and stable results. Cons: more complex to implement.

### Keyset pagination (DB-optimized)

```
GET /users?afterId=123&limit=20
```

Uses indexed columns for efficiency.

---

## Combining filtering, sorting, and pagination

```
GET /users?status=active&sort=-createdAt&page=1&limit=20
```

This is the standard pattern for list endpoints.

---

## Response Metadata

Always include pagination info:

```json
{
  "data": [],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 500,
    "hasNext": true
  }
}
```

---

## Field Selection (bonus optimization)

Allow clients to request only needed fields:

```
GET /users?fields=id,name,email
```

---

## Validation Rules

- Reject invalid filters
- Reject unsupported sort fields
- Enforce a max limit (e.g., 100)

---

## Default Behavior

- Default limit (e.g., 20)
- Default sorting (e.g., `createdAt DESC`)

---

## Common Mistakes

- No pagination (returns huge datasets)
- Unbounded `limit`
- Sorting on non-indexed fields
- Inconsistent query parameter names
- Ignoring validation

---

## Performance Tips

- Add DB indexes on filter and sort fields
- Use cursor pagination for large datasets
- Avoid `OFFSET` for deep pages

---

## Example Full Request

```
GET /orders?status=completed&sort=-createdAt&page=1&limit=20&fields=id,total,status
```

---

## Checklist

- Filtering supported
- Sorting implemented
- Pagination required and enforced
- Max limit enforced
- Metadata included
- Fields validated

---

## Summary

| Feature | Purpose |
|---|---|
| Filtering | Reduce dataset |
| Sorting | Control order |
| Pagination | Limit response size |

---

## AI Agents

Design list endpoints with:
- Filtering via query params
- Sorting via `sort` (support multiple fields, `-` for desc)
- Pagination (page/limit or cursor)
- Metadata (page, limit, total)
- Field selection support
- Input validation
