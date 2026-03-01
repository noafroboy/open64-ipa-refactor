# ipa/main/optimize/ -- Global State Scan

## Status: COMPLETE

Scanned all 16 .cxx files in `osprey/ipa/main/optimize/`. No .c files present.

---

## File-scope globals found

### ipo_main.cxx (the main IPO driver -- heaviest file)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `IPO_Total_Inlined` | `INT` | ipo_main.cxx | 215 | no | Global counter of inlined calls |
| `REC_OVERFLOW_COST` | `float` | ipo_main.cxx | 216 | yes | Register overflow cost constant |
| `OVERFLOW_COST` | `float` | ipo_main.cxx | 217 | yes | Register overflow cost constant |
| `one_got` | `BOOL` | ipo_main.cxx | 218 | no | Global flag (one GOT?) |
| `Recycle_mem_pool` | `MEM_POOL` | ipo_main.cxx | 220 | yes | Memory pool for recycling |
| `IPA_LNO_mem_pool` | `MEM_POOL` | ipo_main.cxx | 221 | no | Memory pool for IPA-LNO summary |
| `IPA_LNO_Summary` | `IPA_LNO_WRITE_SUMMARY*` | ipo_main.cxx | 222 | yes | LNO summary pointer |
| `inline_list` | `vector<inline_info>` | ipo_main.cxx | 233 | no | List of recursive inlining candidates (under `#ifdef KEY`) |
| `Num_In_Calls_Processed` | `NUM_CALLS_PROCESSED*` | ipo_main.cxx | 240 | yes | Aux node data for tracking processed in-calls |
| `Num_Out_Calls_Processed` | `NUM_CALLS_PROCESSED*` | ipo_main.cxx | 241 | yes | Aux node data for tracking processed out-calls |
| `Global_Defs[MAX_GLOBAL_AS_LOCAL]` | `WN*[]` | ipo_main.cxx | 249 | yes | Array of WN defs for global-as-local candidates |
| `Global_Flags[MAX_GLOBAL_AS_LOCAL]` | `DEF_FLAG[]` | ipo_main.cxx | 250 | yes | Flags for global-as-local candidates |
| `Global_Sts[MAX_GLOBAL_AS_LOCAL]` | `ST*[]` | ipo_main.cxx | 251 | yes | ST array for global-as-local candidates |
| `Global_As_Local_Ndx` | `INT` | ipo_main.cxx | 252 | yes | Index into Global_Defs/Flags/Sts |
| `Global_As_Local_Hash` | `GLOBAL_AS_LOCAL_HASH*` | ipo_main.cxx | 254 | yes | Hash table for global-as-local |
| `PU_Parent_Map` | `WN_MAP` | ipo_main.cxx | 255 | yes | Parent map for WN tree |
| `PU_Map_Tab` | `WN_MAP_TAB*` | ipo_main.cxx | 256 | yes | Map table for WN parent tracking |
| `Temp_pool` | `MEM_POOL` | ipo_main.cxx | 257 | yes | Temporary memory pool |
| `REG_FB_INFO_TABLE` | `REG_FB_MAP` | ipo_main.cxx | 184 | yes | Hash map for register feedback info |
| `REG_BUDGET_TABLE` | `REG_BUDGET_MAP` | ipo_main.cxx | 203 | yes | Hash map for register budgets |
| `have_open_input_file` | `BOOL` | ipo_main.cxx | 158 | yes | Flag for open input file state |
| `fin` | `FILE*` | ipo_main.cxx | 159 | yes | File pointer for input |
| `BE_symtab_initialized` | `BOOL` | ipo_main.cxx | 2658 | yes | Flag for BE symbol table init |
| `ipisr_cg` | `vector<mINT32>` | ipo_main.cxx | 2307 | no | IPISR call graph data (under `#if defined(TARG_SL)`) |

#### Extern declarations in ipo_main.cxx

| Declaration | File | Line | Notes |
|-------------|------|------|-------|
| `extern void IPO_WN_Update_For_Struct_Opt(IPA_NODE *)` | ipo_main.cxx | 142 | Function decl |
| `extern void IPO_WN_Update_For_Complete_Structure_Relayout_Legality(IPA_NODE *)` | ipo_main.cxx | 143 | Function decl |
| `extern void IPO_WN_Update_For_Array_Remapping_Legality(IPA_NODE *, int, int *)` | ipo_main.cxx | 144 | Function decl |
| `extern void IPO_Identify_Single_Define_To_HeapAlloced_GlobalVar(WN *wn)` | ipo_main.cxx | 145 | Function decl |
| `extern "C" void add_to_tmp_file_list(char*)` | ipo_main.cxx | 149 | C-linkage function decl |
| `extern MEM_POOL Ipo_mem_pool` | ipo_main.cxx | 152 | References global defined in ipo_inline.cxx |
| `extern WN_MAP Parent_Map` | ipo_main.cxx | 153 | References global defined in ipo_inline.cxx |
| `extern char * preopt_path` | ipo_main.cxx | 155 | Declared in ld/option.c |
| `extern int preopt_opened` | ipo_main.cxx | 156 | Declared in ld/option.c |
| `extern FILE *Y_inlining` | ipo_main.cxx | 157 | Declared in analyze/ipa_inline.cxx |
| `extern void IPO_Process_Virtual_Functions(IPA_NODE *)` | ipo_main.cxx | 652 | Function decl |
| `extern void IPO_Process_Icalls(IPA_NODE *)` | ipo_main.cxx | 653 | Function decl |
| `extern void IPA_update_ehinfo_in_pu(IPA_NODE *)` | ipo_main.cxx | 654 | Function decl |
| `extern void WN_free_input(void *handle, off_t mapped_size)` | ipo_main.cxx | 913 | Function decl |

---

### ipo_inline.cxx (inlining implementation -- second heaviest)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `initial_initv_tab_size` | `INT` | ipo_inline.cxx | 80 | yes | Saved initial INITV table size |
| `Ipo_mem_pool` | `MEM_POOL` | ipo_inline.cxx | 82 | no | **Definition** of global memory pool used across files |
| `Parent_Map` | `WN_MAP` | ipo_inline.cxx | 83 | no | **Definition** of global parent map used across files |
| `dope_str_prefix` | `const char * const` | ipo_inline.cxx | 939 | yes | String constant ".dope." (under `#ifdef KEY`) |
| `dope_str_prefix_len` | `const INT` | ipo_inline.cxx | 940 | yes | Length constant = 6 (under `#ifdef KEY`) |
| `processed_types` | `vector<TY_IDX, mempool_allocator<TY_IDX>>` | ipo_inline.cxx | 1970 | yes | Vector tracking processed types; initialized with Malloc_Mem_Pool |
| `caller_map_tab` | `WN_MAP_TAB*` | ipo_inline.cxx | 1464 | no | Global map table for caller |
| `callee_map_tab` | `WN_MAP_TAB*` | ipo_inline.cxx | 1465 | no | Global map table for callee |

#### Extern declarations in ipo_inline.cxx

| Declaration | File | Line | Notes |
|-------------|------|------|-------|
| `extern FILE *Y_inlining` | ipo_inline.cxx | 85 | Inlining trace file |
| `extern DST_IDX get_abstract_origin(DST_IDX)` | ipo_inline.cxx | 88 | Function decl |
| `extern BOOL Alias_Nystrom_Analyzer` | ipo_inline.cxx | 92 | Flag for Nystrom alias analyzer |
| `extern void Init_Operator_To_Opcode_Table()` | ipo_inline.cxx | 1453 | Function decl |

---

### ipo_struct_opt.cxx (structure optimization)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `candidate_ty_idx` | `mUINT32` | ipo_struct_opt.cxx | 74 | no | Candidate type index for struct split |
| `candidate_fld_id` | `mUINT32` | ipo_struct_opt.cxx | 75 | no | Candidate field ID for struct split |
| `Struct_split_candidate_index` | `mUINT32` | ipo_struct_opt.cxx | 76 | no | Index into struct split candidates |
| `Struct_update_index` | `mUINT32` | ipo_struct_opt.cxx | 77 | no | Index for struct update tracking |
| `PU_Parent_Map` | `WN_MAP` | ipo_struct_opt.cxx | 79 | yes | Parent map (separate copy from ipo_main.cxx) |
| `PU_Map_Tab` | `WN_MAP_TAB*` | ipo_struct_opt.cxx | 80 | yes | Map table (separate copy from ipo_main.cxx) |
| `fld_st[10]` | `ST*[]` | ipo_struct_opt.cxx | 88 | no | Fixed-size array of field STs (TODO: dynamic alloc) |
| `Struct_split_types[20]` | `TY_IDX[]` | ipo_struct_opt.cxx | 89 | no | Fixed-size array of split types (TODO: dynamic alloc) |
| `preg_id` | `WN_OFFSET` | ipo_struct_opt.cxx | 91 | no | Preg offset counter |
| `field_map_info` | `FLD_MAP*` | ipo_struct_opt.cxx | 101 | yes | Field map info pointer |
| `singleDefHeapAllocedGlbls` | `__gnu_cxx::hash_map<ST_IDX, ST*>` | ipo_struct_opt.cxx | 103 | no | Map of single-def heap-allocated globals |
| `global_ptrs_for_complete_struct_relayout_created` | `int` | ipo_struct_opt.cxx | 944 | yes | Flag: global pointers created for struct relayout |
| `complete_struct_relayout_size` | `int` | ipo_struct_opt.cxx | 945 | yes | Size of completely relayed-out struct |
| `orig_complete_struct_relayout_size` | `int` | ipo_struct_opt.cxx | 946 | yes | Original size before relayout |
| `ptr_to_gptr_ty_idx[MAX+1]` | `TY_IDX[]` | ipo_struct_opt.cxx | 947 | yes | Array of TY_IDX for global pointer types |
| `gptr_st[MAX+1]` | `ST*[]` | ipo_struct_opt.cxx | 949 | yes | Array of ST for global pointers |
| `field_offset_in_complete_struct_relayout[MAX+1]` | `int[]` | ipo_struct_opt.cxx | 950 | yes | Array of field offsets |
| `num_fields_in_complete_struct_relayout` | `int` | ipo_struct_opt.cxx | 952 | yes | Number of fields in relayout |
| `continue_with_complete_struct_relayout` | `int` | ipo_struct_opt.cxx | 953 | yes | Flag to continue relayout |
| `encountered_calloc_for_complete_struct_relayout` | `int` | ipo_struct_opt.cxx | 954 | yes | Flag: calloc encountered |
| `continue_with_array_remapping` | `int` | ipo_struct_opt.cxx | 1623 | yes | Flag to continue array remapping |
| `array_remapping_candidate_mtype` | `int` | ipo_struct_opt.cxx | 1624 | yes | Candidate mtype for array remapping |
| `array_remapping_candidate_mtype_size` | `int` | ipo_struct_opt.cxx | 1625 | yes | Size of candidate mtype |
| `array_remapping_malloc_size` | `int` | ipo_struct_opt.cxx | 1626 | yes | Malloc size for array remapping |
| `array_remapping_pre_margin_size` | `int` | ipo_struct_opt.cxx | 1627 | yes | Pre-margin size |
| `array_remapping_loop_depth` | `int` | ipo_struct_opt.cxx | 1628 | yes | Loop depth for remapping |
| `array_remapping_num_groups` | `int` | ipo_struct_opt.cxx | 1629 | yes | Number of groups |
| `array_remapping_loop_num_iterations` | `int` | ipo_struct_opt.cxx | 1630 | yes | Loop iteration count |
| `array_remapping_num_elements_in_pre_margin` | `int` | ipo_struct_opt.cxx | 1631 | yes | Elements in pre-margin |
| `array_remapping_num_elements_in_array` | `int` | ipo_struct_opt.cxx | 1632 | yes | Elements in array |
| `array_remapping_num_elements_in_post_margin` | `int` | ipo_struct_opt.cxx | 1633 | yes | Elements in post-margin |
| `array_remapping_num_elements_in_group` | `int` | ipo_struct_opt.cxx | 1634 | yes | Elements in group |
| `array_remapping_final_checked` | `int` | ipo_struct_opt.cxx | 1635 | yes | Final check flag |
| `new_types_created` | `BOOL` | ipo_struct_opt.cxx | 2595 | yes | Flag: new types created for struct opt |
| `fld_table_updated` | `BOOL` | ipo_struct_opt.cxx | 2696 | yes | Flag: field table updated |

#### Extern declarations in ipo_struct_opt.cxx

| Declaration | File | Line | Notes |
|-------------|------|------|-------|
| `extern BOOL argument_in_callers_is_array_remapping_candidate_malloc(IPA_NODE *, int)` | ipo_struct_opt.cxx | 1637 | Function decl |
| `extern INT struct_field_count(TY_IDX)` | ipo_struct_opt.cxx | 2594 | Function decl |

---

### ipo_split.cxx (common block splitting)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `trace_split_common` | `BOOL` | ipo_split.cxx | 83 | yes | Trace flag for split common |
| `IPO_split_pool` | `MEM_POOL` | ipo_split.cxx | 84 | yes | Memory pool for split common |
| `Field_Map` | `FIELD_MAP_DYN_ARRAY*` | ipo_split.cxx | 110 | yes | Dynamic array of field maps |
| `Split_Name[1000]` | `char[]` | ipo_split.cxx | 111 | yes | Name buffer for split common names |
| `Name_Count` | `INT` | ipo_split.cxx | 118 | yes | **Static local** inside GetName() -- counts split names |

---

### ipo_alias_class.cxx (alias classification)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `Ip_alias_class` | `IP_ALIAS_CLASSIFICATION*` | ipo_alias_class.cxx | 82 | no | Global pointer to alias classification object |
| `Ip_alias_class_files` | `vector<char*>` | ipo_alias_class.cxx | 83 | no | Global vector of alias class file names |
| `IP_ALIAS_CLASS_REP::_last_id_used` | `IDTYPE` | ipo_alias_class.cxx | 89 | no | Static class member definition (last ID used) |
| `IP_ALIAS_CLASS_REP::_free_list` | `IP_ACR_SLIST` | ipo_alias_class.cxx | 90 | no | Static class member definition (free list) |
| `IP_ALIAS_CLASS_REP::_recycled_acr_nodes` | `UINT32` | ipo_alias_class.cxx | 91 | no | Static class member definition (recycled node count) |
| `num_acr_table_entries` | `const int` | ipo_alias_class.cxx | 94 | yes | Debug table size (under `#if Is_True_On`) |
| `acr_table[100]` | `IP_ALIAS_CLASS_REP*[]` | ipo_alias_class.cxx | 95 | no | Debug ACR table (under `#if Is_True_On`) |
| `num_acm_table_entries` | `const int` | ipo_alias_class.cxx | 97 | yes | Debug table size (under `#if Is_True_On`) |
| `acm_table[100]` | `IP_ALIAS_CLASS_MEMBER*[]` | ipo_alias_class.cxx | 98 | no | Debug ACM table (under `#if Is_True_On`) |
| `next_acm_idx` | `int` | ipo_alias_class.cxx | 99 | yes | Debug index counter (under `#if Is_True_On`) |

---

### ipo_tlog_utils.cxx (transformation log utilities)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `rcs_id` | `char*` | ipo_tlog_utils.cxx | 67 | yes | RCS version string (under `#ifdef _KEEP_RCS_ID`) |
| `MAX_WARN_LEN` | `const INT32` | ipo_tlog_utils.cxx | 84 | no | Constant = 1024 |
| `tlog_phase` | `const char*` | ipo_tlog_utils.cxx | 93 | yes | Current TLOG phase name |

---

### ipo_pad.cxx (array padding)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `trace_split_common` | `BOOL` | ipo_pad.cxx | 88 | yes | Trace flag (note: same name as ipo_split.cxx, both static) |

---

### ipo_dce.cxx (dead call elimination)

No file-scope global variables found. The `static UINT32 Compute_Return_Pregs(...)` at line 57 is a static function, not a variable.

---

### ipo_parent.cxx (WN parent utilities)

No file-scope global variables found. All functions take `Parent_Map` and `map_tab` as parameters.

---

### ipo_clone.cxx (cloning)

No file-scope global variables found.

---

### ipo_across_file_utils.cxx (cross-file utilities)

No file-scope global variables found.

---

### ipo_inline_util.cxx (inline utilities)

No file-scope global variables found. All data is local to functions.

---

### ipo_const.cxx (constant propagation)

No file-scope global variables found. All `static` declarations are functions.

---

### ipo_icall.cxx (indirect call optimization)

No file-scope global variables found. All `static` declarations are functions.

---

### ipo_autopar.cxx (auto-parallelization)

No file-scope global variables found.

---

## Summary

### Counts

- **89 file-scope globals found** (variables/arrays/pointers, not counting function declarations)
  - 23 in ipo_main.cxx
  - 8 in ipo_inline.cxx
  - 34 in ipo_struct_opt.cxx
  - 5 in ipo_split.cxx (4 file-scope + 1 static local)
  - 10 in ipo_alias_class.cxx
  - 3 in ipo_tlog_utils.cxx
  - 1 in ipo_pad.cxx
  - 0 in remaining 9 files

- **56 static variables** at file scope
- **27 non-static (truly global) variables**
- **6 static class member definitions** (in ipo_alias_class.cxx)
- **20 extern declarations** (referencing globals defined elsewhere, including function decls)
- **5 MEM_POOL instances** (`Ipo_mem_pool`, `IPA_LNO_mem_pool`, `Recycle_mem_pool`, `Temp_pool`, `IPO_split_pool`)

### Hotspots (files with the most global state)

1. **ipo_struct_opt.cxx** (34 globals) -- dominated by `complete_struct_relayout` and `array_remapping` state. These are clusters of related variables that could be grouped into structs.
2. **ipo_main.cxx** (23 globals) -- the main IPO driver has register feedback tables, global-as-local tracking arrays, memory pools, and miscellaneous counters/flags.
3. **ipo_alias_class.cxx** (10 globals) -- alias classification state including debug tables and static class members.
4. **ipo_inline.cxx** (8 globals) -- the inliner's memory pool, parent map, and map tables.

### Key patterns for refactoring

1. **ipo_struct_opt.cxx `complete_struct_relayout_*` cluster** (lines 944-954): 10 related `static` variables should become a single `CompleteStructRelayoutState` struct.
2. **ipo_struct_opt.cxx `array_remapping_*` cluster** (lines 1623-1635): 13 related `static` variables should become a single `ArrayRemappingState` struct.
3. **ipo_main.cxx `Global_*` cluster** (lines 249-257): 7 related variables for global-as-local analysis should become a `GlobalAsLocalState` struct.
4. **ipo_main.cxx register feedback** (lines 184, 203): Two hash maps plus cost constants could form a `RegisterBudgetState`.
5. **ipo_inline.cxx global maps** (lines 82-83, 1464-1465): `Ipo_mem_pool`, `Parent_Map`, `caller_map_tab`, `callee_map_tab` are the core inliner global state.
6. **ipo_split.cxx** (lines 83-111): All 4 variables could become a `SplitCommonState` struct.
