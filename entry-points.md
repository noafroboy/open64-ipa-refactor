# IPA Entry Points & Control Flow

## Status: COMPLETE

## Executables / Shared Objects

### ipa.so (main IPA shared object)
- **Built from**: `osprey/ipa/main/Makefile.gbase` -- links all files from `ipa/main/analyze/`, `ipa/main/optimize/`, `ipa/common/`
- **Exported symbols** (see `ipa/main/Exported`): `ipa_dot_so_init`, `ipa_driver`, `Signal_Cleanup`, plus various utility symbols
- **Entry points**: `ipa_dot_so_init()` and `ipa_driver()` -- called by the linker (ld) when it detects WHIRL objects
- **How it gets loaded**: The linker (`ld`) dlopen's `ipa.so` when it encounters WHIRL `.o` files during link time. The linker calls `ipa_dot_so_init()` for initialization (possibly during symtab processing), then `ipa_driver()` as the main entry.

### ipl.so (local analysis phase)
- **Built from**: `osprey/ipa/local/Makefile.gbase`
- **Exported symbols** (see `ipa/local/Exported`): `ipl_main`, `Ipl_Init`, `Ipl_Init_From_Ipa`, `Ipl_Fini`, `Ipl_Extra_Output`, `Perform_Procedure_Summary_Phase`, `Summary`, and many array/section utilities
- **Entry point**: `ipl_main()` -- called from the backend (`be.so`) during compilation of individual translation units
- **Purpose**: Generates per-procedure summary information (call sites, formals, globals, array sections, etc.) that IPA later consumes

### inline (standalone inliner executable)
- **Built from**: `osprey/ipa/inline/Makefile.gbase`
- **Has `main()`**: Yes, in `inline_driver.cxx:333`
- **Entry point**: `main()` -> `Inliner()` (in `inline.cxx:1082`)
- **Purpose**: Standalone single-file inliner. Reads a `.B` WHIRL file, builds a call graph within the file, performs inlining decisions, writes a `.I` output file.

### lw_inline (lightweight inliner executable)
- **Built from**: `osprey/ipa/lw_inline/Makefile.gbase`
- **Same source** as `inline` but compiled with `-D_LIGHTWEIGHT_INLINER`
- **Purpose**: A lighter-weight version of the standalone inliner, used during the normal compilation pipeline

### ipa_cord (cord/reordering utility)
- **Built from**: `osprey/ipa/cord/Makefile.gbase`
- **Has `main()`**: Yes, in `ipa_cord.cxx:418`
- **Purpose**: Reads a call graph dump + object file list and produces a procedure ordering specification for code layout optimization

---

## High-Level Phase Order

### Phase 0: Local Summary (ipl.so -- runs BEFORE ipa.so, during per-file compilation)

```
be.so -> ipl_main() -> Process_Command_Line()
be.so -> Ipl_Init()    -- creates Summary object
be.so -> [for each PU] Perform_Procedure_Summary_Phase()  -- summarizes one PU
be.so -> Ipl_Fini()    -- finalizes global addr-taken attributes
be.so -> Ipl_Extra_Output()  -- writes summary sections to .o file
```

### Phases 1-8: IPA Proper (ipa.so -- runs at link time)

The linker calls into `ipa.so` via the following sequence:

```
LINKER
  |
  +--> ipa_dot_so_init()    [Phase 1: Initialization]
  |      |-- MEM_Initialize()
  |      |-- Preconfigure()
  |      |-- IP_set_target()           -- sets Target_ABI, Target_ISA
  |      |-- Dont_Use_WN_Free_List()
  |      |-- Init_Operator_To_Opcode_Table()
  |      |-- Initialize_Symbol_Tables(TRUE)
  |      |-- Initialize_Auxiliary_Tables()
  |      |-- Initialize_Type_Merging_Hash_Tables()
  |      +-- Set_FILE_INFO_ipa()
  |
  +--> [linker reads WHIRL objects, processes symtabs, merges types]
  |    (calls process_whirl32/64 from ipa.so for each WHIRL .o)
  |
  +--> ipa_driver(argc, argv)    [Phase 2: Driver]
         |-- ipa_dot_so_init() if not already done
         |-- Verify_Common_Block_Layout()
         |-- Clear_Extra_Auxiliary_Tables()
         |-- MEM_POOL_Delete(&Type_Merge_Pool)
         |-- Process_IPA_Options()
         |-- detect_whole_program_mode()
         |-- create_tmpdir()
         |-- Pic_optimization() / Fix_up_static_functions()
         |
         +--> Perform_Interprocedural_Optimization()  [Phase 3-8]
                |
                |-- [Phase 3: Analysis setup]
                |   ipa_compile_init()          -- if ipacom enabled
                |   BE_Symtab_Initialize()
                |
                |-- [Phase 4: Analysis]
                |   Perform_Interprocedural_Analysis()  --> see detailed breakdown below
                |
                |-- [Phase 5: AutoGnum]
                |   Autognum_Driver()            -- if enabled
                |
                |-- [Phase 6: Symtab processing]
                |   ipacom_process_symtab()      -- if ipacom enabled
                |
                |-- [Phase 7: Alias classification setup]
                |   IP_ALIAS_CLASSIFICATION init
                |   Ip_alias_class->Init_maps()  -- if enabled
                |
                |-- [Phase 8: Transformations]
                |   IPO_main(IPA_Call_Graph)      --> see detailed breakdown below
                |
                |-- [Phase 9: Finalization]
                |   Ip_alias_class->Classify_initialized_data()
                |   IP_write_global_symtab()
                |   Perform_Alias_Class_Annotation()
                |   BE_Symtab_Finalize()
                |
                +-- [Phase 10: Backend compilation / linking]
                    ipacom_doit()                 -- generates makefile, invokes make, never returns
```

---

## Detailed Entry Points

### ipa_dot_so_init() -- ipc_main.cxx:156
- **Purpose**: One-time initialization of the IPA shared object. Sets up memory, configures target architecture, initializes symbol tables and type merging.
- **Globals initialized**:
  - `ipa_dot_so_initialized` = TRUE
  - `Target_ABI`, `Target_ISA`, `Use_32_Bit_Pointers` (via `IP_set_target`)
  - `Type_Merge_Pool` (MEM_POOL for type merging)
  - Global symbol tables (via `Initialize_Symbol_Tables`)
  - Auxiliary tables (via `Initialize_Auxiliary_Tables`)
  - Type merging hash tables
  - `File_info` (marked as IPA-generated)
  - `IPA_Enable_Relocatable_Opt` (conditionally)
- **Globals read**: `ld_ipa_opt[]` (linker option flags)
- **Calls**: `MEM_Initialize`, `Preconfigure`, `IP_set_target`, `Dont_Use_WN_Free_List`, `Init_Operator_To_Opcode_Table`, `Initialize_Symbol_Tables`, `Initialize_Auxiliary_Tables`, `MEM_POOL_Initialize`, `Initialize_Type_Merging_Hash_Tables`, `Set_FILE_INFO_ipa`

### ipa_driver() -- ipc_main.cxx:274
- **Purpose**: Top-level IPA driver called by the linker after all WHIRL objects have been read. Processes IPA options, performs PIC optimization, then delegates to `Perform_Interprocedural_Optimization()`.
- **Globals initialized**:
  - `IPA_Enable_Cloning` = FALSE (disabled, not ported)
  - `IPA_Enable_AutoGnum` (target-dependent)
  - `IPA_Enable_Whole_Program_Mode` (auto-detected)
  - `IPA_Enable_Picopt`, `IPA_Enable_Relocatable_Opt` (conditionally)
  - `IPA_Max_Jobs` (set to # CPUs if 0)
- **Globals read**: `ipa_dot_so_initialized`, `ld_ipa_opt[]`, `IP_File_header`
- **Calls**: `ipa_dot_so_init` (if needed), `Verify_Common_Block_Layout`, `Clear_Extra_Auxiliary_Tables`, `Process_IPA_Options`, `detect_whole_program_mode`, `create_tmpdir`, `Pic_optimization` or `Fix_up_static_functions`, `Perform_Interprocedural_Optimization`

### Perform_Interprocedural_Optimization() -- ipo_main.cxx:2716
- **Purpose**: Master orchestration function. Calls analysis, then transformation, then output phases in sequence.
- **Globals initialized**:
  - `Ip_alias_class` (IP_ALIAS_CLASSIFICATION pointer)
  - `ip_alias_class_mem_pool`
- **Globals read**: `IP_File_header`, `IPA_Call_Graph`, `IPA_Enable_ipacom`, `IPA_Enable_Alias_Class`, `IPA_Enable_AOT`, various IPA_Enable_* flags
- **Calls** (in order):
  1. `ipa_compile_init()` -- if ipacom enabled
  2. `BE_Symtab_Initialize()`
  3. `Perform_Interprocedural_Analysis()` -- the analysis phase
  4. `Autognum_Driver()` -- if enabled
  5. `ipacom_process_symtab()` -- if ipacom enabled
  6. `Ip_alias_class->Init_maps()` -- if alias class enabled
  7. `IPO_main(IPA_Call_Graph)` -- the transformation phase
  8. `Ip_alias_class->Classify_initialized_data()` -- if alias class enabled
  9. `IP_write_global_symtab()` -- write merged global symtab
  10. `Perform_Alias_Class_Annotation()` -- if alias class enabled
  11. `IPA_NystromAliasAnalyzer::clean()`
  12. `BE_Symtab_Finalize()`
  13. `ipacom_doit()` -- generates makefile and invokes make; **never returns**

### Perform_Interprocedural_Analysis() -- ipa_main.cxx:270
- **Purpose**: The main analysis phase. Processes input files, builds the call graph, then runs all analysis passes (global variable optimization, dead function elimination, devirtualization, alias analysis, constant propagation, cloning analysis, array section analysis, preoptimization, inlining analysis).
- **Globals initialized/modified**:
  - `IPA_Call_Graph` (via `Build_Call_Graph`)
  - `IPA_Class_Hierarchy` (via `Build_Class_Hierarchy`, if devirt enabled)
  - `IPA_Concurrency_Graph` (if siloed ref enabled)
  - `Total_Prog_Size`, `Total_Dead_Function_Weight`
  - `Ipa_cprop_pool`, `Global_mem_pool`, `local_cprop_pool` (MEM_POOLs for cprop)
  - `IPA_Enable_Array_Sections` (may be set to FALSE)
  - `IPA_Enable_Cprop` (may be set to FALSE if no alias info)
  - `IPA_Max_Total_Clones`
- **Globals read**: `IP_File_header`, `IPA_Call_Graph`, `Orig_Prog_Weight`, numerous `IPA_Enable_*` flags
- **Sub-phases** (in order, each with `Temporary_Error_Phase`):

| # | Sub-phase | Function | Condition |
|---|-----------|----------|-----------|
| 1 | Read/process input files | `IPA_Process_File()` for each `IP_File_header[i]` | Always |
| 2 | Padding/Split analysis | `Padding_Analysis()` | `IPA_Enable_Padding \|\| IPA_Enable_Split_Common` |
| 3 | Nystrom alias init | `IPA_NystromAliasAnalyzer::create()` | `Alias_Nystrom_Analyzer` |
| 4 | **Call Graph Construction** | `Build_Call_Graph()` | Always |
| 5 | EH / side-effect propagation | Traverse call graph POSTORDER | `IPA_Enable_EH_Region_Removal \|\| IPA_Enable_Pure_Call_Opt` |
| 6 | Inline script analysis | `Perform_Inline_Script_Analysis()` | `INLINE_Enable_Script` |
| 7 | Nested PU relations | `Build_Nested_Pu_Relations()` | `has_nested_pu` |
| 8 | Global Variable Optimization | `Optimize_Global_Variables()` | `IPA_Enable_DVE \|\| IPA_Enable_CGI` |
| 9 | Field reorder legality | `IPA_reorder_legality_process()` | `IPA_Enable_Reorder` |
| 10 | Struct opt legality | `IPA_struct_opt_legality()` | `IPA_Enable_Struct_Opt` |
| 11 | **Dead Function Elimination** | `Eliminate_Dead_Func()` | `IPA_Enable_DFE` |
| 12 | Fast static VF analysis | `IPA_Fast_Static_Analysis_VF()` | `IPA_Enable_Fast_Static_Analysis_VF` |
| 13 | Devirtualization (legacy) | `Build_Class_Hierarchy()`, `IPA_devirtualization()` | `IPA_Enable_Devirtualization` |
| 14 | **Alias Analysis (IPAA)** | `IPAA::Do_Simple_IPAA()`, `IPA_NystromAliasAnalyzer::solver()` | `IPA_Enable_Simple_Alias` |
| 15 | Cloning analysis | `IPA_FORMALS_IN_ARRAY_SECTION_DF` dataflow | `IPA_Enable_Array_Sections && cloning params` |
| 16 | **Constant Propagation** | `IPA_CPROP_DF_FLOW::Init()/Solve()` | `IPA_Enable_Cprop` |
| 17 | Quasi-to-real clone conversion | Traverse call graph POSTORDER | After cprop |
| 18 | Struct access preprocessing | `Preprocess_struct_access()` | `IPA_Enable_Preopt` |
| 19 | Pre-optimization | `IPA_Preoptimize(node)` per node | `IPA_Enable_Preopt_Set && IPA_Enable_Preopt` |
| 20 | Siloed references | `IPA_Concurrency_Graph->Collect_siloed_references()` | `IPA_Enable_Siloed_Ref` |
| 21 | Array section propagation | `IPA_ARRAY_DF_FLOW::Init()/Solve()` | `IPA_Enable_Array_Sections` |
| 22 | Preopt finalize | `IPA_Preopt_Finalize()` | `IPA_Enable_Preopt` |
| 23 | **Inlining Analysis** | `Perform_Inline_Analysis()` | `IPA_Enable_Inline \|\| IPA_Enable_DCE` |
| 24 | No-return proc identification | `IPA_identify_no_return_procs()` | Always |

### Build_Call_Graph() -- ipa_cg.cxx:1647
- **Purpose**: Constructs the IPA call graph from the per-file summaries.
- **Globals initialized**:
  - `IPA_Call_Graph` = new `IPA_CALL_GRAPH` (allocated from `Malloc_Mem_Pool`)
  - `IPA_Graph_Undirected` (if PU reorder by edge freq)
  - `IPA_Call_Graph_Built` = TRUE
- **Calls**: `For_all_entries(IP_File_header, add_nodes())`, `For_all_entries(IP_File_header, add_edges())`, `Connect_call_graph()`

### IPO_main() -- ipo_main.cxx:2314
- **Purpose**: The main transformation/optimization phase. Walks the call graph in transformation order, applying per-node and per-edge transformations (constant propagation, inlining, dead call elimination, cloning, etc.), then writes out the transformed PUs.
- **Globals initialized**:
  - `Ipo_mem_pool`, `Recycle_mem_pool`, `IPA_LNO_mem_pool` (MEM_POOLs)
  - `IPA_LNO_Summary` (if array sections enabled)
- **Globals read**: `IPA_Call_Graph`, `IPA_Common_Table`, `IPA_Enable_*` flags
- **Calls** (in order):
  1. `IPO_Pad_Symtab()` -- if padding enabled
  2. `IPO_Split_Common()` -- if split common enabled
  3. `IPO_get_new_ordering()`, `IPO_reorder_Fld_Tab()` -- if field reorder enabled
  4. `Init_Num_Calls_Processed()`
  5. `IPA_Create_Builtins()`
  6. `Build_Transformation_Order()` -- determines traversal order
  7. `IPA_Remove_Regions()` -- if EH region removal enabled
  8. Legality checks for struct opt / array remapping (two passes)
  9. **Main transformation loop** (for each node in `walk_order`):
     - `Perform_Transformation(node, cg)` which calls:
       - `IPO_Process_node()` -- per-node transformations (cprop, padding, struct opt, cloning)
       - `IPO_Process_edge()` -- per-edge transformations (inline, DCE, readonly param)
       - `node->Write_PU()` / `Delete_Proc()` -- emit or delete processed PUs

### Perform_Transformation() -- ipo_main.cxx:1362
- **Purpose**: Process a single caller node and all its successor edges in the call graph.
- **Calls**: `Init_inline()`, `IPO_Process_node()` (on caller and each unprocessed callee), `IPO_Process_edge()` (on each edge), then either `Delete_Proc()` or `Write_PU()` for completed nodes.

### IPO_Process_node() -- ipo_main.cxx:658
- **Purpose**: Apply per-node transformations to a single PU before edge processing.
- **Operations**: Read PU info, apply icall optimization, virtual function devirt, padding, common-const propagation, struct optimization, field reorder, cloning, constant propagation.
- **Calls**: `IP_READ_pu_infos`, `IPA_update_ehinfo_in_pu`, `IPO_Process_Icalls`, `IPO_Process_Virtual_Functions`, `IPO_Pad_Whirl`, `IPO_propagate_globals`, `IPO_WN_Update_For_Struct_Opt`, `IPO_Modify_WN_for_field_reorder`, `cg->Create_Clone`, `IPA_Propagate_Constants`

### IPO_Process_edge() -- ipo_main.cxx:753
- **Purpose**: Apply per-edge transformations (inline, DCE, readonly param, cloning rename, pure call opt).
- **Calls**: `Mark_readonly_param`, `Rename_Call_To_Cloned_PU`, `Reset_param_list`, `Delete_Call` (for DCE), `Inline_Call` (for inlining)

### Perform_Inline_Analysis() -- ipa_inline.cxx:2155
- **Purpose**: Analyze the call graph and mark edges for inlining (or not). Does NOT perform actual inlining -- just sets attributes on edges. The actual inlining happens in `IPO_Process_edge()` during the transformation phase.
- **Globals initialized**: `inline_count` (if DFE enabled), `INVOCATION_COST` vector
- **Calls**: `Init_inline_parameters`, `Inline_Analyzer::analyze()`, `Inline_Analyzer::report()`

---

## Local Analysis Phase (ipl.so)

### ipl_main() -- ipl_main.cxx:226
- **Purpose**: Entry point for the local (per-file) summary phase. Called from `be.so` with command-line arguments. Processes IPL-specific options.
- **Globals initialized**: `Optlevel`, various `Do_*` flags
- **Calls**: `Process_Command_Line()`

### Ipl_Init() -- ipl_main.cxx:243
- **Purpose**: Post-global-symtab initialization. Creates the `Summary` object and initializes array section writing.
- **Globals initialized**: `Summary` (SUMMARY object, allocated from `Malloc_Mem_Pool`)
- **Calls**: `CXX_NEW(SUMMARY(...))`, `Init_write_asections()`

### Ipl_Init_From_Ipa() -- ipl_main.cxx:257
- **Purpose**: Variant of `Ipl_Init` called when IPL is invoked from within IPA (for re-summarization during preopt).
- **Globals initialized**: `Summary` (from provided MEM_POOL)

### Perform_Procedure_Summary_Phase() -- ipl_main.cxx:268
- **Purpose**: Summarize a single PU. This is the core local analysis function.
- **Globals read/written**: `Summary`, `Trace_IPA`, `DoPreopt`
- **Calls**: `Summary->Summarize(w)` (the main work), `WB_IPL_Set_Scalar_Summary`, `WB_IPL_Set_Array_Summary`

### Ipl_Fini() -- ipl_main.cxx:309
- **Purpose**: Finalize local analysis. Sets global address-taken attributes.
- **Calls**: `Summary->Set_global_addr_taken_attrib()`

### Ipl_Extra_Output() -- ipl_main.cxx:317
- **Purpose**: Write summary data to the output WHIRL file.
- **Calls**: `IPA_write_summary()`, `IPA_Trace_Summary_File()`, `WN_write_flags()`, `IPL_Write_Elf_Symtab()`

---

## Standalone Inliner (inline executable)

### main() -- inline_driver.cxx:333
- **Purpose**: Entry point for the standalone inliner executable.
- **Flow**:
  1. `Handle_Signals()`, `MEM_Initialize()`, `Dont_Use_WN_Free_List()`
  2. Error handler setup
  3. `Preconfigure()`
  4. `INLINER::Process_Command_Line()` -- parses `-INLINE:*` options, `-O` level, trace flags
  5. If `INLINE_Enable` is FALSE: just symlink input to output
  6. Else: `Configure()`, `Init_Operator_To_Opcode_Table()`, then call `Inliner()`

### Inliner() -- inline.cxx:1082
- **Purpose**: Main body of the standalone inliner.
- **Globals initialized**: `Summary`, `IPA_Call_Graph`, `scope_mpool`, `inline_mpool`, `Orig_Prog_Weight`, `Total_Prog_Size`
- **Flow**:
  1. Push memory pools (`MEM_src_pool`, `MEM_pu_pool`, `MEM_local_pool`)
  2. `Initialize_Symbol_Tables(FALSE)`
  3. `Open_Output_Info()`
  4. `Process_Local_File()` -- read PUs and generate summaries
  5. `Process_Non_Local_Files()` -- read referenced non-local files (full inliner only)
  6. `Process_Non_Local_Libraries()` -- read referenced libraries (full inliner only)
  7. `INLINE_Split_Common()` -- if enabled
  8. `Fix_Aliased_Commons()`
  9. `Build_Call_Graph()` -- constructs the intra-file call graph
  10. `Build_Nested_Pu_Relations()` -- if nested PUs exist
  11. `Eliminate_Dead_Func()` -- if DFE enabled
  12. `Perform_inlining()` -- walks call graph, calls `Perform_Inline_Analysis()` then `Inline_callees_into_caller()`
  13. `Inliner_Write_PUs()` -- write out modified PUs
  14. `Write_Global_Info()`
  15. `Free_Input_Info()`

---

## Compilation Orchestration (ipc_compile.cxx)

### ipa_compile_init() -- ipc_compile.cxx:270
- **Purpose**: Initialize the IPA-to-backend compilation pipeline. Sets up vectors for tracking input/output files, creates a temporary makefile.
- **Globals initialized**: `infiles`, `outfiles`, `outfiles_fullpath`, `commands`, `comments`, `makefile_name`, `makefile`, `command_map`

### ipacom_process_symtab() -- ipc_compile.cxx:565
- **Purpose**: Register the global symtab file (e.g., `symtab.I`) and generate the command to compile it into `symtab.G` and `symtab.o`.

### ipacom_doit() -- ipc_compile.cxx:859
- **Purpose**: Finalize the makefile and invoke `make` to compile all `.I` files into `.o` files and optionally link the final executable. **This function never returns** -- it exec's into `make`.

---

## Key Global Variables

| Variable | Type | Declared | Initialized | Purpose |
|----------|------|----------|-------------|---------|
| `IPA_Call_Graph` | `IPA_CALL_GRAPH*` | `ipa_cg.cxx:122` | `Build_Call_Graph()` at `ipa_cg.cxx:1649` | The central call graph for all IPA analysis |
| `IP_File_header` | `IP_FILE_HDR_TABLE` | `ipc_file.h:361` | Populated by linker as it reads WHIRL objects | Per-file metadata (symbol tables, PU lists, summaries) |
| `Summary` | `SUMMARY*` | `ipl_main.cxx:169` (ipl) / `inline.cxx:170` (inline) | `Ipl_Init()` or `Ipl_Init_From_Ipa()` | Summary information for the local analysis phase |
| `Ip_alias_class` | `IP_ALIAS_CLASSIFICATION*` | (ipo_main.cxx) | `Perform_Interprocedural_Optimization()` at line 2771 | Alias classification object |
| `IPA_LNO_Summary` | `IPA_LNO_WRITE_SUMMARY*` | (ipo_main.cxx) | `IPO_main()` at line 2325 | LNO summary for array section info |
| `IPA_Class_Hierarchy` | (class hierarchy) | (ipa_main.cxx) | `Build_Class_Hierarchy()` at line 602 | Class hierarchy for devirtualization |
| `IPA_Concurrency_Graph` | `IPA_PCG*` | (ipa_main.cxx) | Line 772 | Parallel concurrency graph for siloed references |
| `Orig_Prog_Weight` | `UINT32` | `ipa_cg.h` | Set during call graph construction | Original program size in WHIRL weight |
| `Total_Prog_Size` | `UINT32` | (ipa_main.cxx) | Line 491, updated by DFE and inlining | Current total program size |
| `Ipo_mem_pool` | `MEM_POOL` | (ipo_main.cxx) | `IPO_main()` at line 2316 | Memory pool for inline transformations |
| `Ipa_cprop_pool` | `MEM_POOL` | (ipa_main.cxx) | Line 686 | Memory pool for constant propagation |

---

## Phase-Ordering Constraints

1. **ipl.so runs before ipa.so**: Local summaries must be generated per-file before IPA can read them at link time.
2. **`ipa_dot_so_init()` before `ipa_driver()`**: Symbol tables and type merging must be initialized before analysis.
3. **`IPA_Process_File()` before `Build_Call_Graph()`**: PU infos and summaries must be loaded before the call graph can be built.
4. **`Build_Call_Graph()` before all analysis passes**: The call graph is required by DVE/CGI, DFE, alias analysis, cprop, inlining, etc.
5. **Alias analysis before constant propagation**: If alias info is unavailable, cprop and common_const are disabled (ipa_main.cxx:632).
6. **Constant propagation before preoptimization**: Cprop may create clones that preopt then processes.
7. **All analysis before `IPO_main()`**: Analysis marks edges/nodes with attributes (inline, deletable, cprop'd constants); IPO_main acts on those attributes.
8. **`IPO_main()` (transformation) before `IP_write_global_symtab()`**: Transformations may modify the symtab (new clones, deleted functions).
9. **`IP_write_global_symtab()` before `ipacom_doit()`**: The global symtab file must exist before the backend can compile `.I` files.
10. **`Perform_Inline_Analysis()` is analysis-only**: It marks edges but does NOT inline. Actual inlining happens in `IPO_Process_edge()` during transformation.
11. **`ipacom_doit()` never returns**: It exec's into `make` to run the backend on all `.I` files.

---

## Invocation Summary Diagram

```
                   COMPILATION PIPELINE
                   ====================

  Source files (.c/.f)
       |
       v
  Frontend (cc1/gfec/mfef90)  -->  WHIRL .B files
       |
       v
  Backend (be.so)
       |
       +-- loads ipl.so
       |     |
       |     +-- ipl_main()           -- option processing
       |     +-- Ipl_Init()           -- create Summary
       |     +-- Perform_Procedure_Summary_Phase()  -- per-PU summary
       |     +-- Ipl_Fini()           -- finalize
       |     +-- Ipl_Extra_Output()   -- write summaries to .o
       |
       v
  WHIRL .o files (with IPA summary sections)
       |
       v
  Linker (ld)
       |
       +-- detects WHIRL .o files
       +-- dlopen("ipa.so")
       |     |
       |     +-- ipa_dot_so_init()    -- initialize
       |     +-- [process each WHIRL .o, merge symtabs/types]
       |     +-- ipa_driver()         -- main IPA entry
       |           |
       |           +-- Perform_Interprocedural_Optimization()
       |                 |
       |                 +-- Perform_Interprocedural_Analysis()
       |                 |     |-- IPA_Process_File (read inputs)
       |                 |     |-- Build_Call_Graph
       |                 |     |-- Optimize_Global_Variables (DVE/CGI)
       |                 |     |-- Eliminate_Dead_Func (DFE)
       |                 |     |-- IPAA (alias analysis)
       |                 |     |-- IPA_CPROP_DF_FLOW (constant prop)
       |                 |     |-- Perform_Inline_Analysis (mark edges)
       |                 |     +-- [other analyses]
       |                 |
       |                 +-- IPO_main(IPA_Call_Graph)
       |                 |     |-- Build_Transformation_Order
       |                 |     |-- [for each node in order]:
       |                 |     |     Perform_Transformation
       |                 |     |       IPO_Process_node (transforms)
       |                 |     |       IPO_Process_edge (inline/DCE)
       |                 |     |       Write_PU / Delete_Proc
       |                 |     +-- [handle remaining PUs]
       |                 |
       |                 +-- IP_write_global_symtab()
       |                 +-- ipacom_doit()   -- exec's make; NEVER RETURNS
       |
       v
  Backend (be) compiles each .I file --> .o files
       |
       v
  Linker (ld) links .o files --> final executable
```
