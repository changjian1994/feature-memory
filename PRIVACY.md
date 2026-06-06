# Privacy And Security Guidance

`feature-memory` stores development context as Markdown files in the target project. These files may contain architecture notes, API descriptions, debugging summaries, and handoff context. Treat them as project documentation and apply the same security review standards before sharing or committing them.

## Do Not Record

Never write the following content into `feature-memory/` documents:

- Secrets, private keys, access keys, API keys, passwords, or credentials.
- Tokens, cookies, sessions, JWTs, refresh tokens, or authorization headers.
- Personal data, user identifiers, customer data, or production data dumps.
- Internal domains, private network addresses, service discovery names, or non-public infrastructure details.
- Raw production logs, crash dumps, request bodies, database rows, or trace payloads that may contain sensitive data.
- Proprietary business rules, confidential roadmap details, or unreleased product plans unless the target repository is approved for that level of information.

## Recommended Practices

- Prefer concise conclusions over raw data. Record what was confirmed, excluded, and what to verify next.
- Redact sensitive values before writing examples, logs, API payloads, or database snippets.
- Use placeholders such as `<redacted-token>`, `<internal-domain>`, `<user-id>`, and `<sample-id>`.
- Keep debug outputs in `feature-memory/<feature-name>/debug/.../output/` only when they are safe to store.
- Review generated memory files before committing or sharing them.

## Git Policy

Whether the target project's `feature-memory/` directory should be committed depends on the project:

- Public or open source repositories: do not commit `feature-memory/` unless all content is reviewed and sanitized.
- Internal repositories: follow your team's documentation and data handling policy.
- Sensitive debugging sessions: consider adding `feature-memory/` to the target project's `.gitignore` and sharing only sanitized summaries.

## Agent Behavior

Agents using this skill should:

- Ask for confirmation before storing sensitive-looking logs, API examples, or database details.
- Summarize sensitive evidence instead of copying raw content.
- Mark uncertain information as assumptions or open questions.
- Avoid creating memory files for small, low-risk tasks unless the user asks for persistent context.
