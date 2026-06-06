# Changelog

All notable changes to this project will be documented in this file.

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
