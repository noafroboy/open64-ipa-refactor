# IPA Global State Dependencies

## Status: COMPLETE

98 globals tracked across 62 .cxx files in `osprey/ipa/` (all subdirectories).
436 co-occurring pairs identified (sharing 2+ files).
Additional cross-references checked in `osprey/be/com/`.

---

## Cross-cutting globals (used in 10+ files)

These are the hardest to encapsulate -- they must be in a widely-passed context struct.

| Global | Files | Top co-occurring globals (shared file count) |
|--------|-------|---------------------------------------------|
| `IPA_Call_Graph` | 21 | Aux_Pu_Table (10), IPA_NODE_CONTEXT (8), IP_File_header (6), IPA_Enable_DFE (5), Summary (5), Trace_IPA (5) |
| `Trace_IPA` | 18 | Trace_Perf (12), IPA_NODE_CONTEXT (6), IPA_Call_Graph (5), IPA_Enable_Inline (5), Aux_Pu_Table (4), IPA_Enable_Array_Sections (4) |
| `IPA_NODE_CONTEXT` | 15 | IPA_Call_Graph (8), Aux_Pu_Table (6), Trace_IPA (6), Trace_Perf (6), IPA_Enable_Array_Sections (4), IP_File_header (4) |
| `Summary` | 14 | IPA_Call_Graph (5), Do_Par (3), IPA_NODE_CONTEXT (3), Aux_Pu_Table (2), Aux_Symbol_Info (2), DoPreopt (2) |
| `Aux_Pu_Table` | 13 | IPA_Call_Graph (10), IPA_NODE_CONTEXT (6), IPA_Enable_DFE (5), IPA_Enable_Cloning (4), IP_File_header (4), Orig_Prog_Weight (4) |
| `Trace_Perf` | 12 | Trace_IPA (12), IPA_NODE_CONTEXT (6), IPA_Enable_Inline (5), Aux_Pu_Table (4), IPA_Enable_Array_Sections (4), IPA_Enable_AutoGnum (4) |
| `IP_File_header` | 10 | IPA_Call_Graph (6), Aux_Pu_Table (4), IPA_NODE_CONTEXT (4), IPA_Enable_Array_Sections (3), IPA_Enable_DFE (3), IPA_Enable_Inline (3) |

## Medium-spread globals (used in 3-9 files)

These need context but are more contained than the cross-cutting ones.

| Global | Files | Top co-occurring globals |
|--------|-------|-------------------------|
| `IPA_Enable_DFE` | 7 | Aux_Pu_Table (5), IPA_Call_Graph (5), Orig_Prog_Weight (5), Total_Prog_Size (5), IPA_Enable_Cloning (3) |
| `IPA_Enable_AutoGnum` | 6 | Trace_IPA (4), Trace_Perf (4), Aux_Pu_Table (3), IPA_Enable_Array_Sections (3), IPA_Enable_Inline (3) |
| `Orig_Prog_Weight` | 5 | IPA_Enable_DFE (5), Total_Prog_Size (5), Aux_Pu_Table (4), IPA_Call_Graph (4), IPA_Enable_Inline (3) |
| `Opt_Options_Inconsistent` | 5 | IPA_Enable_Inline (4), IPA_NODE_CONTEXT (4), Trace_IPA (4), Trace_Perf (4), Aux_Pu_Table (3) |
| `Total_Prog_Size` | 5 | IPA_Enable_DFE (5), Orig_Prog_Weight (5), Aux_Pu_Table (4), IPA_Call_Graph (4), IPA_Enable_Inline (3) |
| `IPA_Enable_Inline` | 5 | Trace_IPA (5), Trace_Perf (5), Opt_Options_Inconsistent (4), Aux_Pu_Table (3), IPA_Call_Graph (3) |
| `IPA_Enable_Cloning` | 5 | Aux_Pu_Table (4), IPA_Call_Graph (3), IPA_Enable_DFE (3), IPA_Enable_AutoGnum (2), IPA_Enable_Inline (2) |
| `IPA_Enable_Array_Sections` | 5 | IPA_NODE_CONTEXT (4), Trace_IPA (4), Trace_Perf (4), Aux_Pu_Table (3), IPA_Call_Graph (3) |
| `Verbose` | 5 | IPA_Enable_DFE (3), Trace_IPA (3), Trace_Perf (3), IPA_Call_Graph (2), IPA_Enable_AutoGnum (2) |
| `IPA_Class_Hierarchy` | 4 | IPA_Call_Graph (2), IPA_NODE_CONTEXT (2), Aux_Pu_Table (1), Global_mem_pool (1), IPA_Concurrency_Graph (1) |
| `IPA_Enable_Struct_Opt` | 4 | Trace_IPA (4), IPA_Call_Graph (3), IPA_Enable_Array_Sections (2), IPA_Enable_Cprop (2), IPA_Enable_Inline (2) |
| `Aux_St_Table` | 4 | Aux_Pu_Table (3), Trace_IPA (3), Trace_Perf (3), Aux_St_Tab (2), IPA_Call_Graph (2) |
| `Ipl_Summary_Symbol` | 4 | IPA_Trace_Mod_Ref (2), DoPreopt (1), Do_Par (1), Summary (1), icall_def (1) |
| `IPA_Max_Node_Clones` | 4 | IPA_Call_Graph (4), IPA_Enable_DFE (3), Ipa_cprop_pool (3), Aux_Pu_Table (2), Global_mem_pool (2) |
| `IPA_Max_Depth` | 4 | Aux_Pu_Table (3), IPA_Enable_DFE (3), Orig_Prog_Weight (3), Total_Prog_Size (3), Trace_IPA (3) |
| `IPA_Common_Table` | 3 | IPA_Enable_Padding (2), IP_File_header (2), Trace_IPA (2), Aux_Pu_Table (1), IPA_Call_Graph (1) |
| `Ipa_cprop_pool` | 3 | IPA_Call_Graph (3), IPA_Enable_DFE (3), IPA_Max_Node_Clones (3), Aux_Pu_Table (2), Global_mem_pool (2) |
| `Total_call_freq` | 3 | Aux_Pu_Table (3), IPA_Enable_DFE (3), Orig_Prog_Weight (3), Total_Prog_Size (3), Aux_St_Table (2) |
| `Total_Inlined` | 3 | IPA_Enable_DFE (3), Orig_Prog_Weight (3), Total_Not_Inlined (3), Total_Prog_Size (3), Aux_Pu_Table (2) |
| `Total_Not_Inlined` | 3 | IPA_Enable_DFE (3), Orig_Prog_Weight (3), Total_Inlined (3), Total_Prog_Size (3), Aux_Pu_Table (2) |
| `IPA_Concurrency_Graph` | 3 | IPA_NODE_CONTEXT (2), Global_mem_pool (1), IPA_Call_Graph (1), IPA_Class_Hierarchy (1), IPA_Constant_Count (1) |
| `IPA_Enable_Cprop` | 3 | IPA_Enable_Array_Sections (3), IPA_NODE_CONTEXT (3), Trace_IPA (3), Trace_Perf (3), IPA_Call_Graph (2) |
| `IPA_Enable_Padding` | 3 | IP_File_header (3), IPA_Call_Graph (2), IPA_Common_Table (2), IPA_Enable_Array_Sections (2), IPA_Enable_Cprop (2) |
| `IPA_Enable_SP_Partition` | 3 | IPA_Call_Graph (3), IPA_Enable_Array_Sections (3), IPA_Enable_Inline (3), IPA_NODE_CONTEXT (3), IP_File_header (3) |
| `IPA_Target_Type` | 3 | IPA_Enable_AutoGnum (2), Aux_Pu_Table (1), IPA_Enable_Array_Sections (1), IPA_Enable_Cloning (1), ProMP_Listing (1) |
| `Ip_alias_class` | 3 | Ip_alias_class_files (3), IPA_Enable_Alias_Class (2), IPA_NODE_CONTEXT (2), Opt_Options_Inconsistent (2), ipisr_cg (2) |
| `Ip_alias_class_files` | 3 | Ip_alias_class (3), IPA_Enable_Alias_Class (2), IPA_NODE_CONTEXT (2), Opt_Options_Inconsistent (2), ipisr_cg (2) |
| `cg_display` | 3 | IPA_Call_Graph (2), IPA_Enable_Array_Sections (2), IPA_Enable_DFE (2), IPA_Enable_Inline (2), IPA_Enable_SP_Partition (2) |
| `IPO_Gprel_Sym_Count` | 3 | Trace_IPA (3), Aux_St_Tab (1), Aux_St_Table (1), IPA_Common_Table (1), IPA_Enable_AutoGnum (1) |
| `ST_To_INITO_Map` | 3 | Aux_Pu_Table (2), Aux_St_Tab (2), Aux_St_Table (2), Trace_IPA (2), Trace_Perf (2) |
| `Do_Par` | 3 | Summary (3), DoPreopt (2), Do_Common_Const (1), IPA_Call_Graph (1), IPA_NODE_CONTEXT (1) |
| `merged_access` | 3 | IPA_Call_Graph (3), IPA_Enable_Array_Sections (2), IPA_Enable_DFE (2), IPA_Enable_Inline (2), IPA_Enable_SP_Partition (2) |
| `ProMP_Listing` | 3 | IPA_Enable_AutoGnum (2), Aux_Pu_Table (1), Demangle (1), IPA_Bloat_Factor (1), IPA_Enable_Alias_Class (1) |

## Two-file globals (used in exactly 2 files)

These can potentially be passed via a smaller sub-context.

| Global | Files | Co-occurring globals |
|--------|-------|---------------------|
| `Aux_St_Tab` | `common/ipc_pic.cxx`, `common/ipc_symtab_merge.cxx` | Aux_St_Table (2), ST_To_INITO_Map (2), Trace_IPA (2), Trace_Perf (2) |
| `Aux_Symbol_Info` | `inline/inline.cxx`, `local/ipl_summarize_util.cxx` | Summary (2), Aux_Pu_Table (1), Aux_Symbol (1), IPA_Call_Graph (1) |
| `Common_Block_Elements_Map` | `common/ipc_symtab_merge.cxx`, `main/analyze/ipa_lno_write.cxx` | Aux_Pu_Table (1), Aux_St_Tab (1), Aux_St_Table (1), IPA_LNO_mem_pool (1) |
| `DoPreopt` | `local/ipl_bread_write.cxx`, `local/ipl_main.cxx` | Do_Par (2), Summary (2), Do_Common_Const (1), Ipl_Function_Names (1) |
| `Global_mem_pool` | `main/analyze/ipa_cprop.cxx`, `main/analyze/ipa_main.cxx` | IPA_Call_Graph (2), IPA_Constant_Count (2), IPA_Enable_DFE (2), IPA_Feedback_con_fd (2) |
| `IPA_Bloat_Factor` | `main/analyze/ipa_inline.cxx`, `main/analyze/ipa_option.cxx` | IPA_Max_Depth (2), Trace_IPA (2), Trace_Perf (2), Aux_Pu_Table (1) |
| `IPA_Call_Graph_Built` | `main/analyze/ipa_cg.cxx`, `main/analyze/ipa_summary.cxx` | IPA_Call_Graph (2), Aux_Pu_Table (1), Aux_St_Table (1), IPA_Enable_Array_Sections (1) |
| `IPA_Constant_Count` | `main/analyze/ipa_cprop.cxx`, `main/analyze/ipa_main.cxx` | Global_mem_pool (2), IPA_Call_Graph (2), IPA_Enable_DFE (2), IPA_Feedback_con_fd (2) |
| `IPA_Enable_Alias_Class` | `common/ipc_bwrite.cxx`, `main/optimize/ipo_main.cxx` | IPA_NODE_CONTEXT (2), Ip_alias_class (2), Ip_alias_class_files (2), Opt_Options_Inconsistent (2) |
| `IPA_Feedback_con_fd` | `main/analyze/ipa_cprop.cxx`, `main/analyze/ipa_main.cxx` | Global_mem_pool (2), IPA_Call_Graph (2), IPA_Constant_Count (2), IPA_Enable_DFE (2) |
| `IPA_Feedback_prg_fd` | `main/analyze/ipa_inline.cxx`, `main/analyze/ipa_main.cxx` | IPA_Enable_DFE (2), IPA_Enable_Inline (2), Opt_Options_Inconsistent (2), Orig_Prog_Weight (2) |
| `IPA_LNO_mem_pool` | `main/analyze/ipa_lno_write.cxx`, `main/optimize/ipo_main.cxx` | IPA_NODE_CONTEXT (2), Aux_Pu_Table (1), Common_Block_Elements_Map (1), IPA_Call_Graph (1) |
| `IPA_Max_Total_Clones` | `main/analyze/ipa_cprop.cxx`, `main/analyze/ipa_main.cxx` | Global_mem_pool (2), IPA_Call_Graph (2), IPA_Constant_Count (2), IPA_Enable_DFE (2) |
| `IPA_Skip_List` | `inline/inline_driver.cxx`, `main/analyze/ipa_option.cxx` | Verbose (2), Demangle (1), IPA_Bloat_Factor (1), IPA_Enable_AutoGnum (1) |
| `IPA_Trace_Mod_Ref` | `local/ipl_summary_print.cxx`, `main/analyze/ipaa.cxx` | Ipl_Summary_Symbol (2), icall_def (1), icall_eref (1), icall_kill (1) |
| `IPA_array_prop_pool` | `main/analyze/ipa_section_annot.cxx`, `main/analyze/ipa_section_prop.cxx` | IPA_Call_Graph (2), Trace_IPA_Sections (2), IPA_Max_Node_Clones (1) |
| `IPA_builtins_list` | `common/ipc_bwrite.cxx`, `main/analyze/ipa_builtins.cxx` | IPA_NODE_CONTEXT (2), Aux_Pu_Table (1), IPA_Call_Graph (1), IPA_Enable_Alias_Class (1) |
| `Ipl_Function_Names` | `inline/inline.cxx`, `local/ipl_main.cxx` | Ipl_Symbol_Names (2), Summary (2), Aux_Pu_Table (1), Aux_Symbol_Info (1) |
| `Ipl_Symbol_Names` | `inline/inline.cxx`, `local/ipl_main.cxx` | Ipl_Function_Names (2), Summary (2), Aux_Pu_Table (1), Aux_Symbol_Info (1) |
| `OPT_Cyg_Instrument` | `inline/inline.cxx`, `main/analyze/ipa_inline.cxx` | Aux_Pu_Table (2), IPA_Enable_DFE (2), IPA_Max_Depth (2), Orig_Prog_Weight (2) |
| `Orig_Prog_WN_Count` | `main/analyze/ipa_cg.cxx`, `main/analyze/ipa_inline.cxx` | Aux_Pu_Table (2), IPA_Enable_DFE (2), IPA_Enable_Inline (2), IPA_Max_Depth (2) |
| `Struct_field_layout` | `main/analyze/ipa_struct_opt.cxx`, `main/optimize/ipo_struct_opt.cxx` | IPA_Enable_Struct_Opt (2), Struct_split_count (2), Trace_IPA (2), complete_struct_relayout_type_id (2) |
| `Struct_split_count` | `main/analyze/ipa_struct_opt.cxx`, `main/optimize/ipo_struct_opt.cxx` | IPA_Enable_Struct_Opt (2), Struct_field_layout (2), Trace_IPA (2), complete_struct_relayout_type_id (2) |
| `Total_Dead_Function_Weight` | `main/analyze/ipa_cg.cxx`, `main/analyze/ipa_main.cxx` | IPA_Call_Graph (2), IPA_Enable_Array_Sections (2), IPA_Enable_DFE (2), IPA_Enable_Inline (2) |
| `Total_Must_Inlined` | `main/analyze/ipa_cg.cxx`, `main/analyze/ipa_main.cxx` | IPA_Call_Graph (2), IPA_Enable_Array_Sections (2), IPA_Enable_DFE (2), IPA_Enable_Inline (2) |
| `Total_Must_Not_Inlined` | `main/analyze/ipa_cg.cxx`, `main/analyze/ipa_main.cxx` | IPA_Call_Graph (2), IPA_Enable_Array_Sections (2), IPA_Enable_DFE (2), IPA_Enable_Inline (2) |
| `Total_cycle_count_2` | `main/analyze/ipa_cg.cxx`, `main/analyze/ipa_inline.cxx` | Aux_Pu_Table (2), IPA_Enable_DFE (2), IPA_Enable_Inline (2), IPA_Max_Depth (2) |
| `Trace_CopyProp` | `inline/inline.cxx`, `main/optimize/ipo_inline.cxx` | IPA_Call_Graph (2), Summary (2), Aux_Pu_Table (1), Aux_Symbol_Info (1) |
| `Trace_IPA_Sections` | `main/analyze/ipa_section_annot.cxx`, `main/analyze/ipa_section_prop.cxx` | IPA_Call_Graph (2), IPA_array_prop_pool (2), IPA_Max_Node_Clones (1) |
| `complete_struct_relayout_type_id` | `main/analyze/ipa_struct_opt.cxx`, `main/optimize/ipo_struct_opt.cxx` | IPA_Enable_Struct_Opt (2), Struct_field_layout (2), Struct_split_count (2), Trace_IPA (2) |
| `emit_order` | `main/analyze/ipa_cg.cxx`, `main/optimize/ipo_main.cxx` | Aux_Pu_Table (2), IPA_Call_Graph (2), IPA_Enable_Array_Sections (2), IPA_Enable_AutoGnum (2) |
| `icall_def` | `main/analyze/ipa_cprop_annot.cxx`, `main/analyze/ipaa.cxx` | Aux_Pu_Table (1), IPA_Call_Graph (1), IPA_Enable_Cloning (1), IPA_Enable_DFE (1) |
| `inline_action_log` | `main/analyze/inline_script_parser.cxx`, `main/optimize/ipo_main.cxx` | Aux_Pu_Table (1), IPA_Call_Graph (1), IPA_Common_Table (1), IPA_Enable_Alias_Class (1) |
| `ipisr_cg` | `common/ipc_bwrite.cxx`, `main/optimize/ipo_main.cxx` | IPA_Enable_Alias_Class (2), IPA_NODE_CONTEXT (2), Ip_alias_class (2), Ip_alias_class_files (2) |
| `local_cprop_pool` | `main/analyze/ipa_cprop_annot.cxx`, `main/analyze/ipa_main.cxx` | IPA_Call_Graph (2), IPA_Enable_DFE (2), IPA_Max_Node_Clones (2), IPA_NODE_CONTEXT (2) |
| `one_got` | `common/ipc_pic.cxx`, `main/optimize/ipo_main.cxx` | IPA_Enable_AutoGnum (2), IPA_Enable_Inline (2), Trace_IPA (2), Trace_Perf (2) |
| `reorder_local_pool` | `main/analyze/ipa_reorder.cxx`, `main/optimize/ipo_main.cxx` | IPA_Call_Graph (2), Aux_Pu_Table (1), IPA_Common_Table (1), IPA_Enable_Alias_Class (1) |

## File-local globals (used in 1 file only)

Easiest to encapsulate -- can become file-static struct members or locals.

| Global | File | Other globals in same file |
|--------|------|---------------------------|
| `Aux_Symbol` | `local/ipl_summarize_util.cxx` | `Aux_Symbol_Info`, `Summary` |
| `Demangle` | `main/analyze/ipa_option.cxx` | `IPA_Bloat_Factor`, `IPA_Enable_AutoGnum`, `IPA_Enable_Memtrace`, `IPA_Max_Depth`, `IPA_Skip_List`, `ProMP_Listing`, `Trace_IPA`, `Trace_IPALNO`, ... (+2 more) |
| `Do_Common_Const` | `local/ipl_main.cxx` | `DoPreopt`, `Do_Par`, `Ipl_Function_Names`, `Ipl_Symbol_Names`, `Summary`, `Trace_IPA`, `Trace_Perf` |
| `IPA_CG_Max_Depth` | `main/analyze/ip_graph_trav.cxx` |  |
| `IPA_Enable_Devirtualization` | `main/analyze/ipa_main.cxx` | `Global_mem_pool`, `IPA_Call_Graph`, `IPA_Class_Hierarchy`, `IPA_Concurrency_Graph`, `IPA_Constant_Count`, `IPA_Enable_Array_Sections`, `IPA_Enable_Cprop`, `IPA_Enable_DFE`, ... (+27 more) |
| `IPA_Enable_Memtrace` | `main/analyze/ipa_option.cxx` | `Demangle`, `IPA_Bloat_Factor`, `IPA_Enable_AutoGnum`, `IPA_Max_Depth`, `IPA_Skip_List`, `ProMP_Listing`, `Trace_IPA`, `Trace_IPALNO`, ... (+2 more) |
| `IPA_Feedback_dfe_fd` | `main/analyze/ipa_main.cxx` | `Global_mem_pool`, `IPA_Call_Graph`, `IPA_Class_Hierarchy`, `IPA_Concurrency_Graph`, `IPA_Constant_Count`, `IPA_Enable_Array_Sections`, `IPA_Enable_Cprop`, `IPA_Enable_DFE`, ... (+27 more) |
| `IPA_Feedback_dve_fd` | `main/analyze/ipa_main.cxx` | `Global_mem_pool`, `IPA_Call_Graph`, `IPA_Class_Hierarchy`, `IPA_Concurrency_Graph`, `IPA_Constant_Count`, `IPA_Enable_Array_Sections`, `IPA_Enable_Cprop`, `IPA_Enable_DFE`, ... (+27 more) |
| `IPA_Graph_Undirected` | `main/analyze/ipa_cg.cxx` | `Aux_Pu_Table`, `Aux_St_Table`, `IPA_Call_Graph`, `IPA_Call_Graph_Built`, `IPA_Enable_Array_Sections`, `IPA_Enable_AutoGnum`, `IPA_Enable_DFE`, `IPA_Enable_Inline`, ... (+19 more) |
| `IPA_Num_Total_Clones` | `main/analyze/ipa_cprop.cxx` | `Aux_Pu_Table`, `Aux_St_Table`, `Global_mem_pool`, `IPA_Call_Graph`, `IPA_Constant_Count`, `IPA_Enable_Cloning`, `IPA_Enable_DFE`, `IPA_Feedback_con_fd`, ... (+6 more) |
| `IPO_Pad_Count` | `main/analyze/ipa_pad.cxx` | `IPA_Common_Table`, `IPA_Enable_Padding`, `IP_File_header` |
| `IPO_Total_Inlined` | `main/optimize/ipo_main.cxx` | `Aux_Pu_Table`, `IPA_Call_Graph`, `IPA_Common_Table`, `IPA_Enable_Alias_Class`, `IPA_Enable_Array_Sections`, `IPA_Enable_AutoGnum`, `IPA_Enable_Cloning`, `IPA_Enable_Cprop`, ... (+17 more) |
| `Ipl_Al_Mgr` | `local/loop_info.cxx` | `Ipl_Du_Mgr`, `Summary` |
| `Ipl_Du_Mgr` | `local/loop_info.cxx` | `Ipl_Al_Mgr`, `Summary` |
| `Total_Dead_Function_WN_Count` | `main/analyze/ipa_cg.cxx` | `Aux_Pu_Table`, `Aux_St_Table`, `IPA_Call_Graph`, `IPA_Call_Graph_Built`, `IPA_Enable_Array_Sections`, `IPA_Enable_AutoGnum`, `IPA_Enable_DFE`, `IPA_Enable_Inline`, ... (+19 more) |
| `Trace_IPALNO` | `main/analyze/ipa_option.cxx` | `Demangle`, `IPA_Bloat_Factor`, `IPA_Enable_AutoGnum`, `IPA_Enable_Memtrace`, `IPA_Max_Depth`, `IPA_Skip_List`, `ProMP_Listing`, `Trace_IPA`, ... (+2 more) |
| `Write_ALIAS_CGNODE_Map` | `common/ipc_bwrite.cxx` | `IPA_Enable_Alias_Class`, `IPA_NODE_CONTEXT`, `IPA_builtins_list`, `Ip_alias_class`, `Ip_alias_class_files`, `Opt_Options_Inconsistent`, `ProMP_Listing`, `ipisr_cg` |
| `icall_eref` | `main/analyze/ipaa.cxx` | `IPA_Trace_Mod_Ref`, `Ipl_Summary_Symbol`, `icall_def`, `icall_kill` |
| `icall_kill` | `main/analyze/ipaa.cxx` | `IPA_Trace_Mod_Ref`, `Ipl_Summary_Symbol`, `icall_def`, `icall_eref` |
| `inline_script_log` | `main/analyze/inline_script_parser.cxx` | `inline_action_log` |
| `number_of_partitions` | `common/ipc_option.cxx` | `Trace_IPA` |

## Dependency Clusters

Clusters are derived from co-occurrence analysis: globals that consistently appear
together across multiple files form natural groupings for context structs.

### Cluster 1: Call Graph Core

The central nervous system of IPA. `IPA_Call_Graph` is the most connected global (21 files).
Any context struct MUST include this and its close companions.

- **Globals**: `IPA_Call_Graph`, `IPA_Call_Graph_Built`, `IPA_Graph_Undirected`, `emit_order`
- **Tightly coupled stats**: `Orig_Prog_WN_Count`, `Prog_WN_Count`, `Total_Dead_Function_WN_Count`,
  `Total_Dead_Function_Weight`, `Orig_Prog_Weight`, `Total_call_freq`, `Total_cycle_count_2`
- **Hub files** (most usage):
  - `main/analyze/ipa_cg.cxx` (84 refs, plus 12 co-occurring globals)
  - `main/analyze/ipa_main.cxx` (18 refs, 36 co-occurring globals)
  - `main/analyze/ipa_summary.cxx` (31 refs)
  - `main/optimize/ipo_main.cxx` (19 refs)
- **Reasoning**: `IPA_Call_Graph` co-occurs with `Aux_Pu_Table` in 10 files,
  `IPA_NODE_CONTEXT` in 8 files, `IP_File_header` in 6 files. These form a tight core.

### Cluster 2: Node Context / Per-PU State

State associated with processing individual program units (functions).

- **Globals**: `IPA_NODE_CONTEXT`, `Aux_Pu_Table`, `IP_File_header`
- **Files** (15, 13, 10 respectively -- heavily overlapping):
  - `IPA_NODE_CONTEXT` <-> `IPA_Call_Graph`: 8 shared files
  - `Aux_Pu_Table` <-> `IPA_Call_Graph`: 10 shared files
  - `Aux_Pu_Table` <-> `IPA_NODE_CONTEXT`: 6 shared files
  - `IP_File_header` <-> `IPA_Call_Graph`: 6 shared files
- **Reasoning**: These three globals are the "per-node" infrastructure.
  Whenever the IPA processes a function node, it needs the call graph,
  the PU auxiliary table, the file header, and the node context.
  They should be bundled together in the context struct.

### Cluster 3: Options / Configuration Flags

Read-only configuration. Could be a separate `IPA_Options` struct passed by const-ref.

- **Globals**:
  - IPA_Enable_* flags (12 total): `IPA_Enable_Alias_Class`, `IPA_Enable_Array_Sections`, `IPA_Enable_AutoGnum`, `IPA_Enable_Cloning`, `IPA_Enable_Cprop`, `IPA_Enable_DFE`, `IPA_Enable_Devirtualization`, `IPA_Enable_Inline`, `IPA_Enable_Memtrace`, `IPA_Enable_Padding`, `IPA_Enable_SP_Partition`, `IPA_Enable_Struct_Opt`
  - `IPA_Max_Depth`, `IPA_Max_Node_Clones`, `IPA_Max_Total_Clones`, `IPA_Bloat_Factor`
  - `Trace_IPA`, `Trace_Perf`, `Trace_IPA_Sections`, `Trace_IPALNO`, `Trace_CopyProp`
  - `Verbose`, `Demangle`, `ProMP_Listing`, `IPA_Skip_List`, `OPT_Cyg_Instrument`
- **Files**: 32 unique files reference at least one `IPA_Enable_*` flag
- **Strongest co-occurrence**: `Trace_IPA` <-> `Trace_Perf` (12 shared files)
- **Reasoning**: These are all read-only after initialization. They can be grouped into
  an `IPA_Options` or `IPA_Config` struct. Since they are read-only, they can be
  shared across concurrent IPA instances without synchronization.

### Cluster 4: Inline Statistics / Counters

Accumulated counters for inline decisions. Must be per-context for concurrent use.

- **Globals**: `Total_Inlined`, `Total_Not_Inlined`, `Total_Must_Inlined`,
  `Total_Must_Not_Inlined`, `IPO_Total_Inlined`, `Total_Prog_Size`,
  `Orig_Prog_Weight`, `Orig_Prog_WN_Count`, `Total_Dead_Function_Weight`,
  `Total_Dead_Function_WN_Count`, `Total_call_freq`, `Total_cycle_count_2`
- **Key files**:
  - `main/analyze/ipa_cg.cxx` (12 of these counters)
  - `main/analyze/ipa_inline.cxx` (8 counters)
  - `main/analyze/ipa_main.cxx` (8 counters)
  - `inline/inline.cxx` (6 counters)
- **Co-occurrence with Cluster 1**: `Orig_Prog_Weight` <-> `IPA_Enable_DFE` (5 shared files),
  `Total_Prog_Size` <-> `IPA_Enable_DFE` (5 shared files)
- **Reasoning**: These counters are updated during inlining and dead function elimination.
  They form a natural `IPA_Stats` sub-struct within the main context.

### Cluster 5: Constant Propagation State

Constant propagation has its own tight cluster of pools and counters.

- **Globals**: `Ipa_cprop_pool`, `local_cprop_pool`, `Global_mem_pool`,
  `IPA_Constant_Count`, `IPA_Num_Total_Clones`, `IPA_Max_Total_Clones`,
  `IPA_Feedback_con_fd`
- **Key files**:
  - `main/analyze/ipa_cprop.cxx` (all 7)
  - `main/analyze/ipa_cprop_annot.cxx` (4 of 7)
  - `main/analyze/ipa_main.cxx` (5 of 7, initialization)
- **Reasoning**: These are tightly coupled -- cprop pools, counters, and cloning limits
  are used together during constant propagation. Natural `IPA_Cprop_State` sub-struct.

### Cluster 6: Summary / Local Analysis

The `Summary` pointer and related state used during local analysis (ipl_*).

- **Globals**: `Summary`, `Ipl_Summary_Symbol`, `Ipl_Al_Mgr`, `Ipl_Du_Mgr`,
  `Ipl_Symbol_Names`, `Ipl_Function_Names`, `DoPreopt`, `Do_Par`, `Do_Common_Const`,
  `Aux_Symbol_Info`, `Aux_Symbol`, `IPA_Trace_Mod_Ref`, `Trace_CopyProp`
- **Key files**:
  - `local/ipl_main.cxx` (8 globals)
  - `local/ipl_bread_write.cxx` (4 globals)
  - `local/ipl_summarize_util.cxx` (3 globals)
  - `local/loop_info.cxx` (3 globals)
  - `inline/inline.cxx` (crosses into inline)
- **Reasoning**: The local analysis phase has its own set of globals largely confined
  to `local/`. `Summary` does leak into main/analyze/ (14 total files), but the
  ipl-specific globals (Ipl_*) are contained. This is a natural `IPL_Context` struct.

### Cluster 7: Array Section Analysis

- **Globals**: `IPA_array_prop_pool`, `Trace_IPA_Sections`, `IPA_Max_Node_Clones`
- **Key files**: `main/analyze/ipa_section_annot.cxx`, `main/analyze/ipa_section_prop.cxx`
- **Reasoning**: Tightly contained pair. The pool and trace flag always appear together.
  Can be a small `IPA_Section_State` sub-struct.

### Cluster 8: Struct Optimization

- **Globals**: `Struct_field_layout`, `Struct_split_count`, `complete_struct_relayout_type_id`,
  `IPA_Enable_Struct_Opt`
- **Key files**: `main/analyze/ipa_struct_opt.cxx`, `main/optimize/ipo_struct_opt.cxx`
- **Reasoning**: Perfectly contained 2-file cluster. Easy to encapsulate.

### Cluster 9: Alias Classification

- **Globals**: `Ip_alias_class`, `Ip_alias_class_files`, `IPA_Enable_Alias_Class`,
  `Write_ALIAS_CGNODE_Map`
- **Key files**: `main/optimize/ipo_alias_class.cxx`, `main/optimize/ipo_main.cxx`,
  `common/ipc_bwrite.cxx`
- **Also used in be/com/**: `opt_alias_mgr.cxx`, `opt_alias_rule.cxx`,
  `opt_points_to_non_template.cxx` (3 files reference `Ip_alias_class`)
- **Reasoning**: The alias classification globals cross into be/com/, making them
  harder to encapsulate. Need to coordinate with the optimizer's alias infrastructure.

### Cluster 10: Common Block / Padding

- **Globals**: `IPA_Common_Table`, `IPO_Pad_Count`, `IPA_Enable_Padding`,
  `Common_Block_Elements_Map`
- **Key files**: `main/analyze/ipa_pad.cxx`, `main/optimize/ipo_split.cxx`,
  `main/optimize/ipo_main.cxx`
- **Reasoning**: Small cluster around Fortran common block optimization.

### Cluster 11: Feedback / Diagnostic File Handles

- **Globals**: `IPA_Feedback_dfe_fd`, `IPA_Feedback_dve_fd`, `IPA_Feedback_prg_fd`,
  `IPA_Feedback_con_fd`, `IPA_Feedback_exl_fd`
- **Key file**: `main/analyze/ipa_main.cxx` (all 5, opened here)
- **Reasoning**: File handles for feedback output. Need per-context instances for
  concurrent IPA. Simple to wrap in `IPA_Feedback_State`.

### Cluster 12: Field Reordering

- **Globals**: `merged_access`, `reorder_local_pool`
- **Key files**: `main/analyze/ipa_reorder.cxx`, `main/analyze/ipa_cg.cxx`,
  `main/analyze/ipa_main.cxx`
- **Reasoning**: Contained cluster. `merged_access` does leak into ipa_cg.cxx.

### Cluster 13: Visualization / daVinci

- **Globals**: `cg_display`
- **Key files**: `common/ipc_daVinci.cxx`, `main/analyze/ipa_cg.cxx`, `main/analyze/ipa_main.cxx`
- **Reasoning**: Optional debugging/visualization. Easy to make per-context or remove.

### Cluster 14: Symbol Table Merge Infrastructure

- **Globals**: `Aux_St_Tab`, `Aux_St_Table`, `ST_To_INITO_Map`, `Common_Block_Elements_Map`
- **Key files**: `common/ipc_symtab_merge.cxx`, `common/ipc_pic.cxx`
- **Reasoning**: Used during symbol table merging. These form a natural
  `IPC_Symtab_Merge_State`.

### Cluster 15: Devirtualization / Class Hierarchy

- **Globals**: `IPA_Class_Hierarchy`, `IPA_Enable_Devirtualization`
- **Key files**: `main/analyze/ipa_chg.cxx`, `main/analyze/ipa_devirtual.cxx`,
  `main/analyze/ipa_main.cxx`, `main/analyze/ipa_nystrom_alias_analyzer.cxx`
- **Reasoning**: Class hierarchy is built once and queried during devirtualization.
  Natural `IPA_CHG_State`.

---

## Hub Files (files using 10+ tracked globals)

These files are the hardest to refactor because they touch many global clusters at once.

### `main/analyze/ipa_main.cxx` (36 globals)

| Global | Cluster |
|--------|---------|
| `Global_mem_pool` | 5: Cprop State |
| `IPA_Call_Graph` | 1: Call Graph Core |
| `IPA_Class_Hierarchy` | 15: Devirt/CHG |
| `IPA_Concurrency_Graph` | PCG |
| `IPA_Constant_Count` | 5: Cprop State |
| `IPA_Enable_Array_Sections` | 3: Options/Config |
| `IPA_Enable_Cprop` | 3: Options/Config |
| `IPA_Enable_DFE` | 3: Options/Config |
| `IPA_Enable_Devirtualization` | 3: Options/Config |
| `IPA_Enable_Inline` | 3: Options/Config |
| `IPA_Enable_Padding` | 3: Options/Config |
| `IPA_Enable_SP_Partition` | 3: Options/Config |
| `IPA_Enable_Struct_Opt` | 3: Options/Config |
| `IPA_Feedback_con_fd` | 5: Cprop State |
| `IPA_Feedback_dfe_fd` | 11: Feedback FDs |
| `IPA_Feedback_dve_fd` | 11: Feedback FDs |
| `IPA_Feedback_prg_fd` | 11: Feedback FDs |
| `IPA_Max_Node_Clones` | 3: Options/Config |
| `IPA_Max_Total_Clones` | 5: Cprop State |
| `IPA_NODE_CONTEXT` | 2: Node Context |
| `IP_File_header` | 2: Node Context |
| `Ipa_cprop_pool` | 5: Cprop State |
| `Opt_Options_Inconsistent` | 1/2: Call Graph / Node |
| `Orig_Prog_Weight` | 4: Inline Stats |
| `Total_Dead_Function_Weight` | 4: Inline Stats |
| `Total_Inlined` | 4: Inline Stats |
| `Total_Must_Inlined` | 4: Inline Stats |
| `Total_Must_Not_Inlined` | 4: Inline Stats |
| `Total_Not_Inlined` | 4: Inline Stats |
| `Total_Prog_Size` | 4: Inline Stats |
| `Trace_IPA` | 3: Options/Config |
| `Trace_Perf` | 3: Options/Config |
| `Verbose` | 3: Options/Config |
| `cg_display` | 13: Visualization |
| `local_cprop_pool` | 5: Cprop State |
| `merged_access` | 12: Reorder |

### `main/analyze/ipa_cg.cxx` (28 globals)

| Global | Cluster |
|--------|---------|
| `Aux_Pu_Table` | 2: Node Context |
| `Aux_St_Table` | 14: Symtab Merge |
| `IPA_Call_Graph` | 1: Call Graph Core |
| `IPA_Call_Graph_Built` | 1: Call Graph Core |
| `IPA_Enable_Array_Sections` | 3: Options/Config |
| `IPA_Enable_AutoGnum` | 3: Options/Config |
| `IPA_Enable_DFE` | 3: Options/Config |
| `IPA_Enable_Inline` | 3: Options/Config |
| `IPA_Enable_SP_Partition` | 3: Options/Config |
| `IPA_Graph_Undirected` | 1: Call Graph Core |
| `IPA_Max_Depth` | 3: Options/Config |
| `IPA_NODE_CONTEXT` | 2: Node Context |
| `IP_File_header` | 2: Node Context |
| `Opt_Options_Inconsistent` | 1/2: Call Graph / Node |
| `Orig_Prog_WN_Count` | 4: Inline Stats |
| `Orig_Prog_Weight` | 4: Inline Stats |
| `Total_Dead_Function_WN_Count` | 4: Inline Stats |
| `Total_Dead_Function_Weight` | 4: Inline Stats |
| `Total_Must_Inlined` | 4: Inline Stats |
| `Total_Must_Not_Inlined` | 4: Inline Stats |
| `Total_Prog_Size` | 4: Inline Stats |
| `Total_call_freq` | 4: Inline Stats |
| `Total_cycle_count_2` | 4: Inline Stats |
| `Trace_IPA` | 3: Options/Config |
| `Trace_Perf` | 3: Options/Config |
| `cg_display` | 13: Visualization |
| `emit_order` | 1: Call Graph Core |
| `merged_access` | 12: Reorder |

### `main/optimize/ipo_main.cxx` (26 globals)

| Global | Cluster |
|--------|---------|
| `Aux_Pu_Table` | 2: Node Context |
| `IPA_Call_Graph` | 1: Call Graph Core |
| `IPA_Common_Table` | 10: Common/Padding |
| `IPA_Enable_Alias_Class` | 3: Options/Config |
| `IPA_Enable_Array_Sections` | 3: Options/Config |
| `IPA_Enable_AutoGnum` | 3: Options/Config |
| `IPA_Enable_Cloning` | 3: Options/Config |
| `IPA_Enable_Cprop` | 3: Options/Config |
| `IPA_Enable_Inline` | 3: Options/Config |
| `IPA_Enable_Padding` | 3: Options/Config |
| `IPA_Enable_SP_Partition` | 3: Options/Config |
| `IPA_Enable_Struct_Opt` | 3: Options/Config |
| `IPA_LNO_mem_pool` | Uncategorized |
| `IPA_NODE_CONTEXT` | 2: Node Context |
| `IPO_Total_Inlined` | 4: Inline Stats |
| `IP_File_header` | 2: Node Context |
| `Ip_alias_class` | 9: Alias Class |
| `Ip_alias_class_files` | 9: Alias Class |
| `Opt_Options_Inconsistent` | 1/2: Call Graph / Node |
| `Trace_IPA` | 3: Options/Config |
| `Trace_Perf` | 3: Options/Config |
| `emit_order` | 1: Call Graph Core |
| `inline_action_log` | Inline Script |
| `ipisr_cg` | Link/PIC |
| `one_got` | Link/PIC |
| `reorder_local_pool` | 12: Reorder |

### `main/analyze/ipa_inline.cxx` (18 globals)

| Global | Cluster |
|--------|---------|
| `Aux_Pu_Table` | 2: Node Context |
| `IPA_Bloat_Factor` | 3: Options/Config |
| `IPA_Enable_Cloning` | 3: Options/Config |
| `IPA_Enable_DFE` | 3: Options/Config |
| `IPA_Enable_Inline` | 3: Options/Config |
| `IPA_Feedback_prg_fd` | 11: Feedback FDs |
| `IPA_Max_Depth` | 3: Options/Config |
| `OPT_Cyg_Instrument` | 3: Options/Config |
| `Opt_Options_Inconsistent` | 1/2: Call Graph / Node |
| `Orig_Prog_WN_Count` | 4: Inline Stats |
| `Orig_Prog_Weight` | 4: Inline Stats |
| `Total_Inlined` | 4: Inline Stats |
| `Total_Not_Inlined` | 4: Inline Stats |
| `Total_Prog_Size` | 4: Inline Stats |
| `Total_call_freq` | 4: Inline Stats |
| `Total_cycle_count_2` | 4: Inline Stats |
| `Trace_IPA` | 3: Options/Config |
| `Trace_Perf` | 3: Options/Config |

### `inline/inline.cxx` (16 globals)

| Global | Cluster |
|--------|---------|
| `Aux_Pu_Table` | 2: Node Context |
| `Aux_Symbol_Info` | 6: Summary/Local |
| `IPA_Call_Graph` | 1: Call Graph Core |
| `IPA_Enable_DFE` | 3: Options/Config |
| `IPA_Max_Depth` | 3: Options/Config |
| `IP_File_header` | 2: Node Context |
| `Ipl_Function_Names` | 6: Summary/Local |
| `Ipl_Symbol_Names` | 6: Summary/Local |
| `OPT_Cyg_Instrument` | 3: Options/Config |
| `Orig_Prog_Weight` | 4: Inline Stats |
| `Summary` | 6: Summary/Local |
| `Total_Inlined` | 4: Inline Stats |
| `Total_Not_Inlined` | 4: Inline Stats |
| `Total_Prog_Size` | 4: Inline Stats |
| `Trace_CopyProp` | 3: Options/Config |
| `Verbose` | 3: Options/Config |

### `main/analyze/ipa_cprop.cxx` (15 globals)

| Global | Cluster |
|--------|---------|
| `Aux_Pu_Table` | 2: Node Context |
| `Aux_St_Table` | 14: Symtab Merge |
| `Global_mem_pool` | 5: Cprop State |
| `IPA_Call_Graph` | 1: Call Graph Core |
| `IPA_Constant_Count` | 5: Cprop State |
| `IPA_Enable_Cloning` | 3: Options/Config |
| `IPA_Enable_DFE` | 3: Options/Config |
| `IPA_Feedback_con_fd` | 5: Cprop State |
| `IPA_Max_Node_Clones` | 3: Options/Config |
| `IPA_Max_Total_Clones` | 5: Cprop State |
| `IPA_Num_Total_Clones` | 5: Cprop State |
| `Ipa_cprop_pool` | 5: Cprop State |
| `Orig_Prog_Weight` | 4: Inline Stats |
| `Total_Prog_Size` | 4: Inline Stats |
| `Total_call_freq` | 4: Inline Stats |

### `main/analyze/ipa_option.cxx` (11 globals)

| Global | Cluster |
|--------|---------|
| `Demangle` | 3: Options/Config |
| `IPA_Bloat_Factor` | 3: Options/Config |
| `IPA_Enable_AutoGnum` | 3: Options/Config |
| `IPA_Enable_Memtrace` | 3: Options/Config |
| `IPA_Max_Depth` | 3: Options/Config |
| `IPA_Skip_List` | 3: Options/Config |
| `ProMP_Listing` | 3: Options/Config |
| `Trace_IPA` | 3: Options/Config |
| `Trace_IPALNO` | 3: Options/Config |
| `Trace_Perf` | 3: Options/Config |
| `Verbose` | 3: Options/Config |

### `main/analyze/ipa_cprop_annot.cxx` (10 globals)

| Global | Cluster |
|--------|---------|
| `Aux_Pu_Table` | 2: Node Context |
| `IPA_Call_Graph` | 1: Call Graph Core |
| `IPA_Enable_Cloning` | 3: Options/Config |
| `IPA_Enable_DFE` | 3: Options/Config |
| `IPA_Max_Node_Clones` | 3: Options/Config |
| `IPA_NODE_CONTEXT` | 2: Node Context |
| `Ipa_cprop_pool` | 5: Cprop State |
| `ST_To_INITO_Map` | 14: Symtab Merge |
| `icall_def` | IPAA |
| `local_cprop_pool` | 5: Cprop State |

### `common/ipc_pic.cxx` (10 globals)

| Global | Cluster |
|--------|---------|
| `Aux_St_Tab` | 14: Symtab Merge |
| `Aux_St_Table` | 14: Symtab Merge |
| `IPA_Enable_AutoGnum` | 3: Options/Config |
| `IPA_Enable_Inline` | 3: Options/Config |
| `IPO_Gprel_Sym_Count` | Link/PIC |
| `ST_To_INITO_Map` | 14: Symtab Merge |
| `Trace_IPA` | 3: Options/Config |
| `Trace_Perf` | 3: Options/Config |
| `Verbose` | 3: Options/Config |
| `one_got` | Link/PIC |

---

## Cross-module globals (also referenced in be/com/)

These globals escape the `ipa/` directory and are used by the backend optimizer too.
They require the most careful handling for encapsulation.

| Global | ipa/ files | be/com/ files |
|--------|-----------|--------------|
| `Aux_Pu_Table` | 13 | `ipa_section_print.cxx` |
| `IPA_Call_Graph` | 21 | `ipa_section_print.cxx` |
| `IPA_NODE_CONTEXT` | 15 | `ipa_section_print.cxx` |
| `Ip_alias_class` | 3 | `opt_alias_mgr.cxx`, `opt_alias_rule.cxx`, `opt_points_to_non_template.cxx` |
| `Ipl_Summary_Symbol` | 4 | `ipa_section.cxx` |
| `Summary` | 14 | `constraint_graph_escanal.cxx`, `ipa_section_print.cxx`, `ipa_section.cxx`, `ipl_lno_util.cxx`, `wb_ipl_summary.cxx` |

---

## Recommended Context Struct Hierarchy

Based on the cluster analysis, here is the recommended struct hierarchy for
encapsulating IPA global state:

```cpp
struct IPA_Context {
  // Cluster 1: Call Graph Core
  IPA_CALL_GRAPH*     call_graph;         // IPA_Call_Graph
  BOOL                call_graph_built;    // IPA_Call_Graph_Built
  IPA_CALL_GRAPH*     graph_undirected;    // IPA_Graph_Undirected
  vector<IPA_NODE*>   emit_order;          // emit_order

  // Cluster 2: Per-Node State
  IP_FILE_HDR_TABLE   file_header;         // IP_File_header
  AUX_PU_TAB          aux_pu_table;        // Aux_Pu_Table

  // Cluster 3: Options (read-only after init, can be shared)
  IPA_Options*        options;             // all IPA_Enable_*, Trace_*, limits

  // Cluster 4: Inline Statistics
  IPA_Inline_Stats    inline_stats;        // Total_Inlined, Orig_Prog_Weight, etc.

  // Cluster 5: Constant Propagation
  IPA_Cprop_State     cprop;               // pools, counters, clone limits

  // Cluster 6: Summary / Local Analysis
  IPA_Summary_State   summary;             // Summary*, Ipl_* globals

  // Cluster 7-15: Smaller sub-contexts
  IPA_Section_State   sections;            // array section analysis
  IPA_Struct_Opt      struct_opt;          // struct optimization
  IPA_Alias_Class     alias_class;         // alias classification
  IPA_Common_State    common_blocks;       // common block / padding
  IPA_Feedback_State  feedback;            // feedback file handles
  IPA_Reorder_State   reorder;             // field reordering
  IPA_CHG_State       class_hierarchy;     // class hierarchy / devirt
  daVinci*            cg_display;          // visualization (optional)
};

struct IPA_Options {  // Cluster 3: read-only, shareable
  BOOL  enable_inline;
  BOOL  enable_dfe;
  BOOL  enable_cprop;
  BOOL  enable_cloning;
  BOOL  enable_padding;
  BOOL  enable_array_sections;
  BOOL  enable_alias_class;
  BOOL  enable_autoGnum;
  BOOL  enable_preempt;
  BOOL  enable_sp_partition;
  BOOL  enable_devirtualization;
  BOOL  enable_struct_opt;
  BOOL  enable_memtrace;
  // ... trace flags, limits, etc.
};

struct IPA_Inline_Stats {  // Cluster 4
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
  FB_FREQ total_cycle_count;
  FB_FREQ total_cycle_count_2;
};

struct IPA_Cprop_State {  // Cluster 5
  MEM_POOL ipa_cprop_pool;
  MEM_POOL local_cprop_pool;
  MEM_POOL global_mem_pool;
  INT      constant_count;
  UINT32   num_total_clones;
  UINT32   max_total_clones;
  FILE*    feedback_con_fd;
};
```

---

## Encapsulation Difficulty Ranking

| Difficulty | Cluster | Reason |
|-----------|---------|--------|
| EASY | 7: Array Sections | 2 files, tight coupling |
| EASY | 8: Struct Opt | 2 files, perfect containment |
| EASY | 11: Feedback FDs | Simple file handles, mostly in ipa_main.cxx |
| EASY | 12: Reorder | 3 files, contained |
| EASY | 13: Visualization | 3 files, optional feature |
| EASY | 15: Devirt/CHG | 4 files, clean interface |
| MEDIUM | 3: Options | 32 files, but read-only -- just pass a pointer |
| MEDIUM | 5: Cprop | 3 core files, some init in ipa_main.cxx |
| MEDIUM | 10: Common/Padding | 3 files, Fortran-specific |
| MEDIUM | 14: Symtab Merge | 2 core files, but used during critical merge phase |
| HARD | 4: Inline Stats | 5+ files, counters updated from many places |
| HARD | 6: Summary/Local | Summary used in 14 files, crosses local/main boundary |
| HARD | 9: Alias Class | Escapes to be/com/ (3 files there) |
| HARDEST | 1+2: Call Graph + Node Context | 21 files, 261 refs, the backbone of everything |

---

## Summary Statistics

- **Total globals tracked**: 98
- **Total .cxx files touched**: 62
- **Co-occurring pairs (2+ shared files)**: 436
- **Cross-cutting globals (10+ files)**: 7
- **Single-file globals**: 21
- **Two-file globals**: 37
- **Identified clusters**: 15
- **Hub file (most globals)**: main/analyze/ipa_main.cxx (36 tracked globals)
