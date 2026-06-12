# Decisions

## 2026-06-09: Keep The First Phase Read-Only

- Decision: the first phase provides observation and troubleshooting only.
- Reason: control actions require stronger permissions, audit logs, idempotency, and rollback design.
- Trade-off: users cannot hang up or retry calls from this module yet, but the feature can ship earlier with lower risk.

## 2026-06-09: Use The Session Table As The List Source

- Decision: list queries are based on the call session table.
- Reason: it is the stable lifecycle source and avoids expensive event aggregation.
- Trade-off: event details are still available, but only in the detail timeline.

## 2026-06-09: Load Insights On Demand

- Decision: transcripts, quality results, and recordings are loaded through an on-demand `insights` API.
- Reason: these documents can be large and are not needed for every list row.
- Trade-off: the first list render stays fast; detail views make extra requests when needed.

## 2026-06-09: Fetch Recordings As Authenticated Blobs

- Decision: the frontend fetches recording files through the authenticated HTTP client and creates object URLs for playback.
- Reason: native browser media requests may not include the required auth headers.
- Trade-off: the frontend must manage object URL lifecycle, but playback works without exposing direct storage URLs.

## 2026-06-09: Use Separate WebSocket Lifecycles

- Decision: list refresh and detail refresh use separate WebSocket connections.
- Reason: a user viewing the list should not trigger repeated loading of event timelines, transcripts, quality results, or recording metadata.
- Trade-off: the frontend manages two lifecycles, but backend load stays bounded.
