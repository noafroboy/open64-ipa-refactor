# IPA Global State Inventory

## Status: PHASE 1b COMPLETE — full .cxx scan across all subdirectories

Detailed per-directory scan results in `phase1b/*.md`.

---

## Grand Totals

| Directory | File-scope globals | static | non-static | extern refs | MEM_POOLs |
|-----------|-------------------|--------|------------|-------------|-----------|
| ipa/common/ | 73 | 49 | 19 | 10 | 4 |
| ipa/local/ | 61 | 17 | — | 9 | 4 |
| ipa/main/analyze/ | ~140 | ~72 | ~62 | ~10 | 12 |
| ipa/main/optimize/ | 89 | 56 | 27 | 20 | 5 |
| ipa/inline/ + cord/ | 35 | 16 | 14 | 5 | 2 |
| be/com/ + common/com/ | ~148 | 6 | ~140 | — | — |
| **TOTAL** | **~546** | **~216** | **~262** | **~54** | **~27** |

**~546 mutable file-scope globals** across the IPA subsystem.

~140 of those are in `config_ipa.cxx` alone (command-line option flags).

---

## Worst Offender Files (by global count)

| File | Globals | Key State |
|------|---------|-----------|
| `config_ipa.cxx` | ~140 | ALL `-IPA:` and `-INLINE:` command-line flags |
| `ipa/main/optimize/ipo_struct_opt.cxx` | 34 | Struct relayout + array remapping clusters |
| `ipa/main/analyze/ipa_cg.cxx` | 30 | `IPA_Call_Graph`, stats counters, phi state |
| `ipa/local/ipl_main.cxx` | 25 | `Summary` pointer, WN_MAPs, option tables |
| `ipa/inline/inline.cxx` | 25 | MEM_POOLs, `Summary`, inline counters, timers |
| `ipa/main/optimize/ipo_main.cxx` | 23 | Register feedback, global-as-local arrays |
| `ipa/main/analyze/ipa_cprop_annot.cxx` | 22 | Statics used as implicit params in recursion |
| `ipa/common/ipc_symtab_merge.cxx` | 16 | Symbol table merging state |
| `ipa/main/analyze/ipaa.cxx` | 16 | Alias analysis pools, trace flags, hash tables |
| `ipa/main/analyze/ipa_inline.cxx` | 14 | Inlining statistics, trace files |
| `ipa/common/ipc_ty_hash.cxx` | 14 | Type hash tables for merging |
| `ipa/common/ipc_compile.cxx` | 14 | Post-IPA compilation orchestration |

---

## Natural Refactoring Clusters

These are groups of globals that belong together and could be encapsulated into a single context struct.

### 1. IPA_Config (~140 vars → 1 struct)
**Source**: `config_ipa.cxx`
**Contents**: All `-IPA:` and `-INLINE:` command-line option flags (`IPA_Enable_DFE`, `IPA_Enable_Inline`, `IPA_Enable_Cprop`, `IPA_Bloat_Factor`, `INLINE_Enable`, `INLINE_All`, etc.)
**Impact**: HIGHEST — single highest-impact refactoring target. One struct replaces ~140 globals.
**Difficulty**: MEDIUM — widely read but rarely written after init.

### 2. IPA_CallGraphState (~30 vars → 1 struct)
**Source**: `ipa_cg.cxx`, `ipa_inline.cxx`
**Contents**: `IPA_Call_Graph`, `IPA_Graph_Undirected`, `emit_order`, `IPA_Class_Hierarchy`, `IPA_Concurrency_Graph`, depth/stats counters
**Impact**: HIGH — the central data structure
**Difficulty**: HIGH — referenced everywhere

### 3. MergeContext (~30 vars → 1 struct)
**Source**: `ipc_ty_hash.cxx`, `ipc_type_merge.cxx`, `ipc_symtab_merge.cxx`
**Contents**: Type/symbol hash tables, merge pools, recursion guards
**Impact**: MEDIUM
**Difficulty**: MEDIUM — localized to merge phase

### 4. CompilationContext (~14 vars → 1 struct)
**Source**: `ipc_compile.cxx`
**Contents**: Makefile paths, infiles/outfiles, toolchain state
**Impact**: LOW-MEDIUM
**Difficulty**: LOW — file-local state

### 5. LocalSummaryState (~40 vars → 1 struct)
**Source**: `ipl_main.cxx`, `ipl_summarize_util.cxx`, `ipl_linex.cxx`
**Contents**: `Summary`, `Array_Summary`, WN_MAPs, chi/phi hash maps, entry cache
**Impact**: MEDIUM
**Difficulty**: MEDIUM

### 6. AliasAnalysisState (~16 vars → 1 struct)
**Source**: `ipaa.cxx`
**Contents**: Alias analysis pools, trace flags, hash tables, `icall_eref/def/kill`
**Impact**: MEDIUM
**Difficulty**: MEDIUM — localized to IPAA

### 7. StructOptState (~34 vars → 2 structs)
**Source**: `ipo_struct_opt.cxx`
**Contents**: `complete_struct_relayout_*` cluster (10 vars), `array_remapping_*` cluster (13 vars)
**Impact**: LOW-MEDIUM
**Difficulty**: LOW — highly localized, two clear clusters

### 8. InlineState (~25 vars → 1 struct)
**Source**: `inline.cxx`
**Contents**: MEM_POOLs, `Summary`, PU tracking, inline counters, timing arrays
**Impact**: MEDIUM
**Difficulty**: MEDIUM

### 9. ConstPropState (~22 vars → 1 struct)
**Source**: `ipa_cprop_annot.cxx`, `ipa_cprop.cxx`
**Contents**: Statics used as implicit parameters in deep recursion
**Impact**: MEDIUM
**Difficulty**: MEDIUM — code comments acknowledge this needs fixing

### 10. MemoryPools (~27 MEM_POOLs → 1 pool manager)
**Source**: Scattered across all directories
**Contents**: All `MEM_POOL` instances
**Impact**: HIGH — every allocation depends on these
**Difficulty**: HIGH — must change allocation patterns

---

## Notable Observations

1. **The code knows it has a problem**: Comments in `ipa_cg.cxx` note `IPA_Call_Graph` is "used as a global even inside class member functions." Comments in `ipa_section_prop.cxx` admit statics exist to "avoid passing 6 arguments into deep recursive calls."

2. **DST traversal blocker**: `dst_idx_map` in `ipc_bread.cxx` has an explicit comment saying it *must* be global because the DST API only accepts raw function pointers, not stateful objects. Fixing this requires API changes.

3. **`config_ipa.cxx` dominates**: ~140 of ~546 globals (26%) are just command-line option flags. Wrapping these in a struct is mechanically straightforward but touches many read sites.

4. **9 clean files** in `main/optimize/` have zero globals — proof that not everything needs refactoring.

5. **`ipl_main.cxx:init.cxx`** has 9 extern declarations as a Linux weak-symbol workaround — these are infrastructure, not IPA state.

---

## Detailed per-directory reports
- `phase1b/common.md` — 73 globals, 19 files
- `phase1b/local.md` — 61 globals, 12 files
- `phase1b/main-analyze.md` — ~140 globals, 38 files
- `phase1b/main-optimize.md` — 89 globals, 16 files
- `phase1b/inline-cord.md` — 35 globals, 9 files
- `phase1b/be-common.md` — ~148 globals, 12+ files
