# Step 3: Migrate Struct-Opt Globals to IPA_Context

## Goal
Replace the 6 struct-optimization globals with accessors into `g_ipa_ctx->struct_opt`.

## Globals Being Migrated
All declared in `ipa_struct_opt.h:56-69`, defined in `ipa_struct_opt.cxx:41-49`:

| Global | Type | Default |
|--------|------|---------|
| `Struct_field_layout` | `Field_pos*` | `NULL` |
| `Struct_split_count` | `INT` | `0` |
| `complete_struct_relayout_type_id` | `TYPE_ID` (UINT8) | `0` |
| `struct_with_field_pointing_to_complete_struct_relayout_type_id[32]` | `TYPE_ID[32]` | `{0}` |
| `struct_with_field_pointing_to_complete_struct_relayout_field_num[32]` | `int[32]` | `{0}` |
| `num_structs_with_field_pointing_to_complete_struct_relayout` | `int` | `0` |

## Key Observation
These are LIVE code — used extensively in `ipa_struct_opt.cxx` and `ipo_struct_opt.cxx`.
The `#define` macro approach works because macros are textually replaced before compilation.

## Files Modified

### 1. `ipa_context.h` — Update IPA_Struct_Opt_State
Add the 3 missing array fields. Need to include `mtypes.h` for `TYPE_ID`.
Add `#define` constants for array sizes (or use literal 32/16).

### 2. `ipa_struct_opt.h` — Replace externs with macros
- Keep `Field_pos` typedef, `MAX_NUM_*` defines
- Replace 6 `extern` declarations with `#define` macros to `g_ipa_ctx->struct_opt.*`
- Add `#include "ipa_context.h"`

### 3. `ipa_struct_opt.cxx` — Remove variable definitions
- Remove lines 41-49 (the 6 variable definitions)
- The rest of the file (functions) uses the variable names which will now be macros

## Usage Sites (all LIVE)
- `ipa_struct_opt.cxx`: ~30+ references
- `ipo_struct_opt.cxx`: ~40+ references
- `opt_main.cxx`: ~2 references

## Verification
Docker build must pass — this is live code so the macros must expand correctly everywhere.
