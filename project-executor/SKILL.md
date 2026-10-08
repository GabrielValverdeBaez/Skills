---
name: project-executor
description: Execute a bounded, evidence-driven software project route from local repository documentation; supports status, assisted, autonomous, resume, bootstrap and dry-run. Manual invocation only.
disable-model-invocation: true
argument-hint: "status | assisted <goal> | autonomous <goal> max-tasks=N time-budget=M | resume max-tasks=N | bootstrap | dry-run <goal>"
---
# Project Executor

Invocation arguments: `$ARGUMENTS`. Work in the current session; never fork. This skill is an execution protocol, not a daemon. Follow ordinary Claude Code permissions; do not grant broad tool access.

## Start every invocation
1. Parse the leading mode and natural-language objective, accepting flexible order of limits. Default to `status` when no mode; for `autonomous`, default `max-tasks=1 time-budget=30` (minutes); for `resume`, default `max-tasks=1 time-budget=30`. Reject nonpositive or unreasonably large limits; use the smaller safe limit when uncertain. Track wall-clock time; never start a new task if budget cannot reasonably accommodate verification and handoff. No infinite loops.
2. Locate the actual repository root (`git rev-parse --show-toplevel`), or identify a non-Git project explicitly. Read `git status --porcelain=v1 -uall`, current branch, and relevant diff summaries; do not expose secret content. Preserve every existing modification, including untracked files. If material overlap with another's changes is possible, stop or work around it.
3. Discover orientation in this order, with bounded reads: `CLAUDE.md`, `AGENTS.md`, `PROJECT.md`, `ROUTE.md`, `ARCHITECTURE.md`, `DECISIONS.md`, `README.md`, `docs/`, `TASKS/`, `EVIDENCE/`, requirements, tests and deployment notes. Search project-local subdirectories as necessary. Then inspect only code/config/tests pertinent to a candidate task. Do not bulk-load the repository.
4. Interpret: explicit human intent/decisions > canonical project state > current technical docs > verified tests/behavior > implementation > Git history > labeled assumptions. Documentation specifies intent; tests and code show actual behavior. Investigate contradictions and record them; never silently choose one.
5. Use existing Project Route if present; otherwise adapt existing documentation and task conventions. Obsidian is an optional read-only-derived view, never a prerequisite or authority. Read [project-route-contract.md](references/project-route-contract.md).

## Modes
- `status`: STRICT READ-ONLY, including no handoff, cache, claim, Obsidian or project writes. Report repository/dirty files (paths as appropriate), sources, READY calculation, blockers, contradictions and next recommendation.
- `dry-run`: STRICT READ-ONLY; show task choice, context, proposed edits, verification plan, gates and stop condition. Never claim, run mutating tests, or write evidence.
- `bootstrap`: only on explicit invocation. Preserve existing docs; create the smallest route/state only where missing and needed. Propose instead of writing if conflicting conventions or uncertain authority. Templates under `assets/templates/` are examples, not mandatory.
- `assisted`: execute at most ONE READY task, verify, persist evidence/state/handoff, then stop even if more READY tasks exist.
- `autonomous`: execute sequential READY tasks until bounded task count/time, gate, unresolved conflict or other stop. Recompute READY after each verified completion. Do not parallelize canonical state writes.
- `resume`: inspect persisted state, Git and tests; treat old IN_PROGRESS/claims as untrusted until reconciled with actual artifacts and evidence. Never assume interrupted work succeeded. Then execute bounded tasks as autonomous.

Read [modes-and-limits.md](references/modes-and-limits.md) for mode edge cases and stopping behavior.

## Per-task transaction
1. Determine intent, inputs, dependencies, acceptance and verification; calculate READY, not merely TODO. Announce task and intended touched files.
2. Recheck Git and potential overlap immediately before editing. Record claim only if existing canonical scheme supports it; never steal an active claim. If no claims exist, use a minimal task-state record in the project's existing convention.
3. Implement the smallest relevant reversible change. Add tests where appropriate; no unrelated refactors.
4. Run focused tests and relevant typecheck/lint/build/integration/E2E/security checks as appropriate. Validate behavior, not just exit code. If a required test cannot run, mark REVIEW/BLOCKED, not DONE.
5. Review diffs, regressions, security and compatibility. Store evidence linked to acceptance criteria, with commands, outcomes, artifacts and limitations. Only mark DONE when output, criteria, verification, evidence and required review are all satisfied.
6. Update canonical route/state, decisions if needed, and a persistent handoff (use existing location; otherwise `docs/HANDOFF.md` if safe). Recalculate READY. Continue only if mode and limits permit.
7. On every stop in a mutating mode, write a handoff unless writes are unsafe; if unsafe, report an explicit recovery handoff in chat and leave canonical state unchanged. Read [runner-handoff.md](references/runner-handoff.md).

## Non-negotiable gates
Read [gates-and-replanning.md](references/gates-and-replanning.md). May refine reversible technical tasks and dependencies with traceable history while preserving goals and constraints. Require human approval for changes to goals/scope, material architecture, guardrails, spending, production deployments, destructive migrations, external publication/messages, novel credential handling or hard-to-reverse actions. Do not perform them before approval.

Never `git reset --hard`; never discard/overwrite others' edits, create a clean checkout to bypass a dirty tree, or commit/push/merge/rebase/open PR/deploy without explicit request. Never leak secrets or edit outside the project (except separate skill installation explicitly requested). Normal permissions remain in force.

## Final response
Report mode, repository, selected/completed tasks, READY and blocked tasks, dirty-tree preservation, verification with evidence paths, unverified items, changed files, gate/stop reason, next action and handoff path. Never invent progress percentages. Do not invoke yourself again or schedule future execution.