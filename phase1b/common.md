# ipa/common/ -- Global State Scan

## Status: COMPLETE

19 source files scanned (.cxx and .c) in `osprey/ipa/common/`.

## File-scope globals found:

### ipc_type_merge.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| struct_by_name_idx | hash_map\<STR_IDX, TY_INDEX, ...\> | ipc_type_merge.cxx | 60 | no | Maps struct names to TY indices for type merging |
| duplicated_types | multimap_type (hash_multimap) | ipc_type_merge.cxx | 66 | no | Tracks duplicated struct types in merged table |
| to_update_incomplete_ty | hash_set\<TY_INDEX\> | ipc_type_merge.cxx | 69 | no | Incomplete TYs to update after main merge phase |
| updating_incomplete_ty | hash_set\<TY_INDEX\> | ipc_type_merge.cxx | 72 | no | Old incomplete TYs meeting new complete struct TY |
| file_tables | const IPC_GLOBAL_TABS* | ipc_type_merge.cxx | 106 | yes | Points to current file's global tables during merge |

### ipc_symtab_merge.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Aux_St_Tab | AUX_ST_TAB | ipc_symtab_merge.cxx | 93 | no | Auxiliary symbol table |
| Aux_St_Table | AUX_ST_TABLE | ipc_symtab_merge.cxx | 94 | no | Auxiliary symbol table (alternate) |
| num_predefined_st | UINT32 | ipc_symtab_merge.cxx | 95 | yes | Number of predefined symbols (pregs) |
| Aux_Pu_Table | AUX_PU_TAB | ipc_symtab_merge.cxx | 98 | no | Auxiliary PU (program unit) table |
| current_file_hdr | IP_FILE_HDR* | ipc_symtab_merge.cxx | 100 | yes | Points to current file header during merge |
| tcon_merge_map | TCON_MERGE_MAP* | ipc_symtab_merge.cxx | 182 | yes | Hash map for merging TCONs |
| st_merge_map | ST_MERGE_MAP* | ipc_symtab_merge.cxx | 183 | yes | Hash map for merging CLASS_CONST STs |
| merge_pool | MEM_POOL | ipc_symtab_merge.cxx | 184 | yes | Memory pool for merge operations |
| New_Symstr_Idx | SYMSTR_IDX_MAP* | ipc_symtab_merge.cxx | 190 | yes | Index map: old symstr -> merged symstr |
| New_St_Idx | ST_IDX_MAP* | ipc_symtab_merge.cxx | 191 | yes | Index map: old ST -> merged ST |
| New_Ty_Idx | TY_IDX_MAP* | ipc_symtab_merge.cxx | 192 | yes | Index map: old TY -> merged TY |
| New_Tcon_Idx | TCON_IDX_MAP* | ipc_symtab_merge.cxx | 193 | yes | Index map: old TCON -> merged TCON |
| St_To_Inito_Map | ST_TO_INITO_MAP* | ipc_symtab_merge.cxx | 194 | yes | Maps ST_IDX to INITO for const prop |
| ST_To_INITO_Map | ST_TO_INITO_MAP | ipc_symtab_merge.cxx | 197 | no | Public map: ST_IDX to corresponding INITO |
| Common_Block_Elements_Map | COMMON_BLOCK_ELEMENTS_MAP* | ipc_symtab_merge.cxx | 200 | no | Maps common block ST_IDX to array of its elements |
| Common_map_pool | MEM_POOL | ipc_symtab_merge.cxx | 201 | yes | Memory pool for common block maps |

### ipc_ty_hash.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| ty_map | TY_IDX_MAP* | ipc_ty_hash.cxx | 60 | yes | Maps raw TY indices to merged indices |
| str_map | SYMSTR_IDX_MAP* | ipc_ty_hash.cxx | 61 | yes | Maps raw string indices to merged indices |
| ty_table | const TY* | ipc_ty_hash.cxx | 64 | yes | Base pointer to TY table in file |
| fld_table | const FLD* | ipc_ty_hash.cxx | 65 | yes | Base pointer to FLD table in file |
| arb_table | const ARB* | ipc_ty_hash.cxx | 66 | yes | Base pointer to ARB table in file |
| tylist_table | const TYLIST* | ipc_ty_hash.cxx | 67 | yes | Base pointer to TYLIST table in file |
| ty_to_be_inserted | const TY* | ipc_ty_hash.cxx | 69 | yes | Pointer to TY currently being inserted |
| ty_hash_table | NEW_TY_HASH_TABLE* | ipc_ty_hash.cxx | 1028 | yes | Main type hash table for merging |
| fld_hash_table | FLD_HASH_TABLE* | ipc_ty_hash.cxx | 1029 | yes | Field hash table for merging |
| arb_hash_table | ARB_HASH_TABLE* | ipc_ty_hash.cxx | 1030 | yes | Array bound hash table for merging |
| tylist_hash_table | TYLIST_HASH_TABLE* | ipc_ty_hash.cxx | 1031 | yes | Type list hash table for merging |
| recursive_table | RECURSIVE_TY_HASH_TABLE* | ipc_ty_hash.cxx | 1033 | yes | Hash table for recursive type detection |
| recursive_type | RECURSIVE_TYPE* | ipc_ty_hash.cxx | 1038 | yes | Temp record of all TY_IDX of a recursive type |
| collecting_recursive_ty | UINT | ipc_ty_hash.cxx | 1040 | yes | Flag: currently collecting recursive type |

### ipc_option.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| number_of_partitions | INT | ipc_option.cxx | 93 | no | Number of IPA partitions (conditional on !_STANDALONE_INLINER) |
| user_inline_request | INLINE_PU_MAP | ipc_option.cxx | 119 | yes | Map of user-specified must/never inline PUs |
| must_routines | ARRAY_OF_STRINGS | ipc_option.cxx | 121 | yes | List of routines user requested must-inline |
| skip_request | INLINE_PU_MAP | ipc_option.cxx | 123 | yes | Map of user-specified skip PUs |
| user_inline_edge_request | INLINE_EDGE_MAP | ipc_option.cxx | 139 | yes | Map of user-specified inline edges |
| filenames | ARRAY_OF_STRINGS | ipc_option.cxx | 216 | yes | Standalone inliner: list of input files (conditional on _STANDALONE_INLINER) |
| libnames | ARRAY_OF_STRINGS | ipc_option.cxx | 217 | yes | Standalone inliner: list of library names (conditional on _STANDALONE_INLINER) |
| Multifile_Pool | MEM_POOL | ipc_option.cxx | 218 | yes | Standalone inliner: memory pool for multifile (conditional on _STANDALONE_INLINER) |
| multifile_mempool_initialized | BOOL | ipc_option.cxx | 219 | yes | Standalone inliner: pool init flag (conditional on _STANDALONE_INLINER) |

### ipc_compile.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| makefile_name | char* | ipc_compile.cxx | 143 | yes | Full pathname of the makefile |
| makefile | FILE* | ipc_compile.cxx | 144 | yes | File handle for makefile |
| infiles | vector\<const char*\>* | ipc_compile.cxx | 146 | yes | List of input WHIRL file names |
| outfiles | vector\<const char*\>* | ipc_compile.cxx | 147 | yes | List of output file basenames |
| outfiles_fullpath | vector\<const char*\>* | ipc_compile.cxx | 148 | yes | List of output file full paths |
| commands | vector\<const char*\>* | ipc_compile.cxx | 149 | yes | Command lines for compiling files |
| ProMP_Idx | vector\<UINT32\>* | ipc_compile.cxx | 150 | yes | ProMP index tracking |
| comments | vector\<vector\<const char*\>\>* | ipc_compile.cxx | 151 | yes | Makefile comment lines per file |
| input_symtab_name | char[PATH_MAX] | ipc_compile.cxx | 154 | yes | Name of symtab file from IPA (e.g. symtab.I) |
| whirl_symtab_name | char[PATH_MAX] | ipc_compile.cxx | 158 | yes | Name of WHIRL symtab file (e.g. symtab.G) |
| elf_symtab_name | char[PATH_MAX] | ipc_compile.cxx | 161 | yes | Name of ELF symtab file (e.g. symtab.o) |
| symtab_command_line | const char* | ipc_compile.cxx | 164 | yes | Command for producing WHIRL and ELF symtab |
| symtab_extra_args | const char* | ipc_compile.cxx | 165 | yes | Extra args for symtab compilation |
| command_map | COMMAND_MAP_TYPE* | ipc_compile.cxx | 180 | yes | Map short cmd names to full paths |

### ipc_link.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| ld_flags_part1 | ARGV* | ipc_link.cxx | 61 | no | Linker argv before the first WHIRL obj |
| ld_flags_part2 | ARGV* | ipc_link.cxx | 62 | no | Rest of the linker command line |
| current_ld_flags | ARGV* | ipc_link.cxx | 63 | no | Pointer to currently active ld flags |
| comma_list | ARGV* | ipc_link.cxx | 68 | no | Comma-separated list for archive members |
| comma_list_byte_count | UINT32 | ipc_link.cxx | 69 | no | Byte count of comma_list |

### ipc_bread.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IPA_Has_Feedback | BOOL | ipc_bread.cxx | 72 | no | Set if ANY input file contains feedback info |
| ERROR_VALUE | const int | ipc_bread.cxx | 78 | yes | Constant sentinel value (-1) |
| dst_idx_map | const IPC_GLOBAL_IDX_MAP* | ipc_bread.cxx | 213 | no (unnamed ns) | Global for DST traversal callback (anonymous namespace = internal linkage) |

### ipc_file.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| IP_File_header | IP_FILE_HDR_TABLE | ipc_file.cxx | 43 | no | Master array of per-file headers for all input files |

### ipc_main.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Type_Merge_Pool | MEM_POOL | ipc_main.cxx | 151 | yes | Memory pool for type merging |
| ipa_dot_so_initialized | BOOL | ipc_main.cxx | 153 | yes | Flag: ipa.so initialization done |
| known_library | const char*[] | ipc_main.cxx | 207 | yes | List of known system library names (array/table) |

### ipc_bwrite.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| dummy_pu_list | DUMMY_GLOBAL_PRAGMA_PU (vector\<PU_Info*\>) | ipc_bwrite.cxx | 111 | no | List of dummy PUs holding global pragmas |

### ipc_symtab.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| ipa_dot_so_initialized | BOOL | ipc_symtab.cxx | 427 | yes | Flag: ipa.so initialization done (standalone inliner path) |
| IPA_Target_Type | IP_TARGET_TYPE | ipc_symtab.cxx | 432 | no | Target ABI type (32 vs 64 bit); conditional on KEY |

### ipc_pic.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Refcount_sym_idx | AUX_IDX* | ipc_pic.cxx | 405 | yes | Array of reference-count symbol indices for GP-relative optimization |

### ipc_dst_utils.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| min_block_size | const INT32 | ipc_dst_utils.cxx | 52 | yes | Constant: minimum block size (256) |
| DST_64_align | const INT32 | ipc_dst_utils.cxx | 53 | yes | Constant: 64-bit alignment (8) |
| DST_32_align | const INT32 | ipc_dst_utils.cxx | 54 | yes | Constant: 32-bit alignment (4) |
| DST_char_align | const INT32 | ipc_dst_utils.cxx | 55 | yes | Constant: char alignment (1) |
| DST_default_align | const INT32 | ipc_dst_utils.cxx | 56 | yes | Constant: default alignment = 64-bit (8) |

### init.cxx

| Variable | Type | File | Line | static? | Notes |
|----------|------|------|------|---------|-------|
| Ipa_Initializer | IPA_INIT (struct) | init.cxx | 59 | no | Static-init object that wires up function pointers at load time; conditional on __linux__ |

## Extern declarations found (references to globals defined elsewhere):

| Declaration | File | Line | Notes |
|-------------|------|------|-------|
| extern SUMMARY_SYMBOL* (*IPA_get_symbol_file_array_p)(...) | init.cxx | 48 | Function pointer defined in be.so |
| extern IPA_NODE* (*Get_Node_From_PU_p)(PU_Info*) | init.cxx | 49 | Function pointer defined in be.so |
| extern IP_FILE_HDR_TABLE *IP_File_header_p | init.cxx | 50 | Pointer to file header table in be.so |
| extern hash_map\<...\> struct_by_name_idx | ipc_ty_hash.cxx | 73 | References global in ipc_type_merge.cxx |
| extern ARGV *ld_flags_part2 | ipc_main.cxx | 203 | References global in ipc_link.cxx |
| extern char *psclp_arg | ipc_compile.cxx | 110 | References linker's psclp argument |
| extern "C" void add_to_tmp_file_list(char*) | ipc_bwrite.cxx | 97 | Weak extern to linker utility |
| extern "C" char* create_unique_file(const char*, char) | ipc_bwrite.cxx | 100 | Weak extern to linker utility |
| extern "C" char* create_tmp_file(char*) | ipc_bwrite.cxx | 103 | Weak extern to linker utility |
| extern void WN_free_input(void*, off_t) | ipc_bwrite.cxx | 365 | Extern to WHIRL I/O routine |

## Files with NO file-scope globals:

- **ip_graph.cxx** -- No file-scope variables (only class methods and functions)
- **ipc_daVinci.cxx** -- No file-scope variables
- **ipc_partition.c** -- No file-scope variables (only local functions)

## Summary

- **73 file-scope globals** found total across 13 files (out of 19 files scanned)
- **49 static variables** at file scope (internal linkage)
- **19 non-static globals** (external linkage, visible to other translation units)
- **1 unnamed-namespace global** (dst_idx_map in ipc_bread.cxx -- internal linkage like static)
- **10 extern declarations** (references to globals defined elsewhere)
- **5 static const** values in ipc_dst_utils.cxx (effectively compile-time constants, low concern)
- **4 MEM_POOL instances** (merge_pool, Common_map_pool, Multifile_Pool, Type_Merge_Pool)
- **6 files with NO globals**: ip_graph.cxx, ipc_daVinci.cxx, ipc_partition.c (plus ipc_dst_merge.cxx, ipc_dst_utils.cxx with only const values)

## Key observations for refactoring (Goal 5)

1. **Heaviest global state**: `ipc_symtab_merge.cxx` (16 globals), `ipc_ty_hash.cxx` (14 globals), and `ipc_compile.cxx` (14 globals) are the worst offenders.

2. **Type merging state cluster**: The globals in `ipc_ty_hash.cxx`, `ipc_type_merge.cxx`, and `ipc_symtab_merge.cxx` form a tightly coupled cluster around type/symbol table merging. These would naturally form a "MergeContext" struct.

3. **Compilation state cluster**: The globals in `ipc_compile.cxx` (makefile, infiles, outfiles, commands, etc.) form a self-contained "CompilationContext" that tracks post-IPA compilation state.

4. **Linker state cluster**: The globals in `ipc_link.cxx` (ld_flags_part1/2, comma_list, etc.) form a "LinkerContext".

5. **MEM_POOL globals**: 4 separate memory pools are used as file-scope globals. These would need to be either passed as parameters or grouped into a context struct.

6. **Inline option state**: The maps in `ipc_option.cxx` (user_inline_request, skip_request, etc.) form an "InlineOptionsContext".

7. **The `dst_idx_map` hack**: In `ipc_bread.cxx` line 213, a comment explicitly says "This must be global because the DST traversal routine takes a function pointer, not a function object with local state." This is a classic case where a callback API forces global state.

8. **Conditional compilation**: Several globals are only compiled under specific #ifdef guards (_STANDALONE_INLINER, KEY, etc.), meaning different build configurations have different global state footprints.
