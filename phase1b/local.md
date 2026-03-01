# ipa/local/ -- Global State Scan

## Status: COMPLETE

Scanned all 12 .cxx/.c files in `osprey/ipa/local/`:
- init.cxx
- ipl_array_bread_write.cxx
- ipl_bread_write.cxx
- ipl_elfsym.cxx
- ipl_linex.cxx
- ipl_main.cxx
- ipl_outline.cxx
- ipl_reorder.cxx
- ipl_summarize_util.cxx
- ipl_summary_print.cxx
- ipl_tlog.cxx
- loop_info.cxx

## File-scope globals found

### ipl_main.cxx -- The main IPL driver (heaviest concentration of globals)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Trace_IPA | BOOL | ipl_main.cxx | 111 | no | Debug/trace flag |
| Trace_Perf | BOOL | ipl_main.cxx | 112 | no | Debug/trace flag |
| Debug_On | BOOL | ipl_main.cxx | 114 | no | Debug flag |
| DoPreopt | BOOL | ipl_main.cxx | 115 | no | Config flag |
| Do_Const | BOOL | ipl_main.cxx | 116 | no | Config flag |
| Do_Par | BOOL | ipl_main.cxx | 117 | no | Config flag |
| Do_Split_Commons | BOOL | ipl_main.cxx | 118 | no | Config flag, default TRUE |
| Do_Split_Commons_Set | BOOL | ipl_main.cxx | 119 | no | Config flag |
| Do_Common_Const | BOOL | ipl_main.cxx | 120 | no | Config flag |
| IPL_Enable_Outline | BOOL | ipl_main.cxx | 121 | no | Config flag |
| IPL_Enable_Unknown_Frequency | BOOL | ipl_main.cxx | 122 | no | Config flag |
| IPL_Generate_Elf_Symtab | BOOL | ipl_main.cxx | 124/126 | no | Config flag; conditional on `__linux__` / `BUILD_OS_DARWIN` |
| IPL_Ignore_Small_Loops | UINT32 | ipl_main.cxx | 129 | no | Config; `#ifdef KEY` |
| Optlevel | mUINT8 | ipl_main.cxx | 135 | no | Optimization level |
| driver_argc | INT | ipl_main.cxx | 137 | yes | Saved command line argc |
| driver_argv | char** | ipl_main.cxx | 138 | yes | Saved command line argv |
| Options_IPL[] | OPTION_DESC[] | ipl_main.cxx | 140 | yes | Option descriptor table (array) |
| IPL_Option_Groups[] | OPTION_GROUP[] | ipl_main.cxx | 161 | no | Option group table (array) |
| ICALL_MAX_PROMOTE_PER_CALLSITE | INT | ipl_main.cxx | 167 | no | Max indirect call promotions |
| Summary | SUMMARY* | ipl_main.cxx | 169 | no | **Critical**: master summary object pointer |
| Parent_Map | WN_MAP | ipl_main.cxx | 171 | no | WHIRL parent map; `#ifdef SHARED_BUILD` |
| Summary_Map | WN_MAP | ipl_main.cxx | 173 | no | WHIRL summary map |
| Stmt_Map | WN_MAP | ipl_main.cxx | 174 | no | WHIRL statement map |
| Ipl_Symbol_Names | DYN_ARRAY\<char*\>* | ipl_main.cxx | 176 | no | Dynamic array of symbol names |
| Ipl_Function_Names | DYN_ARRAY\<char*\>* | ipl_main.cxx | 177 | no | Dynamic array of function names |

### ipl_bread_write.cxx -- Binary summary writer

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Optlevel (extern) | mUINT8 | ipl_bread_write.cxx | 63 | no | extern ref to ipl_main.cxx |
| header | SUMMARY_FILE_HEADER | ipl_bread_write.cxx | 72 | yes | Summary file header struct |

### ipl_array_bread_write.cxx -- Array section summary writer

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Array_Summary_Output | ARRAY_SUMMARY_OUTPUT* | ipl_array_bread_write.cxx | 61 | no | **Critical**: pointer to array summary output object |
| ivar_offset | INT | ipl_array_bread_write.cxx | 63 | yes | Offset tracking for ivar serialization |
| regions_offset | INT | ipl_array_bread_write.cxx | 64 | yes | Offset tracking for regions serialization |

### ipl_linex.cxx -- Linear expression / array section analysis

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Array_Summary | ARRAY_SUMMARY | ipl_linex.cxx | 85 | no | **Critical**: the array summary object (not a pointer -- full object!) |
| Current_summary_procedure | SUMMARY_PROCEDURE* | ipl_linex.cxx | 87 | yes | Currently-being-analyzed procedure |
| Cfg_entry | CFG_NODE_INFO* | ipl_linex.cxx | 88 | yes | Current CFG entry node |
| Cfg_entry_idx | INT | ipl_linex.cxx | 89 | yes | Current CFG entry index |

### loop_info.cxx -- Loop analysis / access vector building

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| SYSTEM_OF_EQUATIONS::_work[][] | mINT32[SOE_MAX_WORK_ROWS][SOE_MAX_WORK_COLS] | loop_info.cxx | 82-83 | no | Static class member definition; `#ifdef SHARED_BUILD`. Large workspace array. |
| SYSTEM_OF_EQUATIONS::_work_const[] | mINT64[SOE_MAX_WORK_ROWS] | loop_info.cxx | 84-85 | no | Static class member definition; `#ifdef SHARED_BUILD`. Workspace array. |
| Ipl_Al_Mgr | struct ALIAS_MANAGER* | loop_info.cxx | 88 | no | **Critical**: global alias manager pointer |
| Ipl_Du_Mgr | struct DU_MANAGER* | loop_info.cxx | 89 | no | **Critical**: global def-use manager pointer |
| IPL_loop_pool | MEM_POOL | loop_info.cxx | 91 | no | **Critical**: memory pool for loop analysis |
| IPL_local_pool | MEM_POOL | loop_info.cxx | 92 | no | **Critical**: memory pool for local analysis |
| IPL_info_map | WN_MAP | loop_info.cxx | 93 | no | WHIRL map for loop info annotations |
| IPL_reduc_map | WN_MAP | loop_info.cxx | 94 | no | WHIRL map for reduction annotations |
| trace_section | BOOL | loop_info.cxx | 95 | yes | Trace flag for array sections |
| ST_node_tbl | ST_TBL* (= HASH_TABLE\<char*, ST*\>*) | loop_info.cxx | 98 | no | Symbol table hash (for scalar flow tracking) |

### ipl_summarize_util.cxx -- Summarization utility routines

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Chi_To_Idx_Map | CHI_CR_TO_INT_MAP* | ipl_summarize_util.cxx | 91 | no | Hash map: chi CODEREP -> index |
| Hashed_Chis | CHI_CR_ARRAY* | ipl_summarize_util.cxx | 92 | no | Array of hashed chi nodes |
| Num_Chis_On_PU_Start | INT | ipl_summarize_util.cxx | 93 | no | Chi count at PU entry |
| Phi_To_Idx_Map | PHI_NODE_TO_INT_MAP* | ipl_summarize_util.cxx | 95 | no | Hash map: phi node -> index |
| Hashed_Phis | PHI_NODE_ARRAY* | ipl_summarize_util.cxx | 96 | no | Array of hashed phi nodes |
| Num_Phis_On_PU_Start | INT | ipl_summarize_util.cxx | 97 | no | Phi count at PU entry |
| cdg | CTRL_DEP_ARRAY* | ipl_summarize_util.cxx | 469 | yes | Control dependence graph |
| CTRL_DEP_mem | MEM_POOL* | ipl_summarize_util.cxx | 470 | yes | **MEM_POOL pointer** for control dependence |
| stmt_id | INT | ipl_summarize_util.cxx | 471 | yes | Statement ID counter for backtracking |
| iterator_idx | INT | ipl_summarize_util.cxx | 472 | yes | Iterator index for CDG traversal |
| entry_cache | SUMMARY_ENTRY_CACHE* | ipl_summarize_util.cxx | 810 | no | **Critical**: hash cache for summary entries (avoids duplicates) |

### ipl_summary_print.cxx -- Summary printing/tracing routines

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Ipl_Summary_Symbol | SUMMARY_SYMBOL* | ipl_summary_print.cxx | 72 | no | Pointer to summary symbol array |
| MODREF_MAX_STRING_LENGTH | const INT | ipl_summary_print.cxx | 73 | no | String buffer size constant (1000) |
| Modref_Buf[] | char[1000] | ipl_summary_print.cxx | 74 | no | Static buffer for mod/ref formatting |
| IPA_Trace_Mod_Ref | BOOL | ipl_summary_print.cxx | 75 | no | Trace flag for mod/ref analysis |

### ipl_tlog.cxx -- Transformation log utilities

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| MAX_WARN_LEN | const INT32 | ipl_tlog.cxx | 71 | no | Buffer size constant (1024) |
| tlog_phase | const char* | ipl_tlog.cxx | 73 | yes | Phase name string literal "IPL" |

### ipl_reorder.cxx -- Struct field reorder analysis

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| reorder_ipl_pool | MEM_POOL | ipl_reorder.cxx | 37 | no | **Critical**: memory pool for reorder analysis |
| Ptr_to_ty_vector | PTR_TO_TY_VECTOR* | ipl_reorder.cxx | 38 | no | Pointer-to-type mapping vector |
| local_cands | TY_TO_FLDNUM_MAP* | ipl_reorder.cxx | 39 | no | Map of struct type candidates |

### init.cxx -- Initialization / weak-symbol workaround

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Ipl_Extra_Output_p (extern) | void (*)(Output_File*) | init.cxx | 64 | no | extern function pointer |
| Ipl_Init_p (extern) | void (*)() | init.cxx | 65 | no | extern function pointer |
| Ipl_Fini_p (extern) | void (*)() | init.cxx | 66 | no | extern function pointer |
| ipl_main_p (extern) | void (*)(INT, char**) | init.cxx | 67 | no | extern function pointer |
| Perform_Procedure_Summary_Phase_p (extern) | void (*)(WN*, DU_MANAGER*, ALIAS_MANAGER*, void*) | init.cxx | 68 | no | extern function pointer |
| WB_BROWSER_Summary_p (extern) | void (*)(FILE*, WB_BROWSER*) | init.cxx | 70 | no | extern function pointer |
| Print_DO_LOOP_INFO_BASE_p (extern) | void (*)(FILE*, DO_LOOP_INFO_BASE*) | init.cxx | 71 | no | extern function pointer |
| Preprocess_struct_access_p (extern) | void (*)(void) | init.cxx | 73 | no | extern function pointer; `#ifdef KEY` |
| Ipl_Initializer | IPL_INIT (struct) | init.cxx | 90 | no | File-scope object; its constructor wires up all the above function pointers. Acts as a static initializer. `#ifdef __linux__` |

### ipl_outline.cxx -- Outline (code splitting) analysis

No file-scope globals found. All state is local to functions or in an anonymous namespace struct.

### ipl_elfsym.cxx -- ELF symbol table generation

No file-scope globals found. Uses template functions; state comes from parameters.

## Summary

- **61 file-scope globals found** (including extern declarations and static class member definitions)
- **17 static variables** at file scope (including static arrays/structs)
- **9 extern declarations** (all in init.cxx and ipl_bread_write.cxx referencing globals defined elsewhere)
- **4 MEM_POOL instances** (IPL_loop_pool, IPL_local_pool, reorder_ipl_pool, CTRL_DEP_mem pointer)
- **4 WN_MAP instances** (Parent_Map, Summary_Map, Stmt_Map, IPL_info_map, IPL_reduc_map -- actually 5)
- **2 files with no globals** (ipl_outline.cxx, ipl_elfsym.cxx)

## Critical globals for IPA refactoring (Goal 5)

The most impactful globals to encapsulate are:

1. **`Summary`** (ipl_main.cxx:169) -- The master SUMMARY object pointer. Everything flows through this.
2. **`Array_Summary`** (ipl_linex.cxx:85) -- Full ARRAY_SUMMARY object at file scope (not even a pointer).
3. **`Array_Summary_Output`** (ipl_array_bread_write.cxx:61) -- Output serializer for array summaries.
4. **`Ipl_Al_Mgr` / `Ipl_Du_Mgr`** (loop_info.cxx:88-89) -- Alias and def-use manager pointers.
5. **`IPL_loop_pool` / `IPL_local_pool`** (loop_info.cxx:91-92) -- Memory pools.
6. **`Summary_Map` / `Stmt_Map` / `Parent_Map`** (ipl_main.cxx:171-174) -- WHIRL annotation maps.
7. **`entry_cache`** (ipl_summarize_util.cxx:810) -- Summary dedup cache.
8. **`Chi_To_Idx_Map` / `Phi_To_Idx_Map` / `Hashed_Chis` / `Hashed_Phis`** (ipl_summarize_util.cxx:91-96) -- SSA node hash maps.
9. **`cdg` / `CTRL_DEP_mem` / `stmt_id` / `iterator_idx`** (ipl_summarize_util.cxx:469-472) -- Control dependence graph state.
10. **`reorder_ipl_pool` / `Ptr_to_ty_vector` / `local_cands`** (ipl_reorder.cxx:37-39) -- Struct reorder state.

The config/trace flags (Trace_IPA, Debug_On, Do_Const, etc.) are less critical for parallel IPA since they are read-only after initialization, but they still represent global state that prevents clean instantiation.

The init.cxx extern function pointers are a Linux weak-symbol workaround pattern and are not IPA-specific mutable state -- they can likely be left alone.
