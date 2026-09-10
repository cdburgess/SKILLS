# HTTP Methods

Use methods according to their standard semantics. Do not invent private meanings.

| Method   | Typical Use                                         | Idempotent | Safe |
|----------|-----------------------------------------------------|------------|------|
| `GET`    | Retrieve a resource or collection                   | Yes        | Yes  |
| `POST`   | Create a resource or start a non-idempotent process | No         | No   |
| `PUT`    | Replace the entire resource                         | Yes        | No   |
| `PATCH`  | Partially update a resource                         | No*        | No   |
| `DELETE` | Remove a resource                                   | Yes        | No   |

*PATCH is not required to be idempotent, though many implementations make it so.

## Idempotency Matters

Networks fail. Clients retry. Timeouts happen.

- Sending the same `GET` five times must produce the same effect as sending it once.
- The same general rule applies to `PUT` and `DELETE`.
- `POST` is different: repeating a create request may create multiple resources.

Design every endpoint so that retries behave predictably.

## Common Mistakes

- Using `POST` for everything because “it works”.
- Using `PUT` for partial updates (use `PATCH`).
- Using `GET` with side effects.
- Encoding the action in a query parameter (`?action=delete`) instead of using the proper method.
