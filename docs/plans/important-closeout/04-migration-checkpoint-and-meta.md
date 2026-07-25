# Migration checkpoint and private meta repository

Status: active leader-owned checkpoint; physical migration remains user-gated

## Goal

Freeze an accepted source state, confirm ownership and naming, create the private maintainer superproject, and establish a safe migration workspace before extracting public packages.

## Locked topology and names

- Private: `arcanemachine/inter-agent-meta`.
- Public ecosystem: `arcanemachine/inter-agent`.
- Public children: `arcanemachine/inter-agent-core`, `arcanemachine/inter-agent-pi`, `arcanemachine/inter-agent-claude-code`.
- Private meta contains the public ecosystem repository as a submodule at `ecosystem/`.
- Public ecosystem later contains real child submodules at `core/`, `extensions/pi/`, and `extensions/claude-code/`.

## Mandatory user/maintainer gate

Before any remote creation, history rewrite, directory move, or visibility change, confirm:

- Git hosting owner/organization and exact remote URLs;
- that `inter-agent-meta` is private and every product repository is public;
- authorization for the leader to rename/create repositories; the user performs every push;
- `main` as the new-repository default branch; the user manages branch-protection policy;
- user control of suitable package publisher accounts; registry availability is rechecked by the user at the later publication gate;
- replacement of the current public remote by a clean ecosystem repository after preserving it as `arcanemachine/inter-agent-monorepo`;
- a maintenance window in which no executor is modifying the monorepo.

Never request, display, copy, or commit credentials. If repository creation requires maintainer authentication unavailable in the container, provide exact commands for the maintainer and verify results afterward.

## Freeze procedure

1. Finish and commit all pre-migration behavior work.
2. Run the monorepo full gate plus Pi and Claude package validation.
3. Record the accepted source tag/version and clean commit in private migration notes, not public README instructions.
4. Confirm no attached/running agents are editing the repository.
5. Create a recoverable backup/ref before assembling any new repository.
6. Assemble every target in a separate newly initialized checkout; never rewrite or repurpose the source checkout.
7. Verify complete inventories, file modes, licenses, clean root commits, and the private/public boundary before pushing.

Git submodules necessarily store commit IDs as gitlinks. Do not copy those hashes into README install instructions.

## Private meta contents

Move or recreate maintainer-only coordination here:

- `.agents/`, active plan, role docs, task packets, and future internal execution state;
- maintainer-specific `AGENTS.md` and private workflow rules;
- internal roadmap/plans/decision records not intended as product documentation;
- migration orchestration notes and acceptance records;
- public ecosystem superproject as the `ecosystem/` submodule.

Do not put runtime code, package artifacts, secrets, registry tokens, generated environments, or duplicated child histories in meta.

Public repositories own stable product README/architecture/security/changelog/license material. If generic public contributor instructions are useful, use an intentionally public `CONTRIBUTING.md`; do not copy private agent workflow by default.

## Clean-history repository strategy

Start private meta, ecosystem, Pi, Claude Code, and core as independent repositories with clean history. The archived monorepo, freeze tag, verified bundle, and mirror retain the complete pre-split provenance; new repositories do not inherit monorepo commits.

For every new repository:

1. select the exact approved source commit and path manifest;
2. export only the approved current files into a new checkout with no inherited `.git` directory;
3. apply the reviewed path renames and intentionally rewrite shared material for the target boundary;
4. inspect the complete resulting inventory, permissions, and private/public boundary;
5. run the repository's applicable checks; and
6. create one curated initial commit on `main` for the user's push.

Pi receives `integrations/pi/**`, `src/inter_agent/adapters/pi/**`, Pi tests, Pi docs, and shared files intentionally copied or rebuilt at the child root. Claude receives its integration assets, adapter/helper source, tests, and intentional shared docs/license. Core receives protocol/runtime source, core/conformance tests, generic scripts/docs/package metadata, and excludes host adapters/assets.

Because files come from multiple current paths, retain and review the explicit mapping manifest before copying or renaming anything. Never improvise the target inventory, and never use `git filter-repo` for this split.

## Transition policy

- The split happens before initial registry publication, so no released monorepo package compatibility promise is required.
- Preserve user-facing CLI names where specified.
- Temporary local editable/path dependencies are allowed only during migration and must be removed before publication.
- Default endpoint, state, token, TLS, and protocol behavior must not change.
- Keep the current monorepo usable until all child extraction branches pass their focused checks.

## Acceptance criteria

- User gates are recorded without credentials.
- Private meta exists with correct visibility and public ecosystem placeholder/submodule strategy.
- A clean, tested freeze commit/ref exists.
- A reviewed path ownership/mapping manifest exists for all children.
- Clean repositories are assembled separately while the verified monorepo recovery set remains intact.
- No public artifact contains private workflow material.
- The next active item can extract Pi without unresolved ownership or naming decisions.

## Checks

Planning-only checkpoint changes require `git diff --check`. Any scripts added for mapping/copy verification require focused tests and the applicable repository gate. Remote visibility and submodule URL verification must be observed from Git, not assumed from documentation.
