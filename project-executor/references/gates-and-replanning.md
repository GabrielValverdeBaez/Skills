# Gates and replanning

Allowed without special project-level approval (still subject to Claude Code permissions): split/merge technical tasks, add missing reversible dependencies, reorder tasks, remove redundant work with an auditable explanation, strengthen verification and adjust reversible implementation details. Record old/new task identity, rationale, dependency impact and evidence; never erase history silently.

Stop and ask approval for: change/remove objectives, materially broaden scope, material architecture redesign, weaken security or other guardrails, incur cost, production deployment, destructive migration, external publication or messages, new credential handling, irreversible actions. Treat unknown external side effects as gated. If a gated task is next, do not silently skip it unless an independent READY task can be done safely and the route permits it.

Git: no reset --hard, checkout workaround, discarded local changes, commit, push, merge, rebase, PR or deployment without explicit request. Before each edit inspect touched paths against dirty/untracked files; overlapping work requires a pause or a non-overlapping alternative. Never expose secrets in evidence.