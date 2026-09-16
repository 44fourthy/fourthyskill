# Engineering Workflows

Use the smallest workflow that covers the risk. Maintain one manager-owned plan that separates verified facts, proposals, assumptions, decisions, dependencies, and evidence.

## Change-size and risk classifier

Classify before planning. Raise the class whenever uncertainty or blast radius is higher than the visible diff suggests.

### Lightweight — trivial and low risk

Use when the change is localized, easily reversible, follows an established pattern, and does not affect data shape, trust boundaries, permissions, public contracts, jobs, deployment, dependencies, or shared architecture. Examples include copy, a contained style correction, or an obvious one-path bug with focused coverage.

Inspect the target and nearby convention, state a one- or two-line intent, implement the smallest change, run focused verification, inspect the diff, and report evidence. Do not create a dashboard, specialist team, or full design document. The local-only gate still applies before commit, PR, merge, or deploy.

### Standard — bounded feature or behavioral change

Use when behavior spans a few components or layers but follows known patterns and has limited blast radius. Profile the affected areas, define acceptance criteria and any interface contract, plan reviewable slices, run focused plus relevant suite-level checks, and perform defect-focused review.

### Full — high risk or broad impact

Use when work affects schema or data integrity, auth or permissions, payments, privacy, a public contract, shared architecture, infrastructure, major dependencies, destructive behavior, background processing, webhooks, multi-system integration, production configuration, large migrations, or many users. Also use it when rollback is hard or important facts remain uncertain. Apply the full lifecycle, explicit gates, independent review, proportional security/privacy audit, release sequencing, rollback or forward-fix, and observation.

### Emergency — production incident or hotfix

Contain first, preserve evidence, freeze conflicting work, and choose the smallest reversible mitigation. Urgency may shorten planning but does not waive authorization, data safety, secrets handling, the local-only gate, or verification of the affected path. After stability returns, add regression coverage and schedule root-cause and deferred hardening work.

Reclassify when new evidence changes risk. Do not downgrade merely to save time.

## Universal delivery path

### 1. Discover and audit

- Read the repository profile and relevant instructions.
- Trace similar behavior end to end: route or entry point, UI, contract, service/domain logic, data access, persistence, jobs, tests, deployment, and observation.
- Identify current conventions, duplicated or conflicting patterns, affected users, data and permission boundaries, and exact validation commands.
- Establish the private-context location and effective exclusion before writing manager notes or handoffs.

### 2. Plan

Define the outcome, acceptance criteria, scope and non-goals, current-state findings, likely files, dependencies, risks, migration/compatibility, rollout, observation, and definition of done. Label decisions that need the user.

The plan should be proportional, not ceremonial. It must still make the order of work and proof of completion clear.

### 3. Design the experience

Cover:

- actors, entry points, happy path, decisions, exits, recovery, and permissions;
- loading, empty, success, validation, error, forbidden, stale, partial, offline, and background-refresh states when relevant;
- canonical components, state ownership, server/client boundaries, responsiveness, accessibility, and copy;
- per-surface data, actions, validation, freshness, sorting, filtering, pagination, side effects, and analytics needs.

Figma, v0, Storybook, visual tests, or coded mockups may help, but generated shapes and mock data remain provisional. Keep them behind a replaceable boundary.

### 4. Define architecture and contracts

Derive business entities and invariants before persistence details. Specify fields, relationships, ownership, lifecycle, constraints, indexes, transactions, concurrency, retention, and audit requirements as applicable.

For every backend operation define caller, authentication, authorization, validated input, stable output, version/compatibility, domain errors, side effects, retry/idempotency, caching/invalidation, events, and background execution. For webhooks, also define signature verification, replay protection, raw-body handling, acknowledgement behavior, ordering, deduplication, and recovery. Reconcile this with the UI state matrix. Change the prototype when it conflicts with security, domain integrity, repository patterns, or efficient access.

Plan additive migrations, existing-row handling, constraints, indexes, lock/runtime impact, backfill batching and resumability, read/write compatibility, verification counts or invariants, compatibility windows, deployment order, and rollback or forward-fix. Prefer expand/migrate/contract for risky changes. Do not assume a destructive migration can be rolled back; define backup/restore or roll-forward evidence. Never apply a production migration incidentally.

### 5. Decompose and build

Create reviewable slices with an owner, inputs, outputs, dependencies, file or module boundaries, acceptance evidence, and integration order. A typical order is:

1. schema and safe migration;
2. data access and domain/service logic;
3. handlers/actions/API with validation, auth, and error mapping;
4. focused backend tests;
5. UI integration against the agreed contract;
6. state, accessibility, responsive, analytics, and visual coverage;
7. cleanup of superseded paths.

Follow the repository's natural order when it differs. Replan when evidence invalidates an assumption rather than improvising silently.

### 6. Integrate and verify

Review the combined diff, resolve contract drift, run focused checks after each slice, then run the applicable full quality gates. Test a meaningful failure and forbidden path in addition to the happy path. Separate new failures from verified pre-existing failures.

### 7. Review and audit

Perform a fresh defect-focused review against the request, plan, design, contract, diff, and test evidence. Use an independent reviewer when available and authorized. Review security, privacy, data integrity, accessibility, performance, operations, migration safety, release sequencing, and unintended scope.

### 8. Preview, deploy, observe, and close

Validate the integrated result in the real preview environment when relevant. Define or execute release and observation only within the user's authorization. Confirm cleanup, documentation, and residual risk. End with the quality report in [quality-gates.md](quality-gates.md).

## Missing project capabilities

- **No tests:** do not call the change verified. Use the best executable check and a documented manual reproduction; add the smallest maintainable regression or characterization test when it fits scope. Report the remaining gap.
- **No design system:** reuse the most consistent current pattern and record the local UI contract. Do not create a broad design system for a small change; propose one separately when recurring inconsistency justifies it.
- **No CI:** run discoverable local equivalents and report exact commands. Do not introduce a CI platform without scope and authorization.
- **No preview or observability:** validate in the safest available environment, use existing logs/health evidence, define a tighter manual observation plan, and report the limitation.
- **No migration or rollback tooling:** stop before risky data changes until the actual database path, recovery capability, and authorized operator are known.

Absence of infrastructure turns the corresponding gate into `NOT RUN` or `N/A — reason`, never an invented PASS.

## Existing application or repeated redesign

Before modifying an existing flow:

1. map the current page, canonical components, data sources, backend behavior, tests, routes, and consumers;
2. identify duplicates, deprecated paths, partially completed migrations, and conflicting visual patterns;
3. establish the approved design source and a UI contract for components, states, responsiveness, accessibility, and data;
4. classify each existing path as preserve, adapt, migrate, replace, deprecate, or remove;
5. implement migration and compatibility deliberately;
6. search for remaining references and verify that old and new paths are not both active accidentally.

Do not create parallel implementations merely to avoid understanding the current one. Retain an old path only for a documented compatibility or rollout reason, with an owner and removal condition.

## New application

Before scaffolding heavily, lock the minimum product slice, architecture boundaries, repository conventions, environment strategy, auth and data assumptions, test strategy, deployment platform, and observability baseline. Prove one thin end-to-end path before expanding the system.

## Fix or production incident

Prioritize containment and evidence. Identify impact, affected versions or tenants, recent changes, and current signals; reproduce only when safe. Choose among rollback, flag disablement, traffic isolation, configuration correction, or a minimal forward fix based on recovery time and data compatibility. Protect data, avoid concurrent speculative fixes, add regression coverage, verify affected and adjacent signals, and document root cause plus follow-up work. Do not broaden an urgent fix into an unrequested refactor or declare closure before the observation window is healthy.

## Dependency upgrade

Avoid blanket updates. Establish why the upgrade is needed, supported runtime range, direct and transitive changes, changelog or advisory evidence, peer/configuration/codemod requirements, lockfile impact, and rollback. Isolate major upgrades when practical, exercise affected build/runtime paths, inspect bundle or performance impact where relevant, and stage risky upgrades behind preview or progressive release. A security upgrade may be urgent, but it still needs compatibility and regression evidence.

## Secrets and environment configuration

Inspect names, presence, ownership, environment scope, and configuration wiring without printing secret values. Never copy production secrets into tests, previews, fixtures, logs, prompts, screenshots, or reports. Keep local `.env` variants untracked; a sanitized example file may be shared only when intentional and consistent with repository convention. Use the deployment platform's secret store where available, rotate exposed credentials, and verify least-privilege access and preview/production separation.

## Audit-only mode

Do not modify state. Report evidence-backed findings with severity, affected files or systems, user impact, exploitability or likelihood where relevant, and the smallest practical remediation. Distinguish observed defects from risks and unverified hypotheses.

## Manager status format

Keep the private status brief:

```text
Project — outcome
GREEN  Ready/reviewed item and evidence
YELLOW Active item, owner, next checkpoint
RED    Blocker, impact, decision or action needed
GRAY   Planned or intentionally deferred

Integration: <state>
Quality gates: <passed/failed/not run>
Release/production: <state>
Local-only files checked: <PASS/FAIL/not yet run>
```

Store this dashboard in private operator context. If the team needs a shared update, write a separate sanitized status message or project artifact containing only approved project facts; never publish the private dashboard itself.
