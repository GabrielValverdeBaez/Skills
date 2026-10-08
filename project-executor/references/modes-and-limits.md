# Modes and limits

`status` and `dry-run` are strictly read-only: avoid any command that can mutate files, caches, test outputs, Git metadata or external services. `dry-run` plans verification but does not execute mutating checks. `bootstrap` may write minimal docs with normal permissions, but never replace existing docs.

`assisted` executes exactly zero or one task and stops. `autonomous` and `resume` use finite task/time limits, default 1 task and 30 minutes if absent. `max-tasks` counts attempted tasks, not just successful tasks, preventing repeated failure loops. Stop before deadline with enough time for handoff. On failure, one bounded correction attempt within the same task is permissible if safe; otherwise BLOCKED. Do not use recursion or self-invocation.

`resume` must reconcile: last handoff, claim owner and age, Git working tree, actual artifacts, tests and evidence. Incomplete evidence means not DONE. Detect simultaneous sessions and refuse competing writes. If usage/rate limit appears, stop and record it; never bypass quotas or silently enable paid API.