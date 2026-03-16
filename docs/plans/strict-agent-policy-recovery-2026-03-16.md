# Plan: strict-agent-policy-recovery

Date: 2026-03-16
Owner: Codex
Status: In Progress

## Context and Objective

`opencode` already had the root `AGENTS.md -> CLAUDE.md` link, but it was missing the strict plan/lint/tooling scaffold and both nested scoped `AGENTS.md` files failed the newer scoped-policy contract. The objective of this plan is to add the missing strict governance files, replace the placeholder-heavy root policy with repo-specific guidance, repair the nested scoped policies, and leave the repo passing strict compliance without losing the existing staged onboarding work or untracked repo notes.

## Success Criteria

1. `python3 scripts/plan_lint.py --all --require-files` passes in this repo.
2. `python3 scripts/claude_policy_lint.py --strict` passes for the root policy and nested scoped policies.
3. `python3 scripts/validate_repo_compliance.py . --require-plan-linear --require-plan-semantics --require-claude-quality` passes in this repo.

## ASCII Diagram

```text
+---------------------------+
| Existing staged scaffold  |
+-------------+-------------+
              |
              v
+-------------+-------------+
| Root policy rewrite       |
| + nested AGENTS cleanup   |
+-------------+-------------+
              |
              v
+-------------+-------------+
| Repo-local plan file      |
| + strict validator pass   |
+-------------+-------------+
              |
              v
+-------------+-------------+
| Commit + push branch      |
+---------------------------+
```

## Business Logic Specification

The OpenCode monorepo needs a reliable root policy and valid scoped instructions so agents can work accurately in both the shared monorepo context and the high-variance subtrees. This remediation removes placeholder guidance, keeps the default branch workflow explicit, and turns the existing nested AGENTS files into real scoped policies instead of ad hoc notes.

- Outcome: `opencode` joins the strict fleet baseline with a coherent root-plus-scoped instruction model.
- User impact: future changes in the app and core runtime can rely on scoped local rules without fighting an outdated root policy.
- Acceptance checks:
  - the repo-local strict validator passes
  - the nested scoped AGENTS files pass strict lint

## Technical Implementation Plan

1. Update files/modules:
- `CLAUDE.md`
- `packages/app/AGENTS.md`
- `packages/opencode/AGENTS.md`
- `docs/plans/strict-agent-policy-recovery-2026-03-16.md`
- `scripts/plan_lint.py`

2. Implementation notes:
- Preserve the existing staged onboarding-related files and untracked repo notes unless they are superseded by the new strict policy files.
- Keep `AGENTS.md` as a symlink to the root `CLAUDE.md`.
- Stage the existing `scripts/precommit_linear_hooks.py` because the current pre-commit config already depends on it.

3. Verification strategy:
- `python3 scripts/plan_lint.py --all --require-files`
- `python3 scripts/claude_policy_lint.py --strict`
- `python3 scripts/validate_repo_compliance.py . --require-plan-linear --require-plan-semantics --require-claude-quality`

## Executable Steps

| Step ID | Description | Business Outcome | Technical Deliverable | Linear Issue | State | Evidence | Last Updated |
|---|---|---|---|---|---|---|---|
| S1 | Add the missing strict scaffold and replace placeholder policies | The repo gains a usable root policy and valid scoped instructions instead of generic notes | `CLAUDE.md`, nested `AGENTS.md`, shared governance scaffold | `ROM-1292` | Completed | onboarding scaffold applied, root policy rewritten, and both nested AGENTS files converted to the strict scoped schema | 2026-03-16 01:08 |
| S2 | Validate, commit, and push the onboarding delta | The repo becomes a passing member of the strict fleet | validator output, branch commit, remote push | `ROM-1292` | In Progress | strict repo validator passed; `bun turbo typecheck` still fails on pre-existing `packages/opencode/src/mcp/index.ts` errors unrelated to this rollout | 2026-03-16 01:08 |

## Risks and Rollback

- Risk: the root rewrite or scoped rewrite drops useful repo-local guidance.
- Mitigation: carry forward the existing branch, SDK-regeneration, dev-server, and parallel-tool rules into the strict schema.
- Rollback: restore the previous files from git history if the strict policy blocks normal monorepo work.

## Handoff Snapshot

- Completed decisions: keep the remediation on branch `fix/mcp-schema-validation`; preserve the existing untracked repo notes; keep the nested AGENTS files as scoped policies instead of deleting them.
- Open risks: the pre-existing staged `AGENTS.md.backup-20260211` is not part of the desired strict contract and should stay out of the rollout commit unless it proves necessary as evidence; package typecheck still fails in `packages/opencode/src/mcp/index.ts` before any rollout-only code changes.
- Next command(s):
```bash
python3 scripts/claude_policy_lint.py --strict
```
