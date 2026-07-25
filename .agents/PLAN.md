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
3. the complete current-path ownership and target-layout mapping for private meta, core, Pi, Claude Code, and ecosystem material;
4. the recoverable backup/ref and clean-history repository-assembly strategy;
5. the accepted recommendations, operating responsibilities, and remaining authorization gates;
6. the exact Phase A coordinated `0.2.0` metadata-change boundary and validation procedure.

#### Current execution boundary

Phases A through D are complete. The accepted product source remains annotated tag `pre-split-0.2.0` at `c406276a7a64909f3de926f15d8a876a5ee34419`; the verified bundle and restored mirror remain under the secure transitional migration workspace. The current monorepo is now `arcanemachine/inter-agent-monorepo`; private `arcanemachine/inter-agent-meta` and public `arcanemachine/inter-agent` exist and are empty. The user-pushed archive branch and tag were verified.

The user approved clean history for every new repository. Pre-split provenance remains in the archived monorepo and recovery set; private meta, ecosystem, Pi, Claude Code, and core each begin with one curated initial commit on `main`. No `git filter-repo` will be used.

Item 10 remains leader-owned. The next separately gated step is Phase E: after reading the six preparation-critical files named in the migration record and verifying the final preparation commit is user-pushed, assemble a clean private-meta repository from that immutable archive snapshot at `/workspace/projects/inter-agent-meta`. Phase E uses the exact 69-path manifest, adds curated meta-root guidance, proves a single clean root commit, does not push, and defers the ecosystem submodule until an initial ecosystem `main` commit exists. Executor packets begin with substantive Pi extraction at roadmap item 11, not with item 10 checkpoint work. The user performs every push.

#### Completion standard

The leader verifies the tested freeze state, recovery path, approved repository visibility and URLs, private/public content boundary, and complete ownership mapping. Item 10 is complete only when item 11 can begin without unresolved naming, ownership, safety, or authorization questions.

#### Current authorization boundary

Local leader analysis, non-secret durable planning documentation, source freeze/recovery, and approved GitHub topology operations are complete. No executor dispatch, new local clean-repository assembly, credential use, registry contact, remote change, push, publication, or product extraction is currently authorized. Phase E requires a separate final clean-assembly go/no-go.

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
