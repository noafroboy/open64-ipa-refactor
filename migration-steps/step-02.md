# Step 2: Migrate Feedback FD Globals to IPA_Context

## Goal
Replace the 5 `extern FILE*` feedback globals with accessors into `g_ipa_ctx->feedback`.

## Globals Being Migrated
All declared in `ipa_feedback.h:63-67`, never defined anywhere:
- `IPA_Feedback_exl_fd` → `g_ipa_ctx->feedback.exl_fd`
- `IPA_Feedback_dve_fd` → `g_ipa_ctx->feedback.dve_fd`
- `IPA_Feedback_dfe_fd` → `g_ipa_ctx->feedback.dfe_fd`
- `IPA_Feedback_prg_fd` → `g_ipa_ctx->feedback.prg_fd`
- `IPA_Feedback_con_fd` → `g_ipa_ctx->feedback.con_fd`

## Key Observation
ALL usages are inside `#ifdef TODO` blocks — this is entirely dead code.
The globals are declared extern but never defined. Zero runtime impact.

## Files Modified
1. `osprey/ipa/main/analyze/ipa_feedback.h` — replace 5 externs with `#include "ipa_context.h"` + 5 `#define` macros
2. No .cxx definition files to change (variables were never defined)

## Usage Sites (all inside #ifdef TODO, no changes needed)
- `ipa_main.cxx:506,519` — dve_fd
- `ipa_main.cxx:551,564` — dfe_fd
- `ipa_main.cxx:738-740` — con_fd
- `ipa_main.cxx:842,856` — prg_fd
- `ipa_inline.cxx:241,302,304,305,2184,2193,2198` — prg_fd
- `ipa_cprop.cxx:531` — con_fd

## Verification
Docker build must pass — the macros expand inside `#ifdef TODO` so they won't be compiled, but the header must parse cleanly.
