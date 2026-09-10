# Error Responses

Status codes tell the category of failure. The body must give consistent, actionable detail.

## Recommended Shape

```json
{
  "error": {
    "code": "ORDER_NOT_FOUND",
    "message": "The requested order does not exist.",
    "details": []
  }
}
```

For validation failures:

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "The given data was invalid.",
    "details": [
      {
        "field": "email",
        "message": "The email has already been taken."
      }
    ]
  }
}
```

## Rules

- Always use the same top-level structure across the entire API.
- Provide a machine-readable `code` when useful.
- Provide a human-readable `message`.
- Never expose stack traces, database errors, or internal implementation details.
- For field-level problems, list the exact fields that need attention.

Clients should be able to:
1. Display the message to a user
2. React programmatically to the code
3. Debug without guessing the format
