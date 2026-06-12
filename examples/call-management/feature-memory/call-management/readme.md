# Call Management

## Goal

Add a read-only call-management module to an admin console so operators and developers can inspect AI outbound call sessions.

## Scope

- Call session list and filters.
- Call detail drawer.
- Event timeline.
- Transcript and quality-result display.
- Recording playback and download through an authenticated backend proxy.
- Lightweight WebSocket refresh for list and detail views.

## Out Of Scope

- Hanging up calls.
- Retrying calls.
- Data repair or compensation.
- Persisting raw audio in the admin console.
- Writing raw production logs, tokens, object keys, or customer data into memory.

## Success Criteria

- A new Agent can continue the feature by reading `ai-handoff.md`, `index.md`, and this feature's `handoff.md`.
- Large fields are loaded on demand.
- Recording playback works without exposing storage credentials or direct object URLs.
- Important design trade-offs and resolved bugs are reusable in future tasks.
