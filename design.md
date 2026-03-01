# IPA Context Struct Design — Removing Global State

## Status: DRAFT — for review

## Goal

Encapsulate all IPA global state into context structs so that multiple IPA
instances can run concurrently without interference. Today, ~546 mutable
file-scope globals make this impossible.

---

## Architecture Recap

IPA is a **two-phase system** running in two separate shared objects:

1. **ipl.so** (local analysis) — loaded by `be.so` during per-file compilation.
   Entry: `ipl_main()` → `Ipl_Init()` → `Perform_Procedure_Summary_Phase()` (per PU) → `Ipl_Fini()`.
   Key global: `Summary` (SUMMARY*).

2. **ipa.so** (global analysis + transformation) — loaded by the linker at link time.
   Entry: `ipa_dot_so_init()` → `ipa_driver()` → `Perform_Interprocedural_Optimization()`.
   Key globals: `IPA_Call_Graph`, `IP_File_header`, `Aux_Pu_Table`, 140+ option flags.

3. **inline** / **lw_inline** (standalone executables) — share code with both phases.

These are separate binaries, so they need **separate context types**.

---

## Proposed Context Hierarchy

```
IPA_Options            (read-only after init, shareable across instances)
├── IPA enable flags   (~12 booleans)
├── INLINE enable flags
├── Trace flags        (~5 booleans)
├── Limits             (max depth, bloat factor, max clones, etc.)
└── Skip lists, feedback paths

IPL_Context            (per ipl.so invocation)
├── Summary*
├── Array_Summary
├── WN_MAPs            (Summary_Map, Stmt_Map, etc.)
├── MEM_POOLs          (IPL_loop_pool, IPL_local_pool, etc.)
├── Managers           (Ipl_Al_Mgr, Ipl_Du_Mgr)
├── Name tables        (Ipl_Function_Names, Ipl_Symbol_Names)
└── Option overrides   (DoPreopt, Do_Par, Do_Common_Const)

IPA_Context            (per ipa.so invocation — the big one)
├── IPA_Options*       (shared, const pointer)
├── call_graph         (IPA_CALL_GRAPH*)
├── file_header        (IP_FILE_HDR_TABLE)
├── aux_pu_table       (AUX_PU_TAB)
├── node_context       (IPA_NODE_CONTEXT)
├── IPA_Inline_Stats   (counters sub-struct)
├── IPA_Cprop_State    (pools, counters, clone limits)
├── IPA_Alias_State    (IPAA state, icall refs)
├── IPA_Section_State  (array prop pool, trace)
├── IPA_Struct_Opt     (field layout, split count, relayout IDs)
├── IPA_Common_State   (common table, pad count)
├── IPA_Feedback_State (5 FILE* handles)
├── IPA_Reorder_State  (merged_access, pool)
├── IPA_CHG_State      (class hierarchy, concurrency graph)
├── IPA_Alias_Class    (Ip_alias_class, file list)
├── IPA_Compile_State  (infiles, outfiles, makefile)
├── IPA_Merge_State    (type hash tables, merge pools)
└── IPA_Misc           (visualization, PIC state)

INLINE_Context         (per standalone inliner invocation)
├── IPA_Options*       (shared, const)
├── Summary*
├── call_graph
├── inline stats
└── MEM_POOLs
```

---

## Detailed Sub-Struct Designs

### IPA_Options (~140 fields → 1 struct, const after init)

This is the single highest-impact change: ~140 globals in `config_ipa.cxx`
become fields of one struct. Since they are **read-only after initialization**,
they can be shared via `const IPA_Options*` across concurrent instances.

```cpp
// ipa_options.h (NEW file)
struct IPA_Options {
    // === IPA Enable Flags ===
    BOOL enable_inline;          // IPA_Enable_Inline
    BOOL enable_dfe;             // IPA_Enable_DFE
    BOOL enable_cprop;           // IPA_Enable_Cprop
    BOOL enable_cloning;         // IPA_Enable_Cloning
    BOOL enable_padding;         // IPA_Enable_Padding
    BOOL enable_array_sections;  // IPA_Enable_Array_Sections
    BOOL enable_alias_class;     // IPA_Enable_Alias_Class
    BOOL enable_auto_gnum;       // IPA_Enable_AutoGnum
    BOOL enable_sp_partition;    // IPA_Enable_SP_Partition
    BOOL enable_devirtualization; // IPA_Enable_Devirtualization
    BOOL enable_struct_opt;      // IPA_Enable_Struct_Opt
    BOOL enable_memtrace;        // IPA_Enable_Memtrace
    // ... (all other IPA_Enable_* flags)

    // === INLINE Enable Flags ===
    BOOL inline_enable;          // INLINE_Enable
    BOOL inline_all;             // INLINE_All
    BOOL inline_none;            // INLINE_None
    // ... (all INLINE_* flags)

    // === Trace Flags ===
    BOOL trace_ipa;              // Trace_IPA
    BOOL trace_perf;             // Trace_Perf
    BOOL trace_ipalno;           // Trace_IPALNO
    BOOL trace_mod_ref;          // IPA_Trace_Mod_Ref
    BOOL trace_sections;         // Trace_IPA_Sections
    BOOL trace_copyprop;         // Trace_CopyProp

    // === Limits ===
    INT    max_depth;            // IPA_Max_Depth
    INT    bloat_factor;         // IPA_Bloat_Factor
    UINT32 max_node_clones;      // IPA_Max_Node_Clones
    UINT32 max_total_clones;     // IPA_Max_Total_Clones

    // === Misc ===
    BOOL   verbose;              // Verbose
    BOOL   demangle;             // Demangle
    BOOL   promp_listing;        // ProMP_Listing
    struct skiplist* skip_list;  // IPA_Skip_List
    BOOL   opt_cyg_instrument;   // OPT_Cyg_Instrument
    BOOL   opt_options_inconsistent; // Opt_Options_Inconsistent
};
```

**Migration**: Add `IPA_Options` struct. Initialize it from the existing globals
in `Process_IPA_Options()`. Provide a global `const IPA_Options* IPA_Opts` for
backward compatibility. Gradually replace direct global references with
`IPA_Opts->field` access. Since both old globals and new struct read the same
command-line, they can coexist during migration.

### IPA_Inline_Stats (Cluster 4)

```cpp
struct IPA_Inline_Stats {
    INT     total_inlined;
    INT     total_not_inlined;
    INT     total_must_inlined;
    INT     total_must_not_inlined;
    INT     ipo_total_inlined;
    UINT32  total_prog_size;
    UINT32  orig_prog_weight;
    UINT32  orig_prog_wn_count;
    UINT32  total_dead_function_weight;
    UINT32  total_dead_function_wn_count;
    FB_FREQ total_call_freq;
    FB_FREQ total_cycle_count_2;
};
```

### IPA_Cprop_State (Cluster 5)

```cpp
struct IPA_Cprop_State {
    MEM_POOL ipa_cprop_pool;
    MEM_POOL local_cprop_pool;
    MEM_POOL global_mem_pool;
    INT      constant_count;      // IPA_Constant_Count
    UINT32   num_total_clones;    // IPA_Num_Total_Clones
    FILE*    feedback_con_fd;     // IPA_Feedback_con_fd
};
```

### IPA_Feedback_State (Cluster 11)

```cpp
struct IPA_Feedback_State {
    FILE* dfe_fd;    // IPA_Feedback_dfe_fd
    FILE* dve_fd;    // IPA_Feedback_dve_fd
    FILE* prg_fd;    // IPA_Feedback_prg_fd
    FILE* con_fd;    // IPA_Feedback_con_fd (also in cprop state — alias or move)
    FILE* exl_fd;    // IPA_Feedback_exl_fd
};
```

### IPA_Struct_Opt (Cluster 8)

```cpp
struct IPA_Struct_Opt {
    Field_pos* struct_field_layout;   // Struct_field_layout
    INT        struct_split_count;    // Struct_split_count
    TY_IDX     complete_struct_relayout_type_id;
    // + the ~34 statics in ipo_struct_opt.cxx
    //   (two sub-clusters: relayout + array remapping)
};
```

### IPA_Context (the main struct)

```cpp
// ipa_context.h (NEW file)
struct IPA_Context {
    // === Core ===
    const IPA_Options*    options;
    IPA_CALL_GRAPH*       call_graph;          // IPA_Call_Graph
    BOOL                  call_graph_built;     // IPA_Call_Graph_Built
    IPA_CALL_GRAPH*       graph_undirected;     // IPA_Graph_Undirected
    vector<IPA_NODE*>     emit_order;

    // === Per-Node Infrastructure ===
    IP_FILE_HDR_TABLE     file_header;          // IP_File_header
    // Aux_Pu_Table — type TBD, depends on Open64's aux table mechanism

    // === Sub-States ===
    IPA_Inline_Stats      inline_stats;
    IPA_Cprop_State       cprop;
    IPA_Feedback_State    feedback;
    IPA_Struct_Opt        struct_opt;
    // ... other sub-structs as defined above

    // === Memory Pools ===
    MEM_POOL              ipo_mem_pool;         // Ipo_mem_pool
    MEM_POOL              recycle_mem_pool;     // Recycle_mem_pool
    MEM_POOL              ipa_lno_mem_pool;     // IPA_LNO_mem_pool
    MEM_POOL              reorder_pool;         // reorder_local_pool

    // === Class Hierarchy / Devirt ===
    IPA_CLASS_HIERARCHY*  class_hierarchy;      // IPA_Class_Hierarchy
    IPA_PCG*              concurrency_graph;    // IPA_Concurrency_Graph

    // === Alias Classification ===
    IP_ALIAS_CLASSIFICATION* alias_class;       // Ip_alias_class
    // Ip_alias_class_files — needed for be/com cross-ref

    // === Compilation Orchestration ===
    // infiles, outfiles, makefile state from ipc_compile.cxx

    // === Visualization ===
    // daVinci* cg_display (optional, can be NULL)
};
```

---

## Migration Strategy

### Principle: Incremental, Build-Green, Backward-Compatible

We cannot rewrite 62 files atomically. The migration proceeds in small,
independently-buildable steps. At each step, the compiler builds and the
existing behavior is preserved (no functional changes).

### Phase 3a: Create the structs (no behavior change)

1. Create `ipa_options.h` with `IPA_Options` struct definition.
2. Create `ipa_context.h` with `IPA_Context` and sub-struct definitions.
3. Add initialization code that **populates the structs FROM the existing globals**.
4. Add a global `IPA_Context* g_ipa_ctx` pointer (the transitional shim).
5. No existing code changes. Everything still reads the old globals.

**Verification**: Build succeeds. New headers compile. No functional change.

### Phase 3b: Migrate easy clusters first

Start with clusters that are confined to 1-3 files:

| Order | Cluster | Files | Approach |
|-------|---------|-------|----------|
| 1 | Feedback FDs | 1-2 files | Replace 5 `FILE*` globals with `g_ipa_ctx->feedback.*` |
| 2 | Struct Opt | 2 files | Replace 4 globals with `g_ipa_ctx->struct_opt.*` |
| 3 | Visualization | 3 files | Replace `cg_display` with `g_ipa_ctx->cg_display` |
| 4 | Array Sections | 2 files | Replace pool + trace with sub-struct |
| 5 | Field Reorder | 3 files | Replace `merged_access` + pool |
| 6 | Devirt/CHG | 4 files | Replace class hierarchy + concurrency graph |
| 7 | Common/Padding | 3 files | Replace `IPA_Common_Table` + pad count |

Each step: sed-like rename, rebuild, verify identical behavior.

### Phase 3c: Migrate medium clusters

| Order | Cluster | Files | Notes |
|-------|---------|-------|-------|
| 8 | Cprop State | 3 core files | Pools + counters. Touch ipa_main.cxx (init). |
| 9 | Inline Stats | 5+ files | Counters updated from many places. Mechanical but wide. |
| 10 | Symtab Merge | 2 core files | Critical merge phase — test carefully. |
| 11 | Options | 32 files | Huge reach but mechanical: `IPA_Enable_X` → `ctx->options->enable_x`. Can be done with a macro shim. |

### Phase 3d: Migrate the hard clusters

| Order | Cluster | Files | Notes |
|-------|---------|-------|-------|
| 12 | Summary/Local | 14 files | Crosses ipl.so / ipa.so boundary. Need `IPL_Context` too. |
| 13 | Alias Class | 3 ipa/ + 3 be/com/ | Escapes to backend — requires interface changes. |
| 14 | Call Graph + Node Context | 21 files, 261 refs | The final boss. Every analysis pass touches these. |

### Phase 3e: Remove the global shim

Once all code reads from `g_ipa_ctx->` instead of bare globals:

1. Remove the old global variable declarations.
2. Change `g_ipa_ctx` from a global to a parameter threaded through entry points.
3. Each entry point (`ipa_driver`, `Perform_Interprocedural_Optimization`, etc.)
   receives an `IPA_Context*` parameter.
4. The linker creates an `IPA_Context` on the stack and passes it in.

Now two linkers can run two independent IPA passes simultaneously.

---

## Options Migration: The Macro Shim Approach

Since ~140 option flags are referenced in 32+ files, a mechanical
find-and-replace is feasible but noisy. An alternative transitional approach:

```cpp
// ipa_options_compat.h (TRANSITIONAL — removed at end of migration)
#define IPA_Enable_Inline      (g_ipa_ctx->options->enable_inline)
#define IPA_Enable_DFE         (g_ipa_ctx->options->enable_dfe)
#define IPA_Enable_Cprop       (g_ipa_ctx->options->enable_cprop)
// ... etc for all ~140 flags
```

This allows all 32+ files to keep using `IPA_Enable_Inline` etc. while
the underlying storage moves to the struct. The macros are removed in
Phase 3e when we thread `IPA_Context*` explicitly.

**Risk**: Macros hide the dependency on `g_ipa_ctx`. But this is a
known transitional step, and the macros file serves as documentation
of exactly what needs final migration.

---

## Known Complications

### 1. DST Traversal Callback (ipc_bread.cxx)

`dst_idx_map` in `ipc_bread.cxx` MUST be global because the DST traversal
API (`DST_walk_*`) accepts only raw function pointers, not closures or
stateful objects. To encapsulate this, we would need to change the DST API
to accept a `void* user_data` context parameter — a deeper change.

**Recommendation**: Leave this as a known limitation initially. It only
matters for the file-reading phase, which is inherently sequential per file.

### 2. be/com/ Cross-References

6 globals escape `ipa/` into `be/com/`:
- `Summary` (5 be/com files)
- `Ip_alias_class` (3 be/com files)
- `IPA_Call_Graph`, `IPA_NODE_CONTEXT`, `Aux_Pu_Table`, `Ipl_Summary_Symbol` (1 each)

These require either:
- Passing context through the be/com interfaces, or
- Maintaining thin accessor functions that read from a thread-local or
  explicitly-passed context.

**Recommendation**: Address in Phase 3d. The be/com files are called from
within IPA, so the context can be threaded through the call chain.

### 3. ipacom_doit() Never Returns

`ipacom_doit()` calls `exec()` to replace the process with `make`.
For concurrent IPA, this needs to become `fork() + exec()` or
`posix_spawn()` instead, so the parent process can continue.

**Recommendation**: This is an orthogonal change. Handle separately.

### 4. MEM_POOL System

Open64's `MEM_POOL` system may itself have global state (a global pool
allocator or registry). Creating per-context MEM_POOLs may require
understanding whether `MEM_Initialize()` can be called per-context.

**Recommendation**: Investigate MEM_POOL internals before migrating
pool globals. This is a prerequisite for Phase 3c.

### 5. ipl.so vs ipa.so Boundary

`ipl.so` and `ipa.so` are separate shared objects. `Summary` is the
key global that bridges them — it's created in ipl.so and read back in
ipa.so (via WHIRL file I/O, not shared memory). So the actual sharing
happens through file I/O, not through a shared pointer. The `IPL_Context`
and `IPA_Context` don't need to share state at runtime.

However, `Ipl_Init_From_Ipa()` is called when IPA re-invokes local
analysis during preoptimization. In that case, ipl code runs within the
ipa.so process and DOES share the address space. The `Summary` pointer
created there needs to be part of the `IPA_Context`.

---

## Concurrency Model (end state)

Once globals are encapsulated:

```
Process 1 (linker):
  IPA_Options opts;            // parsed from command line
  IPA_Context ctx1(&opts);     // first compilation unit set
  IPA_Context ctx2(&opts);     // second compilation unit set (e.g., LTO partitions)

  // These can run in parallel:
  thread t1([&]{ run_ipa(&ctx1); });
  thread t2([&]{ run_ipa(&ctx2); });
  t1.join(); t2.join();
```

`IPA_Options` is shared read-only. Each `IPA_Context` owns its own call graph,
memory pools, counters, and output files. No locks needed.

---

## Summary of Changes

| What | Files Created | Files Modified |
|------|--------------|----------------|
| Phase 3a: Create structs | `ipa_options.h`, `ipa_context.h`, `ipa_context.cxx` | 0 (additive only) |
| Phase 3b: Easy clusters | 0 | ~15 files (1-4 per cluster × 7 clusters) |
| Phase 3c: Medium clusters | `ipa_options_compat.h` (transitional macros) | ~40 files |
| Phase 3d: Hard clusters | 0 | ~25 files including be/com |
| Phase 3e: Remove shim | 0 | Entry points + delete compat header |
| **Total** | 3-4 new files | ~62 files (most of IPA) |

---

## Design Decisions (resolved)

1. **`IPA_Options` is a separate struct**, not part of `IPA_Context`.
   It's not frequently used and won't be compute-intensive. Keeping it
   separate allows sharing via `const IPA_Options*` across contexts.

2. **Macro shim for options migration.** Easier to verify everything
   smoothly migrated before removing the shim.

3. **Parameter-passing for context threading** (not thread-local).
   Cleaner design. `g_ipa_ctx` is a transitional global that gets
   replaced by explicit `IPA_Context*` parameters in Phase 3e.

4. **MEM_POOL**: Needs separate investigation. See `mempool-investigation.md`.

5. **Incremental testing**: Deferred — needs more thinking on the right
   verification strategy.

6. **Scope: Globals encapsulated for future parallelism.** We are NOT
   targeting "two contexts running concurrently" in this effort.
   The goal is structural: move globals into structs so that future
   parallelism work has a clean foundation. This means ipacom_doit()
   and MEM_POOL concurrency issues are out of scope for Phase 3.
