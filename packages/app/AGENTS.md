# Scoped Agent Instructions for packages/app

POLICY_VERSION: 2026-03-16

## Scope

This file governs the SolidJS app under `packages/app`, including local UI debugging against the existing development server at `http://localhost:3000`.

## Local Constraints

- Do not restart the app or the shared local dev server unless the user explicitly asks.
- Prefer `createStore` over a spread of independent `createSignal` calls when state needs to stay coherent.
- Keep path-local UI behavior here; repo-wide workflow rules still come from the root `CLAUDE.md`.

## Local Commands

```bash
bun --cwd packages/app dev
bun --cwd packages/app typecheck
bun --cwd packages/app build
```

## Safety Notes

- Use Playwright or equivalent browser automation against the already-running app instead of bouncing local processes.
- Parallelize tool calls when gathering UI context from multiple files.
- Escalate to the root policy when a change also requires SDK or server contract updates.
