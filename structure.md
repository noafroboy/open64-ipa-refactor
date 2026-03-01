# IPA Structure Map

## Directory Organization

**Total: ~120 header files | ~30,088 lines (headers only)**

### Primary Source Locations

| Directory | Prefix | Purpose | Headers |
|-----------|--------|---------|---------|
| `ipa/common/` | IPC_* | Inter-file compilation infrastructure (I/O, symtab merge, linking) | 21 |
| `ipa/local/` | IPL_* | Local (per-file) analysis & summarization | 13 |
| `ipa/main/analyze/` | IPA_* | Main IPA analysis phase (call graph, alias, data flow, cprop) | 63 |
| `ipa/main/optimize/` | IPO_* | IPA optimization phase (inlining, cloning, alias class, DCE) | 15 |
| `ipa/inline/` | inline_* | Standalone inlining phase | 7 |
| `ipa/cord/` | — | Object info (1 header) | 1 |

### IPA Code Outside `ipa/`

| Directory | Files | Purpose |
|-----------|-------|---------|
| `common/com/` | `config_ipa.h`, `be_ipa_util.h` | IPA configuration/options |
| `be/com/` | 11 headers | Backend IPA integration: sections, LNO summaries, IPAA optimizer |

### Largest Files (by line count)

1. `ipl_summarize_template.h` — 3563 lines
2. `ipl_summary.h` — 2961 lines
3. `ipl_analyze_template.h` — 2929 lines
4. `ipa_cg.h` — 1609 lines
5. `ipa_section.h` (BE) — 1590 lines
6. `opt_ipaa_summary.h` (BE) — 942 lines
7. `ipl_summarize.h` — 936 lines
8. `ipo_alias_class.h` — 763 lines

## Key Data Structures

### Call Graph & Program Structure
- `IPA_CALL_GRAPH` — main call graph (ipa_cg.h)
- `IPA_NODE` — function node in call graph
- `IPA_EDGE` — call edge between functions
- `NODE<T>`, `EDGE<T>` — template graph primitives (ip_graph.h)

### Summary Data (Local Analysis)
- `SUMMARY_PROCEDURE` — per-function summary
- `SUMMARY_FORMAL` / `SUMMARY_ACTUAL` — parameter info
- `SUMMARY_GLOBAL` — global symbol summary
- `IVAR` — integer variable summary

### Alias Analysis
- `IPAA` — main alias analyzer class (ipaa.h)
- `IP_ALIAS_CLASSIFICATION` — alias equivalence classes (ipo_alias_class.h)
- `IPA_VIRTUAL_FUNCTION_TRANSFORM` — devirtualization

### Data Flow Framework
- `IPA_DATA_FLOW` — base class for all IPA data flow analyses
- `IPA_CPROP_DF_FLOW` — constant propagation (derived)
- `IPA_SCALAR_MUST_DF_FLOW` — scalar must-define (derived)
- `IPA_ARRAY_DF_FLOW` — array data flow (derived)

### Other
- `IPA_CLASS_HIERARCHY` — class hierarchy graph
- `IPA_PCG` — parallelization concurrency graph
- `COMMON_SNODE_TBL` — common block hash table

## Include Dependency Chain

```
ip_graph.h
  ↓
ipa_cg.h → ip_call.h, ipl_summary.h, ipc_file.h
  ↓
ipa_summary.h, ipa_cprop.h

ipa_df.h (base class)
  ↓
ipa_cprop.h, ipa_section_prop.h (derived)

ipc_main.h → ipc_link.h, ipc_compile.h → ipc_bread.h, ipc_bwrite.h, ipc_file.h
```

## Functional Groupings

### IPC — Inter-file Compilation (`common/`)
- I/O: `ipc_bread.h`, `ipc_bwrite.h`, `ipc_file.h`
- Symbol tables: `ipc_symtab.h`, `ipc_symtab_merge.h`, `ipc_ty_hash.h`, `ipc_type_merge.h`
- Linking: `ipc_link.h`, `ipc_compile.h`
- DST/Debug: `ipc_dst_merge.h`, `ipc_dst_utils.h`

### IPL — Local Analysis (`local/`)
- Core: `ipl_main.h`, `ipl_summary.h`
- Summarization: `ipl_summarize.h`, `ipl_summarize_template.h`
- Analysis: `ipl_analyze_template.h`
- Transformations: `ipl_outline.h`, `ipl_reorder.h`

### IPA — Analysis (`main/analyze/`)
- Graph: `ipa_cg.h`, `ip_graph.h`, `ip_graph_trav.h`
- Alias: `ipaa.h`, `ipa_nystrom_alias_analyzer.h`
- Data flow: `ipa_df.h`, `ipa_cprop.h`, `ipa_section_prop.h`
- Specialization: `ipa_devirtual.h`, `ipa_chg.h`, `ipa_pcg.h`
- Other: `ipa_solver.h`, `ipa_inline.h`, `ipa_cost.h`, `ipa_pad.h`

### IPO — Optimization (`main/optimize/`)
- Inlining: `ipo_inline.h`, `ipo_inline_util.h`
- Cloning: `ipo_clone.h`, `ipo_split.h`
- Alias: `ipo_alias.h`, `ipo_alias_class.h`
- Other: `ipo_const.h`, `ipo_dce.h`, `ipo_pad.h`

### Backend Integration (`be/com/`)
- Section analysis: `ipa_section.h`, `ipa_section_main.h`
- LNO interface: `ipa_lno_file.h`, `ipa_lno_info.h`, `ipa_lno_summary.h`
- IPAA optimizer: `opt_ipaa_summary.h`, `opt_ipaa_io.h`
