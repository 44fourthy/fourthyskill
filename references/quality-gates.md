# Quality and Release Gates

Apply gates in proportion to the change, but do not mark a relevant gate PASS without evidence. Use `N/A — <reason>` only when the concern genuinely does not apply. A failed release-blocking gate stops merge or deploy until fixed or explicitly accepted by the authorized user with the risk recorded.

At each checkpoint declare `GO` or `STOP`:

- **Plan GO:** acceptance criteria, scope, risk class, material decisions, contracts, dependencies, and authorization boundary are clear enough for the next stage.
- **Build GO:** upstream contracts are stable, ownership is safe, and the next slice has a verification method. Otherwise serialize or resolve the decision.
- **Integration GO:** combined behavior and diff are coherent; contract drift, conflicts, and release-blocking failures are resolved.
- **Release GO:** required checks, local-only safety, migration/configuration order, recovery plan, release authority, and observation owner are present.
- **Closure GO:** post-deploy signals meet thresholds for the defined window, documentation is current, and residual risk plus follow-ups are explicit.

`STOP` is required for a failed blocking check, unsafe data or secret handling, unresolved material decision, contract conflict, unknown recovery path, or missing authority. An authorized risk acceptance may produce `GO WITH ACCEPTED RISK`; record the owner, reason, scope, expiry or follow-up, and evidence. Urgency alone is not a waiver.

## 1. Plan and scope

- Outcome, acceptance criteria, scope, non-goals, affected users, and risks are explicit.
- Repository evidence and exact commands are identified.
- Material product, schema, permission, destructive, cost, and rollout decisions are resolved.
- Implementation slices, dependencies, owners, integration order, and rollback/forward-fix are defined where relevant.

## 2. Experience and contract

- User flow and applicable loading, empty, success, validation, error, forbidden, stale, partial, and responsive states are covered.
- Accessibility, canonical components, design-system use, and data requirements are defined.
- Domain/data invariants and backend operations are explicit.
- Authentication, authorization, validation, errors, side effects, idempotency, and compatibility are handled.
- Prototype assumptions were reconciled; the architecture was not shaped blindly around mock data or component state.

## 3. Implementation and integration

- Diff is focused, understandable, and follows repository conventions.
- No unnecessary duplicate or parallel implementation remains.
- Old paths are removed or retained for a documented compatibility/rollout reason.
- Migrations, transactions, concurrency, cache invalidation, jobs, retries, and recovery are safe where applicable.
- API consumers, background jobs, queues, scheduled work, and webhooks preserve compatibility, idempotency, authentication, replay/duplicate handling, and operational recovery where applicable.
- Dependency and lockfile changes are intentional, scoped, reviewed for advisories/licensing/runtime compatibility, and do not hide unrelated upgrades.
- Combined work was inspected after all branches or agents integrated.

## 4. Automated and manual verification

Use repository-provided commands. Consider:

- focused unit tests for business rules and validation;
- integration tests for persistence, auth, permissions, contracts, transactions, retry, and idempotency;
- component and end-to-end coverage for critical flows and state matrices;
- Storybook or equivalent state coverage and reviewed visual diffs;
- accessibility and responsive checks;
- safe local migration generation/status/test;
- typecheck, lint, production build, and CI-equivalent checks;
- manual happy path plus meaningful failure and forbidden paths;
- performance, load, compatibility, and recovery checks when risk warrants them.

Fix failures caused by the work and rerun affected checks. Document verified pre-existing failures separately. `Not run` is not PASS.

When a repository lacks tests, CI, a design system, preview, or observability, follow the fallback in [workflows.md](workflows.md) and report the gap. Do not create PASS evidence from an absent capability.

## 5. Independent review and audit

- A fresh reviewer compares request, plan, design, contract, final diff, and evidence when available and authorized.
- Findings cite exact evidence, severity, impact, and remediation.
- In-scope findings are fixed and relevant checks rerun; deferrals have an owner and rationale.
- A self-review is labeled as such and does not masquerade as independent review.

Audit security and privacy proportionally: trust boundaries, access control, tenant isolation, input/output handling, secrets, logging, personal data, dependencies, injection, abuse/rate limits, webhooks, uploads, SSRF, session behavior, and operational recovery as applicable.

For environment configuration, inspect variable names, presence, scope, and wiring without revealing values. Confirm local, preview, staging, and production separation; secret scanning does not authorize printing or copying secrets.

## 6. Local-only context — release blocking

Run the complete check in [local-context.md](local-context.md) immediately before commit or PR and repeat before merge or deploy.

The report must contain exactly one current result:

```text
Local-only files checked: PASS
```

or

```text
Local-only files checked: FAIL — <reason>
```

PASS requires inspection of status, staged filenames, staged content, suspicious untracked files, effective ignores, tracked-file exceptions, and secrets/sensitive artifacts. FAIL blocks commit, PR, merge, and deploy.

## 7. Preview and release

- Preview environment, configuration, test data, routes, assets, callbacks, flags, and jobs behave as intended.
- Release order covers configuration, additive migrations, code, consumers/workflows, caches, removals, and compatibility windows.
- Feature flags have safe defaults, owner, targeting, kill switch, and cleanup conditions where used.
- Health checks and rollback or forward-fix triggers are explicit.
- Production changes and migrations have the necessary user authorization.
- PR or merge state is clean and current: intended files only, current target/base, required reviews/checks satisfied, no unresolved comments or conflicts, and no accidental generated, debug, local-only, or unrelated changes.
- Data releases have pre/post invariants, backfill progress/restart behavior, lock/runtime expectations, compatibility window, and tested recovery or roll-forward criteria proportional to risk.

## 8. Observe and close

- Relevant logs, error tracking, traces/metrics, alerts, analytics, performance, workflows/queues, and user-impact signals were checked or planned.
- Observation window, owner, thresholds, and response actions are clear.
- Production success is based on health evidence, not deployment status alone.
- Documentation and canonical paths are current; temporary debug code and unsafe fixtures are removed.
- API/schema/design/runbook documentation and private cached context agree with the shipped behavior, or stale artifacts are removed or explicitly marked.
- Remaining risks, unrun checks, deferred items, and follow-up owners are visible.

## Readiness report

```markdown
# Engineering Readiness

- Plan and scope: PASS/FAIL/N/A — evidence
- Experience and contract: PASS/FAIL/N/A — evidence
- Implementation and integration: PASS/FAIL/N/A — evidence
- Automated verification: PASS/FAIL/N/A — commands/results
- Manual/visual/accessibility QA: PASS/FAIL/N/A — evidence
- Independent review: PASS/FAIL/N/A — reviewer/findings
- Security/privacy audit: PASS/FAIL/N/A — evidence/residual risk
- Local-only files checked: PASS/FAIL — evidence or blocker
- Preview/release: PASS/FAIL/N/A — environment/evidence
- Observe/close: PASS/FAIL/N/A — signals/window
- Stage gate: GO / STOP / GO WITH ACCEPTED RISK — reason/owner

Decision: READY / NOT READY / READY WITH EXPLICITLY ACCEPTED RISK
Residual risk:
Unrun checks:
Required user decision:
```

Never replace this with “bug-free.” READY means the agreed gates have evidence and remaining risk is understood.
