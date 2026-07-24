# Item 10 migration-readiness record

Status: leader analysis/planning and Phase A complete; final physical migration gate not authorized

## Purpose

Record the verified source baseline, current repository/package facts, complete ownership mapping, recommended recovery procedure, and decisions required before physical repository migration.

This record contains no credentials or authenticated registry/hosting results. It does not authorize repository or ref creation, remote changes, history filtering, file moves, publication, or extraction.

## Intended topology

- Private maintainer repository: `arcanemachine/inter-agent-meta`.
- Public ecosystem repository: `arcanemachine/inter-agent`.
- Public children: `arcanemachine/inter-agent-core`, `arcanemachine/inter-agent-pi`, and `arcanemachine/inter-agent-claude-code`.
- Private meta contains the ecosystem repository as the `ecosystem/` submodule.
- The ecosystem repository contains child submodules at `core/`, `extensions/pi/`, and `extensions/claude-code/`.

## Verified source baseline

### Tested candidate

- Branch: `master`.
- Exact tested commit: `2ae2a72ee375514914c7c5cdbddf311a40fcd363`.
- Version represented by the tested manifests: `0.1.0`.
- The tested commit had no tag.
- The worktree remained clean after validation.

Observed gates:

- full repository gate: 510 tests passed;
- Ruff: passed;
- Black: 76 files unchanged;
- mypy: 75 source files passed;
- Pi tests: 71 passed;
- Pi typecheck, build, and Prettier: passed;
- root Claude marketplace validation: passed;
- Claude plugin validation: passed;
- `git diff --check`: passed.

The pre-existing ignored `integrations/pi/dist/` directory was preserved, and the generated `integrations/pi/dist-tests/` directory was removed.

### Accepted Phase A source

- Exact passing source commit: `318fc77d62269fc1bf7b63370c6a8ed8f2e1aa03` on `master`.
- Its preceding commit `99eec4e` is the user-authorized pure Prettier remediation for the user-owned Pi command-parsing change; it corrected pre-existing formatting drift without changing behavior.
- All coordinated package, lock, marketplace, and plugin metadata is `0.2.0`; protocol `core.version` remains `0.1`, and the `spec/asyncapi.yaml` document version remains `0.1.0`.
- The complete Phase A gate ran against exact source commit `318fc77`; results are recorded below.
- The worktree was clean after validation. No source tag exists.
- `integrations/pi/dist/` was regenerated from the accepted source and retained; generated root artifact output and `integrations/pi/dist-tests/` were removed.

Approved direction: retain `0.1.0` as the unreleased historical baseline. The later `pre-split-0.2.0` tag targets the exact accepted current `master` commit containing Phase A source commit `318fc77` and this durable closeout, only after the separate physical go/no-go.

## Repository inventory

- Current remote names: `origin` only.
- Sanitized fetch/push destination: SSH transport to `github.com/arcanemachine/inter-agent.git`.
- Upstream: `master` tracks `origin/master`.
- The local `origin/HEAD` symbolic default-branch ref is unresolved.
- Local branches: `master`, `master--backup`, and `unstable`; re-inventory all refs at the final physical gate.
- Local tags: none.
- Submodules and `.gitmodules`: none.
- `git-filter-repo`: available locally.

### Recovery-only local branches

`unstable` has three commits not in `master` and otherwise trails it. Its unique changes affect an obsolete active-plan packet and Pi extension work.

`master--backup` has no unique commits and is fully contained in `master`.

Recommendation: preserve both branches in the full all-refs source backup. Do not propagate either into public child histories unless the user explicitly revives `unstable`; recheck their relation to `master` immediately before creating the recovery set.

## Package inventory

| Boundary | Current identity | Migration disposition |
| --- | --- | --- |
| Root Python distribution | `inter-agent` `0.2.0` | Rename clean core distribution to `inter-agent-core` |
| Root Pi Git-install facade | `inter-agent-pi-package` `0.2.0`, private | Retire after Pi extraction |
| Nested Pi package | `pi-inter-agent` `0.2.0` | Rename canonical child npm package to `inter-agent-pi` |
| Claude marketplace | `inter-agent` `0.2.0` | Move to standalone Claude repository and rewrite source path |
| Claude plugin | `inter-agent` `0.2.0` | Move with coordinated source metadata into the standalone Claude repository |
| Python lock | editable `inter-agent` `0.2.0` | Rebuild for core; generate independent child locks |
| Pi lock | `pi-inter-agent` `0.2.0` | Move with Pi and update identity |

No registry ownership or current name availability was inferred from local metadata.

## Complete current-path ownership manifest

All 235 tracked paths are classified. Counts describe their primary current-source disposition before shared documents are rewritten or copied deliberately.

| Disposition | Count |
| --- | ---: |
| Private meta | 67 |
| Deferred host planning in private meta | 2 |
| Core | 109 |
| Pi | 20 |
| Claude Code | 22 |
| Public ecosystem | 3 |
| Shared/derived public material | 7 |
| Retire or rebuild | 5 |

### Private meta

Move with relevant path history:

- `.agents/**`;
- root `AGENTS.md`;
- `ROADMAP.md` and `TODO.md`;
- `docs/plans/**` and `docs/archive/**`;
- `docs/IDEAS.md`, `docs/IDEAS.USER.md`, and `docs/ideas/**`;
- deferred `integrations/opencode/**` and `integrations/codex/**` planning placeholders.

Public repositories receive curated public contribution material rather than copied private workflow.

### Core

Move with path history:

- `src/inter_agent/core/**`;
- `src/inter_agent/__init__.py`;
- `spec/**`;
- `tests/conformance/**`;
- core tests under `tests/`;
- `pyproject.toml`, `uv.lock`, `MANIFEST.in`, and `run-checks.sh`;
- `scripts/validate-release-build.py`;
- `docs/SECURITY_BASELINE.md` and `docs/THREAT_MODEL.md`.

Core-test ownership includes:

- `tests/conftest.py` as the primary source of shared core fixtures;
- `tests/test_auth_helpers.py`;
- `tests/test_client_helpers.py`;
- `tests/test_config_resolution.py`;
- `tests/test_core_command_api.py`;
- `tests/test_error_codes.py`;
- `tests/test_filesystem_permissions.py`;
- `tests/test_server_lifecycle_state.py`;
- `tests/test_spec_validation.py`;
- `tests/test_status_semantics.py`;
- `tests/test_tls_transport.py`;
- `tests/test_token_management.py`.

Split `tests/test_console_entry_points.py`: generic command coverage stays with core; Pi-specific entry-point coverage moves to Pi.

#### Shared adapter-control bridge

Promote:

```text
src/inter_agent/adapters/control.py
→ src/inter_agent/core/adapter_control.py
```

Move `tests/test_adapter_control.py` with it and document the promoted module as a narrow, host-neutral extension-support API.

Rationale: Pi and Claude use identical local control semantics and security constraints. Keeping one core-owned implementation avoids duplicated security-sensitive runtime logic without retaining host adapter packages in core.

### Pi

Move and rename with path history:

```text
integrations/pi/src/index.ts
→ src/index.ts

integrations/pi/src/mailbox.ts
→ src/mailbox.ts

integrations/pi/tests/**
→ tests/typescript/**

src/inter_agent/adapters/pi/**
→ src/inter_agent_pi/**
```

Move Pi-specific Python tests:

- `tests/test_pi_adapter_cli.py`;
- `tests/test_pi_extension_static.py`;
- `tests/test_pi_listener.py`;
- `tests/integration/test_pi_adapter_live.py`;
- the Pi-specific portion of `tests/test_console_entry_points.py`.

Use these nested files as the canonical child-root inputs:

- `integrations/pi/package.json` → `package.json`;
- `integrations/pi/package-lock.json` → `package-lock.json`;
- `integrations/pi/README.md` → `README.md`;
- `integrations/pi/LICENSE.md` → `LICENSE.md`;
- `integrations/pi/tsconfig.json` and `tsconfig.test.json` at the child root.

Retire the monorepo root Pi facade rather than mapping two package manifests to the same child path.

### Claude Code

Move and rename with path history:

```text
integrations/claude-code/.claude-plugin/plugin.json
→ .claude-plugin/plugin.json

integrations/claude-code/skills/**
→ skills/**

src/inter_agent/adapters/claude/**
→ src/inter_agent_claude/**
```

Move root `.claude-plugin/marketplace.json` into the Claude child and update its source for the validated standalone layout.

Move Claude-specific tests:

- `tests/test_claude_adapter_cli.py`;
- `tests/test_claude_dedup.py`;
- `tests/test_claude_listener.py`;
- `tests/test_claude_plugin_static.py`;
- `tests/test_claude_skill_static.py`;
- `tests/test_claude_wrapper.py`;
- `tests/integration/test_claude_adapter_live.py`.

Use `integrations/claude-code/README.md` as the child README seed and add independent helper-package metadata and tests.

### Public ecosystem

Preserve and rewrite as installed/candidate cross-repository acceptance:

- `tests/integration/test_cross_adapter_pubsub_live.py`;
- `tests/integration/test_cross_adapter_tls_live.py`;
- coordinated compatibility/version validation derived from `tests/test_versioning_docs.py`.

Create clean ecosystem-level README, architecture overview, `COMPATIBILITY.md`, license, submodule layout, and thin orchestration. Do not copy runtime source.

Recommendation: create the ecosystem repository with clean collector history rather than converting monorepo history into collector history. Preserve the source monorepo separately and retain relevant history in the filtered children.

### Shared and derived public material

- `README.md`: ecosystem seed; split core, Pi, and Claude sections into their owning children.
- `ARCHITECTURE.md`: core is canonical; derive a thin ecosystem overview and host-specific child sections.
- `SECURITY.md`: core is canonical; derive host-specific security sections for Pi and Claude.
- `CHANGELOG.md`: split release notes and version policy into core, Pi, and Claude changelogs.
- `LICENSE.md`: copy to every public repository.
- `.gitignore` and `.tool-versions`: rebuild per repository rather than copying monorepo assumptions.

Shared documentation may appear in more than one filtered history only when it is intentionally rewritten into package-owned public documentation.

### Retire or rebuild

Do not migrate directly as runtime files:

- root `inter-agent` monorepo wrapper;
- root transitional `package.json` Pi facade;
- empty `src/inter_agent/adapters/__init__.py`;
- `integrations/pi/AGENTS.md` private agent guidance;
- `tests/test_inter_agent_wrapper.py` tied to the retired monorepo wrapper.

Any useful ecosystem orchestration or public contributor guidance is written afresh for the target boundary.

## Recovery and isolated-filtering strategy

### Terminology

An **isolated filtering clone** is a temporary local clone used only for `git filter-repo` and mapping experiments. It can be regenerated from the verified source backup; its results still require full review.

Accepted extracted repositories are retained. Filtering clones and recovery material are not deleted until the user confirms the migration is complete and recoverable.

### Preconditions

1. Phase A is complete with coordinated `0.2.0` metadata and the full documented gate.
2. Present the exact accepted current `master` commit and reviewed operations for the separate final physical go/no-go.
3. Begin the maintenance window and confirm all repository writers are stopped.
4. Reconfirm a clean worktree, unchanged accepted commit, branch inventory, and absence of the proposed tag.
5. Obtain explicit approval for local tag creation, recovery-set creation, reviewed GitHub operations, and physical migration.

### Source recovery set

After the final physical go/no-go, use a temporary namespaced migration workspace outside every repository checkout, retained through migration acceptance:

1. Create the approved annotated freeze tag at the exact accepted current `master` commit without pushing it.
2. Create a full local Git bundle containing all refs, including the freeze tag, `master--backup`, and `unstable`.
3. Verify the bundle with `git bundle verify` and record its SHA-256 digest privately.
4. Create a local mirror clone from the main checkout without contacting a remote.
5. Run `git fsck --full` and verify the expected branches and freeze tag in the mirror.
6. Record the source commit, tag, bundle digest, branch inventory, and verification results in maintainer migration notes.

The bundle is the portable immutable recovery artifact during migration. The mirror is the convenient source for repeatable local clones. Neither is intended to remain permanently under `/workspace/tmp/`; retain them until the archived monorepo and accepted target repositories provide verified durable recovery, then remove them only with user confirmation.

### Filtering workspace

Create separate local clones from the verified mirror for:

- private meta history;
- core history;
- Pi history;
- Claude Code history.

Create the public ecosystem as a clean collector repository after child histories are accepted.

For every filtered child:

1. start from the accepted freeze branch/ref rather than every source branch;
2. apply only the reviewed path and rename manifest;
3. run `git fsck --full`;
4. compare commit counts and representative history;
5. verify author identity and timestamps on representative files;
6. verify licenses and executable bits;
7. inspect the entire resulting file inventory for private or unrelated content;
8. run the child package gate before any push;
9. retain the clone until leader acceptance and remote verification.

Never run history filtering in the main checkout or the only recovery copy.

## Recommended decisions

| ID | Decision | Leader recommendation | Status |
| --- | --- | --- | --- |
| D1 | Mapping | Accept the ownership manifest above | Approved |
| D2 | Shared control bridge | Promote one host-neutral implementation into core | Approved |
| D3 | `unstable` branch | Preserve in recovery bundle only; do not extract | Approved |
| D4 | Ecosystem history | Create a clean collector repository | Approved |
| D5 | Current public remote | Preserve/archive the monorepo until all children and the ecosystem pass; do not rewrite it in place | Approved |
| D6 | New-repository default branch | Use `main`; retain source `master` only in the archived source history | Approved |
| D7 | Visibility | Private meta; public ecosystem and children, as already locked | Approved |
| D8 | Coordinated version | Keep `0.1.0` as an unreleased historical baseline; bump migration source and initial split generation to `0.2.0`, then version children independently | Approved |
| D9 | Freeze source | Latest accepted `HEAD` after the final full gate | Approved |
| D10 | Freeze ref and version | Use coordinated source version `0.2.0` and annotated freeze tag `pre-split-0.2.0` | Approved |
| D11 | Registry names and checks | Use locked names; user controls suitable PyPI/npm accounts and will handle availability/publication later; no agent registry contact | Approved and deferred |
| D12 | Physical migration | Begin only after all preceding gates and maintenance-window confirmation | Not authorized |
| D13 | GitHub owner and target repositories | Use `arcanemachine/inter-agent-meta`, `arcanemachine/inter-agent`, `arcanemachine/inter-agent-core`, `arcanemachine/inter-agent-pi`, and `arcanemachine/inter-agent-claude-code` | Approved |
| D14 | Monorepo archive name | Rename the current repository to `arcanemachine/inter-agent-monorepo` before creating the clean ecosystem repository at `arcanemachine/inter-agent` | Approved |
| D15 | GitHub operation responsibility | Leader may use an existing authenticated `gh` session for approved rename/create operations; user performs every push | Approved, later authorization still required |
| D16 | Branch protection | No agent action; user manages hosting policy | Approved and user-owned |
| D17 | Migration workspace | Use `/workspace/tmp/inter-agent-migration/` only for transitional bundles, mirrors, filtering clones, and build artifacts; nothing there is a permanent deliverable | Approved |
| D18 | Maintenance workflow | Stop repository writers, run the final gates, and create verified recovery artifacts before migration edits | Approved; exact window pending |
| D19 | Final physical gate | Require a separate final go/no-go before tag creation, GitHub operations, filtering, or pushing | Approved |

## Mandatory user gate

Before repository/ref creation, remote changes, registry contact, history filtering, directory moves, or publication, resolve and record:

- authorization for leader-performed GitHub rename/create operations;
- maintenance-window timing and stopped repository writers;
- confirmation of the exact accepted current `master` commit containing Phase A source commit `318fc77d62269fc1bf7b63370c6a8ed8f2e1aa03` and its durable closeout, plus the proposed local `pre-split-0.2.0` tag;
- authorization for physical migration.

## Approved version direction

1. Keep `0.1.0` as the unreleased historical and tested baseline; it does not need a release or source tag.
2. Bump all coordinated monorepo package, plugin, marketplace, lock, and changelog metadata to `0.2.0` in one source-change step.
3. Run the complete gate against the resulting current `HEAD`.
4. After the physical go/no-go, create annotated freeze tag `pre-split-0.2.0` at that exact passing commit.
5. Begin the split repository generation at coordinated `0.2.0`; child versions become independent afterward.

## Phase A exact change specification

Status: completed at `318fc77d62269fc1bf7b63370c6a8ed8f2e1aa03` after separate user authorization

Change only coordinated release metadata:

- `pyproject.toml`: `[project].version` → `0.2.0`;
- `uv.lock`: editable `inter-agent` package version → generated `0.2.0` value;
- root `package.json`: `version` → `0.2.0`;
- `integrations/pi/package.json`: `version` → `0.2.0`;
- `integrations/pi/package-lock.json`: only the two root-package version fields → `0.2.0`;
- `.claude-plugin/marketplace.json`: marketplace and contained plugin versions → `0.2.0`;
- `integrations/claude-code/.claude-plugin/plugin.json`: `version` → `0.2.0`;
- `CHANGELOG.md`: add a `0.2.0` split-generation entry while retaining `0.1.0` as the unreleased historical baseline.

Do not change protocol `core.version` `0.1` or the `spec/asyncapi.yaml` document version `0.1.0`; neither is a distribution version. Do not change dependencies or contact registries.

After the edit:

1. regenerate only the Python lock metadata with `UV_OFFLINE=1 uv lock`;
2. run the focused version-documentation test;
3. run `UV_OFFLINE=1 ./run-checks.sh`;
4. run Pi tests, typecheck, build, and Prettier check;
5. run strict root marketplace and Claude plugin validation;
6. build Python wheel/sdist offline and validate their version/content;
7. run Pi package dry-run inspection;
8. run `git diff --check` and confirm only the approved metadata files changed;
9. remove generated build outputs;
10. commit the passing metadata change as `chore: bump version to 0.2.0`.

No tag, remote, credential, registry, repository, history-filtering, or push action belonged to Phase A.

### Phase A result

- `318fc77` contains the eight approved coordinated metadata files; `99eec4e` separately corrected the pre-existing Pi formatting drift with user authorization.
- Focused version documentation: 3 passed.
- Full repository gate against `318fc77`: 510 tests passed; Ruff passed; Black left 76 files unchanged; mypy passed 75 source files.
- Pi gate against `318fc77`: 72 tests passed; typecheck, build, and Prettier passed.
- Strict root marketplace and Claude plugin validation passed.
- Offline Python wheel/sdist artifacts were built as `0.2.0` and passed content validation; Pi dry-run inspection reported `pi-inter-agent@0.2.0` with five expected package files.
- `git diff --check` passed and generated outputs were cleaned as recorded above.

## Planned execution runbook

Status: Phase A complete; all later phases remain planning-only and unauthorized

### Phase A — resolved and validated final source

Completed at `318fc77d62269fc1bf7b63370c6a8ed8f2e1aa03`; see the Phase A result above.

### Phase B — final physical go/no-go

Present the exact passing commit, proposed tags, GitHub operations, local recovery commands, workspace inventory, and writer-lock status. Obtain one explicit go/no-go covering only those reviewed actions.

No earlier planning approval substitutes for this gate.

### Phase C — source freeze and temporary recovery set

After the go/no-go:

1. Reconfirm that the maintenance window remains active and no executor or other session has written since the accepted current `master` commit.
2. Create the approved local annotated source tag without pushing it.
3. Create and verify an all-refs bundle under `/workspace/tmp/inter-agent-migration/`.
4. Record its SHA-256 digest in this maintainer migration record.
5. Create a local mirror from the checkout without contacting the remote.
6. Run `git fsck --full` and verify the mirror contains `master`, `master--backup`, `unstable`, and the approved freeze tag.
7. Create no child filtering clone until its roadmap item becomes active.

The bundle and mirror remain transitional recovery material through the extraction program. They are removed only after verified durable repositories exist and the user explicitly confirms cleanup.

### Phase D — approved GitHub topology transition

After separate credential-use authorization:

1. Use the existing authenticated `gh` session without displaying credential details.
2. Rename `arcanemachine/inter-agent` to `arcanemachine/inter-agent-monorepo`.
3. Create private `arcanemachine/inter-agent-meta` without pushing source.
4. Create public `arcanemachine/inter-agent` as the clean ecosystem placeholder without importing monorepo history.
5. Do not create child repositories early; each child repository is created with its extraction item.
6. Update the source checkout's `origin` to the approved archive URL only after the rename is verified.
7. The user performs every push, including the source tag and initial meta/ecosystem branches.
8. The leader verifies repository names and visibility without exposing credentials.

### Phase E — private meta scaffold

Using an isolated filtering clone from the verified local mirror:

1. Filter only the approved meta-owned paths from the accepted freeze branch.
2. Reorganize them into a maintainer-only repository with no runtime code, package artifacts, generated environments, secrets, or copied child histories.
3. Retain the active plan, role/workflow documents, accepted migration record, and internal planning history.
4. Add the clean public ecosystem repository at `ecosystem/` only after the user has pushed an initial ecosystem commit that can be pinned as a submodule.
5. Run `git fsck --full`, inspect the complete file inventory, and run `git diff --check`.
6. Prepare the local meta branch for the user's push; the leader does not push it.

### Phase F — checkpoint verification and closeout

1. Verify the archived monorepo remains complete and recoverable.
2. Verify private meta and public ecosystem visibility and canonical URLs.
3. Verify the meta repository contains no runtime source or secret material.
4. Verify the ecosystem placeholder contains no monorepo/private history.
5. Verify the reviewed path manifest remains usable for Pi, Claude, and core extraction.
6. Record user-performed pushes and observed remote results.
7. Close item 10 only when Pi extraction can begin without unresolved ownership, naming, recovery, or authorization questions.
8. Prepare the concrete Pi-extraction packet as roadmap item 11; do not begin it under item 10.

## Immediate next steps

1. Resolve the maintenance-window timing and stop repository writers.
2. Present the exact accepted current `master` commit containing Phase A source commit `318fc77` and this durable closeout, the proposed local `pre-split-0.2.0` tag, recovery commands, workspace inventory, writer-lock status, and approved GitHub operations for one final physical go/no-go.
3. Stop before any tag, credentialed action, remote operation, filtering, or push unless that gate is explicitly granted.
