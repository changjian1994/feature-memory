# Feature Memory Index

| Feature | Work State | Memory State | Last Updated | Owner | Parent Feature | Dependencies | Next Step | Entry |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| call-management | VERIFYING | VERIFYING | 2026-06-09 | agent/user | - | call-data-platform | Validate real data, playback, and refresh behavior. | `feature-memory/call-management/handoff.md` |

## State Notes

- `Work State` describes engineering progress.
- `Memory State` controls default reading scope.
- New Agents should read only `ACTIVE` and `VERIFYING` memory unless the user asks for archived history.

## Feature Boundary

- `call-management` owns read-only observability for call sessions.
- Control actions such as hang up, retry, compensation, or data repair are out of scope for this feature.
