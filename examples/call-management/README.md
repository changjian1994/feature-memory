# Example: Call Management

This example shows how `feature-memory` helps an Agent continue a long-running feature without rereading the whole conversation history.

The scenario is a sanitized call-management module for an internal admin console. The feature started as a simple read-only call list, then expanded into call details, event timeline, transcripts, quality results, recording playback, WebSocket refresh, and a production-style recording playback bug.

## Why This Example Is Useful

It demonstrates the main value of `feature-memory`:

- A new Agent can start from `feature-memory/ai-handoff.md` instead of scanning every file.
- The feature boundary is explicit: read-only observability first, no call-control actions in the first phase.
- `handoff.md` keeps the current state concise, while `progress.md` keeps the process history.
- `decision.md` records why important trade-offs were made.
- `bug-index.md` and `debug/.../conclusion.md` preserve reusable debugging knowledge.
- Sensitive details are redacted; the example keeps only architecture, decisions, and troubleshooting patterns.

## Example Structure

```text
examples/call-management/
└── feature-memory/
    ├── ai-handoff.md
    ├── index.md
    ├── bug-index.md
    ├── project/
    │   └── architecture.md
    └── call-management/
        ├── readme.md
        ├── design.md
        ├── decision.md
        ├── progress.md
        ├── handoff.md
        └── debug/
            └── BUG-20260609-001-recording-playback-auth/
                └── conclusion.md
```

## How To Read It

1. Start with [`feature-memory/ai-handoff.md`](feature-memory/ai-handoff.md).
2. Open [`feature-memory/index.md`](feature-memory/index.md) to see feature state and entry points.
3. Read [`feature-memory/call-management/handoff.md`](feature-memory/call-management/handoff.md) to understand the current implementation state.
4. Read [`feature-memory/call-management/decision.md`](feature-memory/call-management/decision.md) to understand the important trade-offs.
5. Read [`feature-memory/bug-index.md`](feature-memory/bug-index.md) and the debug conclusion to see how a resolved bug becomes reusable knowledge.

## Privacy Note

This example is intentionally generalized. Product names, domains, database names, object keys, tokens, customer data, and production logs have been removed or replaced with neutral terms.
