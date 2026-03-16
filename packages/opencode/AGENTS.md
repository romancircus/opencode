# Scoped Agent Instructions for packages/opencode

POLICY_VERSION: 2026-03-16

## Scope

This file governs the core OpenCode runtime under `packages/opencode`, including CLI behavior, server endpoints, tool execution, and generated contract touchpoints.

## Local Constraints

- Keep Bun + TypeScript ESM conventions intact and validate inputs with Zod where the package already does so.
- Follow the existing namespace-oriented structure (`Tool.define`, `Session.create`, `App.provide`, `Storage`, `Log.create`) instead of introducing ad hoc patterns.
- When changing `packages/opencode/src/server/server.ts` or other externally consumed contract surfaces, regenerate dependent SDK artifacts before finishing the task.

## Local Commands

```bash
bun --cwd packages/opencode dev
bun --cwd packages/opencode typecheck
bun --cwd packages/opencode test
bun run ./script/generate.ts
```

## Safety Notes

- Avoid silent wire-shape changes that would break `packages/app` or `packages/sdk/js`.
- Prefer local targeted tests before monorepo-wide reruns when only runtime code changed.
- Escalate to the root policy when a change spans multiple packages or release tooling.
