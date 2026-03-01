# IPA Refactor Audit — Master Plan

## Goal
Encapsulate all IPA global state into a context struct so multiple IPA instances can run concurrently (Goal 5 from MEMORY.md).

## Strategy
Coordinator (main context) + Scout subagents + Disk files as shared memory.
- Subagents explore narrow scopes, report summaries
- All findings written to files in this directory
- Main context stays lean, reads only summary files

## Phases

### Phase 1: Exploration & Audit
- [x] Phase 1a: Structure Map — directory tree, file counts, categorization → `structure.md` ✓
- [x] Phase 1b: Global State Inventory — `globals.md` COMPLETE. ~546 globals found, 10 refactoring clusters identified. Details in `phase1b/*.md`
- [x] Phase 1c: Entry Points & Data Flow — `entry-points.md` COMPLETE. 5 build artifacts, 2-phase architecture, 24 analysis sub-phases mapped
- [x] Phase 1d: Dependency Graph — `dependencies.md` COMPLETE. 98 globals traced, 15 clusters, 436 co-occurrence pairs

### Phase 2: Design
- [x] Design `IPA_Context` struct grouping related globals → `design.md` DRAFT
- [x] Define API boundaries and threading strategy → in `design.md`
- [ ] Review open questions in `design.md` with user
- [ ] Investigate MEM_POOL internals (prerequisite for Phase 3c)

### Phase 3: Incremental Migration
- [ ] Convert globals one subsystem at a time, keeping build green
- [ ] Track per-subsystem progress → `migration-progress.md`

### Phase 4: Validation
- [ ] Prove two IPA instances can run without interference

## Files in This Directory
| File | Purpose | Status |
|------|---------|--------|
| `PLAN.md` | This file — master plan and progress | Active |
| `structure.md` | Directory tree, file inventory | Pending |
| `globals.md` | All global/extern/static variables | COMPLETE |
| `phase1b/*.md` | Per-directory detailed scan results (6 files) | COMPLETE |
| `entry-points.md` | IPA entry functions and call flow | COMPLETE |
| `dependencies.md` | Global state dependency clusters (661 lines) | COMPLETE |
| `design.md` | Context struct design (~300 lines) | DRAFT |
| `migration-progress.md` | Per-subsystem migration tracking | Pending |
