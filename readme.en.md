# Feature Memory Skill

Make feature context in AI-assisted development traceable, recoverable, and easy to hand off.

A feature-level memory skill for Code Agents. It helps persist design decisions, progress, handoff context, and debugging evidence inside the project during multi-step development, debugging, and context recovery.

It is intended for Code Agents / IDE Agents that support Markdown-based skills or directory-based skill packages.

## What It Solves

AI-assisted development often runs into these problems:

- A long-running feature loses earlier design decisions across sessions.
- A new Agent needs the same background, progress, and blockers explained again.
- Debugging evidence is scattered across multiple conversation turns.
- Multiple Agents cannot quickly tell what is done, what remains, and why decisions were made.

`feature-memory` is not just a documentation template. It is a context memory workflow for Code Agents, helping engineering teams capture long-running features, complex debugging, and handoff recovery inside the project so the next Agent or teammate can continue from the current state instead of starting over.

## Core Capabilities

- Memory first: read existing feature memory before applicable tasks to avoid duplicate work and repeated debugging.
- Facts versus assumptions: keep verified facts, open questions, and debug hypotheses clearly separated.
- Scoped updates: update memory documents when goals, design, status, risks, handoff context, or debug conclusions change.
- Fast handoff: help a new Agent recover context quickly through `handoff.md`, `progress.md`, and `design.md`.
- Debug convergence: use dedicated Bug directories to track hypotheses, validation steps, logs, and the latest conclusion.

## Repository Structure

```text
.
├── .github/
│   └── ISSUE_TEMPLATE/
├── feature-memory/
│   ├── SKILL.md
│   └── references/
│       ├── debug-workflow.md
│       ├── privacy.md
│       ├── readme.md
│       └── templates.md
├── CHANGELOG.md
├── LICENSE
├── PRIVACY.md
├── readme.md
└── readme.en.md
```

- `feature-memory/SKILL.md`: the skill entry file, including trigger conditions, core principles, directory conventions, and the main workflow.
- `feature-memory/references/templates.md`: templates for project-level, feature-level, and Bug-level documents.
- `feature-memory/references/debug-workflow.md`: complex debugging workflow, `debug.sh` safety rules, and log iteration protocol.
- `feature-memory/references/privacy.md`: built-in privacy, security boundary, and redaction guidance for the skill.
- `feature-memory/references/readme.md`: reference documentation overview and when to read each file.
- `.github/ISSUE_TEMPLATE/`: GitHub Issue templates.
- `CHANGELOG.md`: version history.
- `LICENSE`: MIT open source license.
- `PRIVACY.md`: privacy, redaction, and target-project Git commit guidance.

## Installation

This repository does not contain runtime code and has no dependencies. Import the `feature-memory/` directory from this repository as a skill package into your Code Agent / IDE Agent skills directory.

Common setup:

```bash
git clone https://github.com/changjian1994/feature-memory.git
```

Then copy or symlink the `feature-memory/feature-memory` directory into your Agent's skills directory. The exact skills directory depends on the Agent you use, so follow your tool's documentation.

## When To Use

Use this skill on demand. It is not intended to be enabled blindly for every ordinary coding task. It is useful when an Agent is asked to:

- Implement or continue a multi-step feature.
- Recover development context after an interruption.
- Prepare feature handoff notes.
- Track design, progress, decisions, or risks.
- Investigate complex bugs, test failures, production incidents, or flaky behavior.
- Converge evidence and next validation steps after multiple debugging rounds.

For one-off small edits, formatting changes, simple renames, or low-risk tasks that do not need persistent context, memory documents are usually unnecessary.

## Interactive Entry

Users can simply type:

```text
/feature-memory
```

The Agent will greet the user and ask what to do next.

If the host Agent supports native pickers, Quick Pick, or question selection components, it should prefer that interactive UI. If not, it falls back to numbered text input:

```text
Hello, I am feature-memory.
I can help you recover existing feature context or create memory for a new feature.

Please choose:
1. Continue an existing feature
2. Create a new feature
3. View the current project feature list
4. Exit
```

If the user chooses to continue an existing feature, the Agent reads the target project's `feature-memory/index.md` and lists available features. After the user selects one, the Agent reads that feature's `handoff.md`, `progress.md`, `design.md`, and `decision.md` to recover context.

If the user chooses to create a new feature, the Agent asks for the feature name, goal, and scope, then initializes the corresponding `feature-memory/<feature-name>/` memory directory.

## Memory Structure In Target Projects

The skill asks the Agent to maintain a `feature-memory/` directory at the target project root:

```text
feature-memory/
├── index.md
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

Note: the target project's `feature-memory/` directory is for development memory only. Do not store business code, tests, build artifacts, secrets, tokens, cookies, personal data, internal domains, raw production logs, or unsanitized database content in it.

## Workflow Overview

1. Locate `feature-memory/` at the target project root.
2. If the memory directory does not exist, initialize the minimum project-level document structure.
3. Read `index.md`, `project/*`, and the target feature's key documents.
4. Separate verified facts, assumptions, and open questions.
5. Start implementation, debugging, refactoring, or documentation updates.
6. Before finishing, update relevant memory documents only when the feature goal, design, status, risk, handoff context, or debug conclusion has changed.

## Debugging Convention

Complex issues get a dedicated Bug directory under the related feature:

```text
feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/
├── readme.md
├── timeline.md
├── conclusion.md
├── debug.sh
└── output/
```

- `timeline.md` is append-only and preserves the investigation history.
- `conclusion.md` keeps only the latest conclusion and clearly separates confirmed facts, excluded hypotheses, and remaining hypotheses.
- `debug.sh` should default to read-only diagnostic commands; writing, deleting, migrating, restarting services, or modifying databases requires confirmation first.
- `output/` stores diagnostic outputs from each round so the Agent can continue converging from logs.

## Scope

- Good fit: long-running features, complex debugging, multi-Agent handoff, context recovery, and architecture decision records.
- Not a fit: ordinary one-off small tasks, replacing formal product docs, replacing tests, storing sensitive data, or storing business code.
- Git policy: whether the target project's `feature-memory/` should be committed depends on the project; for public repositories or sensitive content, do not commit it unless reviewed and sanitized.
- Privacy rules: read `PRIVACY.md` before writing architecture, API, database, or log details.

## Contributing

Issues and pull requests are welcome for improving templates, directory conventions, debugging protocols, and installation notes for different Agents.

You can also send bug reports or suggestions to: `changjian1994@sina.com`.

## License

This project is licensed under the MIT License. See `LICENSE` for details.
