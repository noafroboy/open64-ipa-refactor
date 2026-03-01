# ipa/main/analyze/ -- Global State Scan

## Status: COMPLETE

38 .cxx files scanned in `/Users/kevinliu/dev/open64/open64/osprey/ipa/main/analyze/`.

## File-scope globals found

### ipa_cg.cxx -- Call Graph (THE most global-heavy file)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPA_Call_Graph | IPA_CALL_GRAPH* | ipa_cg.cxx | 122 | no | **THE** central call graph singleton; used everywhere |
| IPA_Graph_Undirected | IPA_CALL_GRAPH* | ipa_cg.cxx | 125 | no | Secondary undirected graph for PU reordering (KEY ifdef) |
| IPA_Call_Graph_Tmp | IPA_CALL_GRAPH* | ipa_cg.cxx | 133 | yes | Temp storage when swapping IPA_Call_Graph |
| node_map | hash_map<IPA_NODE*,IPA_NODE*,...> | ipa_cg.cxx | 136 | yes | Maps nodes between IPA_Call_Graph and IPA_Graph_Undirected |
| q_order | vector<Nodes_To_Edge*> | ipa_cg.cxx | 137 | yes | Order for undirected graph processing |
| IPA_Call_Graph_Built | BOOL | ipa_cg.cxx | 139 | no | Flag: has call graph been built? |
| alt_entry_map | ALT_ENTRY_MAP* | ipa_cg.cxx | 142 | no | Map from alt entry to base entry |
| Total_Dead_Function_Weight | UINT32 | ipa_cg.cxx | 144 | no | Dead code elimination weight counter |
| Orig_Prog_Weight | UINT32 | ipa_cg.cxx | 145 | no | Original program weight |
| Orig_Prog_WN_Count | UINT32 | ipa_cg.cxx | 148 | no | Original WHIRL node count |
| Total_Dead_Function_WN_Count | UINT32 | ipa_cg.cxx | 149 | no | Dead function WHIRL node count |
| Total_cycle_count_2 | FB_FREQ | ipa_cg.cxx | 151 | no | Feedback frequency counter |
| Total_Must_Inlined | INT | ipa_cg.cxx | 157 | no | Count of must-inline callsites |
| Total_Must_Not_Inlined | INT | ipa_cg.cxx | 158 | no | Count of must-not-inline callsites |
| Total_call_freq | FB_FREQ | ipa_cg.cxx | 160 | no | Total call frequency |
| Total_cycle_count | FB_FREQ | ipa_cg.cxx | 161 | no | Total cycle count |
| options | std::vector<char*> | ipa_cg.cxx | 502 | no | Optimization options from files |
| Opt_Options_Inconsistent | BOOL | ipa_cg.cxx | 504 | no | Flag: are options inconsistent? |
| IPA_NODE::next_file_id | mINT32 (static class member) | ipa_cg.cxx | 505 | class-static | Static member init: next file ID counter |
| addr_node_map | ADDR_NODE_MAP | ipa_cg.cxx | 696 | yes | Maps address to IPA_NODE* |
| edges | vector<IPA_EDGE*> | ipa_cg.cxx | 1406 | yes | Edges in the changing undirected graph |
| emit_order | vector<IPA_NODE*> | ipa_cg.cxx | 1498 | no | Order in which nodes should be emitted |
| tracked_global_var_st | ST* | ipa_cg.cxx | 2256 | yes | Tracked global var for no-return analysis |
| return_wn | WN* | ipa_cg.cxx | 2379 | yes | Return WN for tree walker |
| continue_with_walk_tree_for_returns_and_global_var | int | ipa_cg.cxx | 2380 | yes | Control flag for tree walker |
| phi_outermost_argument | int | ipa_cg.cxx | 2449 | yes | Phi node analysis state |
| phi_if_true_argument | int | ipa_cg.cxx | 2450 | yes | Phi node analysis state |
| phi_if_false_argument | int | ipa_cg.cxx | 2451 | yes | Phi node analysis state |
| in_if_true_branch | int | ipa_cg.cxx | 2452 | yes | Phi node analysis state |
| in_if_false_branch | int | ipa_cg.cxx | 2453 | yes | Phi node analysis state |
| continue_with_finding_reaching_def | int | ipa_cg.cxx | 2454 | yes | Phi node analysis state |

### ipa_cprop_annot.cxx -- Constant Propagation Annotations

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| cd_status | CONDITION_STATE* | ipa_cprop_annot.cxx | 76 | no | Status of all conditional stmts in a PU |
| Ipa_cprop_pool | MEM_POOL | ipa_cprop_annot.cxx | 79 | no | **MEM_POOL** for expression evaluations |
| local_cprop_pool | MEM_POOL | ipa_cprop_annot.cxx | 80 | no | **MEM_POOL** for local cprop |
| Current_caller | IPA_NODE* | ipa_cprop_annot.cxx | 84 | yes | Current caller node during evaluation |
| callsite_map | IPA_NODE** | ipa_cprop_annot.cxx | 85 | yes | Callsite-to-callee mapping |
| current_gannot | GLOBAL_ANNOT* | ipa_cprop_annot.cxx | 89 | yes | Current global annotation |
| ipa_proc | SUMMARY_PROCEDURE* | ipa_cprop_annot.cxx | 91 | yes | Current summary procedure |
| ipa_callsite | SUMMARY_CALLSITE* | ipa_cprop_annot.cxx | 92 | yes | Current summary callsite |
| ipa_formal | SUMMARY_FORMAL* | ipa_cprop_annot.cxx | 93 | yes | Current summary formals |
| ipa_actual | SUMMARY_ACTUAL* | ipa_cprop_annot.cxx | 94 | yes | Current summary actuals |
| ipa_value | SUMMARY_VALUE* | ipa_cprop_annot.cxx | 95 | yes | Current summary values |
| ipa_expr | SUMMARY_EXPR* | ipa_cprop_annot.cxx | 96 | yes | Current summary expressions |
| ipa_phi | SUMMARY_PHI* | ipa_cprop_annot.cxx | 97 | yes | Current summary phi nodes |
| ipa_chi | SUMMARY_CHI* | ipa_cprop_annot.cxx | 98 | yes | Current summary chi nodes |
| ipa_symbol | const SUMMARY_SYMBOL* | ipa_cprop_annot.cxx | 99 | yes | Current summary symbols |
| ipa_stmt | SUMMARY_STMT* | ipa_cprop_annot.cxx | 100 | yes | Current summary statements |
| ipa_ctrl_dep | SUMMARY_CONTROL_DEPENDENCE* | ipa_cprop_annot.cxx | 101 | yes | Current summary control deps |
| ipa_stid | SUMMARY_STID* | ipa_cprop_annot.cxx | 102 | yes | Current summary STIDs |
| formal_value | VALUE_DYN_ARRAY* | ipa_cprop_annot.cxx | 105 | yes | Propagated formal parameter values |
| eval_value | VALUE_VECTOR* | ipa_cprop_annot.cxx | 109 | yes | Temp values in expression evaluation |
| eval_hash | EVAL_HASH* | ipa_cprop_annot.cxx | 120 | yes | Hash map for evaluation caching |
| indent | INT | ipa_cprop_annot.cxx | 2188 | yes | Indentation level for debug printing |

### ipa_cprop.cxx -- Constant Propagation

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPA_Max_Total_Clones | UINT32 | ipa_cprop.cxx | 83 | no | Maximum total clone count |
| IPA_Num_Total_Clones | UINT32 | ipa_cprop.cxx | 84 | no | Current total clone count |
| IPA_Constant_Count | INT | ipa_cprop.cxx | 86 | no | Number of constants found |
| Global_mem_pool | MEM_POOL | ipa_cprop.cxx | 87 | no | **MEM_POOL** for global cprop |
| max_param_count | const UINT32 | ipa_cprop.cxx | 256 | no | Tuning constant for cloning (=10) |
| min_hotness | const float | ipa_cprop.cxx | 257 | no | Tuning constant for cloning (=7.0) |
| callee_limit | const UINT32 | ipa_cprop.cxx | 258 | no | Tuning constant for cloning (=500) |
| GLOBAL_ANNOT::Size | UINT32 (static class member) | ipa_cprop.cxx | 815 | class-static | Static member: global annotation size |
| GLOBAL_ANNOT::Common_ST | ST_IDX* (static class member) | ipa_cprop.cxx | 816 | class-static | Static member: common ST array |
| GLOBAL_ANNOT::Offset_Size_To_ST | OFFSET_SIZE_TO_ST_IDX_MAP* (static class member) | ipa_cprop.cxx | 817 | class-static | Static member: offset-size map |

### ipaa.cxx -- Interprocedural Alias Analysis

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPAA_Pool | MEM_POOL* | ipaa.cxx | 123 | no | **MEM_POOL** pointer for IPAA |
| Mext_Size | INT | ipaa.cxx | 126 | no | Merged external symbol table size |
| Trace_IPAA | BOOL | ipaa.cxx | 131 | no | General IPAA trace flag |
| Trace_Detail | BOOL | ipaa.cxx | 132 | no | Detail IPAA trace flag |
| Trace_Stats | BOOL | ipaa.cxx | 133 | no | IPAA statistics trace flag |
| Trace_Iterator | BOOL | ipaa.cxx | 134 | no | IPAA solver iterator trace flag |
| Log_IPAA | BOOL | ipaa.cxx | 135 | no | Trace log for IPAA flag |
| icall_eref | IPAA_OBJECT_REF_SET* | ipaa.cxx | 139 | no | Global mod/ref: indirect call eref |
| icall_def | IPAA_OBJECT_REF_SET* | ipaa.cxx | 140 | no | Global mod/ref: indirect call def |
| icall_kill | IPAA_OBJECT_REF_SET* | ipaa.cxx | 141 | no | Global mod/ref: indirect call kill |
| IPAA_Emit_Pool | MEM_POOL | ipaa.cxx | 3015 | no | **MEM_POOL** for summary emission |
| IPAA_Callsite_Pool | MEM_POOL* | ipaa.cxx | 3020 | no | **MEM_POOL** for callsite mapping |
| String_Hash_Table | STRING_HASH_TABLE* | ipaa.cxx | 3026 | no | Hash table for summary strings |
| Hash_Func | String_Hash | ipaa.cxx | 3027 | no | Hash function object |
| Name_Table | INT32* | ipaa.cxx | 3032 | yes | Name lookup table by merged symtab index |
| Symref_Table | SYMREF_IX* | ipaa.cxx | 3037 | yes | Symref lookup table by merged symtab index |

### ipa_option.cxx -- IPA Options

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| rcs_id | char* | ipa_option.cxx | 61 | yes | RCS version string (ifdef _KEEP_RCS_ID) |
| Ipa_File_Name | char* | ipa_option.cxx | 89 | no | IPAA summary file name |
| Ipa_File | FILE* | ipa_option.cxx | 90 | no | IPAA summary file handle |
| IPA_Skip_List | SKIPLIST* | ipa_option.cxx | 95 | no | List of skip options |
| Trace_IPA | BOOL | ipa_option.cxx | 97 | no | Main IPA progress trace flag |
| Trace_Perf | BOOL | ipa_option.cxx | 98 | no | Performance trace flag |
| Trace_IPALNO | BOOL | ipa_option.cxx | 99 | no | IPA-to-LNO correctness trace flag |
| Verbose | BOOL | ipa_option.cxx | 102 | no | Verbose flag |
| Demangle | BOOL | ipa_option.cxx | 103 | no | Demangle flag |
| ProMP_Listing | BOOL | ipa_option.cxx | 105 | no | ProMP listing flag |

### ipa_inline.cxx -- Inlining Analysis

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Total_Prog_Size | INT | ipa_inline.cxx | 96 | no | Size of the final program |
| Total_Inlined | INT | ipa_inline.cxx | 97 | no | Total inlined callsites |
| Total_Not_Inlined | INT | ipa_inline.cxx | 98 | no | Total not-inlined callsites |
| Max_Total_Prog_Size | UINT32 | ipa_inline.cxx | 99 | yes | Max program size allowed |
| Real_Orig_Prog_Weight | INT | ipa_inline.cxx | 100 | yes | Orig_Prog_Weight minus dead code |
| non_aggr_callee_limit | UINT32 | ipa_inline.cxx | 101 | yes | Non-aggressive callee size limit |
| Real_Orig_WN_Count | UINT32 | ipa_inline.cxx | 102 | yes | Orig WN count minus dead code |
| N_inlining | FILE* | ipa_inline.cxx | 104 | no | Trace file for non-inlining reasons |
| Y_inlining | FILE* | ipa_inline.cxx | 105 | no | Trace file for inlining decisions |
| e_weight | FILE* | ipa_inline.cxx | 106 | no | Trace file for edge weights |
| Verbose_inlining | FILE* | ipa_inline.cxx | 107 | no | Verbose inlining trace file |
| OPC_UNKNOWN | OPCODE | ipa_inline.cxx | 111 | yes | Unknown opcode sentinel |
| Stid_Opcode | OPCODE[] | ipa_inline.cxx | 114 | yes (implicit) | Opcode lookup table for STIDs, indexed by MTYPE |
| inline_count | INLINE_COUNTER_ARRAY* | ipa_inline.cxx | 387 | yes | Per-PU inline call counter |

### ipa_reorder.cxx -- Field Reordering

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| reorder_candidate | REORDER_CAND | ipa_reorder.cxx | 80 | no | Reorder candidates list |
| can_be_reordered_types | TYPE_LIST | ipa_reorder.cxx | 81 | no | Types that can be reordered |
| invalid_reorder_types | TYPE_LIST | ipa_reorder.cxx | 82 | no | Types that cannot be reordered |
| Ty_to_cand_map | TY_TO_CAND_MAP | ipa_reorder.cxx | 84 | no | Map from type to candidate |
| Ptr_and_ty_vector | PTR_AND_TY_VECTOR* | ipa_reorder.cxx | 85 | no | Pointer-type vector |
| merged_access | MERGED_ACCESS_VECTOR* | ipa_reorder.cxx | 89 | no | Merged field access info |
| reorder_local_pool | MEM_POOL | ipa_reorder.cxx | 90 | no | **MEM_POOL** for reordering |
| ty_to_access_map | TY_TO_ACCESS_MAP* | ipa_reorder.cxx | 92 | no | Map from type to access info |
| visited | BOOL* | ipa_reorder.cxx | 93 | no | Visited array for traversal |

### ipa_preopt.cxx -- Pre-optimization

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPA_preopt_pool | MEM_POOL | ipa_preopt.cxx | 162 | yes | **MEM_POOL** for preopt |
| IPA_preopt_initialized | BOOL | ipa_preopt.cxx | 163 | yes | Initialization flag |
| Aux_file_info | AUX_FILE_INFO* | ipa_preopt.cxx | 164 | yes | Auxiliary file info array |

### ipa_section_prop.cxx -- Array Section Propagation

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPA_array_prop_pool | MEM_POOL | ipa_section_prop.cxx | 75 | no | **MEM_POOL** for array propagation |
| Trace_IPA_Sections | BOOL | ipa_section_prop.cxx | 76 | no | Trace flag for IPA sections |
| IPA_array_prop_pool_initialized | BOOL | ipa_section_prop.cxx | 78 | yes | Pool initialization flag |
| expressions | SUMMARY_EXPR* | ipa_section_prop.cxx | 842 | yes | Caller summary expressions (set to avoid deep arg passing) |
| values | SUMMARY_VALUE* | ipa_section_prop.cxx | 843 | yes | Caller summary values |
| formals | SUMMARY_FORMAL* | ipa_section_prop.cxx | 844 | yes | Caller summary formals |

### ipa_pad.cxx -- Common Block Padding/Splitting

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPO_Pad_Count | INT | ipa_pad.cxx | 74 | no | Padding count |
| IPA_Common_Table | COMMON_SNODE_TBL* | ipa_pad.cxx | 75 | no | Common block symbol table |
| IPA_Pad_Split_Mem_Pool | MEM_POOL | ipa_pad.cxx | 77 | yes | **MEM_POOL** for pad/split |
| trace_split_common | BOOL | ipa_pad.cxx | 78 | yes | Trace flag for split common |
| Primary_Cache | INT64 | ipa_pad.cxx | 961 | no | Primary cache size constant (16K) |
| Secondary_Cache | INT64 | ipa_pad.cxx | 962 | no | Secondary cache size constant (512K) |
| Primary_Delta | INT64 | ipa_pad.cxx | 963 | no | Primary cache delta (819) |
| Secondary_Delta | INT64 | ipa_pad.cxx | 964 | no | Secondary cache delta (26214) |

### ipa_devirtual.cxx -- Devirtualization

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| live_class | hash_map<TY_INDEX,PU_IDX> | ipa_devirtual.cxx | 74 | yes | Instantiated classes and their constructors |
| base_sub_map | hash_map<TY_INDEX,TY_INDEX> | ipa_devirtual.cxx | 80 | yes | Base-to-subclass map |
| pu_node_index_map | hash_map<PU_IDX,NODE_INDEX> | ipa_devirtual.cxx | 83 | yes | PU to node index map |
| st_inito_map | hash_map<ST_IDX,INITO_IDX> | ipa_devirtual.cxx | 86 | yes | ST to INITO index map |
| conversion_list | queue<CONVERSION_CANDIDATE> | ipa_devirtual.cxx | 89 | yes | Virtual call conversion candidates |
| theDevirCallsiteMap | IPA_VIRTUAL_FUNCTION_TRANSFORM::DevirCallsiteMap | ipa_devirtual.cxx | 817 | yes | Devirtualization callsite map |
| __MISS_VIRTUAL_FUNCTION__ | unsigned int[2000] | ipa_devirtual.cxx | 2602 | no | Miss profiling array (debug/profiling code) |
| __HIT_VIRTUAL_FUNCTION__ | unsigned int[2000] | ipa_devirtual.cxx | 2602 | no | Hit profiling array (debug/profiling code) |

### ipa_builtins.cxx -- IPA Builtins

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPA_builtins_list | std::vector<IPA_BUILTIN*> | ipa_builtins.cxx | 51 | no | List of all IPA builtins |
| IPA_builtin_ctype_b | ST* | ipa_builtins.cxx | 54 | no | ctype_b char map ST |
| IPA_builtin_ctype_tolower | ST* | ipa_builtins.cxx | 55 | no | ctype_tolower char map ST |
| IPA_builtin_ctype_toupper | ST* | ipa_builtins.cxx | 56 | no | ctype_toupper char map ST |
| IPA_Builtins_Mem_Pool | MEM_POOL | ipa_builtins.cxx | 58 | yes | **MEM_POOL** for builtins |
| path__ctype_b | unsigned short int[] | ipa_builtins.cxx | 176 | no | Hardcoded ctype_b lookup table |
| path__ctype_tolower | int[] | ipa_builtins.cxx | 179 | no | Hardcoded ctype_tolower lookup table |
| path__ctype_toupper | int[] | ipa_builtins.cxx | 182 | no | Hardcoded ctype_toupper lookup table |

### ipa_struct_opt.cxx -- Struct Optimization

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Struct_field_layout | Field_pos* | ipa_struct_opt.cxx | 41 | no | Struct field layout array |
| Struct_split_count | INT | ipa_struct_opt.cxx | 42 | no | Split count |
| complete_struct_relayout_type_id | TYPE_ID | ipa_struct_opt.cxx | 44 | no | Relayout type ID |
| struct_with_field_pointing_to_complete_struct_relayout_type_id | TYPE_ID[] | ipa_struct_opt.cxx | 45 | no | Array of type IDs for struct relayout |
| struct_with_field_pointing_to_complete_struct_relayout_field_num | int[] | ipa_struct_opt.cxx | 47 | no | Field numbers for struct relayout |
| num_structs_with_field_pointing_to_complete_struct_relayout | int | ipa_struct_opt.cxx | 49 | no | Count of relayout structs |

### ipa_df.cxx -- Data Flow Framework

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPA_Should_Rebuild_DFN | BOOL | ipa_df.cxx | 247 | no | Flag: rebuild depth-first numbering? |

### ipa_nested_pu.cxx -- Nested PU Handling

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| nested_pu_parent_map | IPA_NESTED_PU_PARENT_MAP* | ipa_nested_pu.cxx | 86 | yes | Parent-child PU relation map |

### ipa_solver.cxx -- Dataflow Solver

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| rcs_id | char* | ipa_solver.cxx | 69 | yes | RCS version string (ifdef _KEEP_RCS_ID) |
| Trace_CG | BOOL | ipa_solver.cxx | 98 | yes | Call graph trace flag |

### ip_graph_trav.cxx -- Graph Traversal

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPA_CG_Max_Depth | INT | ip_graph_trav.cxx | 48 | no | Maximum depth of the call graph |
| Level_Count | INT* | ip_graph_trav.cxx | 50 | yes | Level count array for traversal |

### inline_script_parser.cxx -- Inline Script Parser

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| inline_action_log | const char* | inline_script_parser.cxx | 41 | no | Log filename for inline actions |
| inline_script_log | const char* | inline_script_parser.cxx | 42 | no | Log filename for inline scripts |
| inline_spec_table | CALLER_HTABLE | inline_script_parser.cxx | 44 | yes | Hash table of inline specifications |
| inline_script_init | BOOL | inline_script_parser.cxx | 46 | yes | Initialization flag |

### cgb.cxx -- Call Graph Browser

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| CGB_Current_Phase | CGB_PHASE | cgb.cxx | 62 | yes | Current phase of CG browser |
| CGBS_Command_List | CGB_COMMAND[] | cgb.cxx | 64 | yes | Summary command list (26 entries) |
| CGB_Command_List | CGB_COMMAND[] | cgb.cxx | 97 | yes | Main command list |

### ipa_main.cxx -- Main IPA Driver

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| STDOUT | FILE* | ipa_main.cxx | 96 | no | Alias for stdout |

### ipa_feedback.cxx -- IPA Feedback

(No file-scope variable definitions; only extern declarations referencing globals defined elsewhere.)

### ipa_aot.cxx, ipa_be_write.cxx, ipa_chg.cxx, ipa_cost.cxx, ipa_lno_write.cxx, ipa_nystrom_alias_analyzer.cxx, ipa_pcg.cxx, ipa_reshape.cxx, ipa_section_annot.cxx, ipa_summary.cxx, sparse_bv.cxx, wb_ipa.cxx, cg_browser.cxx, cg_print.cxx, cg_summary.cxx, cgb_carray.cxx, cgb_ipa.cxx

These files have NO file-scope variable globals (they only contain static functions, class method definitions, or extern function declarations).

---

## Extern declarations (references to globals defined elsewhere)

| Declaration | File | Line | Notes |
|-------------|------|------|-------|
| extern void (*Preprocess_struct_access_p)(void) | ipa_main.cxx | 87 | Function pointer for struct access preprocessing |
| extern void IPA_struct_opt_legality(void) | ipa_main.cxx | 262 | Forward declaration |
| extern void IPA_identify_no_return_procs(void) | ipa_main.cxx | 264 | Forward declaration |
| extern TY_TO_CAND_MAP Ty_to_cand_map | ipa_reorder.cxx | 78 | References global in same file |
| extern IPA_FEEDBACK_STRINGS *IPA_Fbk_Strings | ipa_feedback.cxx | 80 | Feedback strings (defined elsewhere) |
| extern IPA_FEEDBACK_STRINGS *IPA_Fbk_Strings | ipa_inline.cxx | 176 | Same, duplicate extern |
| extern "C" void add_to_tmp_file_list(char*) | ipaa.cxx | 120 | Linker utility |
| extern "C" INT32 C_Emit_id_string(const char*) | ipa_feedback.cxx | 83 | String hashing wrapper |
| extern mUINT32 Struct_split_candidate_index | ipa_struct_opt.cxx | 108 | Struct split candidate (defined in another TU) |
| extern mUINT32 Struct_update_index | ipa_struct_opt.cxx | 109 | Struct update index (defined in another TU) |

---

## Summary

- **~140 file-scope globals found** (variables, not counting static functions)
- **~72 static variables** at file scope
- **~62 non-static (externally visible) globals**
- **~10 extern declarations** referencing globals defined in other translation units
- **12 MEM_POOL instances** (critical for memory management):
  - `Global_mem_pool` (ipa_cprop.cxx)
  - `Ipa_cprop_pool` (ipa_cprop_annot.cxx)
  - `local_cprop_pool` (ipa_cprop_annot.cxx)
  - `IPAA_Pool` (ipaa.cxx, pointer)
  - `IPAA_Emit_Pool` (ipaa.cxx)
  - `IPAA_Callsite_Pool` (ipaa.cxx, pointer)
  - `IPA_array_prop_pool` (ipa_section_prop.cxx)
  - `reorder_local_pool` (ipa_reorder.cxx)
  - `IPA_preopt_pool` (ipa_preopt.cxx)
  - `IPA_Pad_Split_Mem_Pool` (ipa_pad.cxx)
  - `IPA_Builtins_Mem_Pool` (ipa_builtins.cxx)
- **3 static class member definitions**: `GLOBAL_ANNOT::Size`, `GLOBAL_ANNOT::Common_ST`, `GLOBAL_ANNOT::Offset_Size_To_ST` (ipa_cprop.cxx); plus `IPA_NODE::next_file_id` (ipa_cg.cxx)

## Key observations for refactoring (Goal 5)

1. **`IPA_Call_Graph`** (ipa_cg.cxx:122) is the single most important global. It is "the" call graph used by all IPA passes. Comments in the source explicitly note it is used as a global even inside class member functions where `this` would be appropriate.

2. **ipa_cg.cxx is the worst offender** with ~30 file-scope globals including the central call graph, program-wide statistics counters, and phi-node analysis state variables.

3. **ipa_cprop_annot.cxx uses 21 static globals** as a substitute for passing arguments -- the comment at line 839 in ipa_section_prop.cxx even admits: "These are made file statics so that we don't pass 6 arguments into possibly deep recursive calls."

4. **MEM_POOLs are scattered everywhere** (12 instances). These would need to be grouped into a context struct for multi-instance support.

5. **Trace/debug flags** (Trace_IPA, Trace_IPAA, Trace_Detail, etc.) are numerous non-static globals that would need to be moved into a configuration context.

6. **Statistics counters** (Total_Prog_Size, Total_Inlined, Orig_Prog_Weight, etc.) are non-static globals spread across ipa_cg.cxx and ipa_inline.cxx.

7. **ipa_devirtual.cxx** uses 6 static hash_map/queue globals for devirtualization state -- these are self-contained and relatively easy to encapsulate.

8. **ipa_reorder.cxx** has 9 non-static globals for field reordering -- all would need encapsulation.
