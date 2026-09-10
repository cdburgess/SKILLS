---
name: rest-api-design
description: Design intuitive, consistent, resource-oriented REST APIs following the 8 laws of senior backend developers. Use when designing new RESTful APIs, creating or refactoring endpoint structures, defining request/response formats, choosing HTTP methods and status codes, implementing versioning, pagination, filtering, error handling, or reviewing an API for REST best practices. Especially useful for Laravel API work.
license: MIT
metadata:
  author: chuck
  framework: laravel
---

# REST API Design

Design REST APIs that are predictable, consistent, and pleasant to consume. Clients should be able to guess how an endpoint works after seeing one or two examples.

The eight laws below are the system of definition. Follow them unless the user explicitly requires a different approach.

Laravel projects already follow Laravel Boost conventions (Form Requests, API Resources, Route Model Binding, etc.). This skill focuses on the HTTP and resource design layer on top of those conventions.

EXECUTE the core design principles immediately when asked to design or review an API. Do not ask for confirmation on the eight laws — they are safe defaults.

---

## 1. Design around resources, not actions

Model endpoints around **resources**, not verbs.

**Good**
```
GET    /api/orders
GET    /api/orders/{order}
POST   /api/orders
PATCH  /api/orders/{order}
DELETE /api/orders/{order}
```

**Bad**
```
GET    /api/getOrders
POST   /api/createOrder
POST   /api/updateOrder
POST   /api/deleteOrder
```

Let the URL identify the resource. Let the HTTP method describe the operation. The same resource can support multiple operations without inventing a new URL for every action.

## 2. Make URLs predictable

Use consistent, plural collection names and a single naming convention everywhere.

**Default conventions (Laravel):**
- URL paths: kebab-case (e.g. `/api/user-profiles`, `/api/order-items`)
- Route parameters: camelCase (e.g. `{userId}`, `{orderId}`) to match controller variable names

Examples:
- Collections: `/api/users`, `/api/orders`, `/api/user-profiles`
- Single resource: `/api/users/{userId}`, `/api/orders/{orderId}`, `/api/user-profiles/{userProfileId}`
- Nested only when the relationship is meaningful and shallow (max ~2 levels): `/api/users/{userId}/orders`

Stick to these conventions throughout the API. A developer should never have to guess how the next endpoint is named.

## 3. Use HTTP methods for their purpose

| Method   | Use                                                     |
|----------|---------------------------------------------------------|
| `GET`    | Retrieve a resource or collection (safe, idempotent)    |
| `POST`   | Create a resource or trigger a non-idempotent operation |
| `PUT`    | Replace a resource entirely (idempotent)                |
| `PATCH`  | Partially update a resource                             |
| `DELETE` | Remove a resource (idempotent)                          |

Respect idempotency. Networks fail and clients retry. `GET`, `PUT`, and `DELETE` should be safe to repeat. `POST` is not.

Do not invent custom rules that force clients to learn your private method semantics.

## 4. Make status codes useful

Never return `200 OK` with a body that says the operation failed. The status code is the primary signal.

Common successful codes:
- `200` — successful read or update
- `201` — resource created
- `202` — accepted for asynchronous processing
- `204` — success with no response body

Common error codes:
- `400` — malformed request
- `401` — missing or invalid authentication
- `403` — authenticated but not authorized
- `404` — resource does not exist
- `409` — conflict with current state
- `422` — validation / business rule failure
- `429` — rate limit exceeded
- `500` / `503` — server errors

Choose the most semantically accurate code. Do not invent a private error protocol on top of HTTP.

## 5. Keep error responses consistent

Status codes communicate the category. The body must give actionable detail in a predictable shape.

Prefer a stable structure such as:

```json
{
  "error": {
    "code": "ORDER_NOT_FOUND",
    "message": "The requested order does not exist.",
    "details": []
  }
}
```

For validation failures include the specific fields. Clients should be able to display the message, react to the code, and debug without guessing.

## 6. Not everything belongs in the path

The path identifies the resource. Query parameters refine the result.

**Good**
```
GET /api/products?category=books&status=in_stock&sort=-price&page=2
```

**Bad**
```
GET /api/products/books/in-stock/sort-by-price/page-2
GET /api/products?action=delete
```

Never use query parameters to invent actions. Paths identify, query parameters filter/sort/paginate, HTTP methods operate.

## 7. Treat API changes carefully

Adding an optional field is usually safe. Changing or removing existing behavior is a breaking change.

When a breaking change is required:
- Choose one versioning strategy and apply it consistently (URL prefix `/api/v1/...` is the most common and obvious).
- Give consumers a clear migration path.
- Do not silently break existing clients.

Non-breaking changes should preserve endpoint behavior, required fields, response fields, and resource semantics.

## 8. Keep request and response formats consistent

Pick conventions once and apply them everywhere:

- JSON as the primary format
- JSON keys: camelCase (e.g. `{"firstName": "John", "createdAt": "..."}`) — aligns with JavaScript/TypeScript frontends and is typically produced by Laravel API Resources
- Consistent date format (ISO 8601)
- Consistent pagination envelope
- Consistent error shape
- Consistent collection vs single-resource envelopes

When a developer learns one endpoint they should be able to make reasonable assumptions about the others. Consistency is one of the highest-value features an API can have.

---

## Laravel Notes

Laravel already provides excellent primitives (API Resources, Form Requests, Route Model Binding, API rate limiting, Sanctum/Passport). Use them. This skill only constrains the HTTP surface so the resulting API remains predictable across endpoints.

**Naming defaults:**
- URL paths → kebab-case (`/api/user-profiles`)
- Route parameters → camelCase (`{userId}`)
- JSON keys → camelCase (`firstName`, `createdAt`) via API Resources or middleware

Prefer:
- Plural resource route names
- Explicit status codes via `response()->json(..., $status)`
- Consistent API Resource transformers that emit camelCase
- Form Request validation that maps cleanly to `422` responses

## Reference Guides

When deeper detail is needed, load the matching reference:

| Guide                            | Contents                                         |
|----------------------------------|--------------------------------------------------|
| `references/resource-naming.md`  | Resource naming, nesting, and URL predictability |
| `references/http-methods.md`     | HTTP method semantics and idempotency            |
| `references/status-codes.md`     | Status code selection                            |
| `references/error-responses.md`  | Consistent error shapes                          |
| `references/query-parameters.md` | Filtering, sorting, pagination                   |
| `references/versioning.md`       | Safe API evolution                               |
| `references/response-formats.md` | Envelopes, dates, naming consistency             |
| `references/laravel-patterns.md` | Laravel-specific mapping of the eight laws       |

## Design Checklist

Before finalizing an API design verify:

- [ ] Resources use nouns, not verbs
- [ ] Collections use plural names
- [ ] Naming is consistent across the entire API
- [ ] HTTP methods match standard semantics
- [ ] Nested resources stay shallow
- [ ] Status codes accurately reflect the outcome
- [ ] Errors have a single predictable shape
- [ ] Filtering/sorting/pagination live in query parameters
- [ ] Breaking changes are versioned
- [ ] Request and response formats are uniform
- [ ] Existing clients will not be silently broken

## Decision Rule

When multiple designs are possible, prefer the one that is:

1. Most intuitive for API consumers
2. Most consistent with the rest of the API
3. Most aligned with standard HTTP semantics
4. Least surprising to existing clients
5. Simplest to document and maintain

A good API is one that clients can understand without constantly checking documentation.
