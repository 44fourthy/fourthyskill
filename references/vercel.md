# Vercel Delivery Lifecycle

Use this reference only when Vercel is verified as the deployment platform or the user requests it. Inspect the repository and current platform capabilities before assuming a feature is available. The lifecycle is **Preview → Deploy → Observe**; planning it does not authorize a production change.

## Preview

- Validate build and runtime behavior with non-production environment variables and safe test data.
- Exercise complete flows, auth and permission states, routes, assets/fonts, responsive layout, accessibility, errors, third-party callbacks, and environment-dependent behavior.
- Combine component-level Storybook/visual review with integrated Preview testing when those systems exist.
- Confirm migrations, flags, cron triggers, Workflows, Queues, webhooks, and external services cannot affect production unintentionally.
- Record the deployment identifier or URL and concrete evidence without exposing sensitive data.

## Deploy

- Order configuration, additive migrations, application code, consumers or workflows, cache changes, backfills, removals, and contract deprecations safely.
- Prefer backward-compatible changes during staggered rollout.
- Use flags when separating deployment from release, progressive rollout, internal targeting, experiments, or a kill switch reduces risk. Define server-side enforcement where the flag protects access or behavior, plus default, owner, targeting, monitoring, and cleanup.
- Confirm health checks, compatibility window, migration owner, rollback/forward-fix plan, and release authorization.

## Workflows and Queues

Choose the simplest existing mechanism. Consider durable workflows for resumable multi-step work and queues for decoupled message processing only when they fit the verified platform and requirement.

Define trigger, payload and versioning, authentication, idempotency, concurrency, retries/backoff, timeout, ordering, deduplication, dead-letter or poison-message handling, observability, operator recovery, and compatibility with in-flight executions. Do not introduce platform machinery for work that is safer inline.

## Observe

Inspect relevant runtime/build logs, function errors, error tracking, traces/metrics, alerts, traffic, usage, and cost. Where enabled and privacy-compatible, use real-user performance and web analytics to evaluate affected pages. Check workflow/queue failures, retry rates, latency, backlog, stuck runs, and recovery.

Define an observation window and thresholds that trigger rollback, flag disablement, or follow-up. Compare to a useful baseline when available. A successful deployment is not evidence of a healthy feature.

## Close

Record production evidence, anomalies, remediation, flag cleanup date, migration or compatibility cleanup, and remaining risk. Re-run the local-only gate before deploy artifacts or release commits are finalized.
