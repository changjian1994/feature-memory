# Project Architecture

## Overview

The sample project is a web admin console for AI outbound call observability.

## Layers

- Frontend: route, page, API client, WebSocket status, and detail drawer.
- Backend handler: HTTP and WebSocket routing.
- Service layer: query orchestration, response shaping, validation, and redaction.
- Data sources: call session table, call events table, transcript store, quality-result store, and object storage.

## Important Boundaries

- List APIs return lightweight rows only.
- Detail APIs load large JSON fields on demand.
- Recording files are served through an authenticated backend proxy.
- WebSocket list refresh should not push transcripts, quality JSON, event payloads, or audio bytes.
- Sensitive data is redacted before being written to feature memory.
