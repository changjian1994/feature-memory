# Feature Memory Skill

Make feature context in AI-assisted development traceable, recoverable, and easy to hand off.

`feature-memory` is a Markdown-based skill for Code Agents / IDE Agents. It persists important context from long-running features, complex debugging, and handoff recovery inside the project, so the next Agent or teammate can quickly understand what is done, why it was done, and what to do next.

> Intended for Code Agents / IDE Agents that support directory-based skill packages or Markdown-based skills.

## Why It Exists

AI-assisted development often runs into these problems:

- A long-running feature loses earlier design decisions across sessions.
- A new Agent needs the same background, progress, and blockers explained again.
- Debugging hypotheses, logs, and conclusions are scattered across conversations.
- Multiple Agents or teammates cannot quickly tell the current project state.

`feature-memory` is not just a documentation template. It is a context memory workflow for Code Agents. It helps the Agent recover memory before applicable tasks and continue from the current state instead of starting over.

## What It Does

- **Context recovery**: when continuing an existing feature, read project-level `ai-handoff.md` and key feature documents first.
- **Scoped updates**: update memory only when goals, design, status, risks, handoff context, or debug conclusions change.
- **Complex debugging**: create dedicated Bug directories for hypotheses, validation steps, logs, and latest conclusions.
- **Bug knowledge base**: reuse historical debugging conclusions through `bug-index.md` to avoid repeated investigations.
- **Feature relationship management**: handle parent-child, dependency, cross-cutting, duplicate, and aggregate features.
- **Bounded memory cost**: read active memory by default, skip memory updates for small edits, and update only the most important 1-3 files per task.

## Installation

This repository does not contain runtime code and has no dependencies.

```bash
git clone https://github.com/changjian1994/feature-memory.git
```

Then copy or symlink the `feature-memory/feature-memory` directory into your Agent's skills directory.

The exact skills directory depends on the Agent you use, so follow your tool's documentation.

## Quick Start

In a supported Agent, type:

```text
/feature-memory
```

The Agent will ask you to choose:

```text
1. Continue an existing feature
2. Create a new feature
3. View the current project feature list
4. Exit
```

If the host Agent supports native pickers, Quick Pick, or question selection components, it should prefer that UI. Otherwise, it falls back to numbered text input.

## When To Use

Use this skill on demand. It is not intended to be enabled blindly for every ordinary coding task.

Good fit:

- Implementing or continuing a multi-step feature.
- Recovering development context after an interruption.
- Preparing feature handoff notes.
- Tracking design, progress, decisions, or risks.
- Investigating complex bugs, test failures, production incidents, or flaky behavior.
- Converging evidence and next validation steps after multiple debugging rounds.

Not a fit:

- One-off small edits.
- Simple renames, formatting, comments, or copy tweaks.
- Replacing formal product docs, tests, or project management systems.
- Storing business code, secrets, tokens, cookies, personal data, or raw production logs.

## Target Project Structure

The Agent maintains a `feature-memory/` directory at the target project root:

```text
feature-memory/
├── ai-handoff.md
├── index.md
├── bug-index.md
├── project/
│   ├── overview.md
│   ├── architecture.md
│   ├── database.md
│   └── api.md
└── <feature-name>/
    ├── readme.md
    ├── design.md
    ├── progress.md
    ├── decision.md
    ├── handoff.md
    └── debug/
        └── BUG-YYYYMMDD-XXX-description/
```

Key files:

- `ai-handoff.md`: project-level first-read entry for new Agents.
- `index.md`: global feature index with status, relationships, dependencies, and entry links.
- `bug-index.md`: global knowledge base index for complex Bugs.
- `<feature-name>/handoff.md`: current handoff view for one feature.
- `<feature-name>/progress.md`: process history and status changes.
- `<feature-name>/debug/`: multi-round debugging records for complex Bugs.

## How It Works

In a typical task, the Agent will:

1. Locate `feature-memory/` at the target project root.
2. Read `ai-handoff.md` first, then `index.md` and target feature documents on demand.
3. Read only `ACTIVE` and `VERIFYING` memory by default.
4. Separate verified facts, assumptions, and open questions.
5. Start implementation, debugging, refactoring, or documentation updates.
6. Update memory only when important information changes.
7. Update only the most important 1-3 memory files per task by default.

## Advanced Capabilities

- **Debug loop**: complex issues can generate `debug.sh`; each round writes `output/vN.log`, the user sends the log back unchanged, and the Agent keeps converging. See [`debug-workflow.md`](feature-memory/references/debug-workflow.md).
- **Bug knowledge base**: resolved or blocked complex Bugs are registered in `bug-index.md`, so similar future issues can reuse historical conclusions. See [`templates.md`](feature-memory/references/templates.md).
- **Feature relationship management**: before creating a new feature, the Agent checks existing features to avoid duplicates while keeping features small and independent. See [`feature-relationship.md`](feature-memory/references/feature-relationship.md).
- **Memory lifecycle**: `ACTIVE`, `VERIFYING`, `ARCHIVED`, and `LEGACY` states control reading scope and archive compaction. See [`memory-lifecycle.md`](feature-memory/references/memory-lifecycle.md).
- **Privacy and redaction**: do not record secrets, tokens, cookies, personal data, internal domains, raw production logs, or unsanitized database content. See [`privacy.md`](feature-memory/references/privacy.md).

## Repository Structure

```text
.
├── feature-memory/
│   ├── SKILL.md
│   └── references/
├── CHANGELOG.md
├── LICENSE
├── PRIVACY.md
├── readme.md
└── readme.en.md
```

- `feature-memory/SKILL.md`: the skill entry file using a Lean Core routing structure.
- `feature-memory/references/`: detailed rules, templates, and safety guidance loaded on demand.
- `PRIVACY.md`: user-facing privacy and redaction guidance.
- `CHANGELOG.md`: version history.

## Documentation

- [中文 README](readme.md)
- [Changelog](CHANGELOG.md)
- [Privacy](PRIVACY.md)
- [Debug workflow](feature-memory/references/debug-workflow.md)
- [Feature relationship](feature-memory/references/feature-relationship.md)
- [Memory lifecycle](feature-memory/references/memory-lifecycle.md)
- [Templates](feature-memory/references/templates.md)

## Contributing

Issues and pull requests are welcome for improving templates, directory conventions, debugging protocols, and installation notes for different Agents.

You can also send bug reports or suggestions to: `changjian1994@sina.com`.

## License

This project is licensed under the MIT License. See [`LICENSE`](LICENSE) for details.
