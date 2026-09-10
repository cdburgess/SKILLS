# HTTP Status Codes

The status code is the primary signal. Never return `200 OK` when the request failed.

## Success

| Code | Meaning | Typical Use |
|------|---------|-------------|
| 200  | OK | Successful retrieval or update |
| 201  | Created | Resource successfully created |
| 202  | Accepted | Request accepted for asynchronous processing |
| 204  | No Content | Success with empty body (common for DELETE) |

## Client Errors

| Code | Meaning | Typical Use |
|------|---------|-------------|
| 400  | Bad Request | Malformed syntax or invalid parameters |
| 401  | Unauthorized | Missing or invalid authentication |
| 403  | Forbidden | Authenticated but not allowed to perform the action |
| 404  | Not Found | Resource does not exist |
| 409  | Conflict | Request conflicts with current resource state |
| 422  | Unprocessable Content | Validation or business-rule failure |
| 429  | Too Many Requests | Rate limit exceeded |

## Server Errors

| Code | Meaning | Typical Use |
|------|---------|-------------|
| 500  | Internal Server Error | Unexpected server failure |
| 503  | Service Unavailable | Temporary outage or overload |

## Guidance

- Prefer the most specific accurate code.
- Do not invent a custom error protocol that ignores HTTP semantics.
- Clients should be able to branch on the status code without parsing the body first.
