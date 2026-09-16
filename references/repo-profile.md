# Repository Profile

Build a lightweight profile before planning substantial work. It adapts the universal operating system to the repository without forcing every project into the same stack or architecture.

## Two context layers

### Shared repo truth

Use existing tracked, project-owned evidence:

- product and architecture documentation;
- source code, schemas, migrations, APIs, services, jobs, tests, and fixtures;
- package scripts, CI, deployment configuration, environment templates, and operational runbooks;
- design tokens, shared components, accessibility patterns, visual tests, and approved design sources;
- tracked contributor or agent instructions that the project intentionally shares.

Shared repo truth is authoritative for how the project currently works, though contradictory or outdated evidence must be called out.

### Private operator context

Keep user-specific material local only:

- manager notes, current-work dashboards, cross-agent handoffs, local rules, and AI instructions;
- temporary decisions, hypotheses, scratchpads, personal checklists, and machine-specific workflow settings.

Follow [local-context.md](local-context.md) for location, exclusion, worktree handling, and pre-commit safety. Private context may summarize shared truth, but it does not silently override code, tracked documentation, or explicit user direction.

## Evidence and precedence

When sources conflict, use this order and report the conflict:

1. explicit current user requirements and authorized decisions;
2. current executable behavior, schema, tests, and deployment configuration;
3. applicable tracked project documentation and conventions;
4. private operator notes as navigation or temporary intent;
5. framework defaults and general best practice.

Do not treat aspirational docs, stale notes, or an early UI mockup as proof of current behavior.

## Profile fields

Capture only what affects the requested work:

```markdown
# Repository Profile

## Identity
- Product purpose and users:
- Change mode: new app / new feature / existing feature / redesign / refactor / fix / audit
- Change class: lightweight / standard / full / emergency — evidence:
- Active branch or worktree:

## Architecture
- Frameworks and versions:
- Entry points and module boundaries:
- Data model, persistence, and migrations:
- API/server-action/service patterns:
- Auth, authorization, tenancy, and audit:
- Jobs, queues, workflows, cache, webhooks, and flags:

## Experience
- Design system and canonical components:
- Layout, responsiveness, and accessibility:
- Forms, state, fetching, errors, and notifications:
- Approved design source and visual-test setup:

## Quality and delivery
- Focused test commands:
- Typecheck, lint, and build commands:
- CI requirements:
- Preview and deployment platform:
- Logs, errors, analytics, performance, and alerts:
- Capability gaps and fallback evidence:

## Local operator context
- Private context location:
- Effective local/global exclusion:
- Manager/handoff/status convention:
- Local-only safety status:

## Conflicts, gaps, and risks
- Verified conflict:
- Unknown that matters:
- Conservative assumption:
```

Keep the profile in memory for small work. If persistence is useful, store the operator version in the established ignored local context. Create or modify a tracked project document only when the user or team explicitly wants the profile shared.

## Refresh triggers

Refresh affected fields when the branch/worktree changes, dependencies or schema move, repository evidence contradicts the profile, another agent lands relevant changes, or release configuration changes. Never let a cached profile overrule the current repository.
