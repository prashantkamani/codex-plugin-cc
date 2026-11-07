# Changelog

## 1.0.6-aria.2

- Disable codebase-memory MCP for fresh and resumed companion tasks, and direct graph queries to its CLI to avoid leaving MCP processes in the shared broker.
- Preserve configured developer instructions and existing review/import behavior.

## 1.0.6-aria.1

- Add task-level `--sandbox` selection with `read-only`, `workspace-write`, `danger-full-access`, and configuration-inheriting modes.
- Preserve sandbox selection across foreground, background, and resumed tasks while keeping existing defaults unchanged.

## 1.0.0

- Initial version of the Codex plugin for Claude Code
