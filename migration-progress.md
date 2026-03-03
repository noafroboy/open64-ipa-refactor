# IPA Refactor Phase 3 — Migration Progress

## Current Status
- **Current Step**: 17 (completed — lifecycle calls wired up)
- **Branch**: `refactor/ipa-context` on `noafroboy/open64`
- **Base**: `origin/develop`

## Step Log

| Step | Description | Status | Commit | Date |
|------|-------------|--------|--------|------|
| 0 | Create branch & tracking | done | — | 2026-03-02 |
| 1 | Add header files (ipa_options.h, ipa_context.h, ipa_context.cxx) | done | 2be11a1b | 2026-03-02 |
| 2 | Migrate feedback FDs | done | 3119d5cb | 2026-03-02 |
| 3 | Migrate struct-opt state | done | 208ce0bb | 2026-03-02 |
| 4 | Migrate visualization state | done | 4e092e6e | 2026-03-02 |
| 5 | Migrate array-section state | done | 4e092e6e | 2026-03-02 |
| 6 | Migrate reorder state | done | 4e092e6e | 2026-03-02 |
| 7 | Migrate CHG state | done | 4e092e6e | 2026-03-02 |
| 8 | Migrate common-block state | done | cf7aec30 | 2026-03-02 |
| 9 | Migrate compile state | deferred | — | — file-static, not extern; can revisit in Step 20 |
| 10 | Migrate alias-analysis state | deferred | — | — file-static, not extern; can revisit in Step 20 |
| 11 | Migrate inline stats + IPA_Graph_Undirected | done | 5006fe9d | 2026-03-02 |
| 12 | Migrate cprop state | done | 5006fe9d | 2026-03-02 |
| 13 | Migrate merge state | done | db8d8cb5 | 2026-03-03 |
| 14 | Migrate IPA_Options reads (config_ipa globals) | deferred | — | — Hundreds of replacements; can be done incrementally later |
| 15 | Migrate call-graph pointer + IPA_Call_Graph_Built | done | 9b4af881 | 2026-03-03 |
| 16 | Migrate file-header table | partial | 9b4af881 | 2026-03-03 — Opt_Options_Inconsistent done; IP_File_header deferred (static-constructor init pattern) |
| 17 | Add IPA_Context lifecycle calls at entry points | done | bceb780b | 2026-03-03 |
| 18 | Thread IPA_Context through analysis passes | deferred | — | — ~60-70 function signatures across 25 files |
| 19 | Thread IPA_Context through optimization passes | deferred | — | — Same scope as Step 18 |
| 20 | Remove transitional globals, final cleanup | pending | — | — |

## Notes
- 2026-03-02: History rebased to remove bogus copyright headers and "Phase 3" references from file content and commit messages. All SHAs changed. Review fix (lowercase IPA_Options fields) added as commit 1aa9c190.
- Steps 9-10 are deferred, not cancelled. Their globals are file-static (not extern), so they don't contribute to cross-module coupling.
- Step 14: The ~140 config_ipa.h globals are already snapshotted into g_ipa_options by IPA_Context_Init(). Replacing direct reads (e.g., `IPA_Enable_DFE` → `g_ipa_options->enable_DFE`) across ~30 files is tedious but mechanical. Can be done incrementally.
- Step 16: IP_File_header has a static-constructor pattern in init.cxx that takes its address before main(). Needs special handling.
- Steps 18-19: Function parameter threading (~60-70 signatures, ~25 files) is the largest remaining task. Should be planned carefully with the user.

## Summary of What's Done
All extern IPA globals identified in the audit are now encapsulated in IPA_Context sub-structs with backward-compatible #define macros:
- Feedback FDs, struct-opt, visualization, array-section, reorder, CHG, common-block, inline stats, cprop, merge state (Aux_St_Tab, Aux_Pu_Table, etc.), call-graph pointer, Opt_Options_Inconsistent
- IPA_Context_Init() is called at the main IPA driver entry point
- IPA_Context_Alloc() is called at the inliner entry point
- All 10 commits build successfully across all 3 targets (ipa, inline, lw_inline)

## Recovery Protocol
1. Read this file to find current step
2. Check `git log refactor/ipa-context` on `noafroboy/open64` fork
3. Read `migration-steps/step-NN.md` for the current step's plan
4. Resume work from where the last commit left off
