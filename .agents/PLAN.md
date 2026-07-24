# Active Plan

`.agents/PLAN.md` is for short-term work that is actively being done or ready to be done next in the current session. It is intentionally small.

Use [`../ROADMAP.md`](../ROADMAP.md) for accepted medium- and long-term direction. Use [`../docs/plans/`](../docs/plans/) for future execution notes. Use [`../docs/IDEAS.md`](../docs/IDEAS.md) for exploratory or unaccepted ideas.

Detailed active task packets live in `.agents/plans/<plan-name>/`.

## Current active work

### Closeout item 10 — migration checkpoint and private meta repository

#### Purpose

Establish a safe, explicitly approved starting point for splitting the monorepo. This checkpoint prepares the migration; it does not itself authorize repository extraction, publication, credential use, or remote changes.

The intended topology remains:

- private maintainer repository `arcanemachine/inter-agent-meta`;
- public ecosystem repository `arcanemachine/inter-agent`;
- public child repositories `arcanemachine/inter-agent-core`, `arcanemachine/inter-agent-pi`, and `arcanemachine/inter-agent-claude-code`.

#### Durable working record

Verified facts, the ownership manifest, recovery design, recommendations, and pending decisions are maintained in [`migration/10-migration-readiness.md`](migration/10-migration-readiness.md). Keep that record non-secret and update it as decisions are approved.

#### Leader-owned migration-readiness analysis

The leader completed and durably recorded:

1. the tested source baseline and full monorepo, Pi, and Claude validation results;
2. current package, repository, branch, and safe remote facts;
3. the complete path and history-ownership mapping for private meta, core, Pi, Claude Code, and ecosystem material;
4. the recoverable backup/ref and isolated-filtering strategy;
5. the accepted recommendations, operating responsibilities, and remaining authorization gates;
6. the exact Phase A coordinated `0.2.0` metadata-change boundary and validation procedure.

#### Current execution boundary

Phase A is complete at source commit `318fc77d62269fc1bf7b63370c6a8ed8f2e1aa03`: coordinated package/plugin/marketplace/lock/changelog metadata is `0.2.0`, and the documented focused/package/full gates passed. The preceding user-authorized formatting commit `99eec4e` corrected pre-existing Pi Prettier drift; it is part of the accepted source state. Phase A created no tag and performed no migration work.

Item 10 remains leader-owned. The next step is the separate Phase B final physical go/no-go. Only after later explicit physical authorization does the leader perform its recovery, GitHub-topology, and private-meta checkpoint work directly. Executor packets begin with substantive extraction work at roadmap item 11, not with item 10 decision or checkpoint work. The user performs every push.

#### Completion standard

The leader verifies the tested freeze state, recovery path, approved repository visibility and URLs, private/public content boundary, and complete ownership mapping. Item 10 is complete only when item 11 can begin without unresolved naming, ownership, safety, or authorization questions.

#### Current authorization boundary

Local leader analysis, non-secret durable planning documentation, and Phase A are complete. No executor dispatch, repository or ref creation, credential use, registry contact, remote change, history rewrite, file move, publication, or extraction is currently authorized. Phase B requires a separate final physical go/no-go.

Detailed accepted requirements remain in [`../docs/plans/important-closeout/04-migration-checkpoint-and-meta.md`](../docs/plans/important-closeout/04-migration-checkpoint-and-meta.md). Continuity is tracked in [`../ROADMAP.md`](../ROADMAP.md#closeout-execution-queue).

## Planning workflow

1. When no active work or user task is selected, follow the next concrete accepted activation step in `ROADMAP.md`; do not start from exploratory ideas or ask the user to choose among unrelated roadmap directions.
2. Keep `README.md` focused on present, implemented behavior.
3. Keep prospective or not-yet-implemented work out of the supported integration list.
4. Track accepted medium- and long-term direction in `ROADMAP.md`.
5. Track future execution notes in `docs/plans/**` when they are more detailed than `ROADMAP.md` but not active enough for `.agents/PLAN.md`.
6. Track rough or exploratory ideas in `docs/IDEAS.md` until the user accepts them for the roadmap or active plan.
7. When a roadmap item becomes active, copy only the next concrete slice into `.agents/PLAN.md`.
8. When an active item is completed, remove it from this file and update product docs only for behavior that now exists.

## Completion standard

Before handing back completed code or checked documentation work, run the relevant checks for the changed area. For normal code changes, use the repository gate:

```bash
./run-checks.sh
```

For documentation-only wording or planning changes that do not touch generated or checked artifacts, `git diff --check` is sufficient unless the user asks for the full gate.
