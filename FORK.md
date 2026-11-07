# Aria fork maintenance

This repository is a maintained fork of `openai/codex-plugin-cc`.

## Fork delta

The fork adds an explicit sandbox selector to companion task dispatch:

- `--sandbox read-only`
- `--sandbox workspace-write`
- `--sandbox danger-full-access`
- `--sandbox inherit`, which omits the app-server thread override and lets Codex configuration decide

Omitting `--sandbox` preserves upstream behavior: tasks are read-only unless `--write` selects workspace-write. Reviews remain read-only. Full access is never selected implicitly.

New task threads disable the configured `codebase-memory` MCP server through native per-thread config and receive instructions to use `codebase-memory-mcp cli` instead. Tasks send the same config on resume; an already-loaded thread created before this fix may ignore resume overrides, so start a fresh task to apply the policy. Parallel companion audits can otherwise leave per-thread MCP processes resident in the shared broker and block graph-health CLI checks. The CLI reads the existing graph without registering a watcher; normal graph freshness checks still apply. Review and import flows retain their existing configuration.

## Updating from upstream

1. Fetch `origin` and compare the new tag or `origin/main` with the current fork base.
2. Rebase the fork's sandbox commit onto that upstream revision.
3. Inspect changes to `codex-companion.mjs`, `lib/codex.mjs`, task persistence, manifests, and command/agent/skill routing before resolving conflicts.
4. Run `npm test`, `npm run build`, and `npm run check-version`.
5. Bump the `-aria.N` version, push the fork branch, update the local Claude marketplace, and start a new Claude Code session.

The personal remote is named `fork`; the OpenAI repository remains `origin`. Do not patch Claude's installed plugin cache.
