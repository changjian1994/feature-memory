# Call Management Design

## Background

The call platform stores call sessions, call events, transcripts, quality results, and recording metadata across multiple systems. The admin console should help users inspect calls and troubleshoot issues without becoming a control plane.

## Recommended Design

- Use the call session table as the base source for list and detail views.
- Use the call events table for timeline and recording metadata.
- Use an `insights` API to aggregate transcripts, quality results, and recording summaries on demand.
- Serve recording files through a backend proxy that validates ownership and authorization.
- Use WebSocket refresh for lightweight list/detail updates, but never push audio bytes or large JSON payloads through WebSocket.

## Data Loading Rules

- List page: lightweight fields only.
- Detail drawer: base info and selected large fields on demand.
- Event timeline: event summaries first; raw payload behind a "view JSON" action.
- Recordings: playable files only; lifecycle events remain part of status summary.
- Transcripts and quality results: loaded only when the user opens detail or expands a row.

## Risks

- Loading large payloads in list queries can hurt performance.
- Direct object storage URLs can leak implementation details or credentials.
- Browser-native media requests may bypass the authenticated HTTP client.
- Debug logs and feature memory must not contain secrets or raw production data.
