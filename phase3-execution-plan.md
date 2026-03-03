# IPA Refactor — Branching Strategy, Execution Plan & Recovery Protocol

## Context

We're starting Phase 3 of the IPA refactor: actual code changes to encapsulate ~546 globals into context structs across ~62 files. This is a large, multi-session effort. Every step must be logged durably so any Claude session can pick up where the last one left off.

### Current State (verified 2026-03-02)
- **All 10 build-fix PRs are merged to `origin/develop`** (#45-#57)
- `build/macos-test-v2` is now obsolete — develop has everything
- Build script: `/Users/kevinliu/dev/open64/build-open64-docker.sh`
- Audit/design docs: `noafroboy/open64-ipa-refactor` (separate GitHub repo)
- Design decisions resolved in `ipa-audit/design.md`

---

## 1. Branching Strategy

### Branch Layout
```
origin/develop          (all build fixes merged as of 2026-03-02)
  └── refactor/ipa-context  (NEW — all Phase 3 IPA refactor work)
```

### Why This Is Simple Now
- `origin/develop` already has all the build fixes — no integration branch needed
- Branch directly off develop, PR directly back to develop
- Each cluster migration can be its own PR if desired
- `build/macos-test-v2` can be archived/deleted

### PR Strategy
- Can PR directly against `origin/develop` — no dependencies to wait for
- Option A: One large PR with atomic commits per cluster
- Option B: Multiple small PRs, one per cluster (cleaner for review)
- Decision: defer until we see how the first few steps go

---

## 2. Commit Plan (one per migration step)

Each commit must be **build-green** (verified via Docker build).

| Step | Commit Message | Phase | Files |
|------|---------------|-------|-------|
| 0 | Create branch `refactor/ipa-context` | Setup | 0 |
| 1 | Add IPA_Options, IPA_Context, sub-struct header files | 3a | 3 new |
| 2 | Migrate feedback FD globals to IPA_Context | 3b.1 | 1-2 |
| 3 | Migrate struct opt globals to IPA_Context | 3b.2 | 2 |
| 4 | Migrate visualization globals to IPA_Context | 3b.3 | 3 |
| 5 | Migrate array section globals to IPA_Context | 3b.4 | 2 |
| 6 | Migrate field reorder globals to IPA_Context | 3b.5 | 3 |
| 7 | Migrate devirt/CHG globals to IPA_Context | 3b.6 | 4 |
| 8 | Migrate common/padding globals to IPA_Context | 3b.7 | 3 |
| 9 | Migrate cprop state globals to IPA_Context | 3c.1 | 3 |
| 10 | Migrate inline stats globals to IPA_Context | 3c.2 | 5+ |
| 11 | Migrate symtab merge globals to IPA_Context | 3c.3 | 2 |
| 12 | Add ipa_options_compat.h macro shim | 3c.4 | 1 new |
| 13 | Migrate ~140 option flags via macro shim | 3c.4 | 32+ |
| 14 | Migrate Summary/Local globals (IPL_Context) | 3d.1 | 14 |
| 15 | Migrate alias class globals (crosses be/com) | 3d.2 | 6 |
| 16 | Migrate call graph + node context globals | 3d.3 | 21 |
| 17 | Thread IPA_Context* through entry points, remove g_ipa_ctx shim | 3e | entry points |
| 18 | Remove ipa_options_compat.h macro shim | 3e | 32+ |

---

## 3. Recovery Protocol — Surviving Compaction & Session Boundaries

### The Problem
Each step touches many files. Context compaction or session restarts lose the working state. A new session must be able to:
1. Know what step we're on
2. Know what's been done
3. Know what's left
4. Have enough detail to continue without re-reading everything

### The Solution: Three-Layer Durability

**Layer 1: Git commits** (most durable)
- Every completed step is a commit on `refactor/ipa-context`
- `git log --oneline` shows exactly what's done
- Pushed to `fork` after each step for remote backup

**Layer 2: Progress log file** (`migration-progress.md`)
- Updated after every step, committed with the step or separately
- Format:

```markdown
# IPA Refactor Migration Progress

## Current State
- Branch: refactor/ipa-context
- Last completed step: N (description)
- Last verified build: YES/NO (commit hash)
- Next step: N+1 (description)

## Step Log
| Step | Status | Commit | Build | Notes |
|------|--------|--------|-------|-------|
| 0 | DONE | abc1234 | N/A | Branch created |
| 1 | DONE | def5678 | PASS | Headers created |
| ... | ... | ... | ... | ... |
```

**Layer 3: Per-step instructions** (in `migration-steps/`)
- Before starting each step, write a `step-NN.md` file with:
  - Exact files to modify
  - Exact globals to move
  - Exact search/replace patterns
  - Expected build result
- If a session dies mid-step, the next session reads the instruction file and either completes or reverts

### Session Start Protocol

Every new Claude session working on this refactor MUST:

1. Read `PLAN.md` (master plan)
2. Read `migration-progress.md` (where are we?)
3. Check `git log --oneline refactor/ipa-context -5` (verify git matches log)
4. Check `git status` (any uncommitted work?)
5. If uncommitted work exists, read `migration-steps/step-NN.md` for the in-progress step
6. Resume or start the next step

### Session End Protocol

Before ending any session:

1. If mid-step: commit WIP or stash, note in `migration-progress.md`
2. If step complete: commit, update `migration-progress.md`, push to fork
3. Always push to fork — never leave work only local

---

## 4. Execution Workflow (per step)

```
1. Read migration-progress.md → identify next step N
2. Read migration-steps/step-N.md (or write it if doesn't exist)
3. Make the code changes
4. Build: ./build-open64-docker.sh
5. If build fails → fix → rebuild
6. Commit with descriptive message
7. Update migration-progress.md (status, commit hash, build result)
8. Commit the progress update
9. Push to fork
10. Proceed to step N+1
```

---

## 5. What Lives Where

| Content | Location |
|---------|----------|
| Audit, design, progress logs, step instructions | `noafroboy/open64-ipa-refactor` repo |
| Actual compiler code changes | `refactor/ipa-context` branch on `noafroboy/open64` fork |
| Per-step instructions | `migration-steps/step-NN.md` |
| Build verification | Docker build via `build-open64-docker.sh` |
| Session recovery | Read `migration-progress.md` → `git log` → resume |

---

## 6. First Actions

1. `git checkout -b refactor/ipa-context origin/develop`
2. Create `migration-progress.md` with initial state
3. Create `migration-steps/` directory
4. Write `step-01.md` (Phase 3a: create header files)
5. Execute step 1
6. Build, verify, commit, push
