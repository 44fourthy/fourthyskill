# Optional Tooling Guidance

Tools support the repository's workflow; they do not define it. Prefer verified tools already used by the project or explicitly chosen by the user. Do not install, connect, purchase, or mutate an external service merely because it appears here. Keep a single source of truth for each kind of information and preserve authorization boundaries.

## Planning and source control

- **GitHub:** use existing issue, branch, pull-request, review, and required-check conventions. Keep PRs focused, inspect the final diff and checks, and never treat an open or merged PR as proof of a healthy release.
- **Linear:** use when the user or team already tracks work there. Link the outcome, acceptance criteria, decisions, dependencies, release state, and follow-ups without copying private manager notes or creating a second conflicting plan.

## Product and interface design

- **Figma:** treat the approved file, page, frame, and version as design evidence. Reconcile it with repository behavior, accessibility, and data constraints; do not assume a mockup is the backend contract.
- **v0 or other generators:** use for replaceable prototypes or implementation accelerators. Adapt generated code to the repository's components, tokens, architecture, security, and tests; do not introduce a parallel design system casually.
- **Storybook:** use for isolated component states and interaction coverage when present or when setup cost is justified by reusable UI complexity.
- **Chromatic or equivalent:** use reviewed visual diffs for meaningful surfaces. A clean screenshot diff does not replace functional, responsive, or accessibility QA.

## Production evidence

- **Sentry or equivalent:** use release-linked errors, traces, and performance evidence when configured. Avoid recording secrets or unnecessary personal data; verify sampling and environment separation.
- **PostHog or equivalent:** use analytics, session replay, experiments, or flags only with appropriate consent, privacy, retention, and access controls. Define event ownership and avoid sensitive payloads.
- **Vercel:** when verified, use the dedicated [vercel.md](vercel.md) lifecycle. Do not force Vercel onto a repository using another platform.

If a named tool is absent, use the repository's equivalent or report the capability gap. Do not add a service solely to satisfy this skill.
