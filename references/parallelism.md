# Manager-Led Parallelism

Use multiple agents to reduce elapsed time only when ownership and integration remain clear. The user is the product owner and final decision maker; the manager coordinates work, reports status, and protects quality and local-only boundaries.

## Manager responsibilities

- Understand the request and repository before delegating.
- Own the plan, task graph, contracts, decisions, status, integration order, and final readiness report.
- Give each worker a bounded outcome, evidence, scope, allowed files or modules, dependencies, acceptance criteria, checks, and explicit non-goals.
- Keep private manager notes and handoffs out of branches, commits, and PRs.
- Review returned work and repository state directly; do not treat a worker's completion claim as proof.
- Stop or resequence work when upstream decisions change.

The manager should not absorb all implementation by default. It may handle integration, small connective edits, or urgent unblockers while specialists own coherent workstreams.

## Role contracts

Use only roles the task needs:

- **Discovery/Architecture:** maps existing behavior, constraints, patterns, contracts, and risk; does not redesign the product silently.
- **Product/Design:** owns flows, state matrices, component contracts, responsive behavior, accessibility, and design-system consistency; does not invent server contracts as facts.
- **Frontend:** implements the approved experience against the contract; keeps domain rules and permission enforcement out of presentation code.
- **Backend:** owns services, APIs/actions, validation, authorization, error mapping, jobs, and integration behavior; does not reshape the domain merely to match a mockup.
- **Data:** owns schema, constraints, indexes, migrations, compatibility, transactions, backfills, retention, and recovery.
- **Testing:** adds focused unit, integration, component, end-to-end, migration, and visual coverage against acceptance criteria and risk.
- **QA:** tries to break the integrated experience across states, permissions, devices, failures, and regressions; it does not accept implementation claims without evidence.
- **Reviewer:** independently examines the combined diff for correctness, architecture, maintainability, scope, and missing tests.
- **Security/Audit:** inspects trust boundaries, secrets, authorization, data exposure, dependencies, abuse cases, and operational controls.
- **Release/Observability:** checks preview, configuration, migration order, flags, rollback, health signals, alerts, workflows/queues, and post-deploy evidence.

## What to parallelize

Good parallel work has stable inputs and low overlap, for example:

- repository, UX, test, and security discovery;
- backend implementation after a stable contract plus independent component preparation against that contract;
- functional QA, visual/accessibility QA, code review, and security review after integration.

Keep work sequential when one stream depends on another's unresolved design, schema, public contract, shared-file changes, or migration outcome. Controlled parallelism is more valuable than maximum concurrency.

Parallelize only when all of these are true:

- inputs and shared contracts are stable enough for the work window;
- ownership is non-overlapping, or overlap has an explicit single writer and sequence;
- each stream has an independently testable output and useful stop condition;
- shared databases, environments, generated files, lockfiles, and external services will not be mutated concurrently in unsafe ways;
- integration owner, order, conflict strategy, and rerun checks are known;
- coordination cost is lower than the expected time saved.

If any condition is false, run discovery in parallel if safe, then synthesize and serialize implementation. Never parallelize production migrations, competing hotfixes, or edits to the same contract merely to keep agents busy.

## Workspace and worktree choice

Use separate branches/worktrees/workspaces when work is independently reviewable and mergeable with clear module ownership. Use one shared code state when streams are tightly coupled and must observe the same evolving implementation, but allow multiple agents there only for non-overlapping tasks or explicitly serialized turns. Do not assume shared-workspace edits are conflict-safe.

Before dispatch, record:

- branch/worktree and starting revision;
- owned outcome and file/module boundary;
- upstream contracts and dependencies;
- prohibited changes and local-only policy;
- expected evidence and handoff format;
- merge/integration owner and order.

Avoid concurrent edits to the same files. If overlap becomes necessary, pause one owner, integrate the upstream change, refresh the dependent context, and reassign explicitly.

## Deadlock and drift prevention

- Give one owner authority for each shared contract, schema area, lockfile, generated artifact, and release decision.
- Do not create circular dependencies between workers. When two streams need each other's unfinished decision, pause them, have the manager choose or prototype the smallest contract, then re-brief both.
- Time-box blocked discovery and surface the exact missing decision instead of polling indefinitely.
- When an upstream contract changes, stop affected downstream work, integrate the change, invalidate stale assumptions, and resend the contract before resuming.
- Cancel obsolete workstreams after a product or architecture decision; do not merge parallel implementations for convenience.

## Worker handoff

Store handoffs in the chosen ignored local context or pass them through the orchestration system without writing them into tracked files. A handoff should contain:

```text
Outcome:
Files changed:
Contract or behavior changed:
Checks run and results:
Unrun checks:
Assumptions and risks:
Integration steps:
Local-only safety status:
```

Do not include secrets, personal paths, unnecessary transcripts, or raw private manager notes. The integrator inspects the actual diff and reruns relevant checks.

## Integration checkpoints

Integrate at contract or vertical-slice boundaries, not after every keystroke. At each checkpoint:

1. confirm the branch still targets the agreed contract and base;
2. inspect the combined diff for overlap, drift, duplicate implementations, and unrelated changes;
3. run focused checks plus any cross-stream integration tests;
4. update dependencies and re-brief downstream workers;
5. run the local-context safety check before any commit or PR.

After all streams land, perform combined QA and independent review. Parallel reviews may find issues, but the manager owns deduplication, severity, fixes, reruns, and final sign-off.

## User visibility

Report meaningful transitions: plan ready, workstreams dispatched, contract changed, integration completed, gate failed, decision needed, preview ready, release completed, or production anomaly found. Do not bury the user in agent-by-agent narration. Maintain a compact private dashboard using [workflows.md](workflows.md).
