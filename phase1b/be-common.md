# be/com/ + common/com/ (IPA-related) -- Global State Scan

## Status: COMPLETE

## Files scanned

### be/com/ (.cxx)
- `ipa_be_read.cxx`
- `ipa_cost_util.cxx`
- `ipa_lno_file.cxx`
- `ipa_lno_summary.cxx`
- `ipa_lno_util.cxx`
- `ipa_section.cxx`
- `ipa_section_main.cxx`
- `ipa_section_print.cxx`
- `opt_ipaa_io.cxx`

### be/com/ (.h)
- `ipa_be_read.h`
- `ipa_be_summary.h`
- `ipa_cost_util.h`
- `ipa_lno_file.h`
- `ipa_lno_info.h`
- `ipa_lno_summary.h`
- `ipa_lno_util.h`
- `ipa_section.h`
- `ipa_section_main.h`
- `opt_ipaa_io.h`
- `opt_ipaa_summary.h`

### common/com/
- `config_ipa.cxx`
- `config_ipa.h`
- `be_ipa_util.h`

---

## File-scope globals found

### be/com/ipa_be_read.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `CURRENT_BE_SUMMARY` | `IPA_BE_SUMMARY` | ipa_be_read.cxx | 33 | no | Initialized with `Malloc_Mem_Pool`; the single global IPA BE summary object |

### be/com/ipa_lno_file.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `IPALNO_REVISION` | `char[]` | ipa_lno_file.cxx | 90 | yes | Static string constant for IPA-LNO file revision |
| `old_sigsegv` | `void (*)(int)` | ipa_lno_file.cxx | 92 | yes | Previous signal handler saved during mmapped I/O |
| `old_sigbus` | `void (*)(int)` | ipa_lno_file.cxx | 93 | yes | Previous signal handler saved during mmapped I/O |

### be/com/ipa_cost_util.cxx

No file-scope globals found. All `static` and `extern` declarations at line-start are function definitions (they take parameters), not variable declarations.

### be/com/ipa_lno_summary.cxx

No file-scope globals found.

### be/com/ipa_lno_util.cxx

No file-scope globals found.

### be/com/ipa_section.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `Summary` | `SUMMARY*` | ipa_section.cxx | 94 | no (extern) | References global from IPL |
| `Array_Summary` | `ARRAY_SUMMARY` | ipa_section.cxx | 95 | no (extern) | References global from ipl_linex |
| `Trace_Sections` | `BOOL` | ipa_section.cxx | 97 | no | Trace flag, initialized to FALSE |
| `Ivar` | `IVAR_ARRAY*` | ipa_section.cxx | 99 | no | Global pointer to IVAR array, init NULL |
| `IPL_Loopinfo_Map` | `LOOPINFO_TO_DLI_MAP*` | ipa_section.cxx | 102 | no | Map carrying loop info from IPL to LNO |
| `IPL_Access_Array_Map` | `PROJ_REGION_TO_ACCESS_ARRAY_MAP*` | ipa_section.cxx | 103 | no | Map carrying array access info from IPL to LNO |
| `SYSTEM_OF_EQUATIONS::_work_cols` | `mINT32` | ipa_section.cxx | 107 | no | Class static member definition (only in `SHARED_BUILD`) |
| `SYSTEM_OF_EQUATIONS::_work_rows_eq` | `mINT32` | ipa_section.cxx | 109 | no | Class static member definition (only in `SHARED_BUILD`) |
| `SYSTEM_OF_EQUATIONS::_work_rows` | `mINT32` | ipa_section.cxx | 111 | no | Class static member definition (only in `SHARED_BUILD`) |

### be/com/ipa_section_main.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `IPA_Ivar_Global_Count` | `INT` | ipa_section_main.cxx | 76 | no | Count of global IVARs (guarded by `#ifndef IPA_SUMMARY`) |
| `IPA_Ivar_Count` | `INT` | ipa_section_main.cxx | 77 | no | Total IVAR count (guarded by `#ifndef IPA_SUMMARY`) |
| `SYSTEM_OF_EQUATIONS::_work_cols` | `mINT32` | ipa_section_main.cxx | 79 | no | Class static member definition (guarded by `#ifndef IPA_SUMMARY`) |
| `SYSTEM_OF_EQUATIONS::_work_rows_eq` | `mINT32` | ipa_section_main.cxx | 81 | no | Class static member definition (guarded by `#ifndef IPA_SUMMARY`) |
| `SYSTEM_OF_EQUATIONS::_work_rows` | `mINT32` | ipa_section_main.cxx | 83 | no | Class static member definition (guarded by `#ifndef IPA_SUMMARY`) |
| `MAT<int>::_default_pool` | `MEM_POOL*` | ipa_section_main.cxx | 85 | no | Template static member definition (guarded by `#ifndef IPA_SUMMARY`) |

### be/com/ipa_section_print.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `IPA_Symbol` | `SUMMARY_SYMBOL*` | ipa_section_print.cxx | 85 | yes | Used for printing; set by `Init_IPA_Print_Arrays()` (guarded by `#ifdef IPA_SUMMARY`) |
| `Summary` | `SUMMARY*` | ipa_section_print.cxx | 114 | no (extern) | References global from IPL (guarded by `#else` / non-IPA_SUMMARY path) |
| `Ivar` | `IVAR_ARRAY*` | ipa_section_print.cxx | 115 | no (extern) | References global IVAR array (guarded by `#else`) |

### be/com/opt_ipaa_io.cxx

No file-scope globals found. All `extern "C"` entries are function definitions.

---

## Header file extern/global declarations

### be/com/ipa_be_read.h

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `CURRENT_BE_SUMMARY` | `IPA_BE_SUMMARY` | ipa_be_read.h | 30 | no (extern) | Declares the global defined in ipa_be_read.cxx |
| `IPA_read_alias_summary` | function | ipa_be_read.h | 32 | no (extern) | (function, not a variable) |

### be/com/ipa_cost_util.h

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `DEFAULT_TRIP_COUNT` | `const INT` | ipa_cost_util.h | 41 | no | Constant = 100 |
| `DEFAULT_CALL_COST` | `const INT64` | ipa_cost_util.h | 42 | no | Constant = 100 |
| `MAX_VALUE_COUNT` | `const INT` | ipa_cost_util.h | 43 | no | Constant = 100 |
| `MAX_EXPR_COUNT` | `const INT` | ipa_cost_util.h | 44 | no | Constant = 100 |
| `MAX_CALL_EXPR_COUNT` | `const INT` | ipa_cost_util.h | 45 | no | Constant = 15 |

### be/com/ipa_lno_file.h

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `IPALNO_SUCCESS` | `const INT` | ipa_lno_file.h | 74 | no | Error code constant = 0 |
| `IPALNO_FORMAT_ERROR` | `const INT` | ipa_lno_file.h | 75 | no | Error code constant = -1 |
| `IPALNO_ABI_MISMATCH` | `const INT` | ipa_lno_file.h | 76 | no | Error code constant = -2 |
| `IPALNO_REVISION_MISMATCH` | `const INT` | ipa_lno_file.h | 77 | no | Error code constant = -3 |
| `IPALNO_READER_ERROR` | `const INT` | ipa_lno_file.h | 78 | no | Error code constant = -4 |
| `IPALNO_CLOSE_ERROR` | `const INT` | ipa_lno_file.h | 79 | no | Error code constant = -5 |
| `MAX_FILE_REVISION` | `const INT` | ipa_lno_file.h | 86 | no | Constant = 80 |

### be/com/ipa_section.h

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `Trace_Sections` | `BOOL` | ipa_section.h | 82 | no (extern) | Declares the global defined in ipa_section.cxx |
| `Ivar` | `IVAR_ARRAY*` | ipa_section.h | 409 | no (extern) | Guarded by `#ifdef IPA_SUMMARY` |
| `IPL_Loopinfo_Map` | `LOOPINFO_TO_DLI_MAP*` | ipa_section.h | 414 | no (extern) | Guarded by `#ifdef IPA_SUMMARY` |
| `IPL_Access_Array_Map` | `PROJ_REGION_TO_ACCESS_ARRAY_MAP*` | ipa_section.h | 420 | no (extern) | Guarded by `#ifdef IPA_SUMMARY` |

### be/com/ipa_section_main.h

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `IPA_Ivar_Count` | `INT` | ipa_section_main.h | 62 | no (extern) | Guarded by `#ifndef IPA_SUMMARY` |

### be/com/opt_ipaa_io.h

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `opt_ipaa_io_rcs_id` | `char*` | opt_ipaa_io.h | 63 | yes | RCS ID string (guarded by `_KEEP_RCS_ID`) |
| `IPAA_Local_Map` | `void*` | opt_ipaa_io.h | 76 | no (extern "C") | Global pointer to local map, defined in ir_bcom.c |

### be/com/opt_ipaa_summary.h

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `opt_ipaa_summary_rcs_id` | `char*` | opt_ipaa_summary.h | 103 | yes | RCS ID string (guarded by `_KEEP_RCS_ID`) |
| `Trace_IPAA_Summary` | `BOOL` | opt_ipaa_summary.h | 107 | no (extern) | Trace flag for IPAA summary build/output |
| `IPAA_Summary` | `IPAA_SUMMARY*` | opt_ipaa_summary.h | 119 | no (extern) | Global pointer to current IPAA summary instance |
| `SYMREF_IX_INVALID` | `const SYMREF_IX` | opt_ipaa_summary.h | 195 | no | Constant = 0 |
| `SYMREF_IX_UNKNOWN` | `const SYMREF_IX` | opt_ipaa_summary.h | 196 | no | Constant = 1 |
| `SYMREF_IX_FIRST` | `const SYMREF_IX` | opt_ipaa_summary.h | 197 | no | Constant = 2 |
| `MODREF_CONSERVATIVE` | `const REFBITS` | opt_ipaa_summary.h | 322 | no | Bitmask constant |

### be/com/ipa_be_summary.h, ipa_lno_info.h, ipa_lno_summary.h, ipa_lno_util.h

No file-scope globals or extern declarations found (only class/struct definitions and function prototypes).

---

## common/com/be_ipa_util.h

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `Mod_Ref_Info_Table` | `MOD_REF_INFO_TAB` | be_ipa_util.h | 238 | no (extern) | Global mod/ref info table |

---

## common/com/config_ipa.cxx -- IPA Configuration Globals

This file is the **largest single source of IPA global state** in the entire codebase. It defines the `-IPA:` and `-INLINE:` option groups as global variables.

### IPA option variables (config_ipa.cxx)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `Ipa_File_Name` | `char*` | config_ipa.cxx | 75 | no | IPAA summary file name |
| `Feedback_Filename` | `char*` | config_ipa.cxx | 76 | no | Feedback file name |
| `Annotation_Filename` | `char*` | config_ipa.cxx | 77 | no | Annotation file name |
| `Ipa_File` | `FILE*` | config_ipa.cxx | 78 | no | IPAA summary file handle |
| `DEFAULT_MIN_PROBABILITY` | `const float` | config_ipa.cxx | 90 | no | Constant = 0.20 (guarded by KEY) |
| `IPA_Enable_DFE` | `BOOL` | config_ipa.cxx | 125 | no | Dead Function Elimination |
| `IPA_Enable_DFE_Set` | `BOOL` | config_ipa.cxx | 126 | no | Explicitly set flag |
| `IPA_Enable_Inline` | `BOOL` | config_ipa.cxx | 127 | no | Inlining |
| `IPA_Enable_Picopt` | `BOOL` | config_ipa.cxx | 128 | no | PIC optimization |
| `IPA_Enable_AutoGnum` | `BOOL` | config_ipa.cxx | 129 | no | AutoGnum optimization |
| `IPA_Enable_Opt_Alias` | `BOOL` | config_ipa.cxx | 130 | no | WOPT uses alias/mod/ref |
| `IPA_Enable_Simple_Alias` | `BOOL` | config_ipa.cxx | 131 | no | Simple alias/mod/ref |
| `IPA_Enable_Addressing` | `BOOL` | config_ipa.cxx | 132 | no | Addr_taken analysis |
| `IPA_Enable_BarrierFarg` | `BOOL` | config_ipa.cxx | 133 | no | Barrier for aliased Fortran actual |
| `IPA_Enable_Alias_Class` | `BOOL` | config_ipa.cxx | 134 | no | Alias classification |
| `IPA_Debug_AC_Temp_Files` | `BOOL` | config_ipa.cxx | 135 | no | Save alias class temp files |
| `IPA_Enable_Readonly_Ref` | `BOOL` | config_ipa.cxx | 136 | no | Readonly ref parameter |
| `IPA_Enable_Cprop` | `BOOL` | config_ipa.cxx | 137 | no | Constant Propagation |
| `IPA_Enable_Cprop2` | `BOOL` | config_ipa.cxx | 138 | no | Aggressive constant propagation |
| `IPA_Enable_Assert` | `BOOL` | config_ipa.cxx | 139 | no | Assert statement for cprop debug |
| `IPA_Enable_daVinci` | `BOOL` | config_ipa.cxx | 141 | no | Graphical display of call graph |
| `IPA_Enable_ipacom` | `BOOL` | config_ipa.cxx | 142 | no | Call ipacom after IPA |
| `IPA_Enable_final_link` | `BOOL` | config_ipa.cxx | 144/146 | no | Final link step (ifdef-dependent) |
| `IPA_Enable_Memtrace` | `BOOL` | config_ipa.cxx | 148 | no | Memory trace |
| `IPA_Enable_DST` | `BOOL` | config_ipa.cxx | 149 | no | Generate DST |
| `IPA_Enable_DCE` | `BOOL` | config_ipa.cxx | 150 | no | Dead Call Elimination |
| `IPA_Enable_Exc` | `BOOL` | config_ipa.cxx | 151 | no | Exception handling |
| `IPA_Enable_Recycle` | `BOOL` | config_ipa.cxx | 152 | no | Recycling of variables |
| `IPA_Enable_DVE` | `BOOL` | config_ipa.cxx | 153 | no | Dead Variable Elimination |
| `IPA_Enable_CGI` | `BOOL` | config_ipa.cxx | 154 | no | Constant Global Variable Identification |
| `IPA_Enable_Copy_Prop` | `BOOL` | config_ipa.cxx | 156 | no | Copy propagation during inlining |
| `IPA_Enable_Padding` | `BOOL` | config_ipa.cxx | 157 | no | Intra-Dimension padding |
| `IPA_Common_Pad_Size` | `UINT32` | config_ipa.cxx | 159 | no | Amount by which to pad commons |
| `IPA_Enable_Cloning` | `BOOL` | config_ipa.cxx | 161 | no | Cloning with constant propagation |
| `IPA_Enable_Partial_Inline` | `BOOL` | config_ipa.cxx | 163 | no | Partial inlining |
| `IPA_Enable_Lang` | `BOOL` | config_ipa.cxx | 164 | no | Inlining across language |
| `IPA_Enable_Relocatable_Opt` | `BOOL` | config_ipa.cxx | 165 | no | -call_shared opt of relocatable objects |
| `IPA_Enable_Split_Common` | `BOOL` | config_ipa.cxx | 166 | no | Split common inside IPA |
| `IPA_Enable_Array_Sections` | `BOOL` | config_ipa.cxx | 167 | no | Array section analysis in IPA |
| `IPA_Enable_Array_Summary` | `BOOL` | config_ipa.cxx | 168 | no | Array section summary in IPL |
| `IPA_Enable_Scalar_Euse` | `BOOL` | config_ipa.cxx | 170 | no | Scalar euse |
| `IPA_Enable_Scalar_Kill` | `BOOL` | config_ipa.cxx | 171 | no | Scalar kill |
| `IPA_Enable_Common_Const` | `BOOL` | config_ipa.cxx | 172 | no | Const prop of common block vars |
| `IPA_Enable_Feedback` | `BOOL` | config_ipa.cxx | 173 | no | Create pragma files |
| `IPA_Echo_Commands` | `BOOL` | config_ipa.cxx | 176 | no | Echo back end command lines |
| `IPA_Bloat_Factor` | `UINT32` | config_ipa.cxx | 179 | no | Max bloat % |
| `IPA_Bloat_Factor_Set` | `BOOL` | config_ipa.cxx | 180 | no | Explicitly set flag |
| `IPA_Min_Branch_Prob` | `float` | config_ipa.cxx | 183 | no | Min branch probability (KEY) |
| `IPA_PU_Limit` | `UINT32` | config_ipa.cxx | 185 | no | Max nodes per PU after inlining |
| `IPA_PU_Limit_Set` | `BOOL` | config_ipa.cxx | 186 | no | Explicitly set flag |
| `IPA_PU_Hard_Limit` | `UINT32` | config_ipa.cxx | 189 | no | Absolute max nodes per PU |
| `IPA_PU_Hard_Limit_Set` | `BOOL` | config_ipa.cxx | 190 | no | Explicitly set flag |
| `IPA_PU_Minimum_Size` | `UINT32` | config_ipa.cxx | 193 | no | Size of small PU always inlined |
| `IPA_Small_Callee_Limit` | `UINT32` | config_ipa.cxx | 196 | no | Callee size limit |
| `IPA_Max_Depth` | `UINT32` | config_ipa.cxx | 198 | no | Maximum depth to inline |
| `IPA_Force_Depth` | `UINT32` | config_ipa.cxx | 199 | no | Force inlining to depth n |
| `IPA_Force_Depth_Set` | `BOOL` | config_ipa.cxx | 201 | no | Explicitly set flag |
| `IPA_Enable_Merge_ty` | `BOOL` | config_ipa.cxx | 202 | no | Merge types across files |
| `IPA_Max_Jobs` | `UINT32` | config_ipa.cxx | 205/207 | no | Concurrent backend compilations |
| `IPA_Max_Jobs_Set` | `BOOL` | config_ipa.cxx | 209 | no | Explicitly set flag |
| `IPA_Min_Freq` | `UINT32` | config_ipa.cxx | 212 | no | Min call freq for inlining |
| `IPA_Max_Density` | `UINT32` | config_ipa.cxx | 216 | no | Max density for inlining tuning |
| `IPA_Rela_Freq` | `UINT32` | config_ipa.cxx | 219 | no | % time callee called by caller |
| `IPA_Min_Hotness` | `UINT32` | config_ipa.cxx | 222 | no | Minimum hotness for inlining |
| `IPA_Use_Effective_Size` | `BOOL` | config_ipa.cxx | 225 | no | Ignore zero-freq stmts for size |
| `IPA_Gspace` | `UINT32` | config_ipa.cxx | 228 | no | GP-relative space for auto Gnum |
| `IPA_Extgot_Factor` | `UINT32` | config_ipa.cxx | 231 | no | % of estimated external GOT |
| `IPA_Num_Fortran_Intrinsics` | `UINT32` | config_ipa.cxx | 235 | no | Number of Fortran intrinsics |
| `IPA_Has_Fortran` | `BOOL` | config_ipa.cxx | 236 | no | Executable has Fortran |
| `IPA_user_gnum` | `UINT32` | config_ipa.cxx | 239 | no | User specified -G num |
| `IPA_Skip` | `OPTION_LIST*` | config_ipa.cxx | 241 | no | List of skip options |
| `IPA_Skip_Report` | `BOOL` | config_ipa.cxx | 242 | no | Report skip count |
| `IPA_Enable_Preempt` | `BOOL` | config_ipa.cxx | 244 | no | Functions not preemptible |
| `IPA_Enable_Flow_Analysis` | `BOOL` | config_ipa.cxx | 247 | no | Flow-sensitive analysis |
| `IPA_Map_Limit` | `UINT32` | config_ipa.cxx | 252 | no | Max mapped address space |
| `IPA_Enable_SP_Partition` | `BOOL` | config_ipa.cxx | 254 | no | SP partition of call-graph |
| `IPA_Enable_GP_Partition` | `BOOL` | config_ipa.cxx | 258 | no | GP partition of call-graph |
| `IPA_Space_Access_Mode` | `BOOL` | config_ipa.cxx | 262 | no | Space access mode |
| `IPA_Enable_Keeplight` | `BOOL` | config_ipa.cxx | 264 | no | Only keep .I and .o |
| `IPA_Enable_Icall_Opt` | `BOOL` | config_ipa.cxx | 269 | no | Allow icall to call (KEY) |
| `IPA_Enable_EH_Region_Removal` | `BOOL` | config_ipa.cxx | 270 | no | Remove useless EH regions (KEY) |
| `IPA_Enable_Branch_Heuristic` | `BOOL` | config_ipa.cxx | 271 | no | Branch prob for inlining (KEY) |
| `IPA_Check_Options` | `BOOL` | config_ipa.cxx | 272 | no | Check inconsistent options (KEY) |
| `IPA_Clone_List_Actions` | `BOOL` | config_ipa.cxx | 273 | no | Report cloner actions (KEY) |
| `IPA_Enable_Pure_Call_Opt` | `BOOL` | config_ipa.cxx | 274 | no | Optimize callsites w/o side-effects (KEY) |
| `IPA_Pure_Call_skip_before` | `INT32` | config_ipa.cxx | 275 | no | (KEY) |
| `IPA_Enable_Cord` | `BOOL` | config_ipa.cxx | 277/303 | no | Procedure reordering (ifdef-dependent) |
| `IPA_Enable_PU_Reorder` | `PU_REORDER_SCHEME` | config_ipa.cxx | 279 | no | Procedure reordering scheme |
| `IPA_Enable_PU_Reorder_Set` | `BOOL` | config_ipa.cxx | 280 | no | Explicitly set flag |
| `IPA_Enable_Ctype` | `BOOL` | config_ipa.cxx | 281 | no | Insert ctype.h array (KEY) |
| `IPA_Consult_Inliner_For_Icall_Opt` | `BOOL` | config_ipa.cxx | 282 | no | Consult inliner for icall opt (KEY) |
| `IPA_Icall_Min_Freq` | `UINT32` | config_ipa.cxx | 284 | no | Min freq for icall opt (KEY) |
| `IPA_Icall_Target_Min_Rate` | `UINT32` | config_ipa.cxx | 290 | no | Min rate for icall target (KEY) |
| `IPA_Enable_Source_PU_Order` | `BOOL` | config_ipa.cxx | 293 | no | Source PU order (KEY) |
| `IPA_Enable_Struct_Opt` | `UINT32` | config_ipa.cxx | 295/298 | no | Struct optimization (ifdef-dependent) |
| `IPA_Enable_Global_As_Local` | `UINT32` | config_ipa.cxx | 296/299 | no | Global-as-local optimization (ifdef-dependent) |
| `IPA_Update_Struct` | `UINT32` | config_ipa.cxx | 301 | no | Temporary struct update flag |
| `IPA_Enable_Linearization` | `BOOL` | config_ipa.cxx | 305 | no | Linearization of array |
| `IPA_Use_Intrinsic` | `BOOL` | config_ipa.cxx | 306 | no | Load intrinsic libraries |
| `IPA_Enable_Inline_Nested_PU` | `BOOL` | config_ipa.cxx | 308 | no | Inlining nested PU for f90 |
| `IPA_Enable_Reshape` | `BOOL` | config_ipa.cxx | 310 | no | Reshape analysis for arrays |
| `IPA_Enable_Inline_Struct` | `BOOL` | config_ipa.cxx | 311 | no | Inlining of STRUCT for f90 |
| `IPA_Enable_Inline_Char_Array` | `BOOL` | config_ipa.cxx | 312 | no | Inlining of Character Array for f90 |
| `IPA_Enable_Inline_Optional_Arg` | `BOOL` | config_ipa.cxx | 313 | no | Inlining with optional arguments |
| `IPA_Enable_Inline_Struct_Array_Actual` | `BOOL` | config_ipa.cxx | 314 | no | Inlining STRUCT with array actuals |
| `IPA_Enable_Inline_Var_Dim_Array` | `BOOL` | config_ipa.cxx | 315 | no | Inlining variable-dimensioned array |
| `IPA_Enable_Reorder` | `BOOL` | config_ipa.cxx | 316 | no | Field reordering |
| `IPA_Enable_Preopt` | `BOOL` | config_ipa.cxx | 319 | no | Call preopt during IPA |
| `IPA_Enable_Preopt_Set` | `BOOL` | config_ipa.cxx | 320 | no | Explicitly set flag |
| `IPA_Enable_Siloed_Ref` | `BOOL` | config_ipa.cxx | 323 | no | Siloed reference analysis |
| `IPA_Enable_Siloed_Ref_Set` | `BOOL` | config_ipa.cxx | 324 | no | Explicitly set flag |
| `IPA_Max_Node_Clones` | `UINT32` | config_ipa.cxx | 327 | no | Max clones per call graph node |
| `IPA_Max_Node_Clones_Set` | `BOOL` | config_ipa.cxx | 328 | no | Explicitly set flag |
| `IPA_Max_Clone_Bloat` | `UINT32` | config_ipa.cxx | 331 | no | Max % increase from cloning |
| `IPA_Max_Output_File_Size` | `UINT32` | config_ipa.cxx | 334 | no | Max output file size |
| `IPA_Output_File_Size` | `INT32` | config_ipa.cxx | 337 | no | % change of max output file size |
| `IPA_Enable_Old_Type_Merge` | `BOOL` | config_ipa.cxx | 341/343 | no | Old type merge phase (ifdef-dependent) |
| `IPA_Enable_Devirtualization` | `BOOL` | config_ipa.cxx | 347 | no | Devirtualization |
| `IPA_Enable_Fast_Static_Analysis_VF` | `BOOL` | config_ipa.cxx | 348 | no | Fast static analysis VF |
| `IPA_Enable_Original_VF` | `BOOL` | config_ipa.cxx | 349 | no | Original VF |
| `IPA_Enable_New_VF` | `BOOL` | config_ipa.cxx | 350 | no | New VF |
| `IPA_Inline_Original_VF` | `BOOL` | config_ipa.cxx | 351 | no | Inline original VF |
| `IPA_Inline_New_VF` | `BOOL` | config_ipa.cxx | 352 | no | Inline new VF |
| `IPA_Devirtualization_Input_File` | `const char*` | config_ipa.cxx | 353 | no | Devirtualization input file path |
| `IPA_During_Original_VF` | `BOOL` | config_ipa.cxx | 355 | no | Currently in original VF phase |
| `IPA_During_New_VF` | `BOOL` | config_ipa.cxx | 356 | no | Currently in new VF phase |
| `IPA_Enable_Whole_Program_Mode` | `BOOL` | config_ipa.cxx | 359 | no | Whole program mode |
| `IPA_Enable_Whole_Program_Mode_Set` | `BOOL` | config_ipa.cxx | 360 | no | Explicitly set flag |
| `IPA_Enable_Scale` | `BOOL` | config_ipa.cxx | 362 | no | Scale optimization |
| `IPA_Enable_AOT` | `BOOL` | config_ipa.cxx | 363 | no | AOT optimization |
| `Options_IPA` | `OPTION_DESC[]` | config_ipa.cxx | 365 | yes | Static option descriptor table for -IPA group |

### INLINE option variables (config_ipa.cxx)

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `INLINE_Enable` | `BOOL` | config_ipa.cxx | 715 | no | Master inliner enable |
| `INLINE_All` | `BOOL` | config_ipa.cxx | 716 | no | Inline everything possible |
| `INLINE_Optimize_Alloca` | `BOOL` | config_ipa.cxx | 717 | no | Fix stack for alloca inlining |
| `INLINE_Enable_Copy_Prop` | `BOOL` | config_ipa.cxx | 718 | no | Copy propagation during inlining |
| `INLINE_Enable_Subst_Copy_Prop` | `BOOL` | config_ipa.cxx | 719 | no | Aggressive substitution copy prop |
| `INLINE_F90` | `BOOL` | config_ipa.cxx | 720 | no | F90 parameter type compatibility |
| `INLINE_None` | `BOOL` | config_ipa.cxx | 721 | no | Inline nothing |
| `INLINE_Exceptions` | `BOOL` | config_ipa.cxx | 722 | no | Inline exception code |
| `INLINE_Keep_PU_Order` | `BOOL` | config_ipa.cxx | 723 | no | Retain input PU order |
| `INLINE_List_Actions` | `BOOL` | config_ipa.cxx | 724 | no | List inline actions |
| `INLINE_Max_Pu_Size` | `UINT32` | config_ipa.cxx | 725 | no | Max PU size default 5000 |
| `INLINE_Preemptible` | `BOOL` | config_ipa.cxx | 727 | no | Inline preemptible PUs |
| `INLINE_Static` | `BOOL` | config_ipa.cxx | 728 | no | Inline static functions |
| `INLINE_Static_Set` | `BOOL` | config_ipa.cxx | 729 | no | Explicitly set flag |
| `INLINE_Aggressive` | `BOOL` | config_ipa.cxx | 730 | no | Inline non-leaf, out-of-loop |
| `INLINE_First_Inline_Calls_In_Loops` | `BOOL` | config_ipa.cxx | 731 | no | Prioritize calls in loops |
| `INLINE_Enable_Split_Common` | `BOOL` | config_ipa.cxx | 732 | no | Split common for inliner |
| `INLINE_Enable_Auto_Inlining` | `BOOL` | config_ipa.cxx | 733 | no | Automatic inlining analysis |
| `INLINE_Enable_Restrict_Pointers` | `BOOL` | config_ipa.cxx | 734 | no | Allow restrict pointers |
| `INLINE_Recursive` | `BOOL` | config_ipa.cxx | 737 | no | Recursive inlining (KEY) |
| `INLINE_Param_Mismatch` | `BOOL` | config_ipa.cxx | 738 | no | Inline despite param mismatch (KEY) |
| `INLINE_Type_Mismatch` | `BOOL` | config_ipa.cxx | 739 | no | Inline despite type mismatch (KEY) |
| `INLINE_Check_Compatibility` | `CHECK_PARAM_COMPATIBILITY` | config_ipa.cxx | 741 | no | Parameter compatibility check (KEY) |
| `INLINE_Ignore_Bloat` | `BOOL` | config_ipa.cxx | 742 | no | Ignore code bloat (KEY) |
| `INLINE_Callee_Limit` | `UINT32` | config_ipa.cxx | 743 | no | Callee size limit for inline fns (KEY) |
| `INLINE_List_Names` | `OPTION_LIST*` | config_ipa.cxx | 747 | no | Must/never/file options |
| `INLINE_Spec_Files` | `OPTION_LIST*` | config_ipa.cxx | 748 | no | Specification files |
| `IPA_Group_Names` | `OPTION_LIST*` | config_ipa.cxx | 749 | no | Partition groupings |
| `IPA_Spec_Files` | `OPTION_LIST*` | config_ipa.cxx | 750 | no | IPA specification files |
| `INLINE_Skip_After` | `UINT32` | config_ipa.cxx | 751 | no | Skip inline edges after N |
| `INLINE_Skip_Before` | `UINT32` | config_ipa.cxx | 752 | no | Skip inline edges before N |
| `INLINE_Array_Bounds` | `BOOL` | config_ipa.cxx | 753 | no | Conforming array bounds |
| `INLINE_Use_Malloc_Mempool` | `BOOL` | config_ipa.cxx | 754 | no | Use malloc mempool for cloning |
| `INLINE_Free_Malloc_Mempool` | `BOOL` | config_ipa.cxx | 755 | no | Free malloc mempool for cloning |
| `INLINE_Inlined_Pu_Call_Graph` | `BOOL` | config_ipa.cxx | 757 | no | Lightweight inliner impl 2 |
| `INLINE_Inlined_Pu_Call_Graph2` | `BOOL` | config_ipa.cxx | 758 | no | Lightweight inliner impl 3 |
| `INLINE_Get_Time_Info` | `BOOL` | config_ipa.cxx | 759 | no | Generate timing info |
| `INLINE_Script_Name` | `char*` | config_ipa.cxx | 761 | no | Script name for inlining |
| `INLINE_Enable_Script` | `BOOL` | config_ipa.cxx | 762 | no | Enable inline script |
| `INLINE_Enable_Devirtualize` | `BOOL` | config_ipa.cxx | 763 | no | Devirtualization during inlining |
| `Options_INLINE` | `OPTION_DESC[]` | config_ipa.cxx | 765 | yes | Static option descriptor table for -INLINE group |

---

## Summary

### Counts by category

- **Mutable file-scope globals (definitions in .cxx files)**: ~148
  - `config_ipa.cxx` IPA flags: ~97 mutable globals
  - `config_ipa.cxx` INLINE flags: ~39 mutable globals
  - `config_ipa.cxx` file/misc: 4 (`Ipa_File_Name`, `Feedback_Filename`, `Annotation_Filename`, `Ipa_File`)
  - `ipa_section.cxx`: 5 (`Trace_Sections`, `Ivar`, `IPL_Loopinfo_Map`, `IPL_Access_Array_Map`, plus class static member defs)
  - `ipa_section_main.cxx`: 2 (`IPA_Ivar_Global_Count`, `IPA_Ivar_Count`) + class static member defs
  - `ipa_be_read.cxx`: 1 (`CURRENT_BE_SUMMARY`)
  - `ipa_section_print.cxx`: 1 (`IPA_Symbol`)
  - `ipa_lno_file.cxx`: 3 (`IPALNO_REVISION`, `old_sigsegv`, `old_sigbus`)

- **Static file-scope variables**: 6
  - `IPALNO_REVISION` (ipa_lno_file.cxx:90)
  - `old_sigsegv` (ipa_lno_file.cxx:92)
  - `old_sigbus` (ipa_lno_file.cxx:93)
  - `IPA_Symbol` (ipa_section_print.cxx:85)
  - `Options_IPA[]` (config_ipa.cxx:365)
  - `Options_INLINE[]` (config_ipa.cxx:765)
  - (Also RCS ID strings in headers, guarded by `_KEEP_RCS_ID`)

- **Extern declarations (in headers)**: ~130+ (mostly in config_ipa.h mirroring all the config_ipa.cxx definitions, plus a handful in ipa_section.h, ipa_section_main.h, ipa_be_read.h, opt_ipaa_io.h, opt_ipaa_summary.h, be_ipa_util.h)

- **Constants (const at file scope in headers)**: ~14
  - `ipa_cost_util.h`: 5 constants
  - `ipa_lno_file.h`: 7 constants
  - `opt_ipaa_summary.h`: 4 constants (SYMREF_IX_*, MODREF_CONSERVATIVE)
  - `config_ipa.cxx`: 1 (`DEFAULT_MIN_PROBABILITY`)

### Key observations

1. **config_ipa.cxx is by far the largest source of global state** -- approximately 140 mutable globals defining IPA and inliner configuration. These are all read/written as command-line-controlled flags. Encapsulating them into a config struct would be the single highest-impact change.

2. **ipa_section.cxx** has important runtime state: `Ivar`, `IPL_Loopinfo_Map`, `IPL_Access_Array_Map`, `Trace_Sections`. These are used during IPA/IPL array section analysis and would need to be part of an IPA context.

3. **ipa_section_main.cxx** defines `IPA_Ivar_Global_Count` and `IPA_Ivar_Count` which are counters used across the section analysis.

4. **CURRENT_BE_SUMMARY** (ipa_be_read.cxx) is a singleton summary object -- classic global state.

5. **IPAA_Summary** and **IPAA_Local_Map** (declared in opt_ipaa_summary.h and opt_ipaa_io.h) are global pointers to IPAA state, critical for alias analysis.

6. **Mod_Ref_Info_Table** (be_ipa_util.h) is a global mod/ref info table.

7. The `SYSTEM_OF_EQUATIONS` and `MAT<int>` class static members are technically class-level globals but are still shared mutable state.
