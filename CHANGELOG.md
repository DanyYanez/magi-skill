# Changelog

All notable changes to this skill are documented in this file.

Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] — 2026-08-26

### Added
- **Persistent report** at `.magi/report.md` in the target repo root. Findings survive between runs.
- **Finding statuses**: `pending`, `false positive`, `by design`, `fixed` (with `How:` field describing the fix).
- **Status tags** in finding titles (e.g. `[PENDING]`, `[FIXED]`) for fast visual scanning.
- **Carry-over between runs**: on each new run, Claude Code loads the previous report, discards findings already closed, and keeps `pending` ones.
- **On-the-fly updates**: as the user reviews findings, Claude Code edits `.magi/report.md` inline to reflect status changes.
- **Run metadata**: last run timestamp and total run count in the report header.

### Changed
- `SKILL.md` translated to English (behavior is still multilingual via the "detect conversation language" instruction).
- Execution flow expanded from 4 to 5 steps to accommodate load / persist / update.

## [1.0.0] — 2026-05-07

### Added
- Initial release.
- Three parallel sub-agents: **Melchior** (logic, architecture, SOLID), **Balthasar** (security, testing, best practices), **Casper** (performance, UX, DX).
- Consolidated single report by severity (🔴 critical / 🟡 medium / 🟢 minor).
- Language auto-detection from the conversation.
- No automatic fixes — the user always decides what to touch.
