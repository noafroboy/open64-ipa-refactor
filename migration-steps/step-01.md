# Step 1: Add Header Files (Phase 3a — No Behavior Change)

## Goal
Add three new files to `osprey/ipa/common/` that define the context structs.
No existing code is modified. The new code compiles but is never called.

## New Files

### 1. `ipa_options.h`
- `IPA_Options` struct with ~140 fields copied from `config_ipa.h` externs
- Grouped by: core enables, inlining heuristics, trace flags, limits, misc
- `extern const IPA_Options* g_ipa_options` transitional global

### 2. `ipa_context.h`
- Forward declarations for heavy types (IPA_CALL_GRAPH, etc.)
- Sub-structs: IPA_Feedback_State, IPA_Struct_Opt_State, IPA_Visualization_State,
  IPA_Array_Section_State, IPA_Reorder_State, IPA_CHG_State, IPA_Common_State,
  IPA_Compile_State, IPA_Alias_Analysis_State, IPA_Inline_Stats, IPA_Cprop_State,
  IPA_Merge_State
- Main `IPA_Context` struct composing all sub-structs
- `extern IPA_Context* g_ipa_ctx` transitional global

### 3. `ipa_context.cxx`
- `IPA_Context_Init()` — allocates g_ipa_ctx, copies globals into sub-structs
- `IPA_Options_Init()` — copies config_ipa globals into g_ipa_options
- `IPA_Context_Fini()` — cleanup

## Build Integration
- Add `ipa_context.cxx` to `IPA_COMMON_CXX_SRCS` in `osprey/ipa/main/Makefile.gbase`

## Verification
- `./build-open64-docker.sh` passes
- No behavior change (init/fini are defined but never called)

## Source of Truth
- `config_ipa.h` lines 84-387 → IPA_Options fields
- `ipa_feedback.h` → IPA_Feedback_State fields
- `ipa_struct_opt.h` → IPA_Struct_Opt_State fields
- `ipc_daVinci.h` → IPA_Visualization_State fields
- `ipa_section_prop.h` → IPA_Array_Section_State fields
- `ipa_reorder.h` → IPA_Reorder_State fields
- `ipa_chg.h` → IPA_CHG_State fields
- `ipa_pad.h` → IPA_Common_State fields
- `ipa_inline.h` / `ipa_cg.h` → IPA_Inline_Stats fields
- `ipa_cprop.h` → IPA_Cprop_State fields
