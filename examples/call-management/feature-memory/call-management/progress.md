# Progress

`progress.md` records important process history and state changes. It should not duplicate the full current-state summary from `handoff.md`.

## 2026-06-08

- Confirmed the first phase scope: read-only call observability.
- Added call list, detail drawer, event timeline, and placeholder insight sections.
- Confirmed privacy boundaries for phone numbers, raw payloads, and recording metadata.

## 2026-06-09

- Added transcript, quality-result, and recording aggregation through an on-demand insights API.
- Added recording playback through a backend proxy.
- Resolved a browser playback `401` issue by switching from native media URL loading to authenticated blob fetching.
- Added lightweight WebSocket refresh for list and detail views.

## 2026-06-10

- Added status diagnostics to the detail view.
- Reorganized menu entries so call-related pages are grouped under a call platform section.
- Moved the feature from `ACTIVE` to `VERIFYING`.

## Current Verification Tasks

- Validate real-data filters and pagination.
- Validate transcript, quality result, and recording display in the detail drawer.
- Validate recording playback and download in staging.
- Validate WebSocket active-window behavior.
