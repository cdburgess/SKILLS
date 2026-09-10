# HTTP Status Codes

The status code is the primary signal. Never return `200 OK` when the request failed.

## Success

| Code | Meaning    | Typical Use                                  |
|------|------------|----------------------------------------------|
| 200  | OK         | Successful retrieval or update               |
| 201  | Created    | Resource successfully created                |
| 202  | Accepted   | Request accepted for asynchronous processing |
| 204  | No Content | Success with empty body (common for DELETE)  |

## Client Errors

| Code | Meaning               | Typical Use                                                              |
|------|-----------------------|--------------------------------------------------------------------------|
| 400  | Bad Request           | Invalid request structure, malformed JSON, invalid query/path parameters |
| 401  | Unauthorized          | Missing, invalid, or expired authentication                              |
| 403  | Forbidden             | User is authenticated but lacks permission                               |
| 404  | Not Found             | Resource or endpoint doesn't exist                                       |
| 409  | Conflict              | Resource state conflicts with the requested operation                    |
| 422  | Unprocessable Content | Validation/business-rule failure                                         |
| 429  | Too Many Requests     | Rate limit exceeded                                                      |

## Server Errors

| Code | Meaning               | Typical Use                                                               |
|------|-----------------------|---------------------------------------------------------------------------|
| 500  | Internal Server Error | Unexpected server-side failure or unhandled application error             |
| 502  | Bad Gateway           | Server received an invalid response from an upstream service              |
| 503  | Service Unavailable   | Service is temporarily unavailable, overloaded, or undergoing maintenance |
| 504  | Gateway Timeout       | Upstream service failed to respond within the allowed time                |

## Guidance

- Prefer the most specific accurate code.
- Do not invent a custom error protocol that ignores HTTP semantics.
- Clients should be able to branch on the status code without parsing the body first.
