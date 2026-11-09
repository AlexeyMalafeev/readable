# Changelog

## [1.0.0] - 2026-11-09

First stable release, including the work published before releases were tagged.

### Added

- The Readable skill in the open Agent Skills format, with rules for clear outcomes, plain language, concrete conclusions and self-contained messages. Originally introduced in [cbbe258](https://github.com/AlexeyMalafeev/readable/commit/cbbe258).
- A short version, `always-on.md`, for global agent instructions, plus installation and usage guidance for Claude Code and Codex. Originally introduced in [cbbe258](https://github.com/AlexeyMalafeev/readable/commit/cbbe258).
- Installation, update and removal instructions for the skills CLI, alongside the repository-cloning option. Added in [27811e8](https://github.com/AlexeyMalafeev/readable/commit/27811e8).
- Guidance to name the objects of actions, explain the units behind counts, and state why substantial work was needed and why a method was chosen. Includes examples and stronger checks before sending a message. Added in [9c8807e](https://github.com/AlexeyMalafeev/readable/commit/9c8807e).
- This changelog, covering the repository history up to the first release.

### Changed

- Keep familiar context when it supplies a missing premise or explains a choice, instead of removing it merely because it may seem obvious. Updated the full skill, short rules and README in [9c8807e](https://github.com/AlexeyMalafeev/readable/commit/9c8807e).

### Removed

- The README's early-version status notice.

[1.0.0]: https://github.com/AlexeyMalafeev/readable/releases/tag/v1.0.0
