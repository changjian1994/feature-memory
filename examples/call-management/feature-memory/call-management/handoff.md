# Call Management Handoff

## Current Goal

Validate the call-management module with real data after the main read-only feature has been implemented.

## Current State

- Work state: `VERIFYING`
- Memory state: `VERIFYING`
- Call list, detail drawer, event timeline, transcript display, quality results, recording playback, and WebSocket refresh are implemented.
- A resolved recording playback issue is documented under `debug/BUG-20260609-001-recording-playback-auth/`.

## What The Next Agent Must Know

- The feature is intentionally read-only.
- The list API should stay lightweight and must not load large JSON payloads, transcripts, quality results, or audio bytes.
- Details, events, transcripts, quality results, and recordings are fetched on demand.
- Recording playback must use the authenticated backend proxy.
- Browser-native `<audio>` or `window.open` requests may not carry auth headers; use the authenticated HTTP client to fetch a blob, then create an object URL.
- Phone numbers, object storage keys, tokens, cookies, and raw production logs must be redacted.

## Recommended Next Steps

1. Validate list filters and pagination with real data.
2. Validate the detail drawer with a call that has transcript, quality result, and recording metadata.
3. Validate WebSocket refresh during the configured active time window.
4. Confirm that recording playback and download still use the authenticated blob flow.
