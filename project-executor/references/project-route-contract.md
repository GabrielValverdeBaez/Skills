# Project Route contract

Use Intent, Objectives, Tasks, Dependencies, Outputs, Acceptance criteria, Verification, Evidence, Decisions, Gates, State, Handoffs and Replans when present. Preserve project-local file names and schema; do not impose a migration.

Lifecycle: TODO -> READY -> IN_PROGRESS -> REVIEW -> DONE; lateral BLOCKED and CANCELLED. READY is computed only if task is active, all hard dependencies are DONE with evidence, required inputs exist, necessary context is resolvable, no blocker exists, and prior human gates are cleared. A checkbox is not proof. REVIEW means implemented but awaiting verification/review; DONE requires output + criteria + verification + evidence + required review.

Canonical state has one writer. If a claim is present, check owner, freshness, task and Git before treating it as abandoned. Never override a possibly active claim. When state is ambiguous, stop or report BLOCKED. Existing project conventions win over template examples. Prefer append-only decision/replan records or clearly attributed edits that preserve previous reasoning.

Without Project Route: derive tasks from current docs and tests, distinguish explicit requirements from inference, identify the existing status source, and request minimal persistence only when necessary. Do not auto-create PROJECT.md/ROUTE.md outside explicit bootstrap unless indispensable, safe and non-conflicting.