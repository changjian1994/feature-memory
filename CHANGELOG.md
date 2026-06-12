# Changelog

All notable changes to this project will be documented in this file.

## 1.5.1 - 2026-06-10

### Changed

- Simplified Chinese and English README files to focus on user-facing value, installation, quick start, usage scenarios, and documentation links.
- Moved detailed operational rules out of the main README narrative and kept references as the source of truth for debug, feature relationships, memory lifecycle, templates, and privacy.

## 1.5.0 - 2026-06-10

### Added

- Added `references/memory-lifecycle.md` with AI first-read entry, memory states, default reading scope, update limits, file responsibility boundaries, and archive compaction rules.
- Added `feature-memory/ai-handoff.md` as the project-level first-read entry template.
- Added memory state guidance for `ACTIVE`, `VERIFYING`, `ARCHIVED`, and `LEGACY`.

### Changed

- Updated `SKILL.md` startup flow to read `ai-handoff.md` first and default to `ACTIVE` / `VERIFYING` memory.
- Updated completion flow to limit routine memory updates to the most important 1-3 files.
- Updated templates and README files to distinguish `progress.md` process history from `handoff.md` current handoff view.
- Updated `index.md` template to separate work status from memory status.

## 1.4.1 - 2026-06-10

### Added

- Added a Git pre-commit checkpoint flow in `SKILL.md`, prompting users to check or update feature-memory before commit, tag, push, release, or branch merge.
- Added interactive options to update memory, check without modification, skip low-risk commits, or cancel the commit flow.

## 1.4.0 - 2026-06-10

### Added

- Added `references/feature-relationship.md` for feature merging, splitting, parent-child relationships, dependencies, cross-cutting relationships, and aggregate navigation.

### Changed

- Refactored `SKILL.md` into a Lean Core routing structure that keeps trigger boundaries, hard rules, task routing, startup flow, completion flow, and success criteria in the main entry file.
- Moved detailed feature relationship guidance out of `SKILL.md` into an on-demand reference file.
- Expanded `debug-workflow.md` with explicit `bug-index.md` pre-check and post-debug registration rules.
- Updated README files and reference documentation to describe the Lean Core routing structure and the new feature relationship reference.

## 1.3.0 - 2026-06-09

### Added

- Added global Bug knowledge base index `feature-memory/bug-index.md` with Bug ID, root cause category, keyword tags, and entry document pointer.
- Added "Memory Index" and "Prerequisite Knowledge" blocks at the top of `conclusion.md` template, linking back to `bug-index.md`.
- Added requirement: before entering a debug workflow, the Agent must first read `bug-index.md` to look for similar Bugs by keyword / root cause category.
- Added requirement: after a Bug ends (RESOLVED / BLOCKED / VERIFYING), the Agent must register a new row in `bug-index.md`.
- Added `bug-index.md` to directory structure in SKILL.md and both README files.
- Added dedicated section in both Chinese and English README explaining the Bug knowledge base index and its value.

### Changed

- Updated the debug workflow steps in SKILL.md to include the bug-index pre-check and post-check registration.

## 1.2.0 - 2026-06-09

### Added

- Added feature relationship management to handle similar, overlapping, parent-child, and cross-cutting features.
- Added "Parent Feature" and "Dependencies" columns to `index.md` for tracking relationships.
- Added "Includes" and "Excludes" sections to feature `readme.md` for clear boundary definition.
- Added overlap detection strategy: before creating a new feature, check if it overlaps >60% with existing features.
- Added preference for merging highly overlapping features rather than creating duplicates.
- Added "small but independent" principle: features should be small enough to stay focused but independent to avoid coupling.
- Added aggregate navigation mechanism: when creating a large feature, existing child features can be linked via index tree without duplication.
- Added aggregate feature template with child feature index tree structure.

### Changed

- Renumbered core principles: added "Principle 2: Clear Boundaries and Relationships".
- Updated handling strategy to include independent splitting and aggregate navigation.

## 1.1.2 - 2026-06-07

### Changed

- Strengthened debug trigger rules for debugging, log analysis, test failures, production incidents, flaky issues, and multi-round investigations.
- Clarified that complex debugging must create or maintain `feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/`.
- Added explicit debug initialization steps for `readme.md`, `timeline.md`, `conclusion.md`, `debug.sh`, and `output/`.
- Added a one-command debug loop where `debug.sh` writes versioned logs such as `output/v1.log`, `output/v2.log`, and `output/v3.log`.
- Clarified that users can run `./debug.sh` and paste the generated `output/vN.log` content back unchanged for the next analysis round.
- Added Bug-level document templates and README guidance to avoid recording complex debugging only in feature-level `progress.md`.

## 1.1.1 - 2026-06-06

### Changed

- Clarified that `/feature-memory` should prefer host-native interactive pickers, Quick Pick, or question selection components when available.
- Documented numbered text input as the fallback interaction mode when native picker UI is unavailable.

## 1.1.0 - 2026-06-06

### Added

- Added an interactive `/feature-memory` entry flow for choosing whether to continue an existing feature, create a new feature, view the feature list, or exit.
- Added feature discovery behavior that reads `feature-memory/index.md` and lists existing features before loading detailed context.
- Added new-feature onboarding guidance for collecting the feature name, goal, and scope before initializing memory files.
- Documented the interactive entry flow in both Chinese and English README files.

## 1.0.2 - 2026-06-06

### Changed

- Improved the README value proposition to clarify that `feature-memory` is a context memory workflow for Code Agents, not just a documentation template.
- Added clearer Chinese and English project taglines for traceable, recoverable, and handoff-friendly feature context.

## 1.0.1 - 2026-06-06

### Changed

- Narrowed the skill trigger conditions to focus on long-running features, complex bugs, multi-round debugging, handoff, and context recovery.
- Changed memory updates from mandatory for every implementation task to scoped updates when goals, design, status, risks, handoff context, or debug conclusions change.
- Clarified that one-off small edits, formatting changes, simple renames, and low-risk tasks usually do not need memory documents.
- Added privacy and redaction guidance for secrets, tokens, cookies, personal data, internal domains, raw production logs, and unsanitized database content.
- Added guidance for deciding whether the target project's `feature-memory/` directory should be committed to Git.

## 1.0.0 - 2026-06-06

### Added

- Initial release of the `feature-memory` skill.
- Project-level memory structure for overview, architecture, database, and API notes.
- Feature-level memory structure for readme, design, progress, decision, and handoff documents.
- Debug workflow for complex issues with timeline, conclusion, diagnostic script, and output logs.
- Templates for project-level, feature-level, and Bug-level memory documents.
- GitHub Issue templates for bug reports and improvement suggestions.
- Basic `.gitignore` for common local files and build outputs.
- MIT License.
