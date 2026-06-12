# AI Handoff

## Current Active Features

| Feature | Memory State | Work State | Next Step | Entry |
| --- | --- | --- | --- | --- |
| call-management | VERIFYING | VERIFYING | Validate with real call data and confirm recording playback in staging. | `feature-memory/call-management/handoff.md` |

## Project Context

- The project is an admin console for observing AI outbound call sessions.
- The first phase is read-only: call list, call details, event timeline, transcripts, quality results, recordings, and refresh status.
- The Agent should avoid adding call-control actions such as hang up, retry, or compensation until permissions, audit logs, and idempotency are designed.

## Current Risks

- Recording playback must go through the authenticated backend proxy; direct browser resource requests may miss auth headers.
- Large JSON payloads, transcripts, and recording metadata should be loaded on demand, not in list queries.
- Phone numbers, object storage keys, tokens, cookies, and raw production logs must not be written into memory.

## Recently Useful Knowledge

- Use `call_session` as the list and detail base table; use `call_events` only for event timeline and recording metadata.
- Keep transcripts, quality results, and recordings behind an on-demand `insights` API.
- If browser playback fails with `401`, first check whether the request is made by native `<audio>` / `window.open` instead of the authenticated HTTP client.
