# Local-Only Context and Rules

Apply this policy whenever creating, reading, moving, ignoring, staging, committing, merging, or packaging operator-specific files. Its purpose is to keep personal AI orchestration out of repository history and review surfaces.

## Hard boundary

Treat the following as **private operator context** by default:

- AI orchestration files and per-repository personal workflow configuration;
- local rules, manager notes, agent instructions, handoffs, status dashboards, decision scratchpads, temporary plans, and current-work trackers;
- prompts, transcripts, generated review notes, experiments, scratch files, and temporary artifacts;
- secrets, credentials, tokens, private keys, local environment files, personal tool configuration, and machine-specific paths.

Never add, stage, commit, push, attach to a PR, or include in a release artifact any such file. A general request to commit the feature does not change this boundary. If information from private context should become shared, extract and rewrite only the approved, sanitized decision into a deliberate team-owned document; do not commit the private source file.

Tracked project documentation, code, tests, checked-in configuration, and team-owned agent instructions are **shared repo truth**. Do not reclassify or hide an existing tracked file merely because it contains instructions. Assess ownership and tracking status first.

## Location selection

Respect an established, verified repository convention for private context. Otherwise prefer an obvious local-only directory such as:

```text
.context/
.ai-local/
.local-rules/
```

Choose one canonical location rather than creating several competing folders. Use names within it that reveal purpose, for example `CURRENT-WORK.md`, `MANAGER.md`, `HANDOFFS/`, or `scratch/`. Do not create these files until they are useful.

Private context may instead live outside the repository when it must span many worktrees or repositories. Record only a non-sensitive locator in private configuration; do not add personal absolute paths to tracked project files.

## Exclusion strategy

### Repository-specific files

Prefer the repository's local Git exclude file, resolved with:

```bash
git rev-parse --git-path info/exclude
```

Add the narrow directory or filename pattern there, such as `/.context/`. This keeps the rule local and leaves the tracked `.gitignore` unchanged. Preserve existing entries and comments. Confirm the path is actually ignored and remains untracked.

Do **not** silently edit `.gitignore` to hide personal AI or workflow files. Change the tracked `.gitignore` only when the user or team explicitly wants a shared repository policy, then review that change like any other project change.

### Universal personal patterns

For the same personal-only names across many repositories, optionally use a global excludes file configured through Git's `core.excludesFile`. First inspect the existing setting and file. Propose or configure it only when the user wants machine-wide behavior; preserve unrelated patterns and avoid overwriting an existing file. Repository-specific exclusions should remain in the repository-local exclude file.

Global or local ignore rules are guardrails, not proof of safety. They do not affect files already tracked and do not replace the staged-content check.

## Files that are already tracked

Ignore rules cannot hide a tracked file. If a tracked project file mixes shared defaults with personal settings:

1. prefer a supported, ignored override such as `config.local.*`, `.env.local`, or a private operator file;
2. keep the tracked base safe and shareable;
3. update application loading or documentation only if that repository change is part of the authorized scope;
4. do not automatically remove a team-owned file from tracking merely to make it private.

Do not use `skip-worktree` or `assume-unchanged` as the default. They can hide local differences, confuse pulls and merges, and leave agents with inconsistent state. Use either only deliberately for an understood edge case, document what was set, how to inspect or undo it, and the risk it creates.

If a secret or sensitive private file was already committed, ignoring it is insufficient. Stop, report the exposure, avoid repeating the value, and propose credential rotation plus history remediation appropriate to the repository and the user's authorization.

## Worktrees, workspaces, and agent handoffs

- Preserve the shared/private boundary in every Conductor workspace, Git worktree, branch, and agent session.
- Do not assume an untracked file exists in another worktree or that every workspace resolves excludes identically; verify before creating context there.
- Store manager notes and handoffs in the chosen ignored path or an external private location. Pass the minimum task facts to workers and never copy private context into source files, commit messages, PR bodies, fixtures, logs, or generated artifacts.
- Worker branches must contain implementation and project-owned documentation only. The integrator repeats the safety check after combining branches because a clean worker branch does not prove a clean integration branch.
- If a local context file contains a decision that belongs to the team, extract and rewrite only the approved decision into an appropriate tracked document; do not commit the private notebook wholesale.

## Mandatory local-context safety check

Run this check immediately before every commit or PR and repeat it before merge or deploy. Inspect both tracked/staged changes and suspicious untracked files.

1. Establish the repository root and active worktree/branch.
2. Inspect `git status --short` and the full staged filename list.
3. Inspect the staged diff and summary, including new, renamed, and binary files.
4. Look for private-context names and content: local rules, manager or agent notes, handoffs, dashboards, prompts, transcripts, scratch/temp artifacts, personal configuration, absolute local paths, `.env*`, credentials, tokens, keys, generated archives, and unexpected large files.
5. Verify intended local-only paths are untracked and ignored using the effective ignore source. Remember that tracked files can appear unchanged by ignore rules.
6. Run the repository's secret or sensitive-data scan when available. Perform a focused manual review even when automation passes.
7. Remove accidental files from the staged set without deleting the user's local copy, fix the exclusion safely, then reinspect the complete staged diff.

The gate passes only when every staged file is intentional project content, no sensitive or private operator material is present, and exclusions are effective for the relevant untracked paths.

Record exactly one of:

```text
Local-only files checked: PASS
```

```text
Local-only files checked: FAIL — <files or unresolved risk>
```

A FAIL blocks commit, PR, merge, and deploy. If the check could not be run, report FAIL or NOT RUN in working notes; never convert missing evidence into PASS.
