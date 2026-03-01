# ipa/inline/ + ipa/lw_inline/ + ipa/cord/ -- Global State Scan

## Status: COMPLETE

**Note**: `ipa/lw_inline/` does not exist as a directory. The lightweight inliner code is compiled from the same `ipa/inline/` sources using the `_LIGHTWEIGHT_INLINER` preprocessor macro.

## Files scanned

### ipa/inline/ (7 files)
- `inline.cxx` -- Main standalone inliner
- `inline_driver.cxx` -- Command-line processing and main() for inliner
- `inline_skip.cxx` -- Skip-list filter for inlining
- `inline_split_common.cxx` -- Split common blocks optimization
- `inline_stub.cxx` -- Stub functions for unresolved symbols
- `inline_utils.cxx` -- Utilities (file header setup, common block aliasing)
- `timelib.cxx` -- Timer library

### ipa/cord/ (2 files)
- `ipa_cord.cxx` -- Procedure cord (code reordering) tool
- `obj_info.cxx` -- ELF object file reader

---

## File-scope globals found

### inline.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `inliner_main_file_index` | `const UINT32` | inline.cxx | 136 | no | Constant, always 0 |
| `Inliner_Aux_Pu_Table` | `SCOPE**` | inline.cxx | 137 | no | Maps PUs to SCOPE |
| `scope_mpool` | `MEM_POOL` | inline.cxx | 139 | yes | Memory pool for scopes |
| `inline_mpool` | `MEM_POOL` | inline.cxx | 140 | yes | Memory pool for inliner |
| `pu` | `WN*` | inline.cxx | 142 | yes | Current program unit WHIRL tree |
| `is_cplusplus` | `BOOL` | inline.cxx | 143 | yes | Language flag |
| `dem_name` | `char[MAXDBUF]` | inline.cxx | 145 | yes | Demangled name buffer (#ifndef _LIGHTWEIGHT_INLINER) |
| `dem_name2` | `char[MAXDBUF]` | inline.cxx | 146 | yes | Second demangled name buffer (#ifndef _LIGHTWEIGHT_INLINER) |
| `ITotal_Inlined` | `int` | inline.cxx | 149 | yes | Counter: total calls inlined |
| `ITotal_Not_Inlined` | `int` | inline.cxx | 150 | yes | Counter: total calls not inlined |
| `result_times` | `double[LAST_PHASE]` | inline.cxx | 166 | yes | Array of timing results per phase (8 entries) |
| `PU_pool` | `MEM_POOL*` | inline.cxx | 168 | no | Pointer to PU memory pool |
| `Summary` | `SUMMARY*` | inline.cxx | 170 | no | Pointer to summary info |
| `Ipo_mem_pool` | `MEM_POOL` | inline.cxx | 172 | extern | References global from IPO |
| `Parent_Map` | `WN_MAP` | inline.cxx | 173 | extern | References global WHIRL parent map |
| `Debug_On` | `BOOL` | inline.cxx | 174 | extern | References global debug flag |
| `Verbose` | `BOOL` | inline.cxx | 175 | extern | References global verbose flag |
| `Trace_CopyProp` | `BOOL` | inline.cxx | 176 | no | Copy propagation tracing flag |
| `Trace_Inline` | `BOOL` | inline.cxx | 177 | no | Inline tracing flag |
| `Orig_Scope_tab` | `SCOPE*` | inline.cxx | 179 | no | Original scope table (#ifdef _LIGHTWEIGHT_INLINER) |
| `IP_File_header` | `IP_FILE_HDR_TABLE` | inline.cxx | 183 | extern | References global per-file header table |
| `Aux_Pu_Table` | `AUX_PU_TAB` | inline.cxx | 187 | no | Auxiliary PU table (#ifdef _LIGHTWEIGHT_INLINER) |
| `Ipl_Symbol_Names` | `DYN_ARRAY<char*>*` | inline.cxx | 196 | no | Dynamic array of symbol names |
| `Ipl_Function_Names` | `DYN_ARRAY<char*>*` | inline.cxx | 197 | no | Dynamic array of function names |
| `inline_counters` | `INLINE_COUNTERS*` | inline.cxx | 215 | yes | Per-node inline call counters |

### inline_driver.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `Irb_Output_Name` | `char*` | inline_driver.cxx | 92 | no | Output WHIRL IR filename |
| `Verbose` | `BOOL` | inline_driver.cxx | 97 | no | Verbose output flag (definition; extern'd in inline.cxx) |
| `IPA_Skip_List` | `SKIPLIST*` | inline_driver.cxx | 103 | no | List of skip options (always NULL here) |

### inline_split_common.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `trace_split_common` | `BOOL` | inline_split_common.cxx | 59 | yes | Tracing flag for split common |
| `IPA_SPLIT_COMMON` | `UINT32` | inline_split_common.cxx | 60 | yes | Split common threshold (128) |
| `Primary_Cache` | `const UINT` | inline_split_common.cxx | 276 | yes | Cache size constant (16KB) |
| `Secondary_Cache` | `const UINT` | inline_split_common.cxx | 277 | yes | Cache size constant (512KB) |
| `Primary_Delta` | `const UINT` | inline_split_common.cxx | 278 | yes | 5% of primary cache |
| `Secondary_Delta` | `const UINT` | inline_split_common.cxx | 279 | yes | 5% of secondary cache |

### timelib.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `cumtime` | `double[MAX_TIMERS]` | timelib.cxx | 74 | yes | Cumulative time per timer (100 entries) |
| `start_time` | `double[MAX_TIMERS]` | timelib.cxx | 75 | yes | Start time per timer (100 entries) |
| `timer_running` | `int[MAX_TIMERS]` | timelib.cxx | 76 | yes | Running flag per timer (100 entries) |
| `initialized` | `int` | timelib.cxx | 77 | yes | Package init flag |

### inline_stub.cxx

No file-scope mutable globals. Contains only stub function definitions inside `extern "C"` blocks. The `struct ip_file_hdr` at line 45 is a forward declaration, not a variable.

### inline_skip.cxx

No file-scope globals. All state is inside the `INL_SKIPLST` class methods.

### inline_utils.cxx

No file-scope globals. All functions use parameters or local variables. Typedefs and structs are defined at file scope but no variables.

---

### ipa_cord.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| `debug` | `int` | ipa_cord.cxx | 61 | no | Debug flag (non-static!) |
| `LEN` | `const int` | ipa_cord.cxx | 62 | no | Max line size constant (1000) |
| `MIN_FREQ` | `const int` | ipa_cord.cxx | 63 | no | Minimum frequency threshold (15) |
| `src_order` | `int` | ipa_cord.cxx | 205 | no | Source-order counter for procedures |

### obj_info.cxx

No file-scope globals. The `error()` function at line 55 is a free function but not a variable. All state is in the `File_Info` class.

---

## Summary

- **35 file-scope globals found** (including externs and const)
- **21 static variables** (file-scope `static`)
- **5 extern declarations** (referencing globals defined elsewhere)
- **9 non-static, non-extern mutable globals** (the most concerning for concurrency)

### Breakdown by category

**Critical mutable globals (non-static, non-extern, non-const):**
1. `Inliner_Aux_Pu_Table` (SCOPE**) -- inline.cxx:137
2. `PU_pool` (MEM_POOL*) -- inline.cxx:168
3. `Summary` (SUMMARY*) -- inline.cxx:170
4. `Trace_CopyProp` (BOOL) -- inline.cxx:176
5. `Trace_Inline` (BOOL) -- inline.cxx:177
6. `Orig_Scope_tab` (SCOPE*) -- inline.cxx:179 (conditional)
7. `Aux_Pu_Table` (AUX_PU_TAB) -- inline.cxx:187 (conditional)
8. `Ipl_Symbol_Names` (DYN_ARRAY<char*>*) -- inline.cxx:196
9. `Ipl_Function_Names` (DYN_ARRAY<char*>*) -- inline.cxx:197
10. `Irb_Output_Name` (char*) -- inline_driver.cxx:92
11. `Verbose` (BOOL) -- inline_driver.cxx:97
12. `IPA_Skip_List` (SKIPLIST*) -- inline_driver.cxx:103
13. `debug` (int) -- ipa_cord.cxx:61
14. `src_order` (int) -- ipa_cord.cxx:205

**Static mutable globals (internal linkage, still problematic for multi-instance):**
1. `scope_mpool` (MEM_POOL) -- inline.cxx:139
2. `inline_mpool` (MEM_POOL) -- inline.cxx:140
3. `pu` (WN*) -- inline.cxx:142
4. `is_cplusplus` (BOOL) -- inline.cxx:143
5. `dem_name` (char[MAXDBUF]) -- inline.cxx:145 (conditional)
6. `dem_name2` (char[MAXDBUF]) -- inline.cxx:146 (conditional)
7. `ITotal_Inlined` (int) -- inline.cxx:149
8. `ITotal_Not_Inlined` (int) -- inline.cxx:150
9. `result_times` (double[8]) -- inline.cxx:166
10. `inline_counters` (INLINE_COUNTERS*) -- inline.cxx:215
11. `trace_split_common` (BOOL) -- inline_split_common.cxx:59
12. `IPA_SPLIT_COMMON` (UINT32) -- inline_split_common.cxx:60
13. `cumtime` (double[100]) -- timelib.cxx:74
14. `start_time` (double[100]) -- timelib.cxx:75
15. `timer_running` (int[100]) -- timelib.cxx:76
16. `initialized` (int) -- timelib.cxx:77

**Static const (safe -- immutable):**
1. `Primary_Cache` (const UINT) -- inline_split_common.cxx:276
2. `Secondary_Cache` (const UINT) -- inline_split_common.cxx:277
3. `Primary_Delta` (const UINT) -- inline_split_common.cxx:278
4. `Secondary_Delta` (const UINT) -- inline_split_common.cxx:279

**Non-static const (safe -- immutable):**
1. `inliner_main_file_index` (const UINT32) -- inline.cxx:136
2. `LEN` (const int) -- ipa_cord.cxx:62
3. `MIN_FREQ` (const int) -- ipa_cord.cxx:63

**Extern declarations (reference globals elsewhere):**
1. `Ipo_mem_pool` (MEM_POOL) -- inline.cxx:172
2. `Parent_Map` (WN_MAP) -- inline.cxx:173
3. `Debug_On` (BOOL) -- inline.cxx:174
4. `Verbose` (BOOL) -- inline.cxx:175
5. `IP_File_header` (IP_FILE_HDR_TABLE) -- inline.cxx:183

### Notes on ipa/cord/

The cord directory implements a standalone utility (`gen_cord`) for procedure reordering based on call-graph profiling. It has its own `main()` function and runs as a separate process, so its globals (`debug`, `src_order`) are less relevant to the IPA refactoring effort. However, if this tool were ever embedded into the compiler process, those globals would need to be encapsulated.

### Notes on timelib.cxx

The timer library uses a classic C pattern of module-level static arrays as a timer pool. All four statics (`cumtime`, `start_time`, `timer_running`, `initialized`) form a cohesive unit of global state that could be encapsulated into a `TimerContext` struct.

### MEM_POOL inventory

Two `MEM_POOL` objects are declared in these files:
1. `scope_mpool` (static, inline.cxx:139)
2. `inline_mpool` (static, inline.cxx:140)

Plus one extern reference:
3. `Ipo_mem_pool` (extern, inline.cxx:172) -- defined elsewhere in the IPO subsystem

These are the primary memory allocation contexts for the inliner and would need to be per-instance in a refactored design.
