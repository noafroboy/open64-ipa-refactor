# MEM_POOL System Investigation

## Status: COMPLETE

## MEM_POOL Struct Definition

From `/Users/kevinliu/dev/open64/open64/osprey/common/util/mempool.h` (lines 477-511):

```c
struct mem_pool {
  const char      *name;            /* Name of the pool for debugging only. */
  MEM_POOL_BLOCKS *blocks;          /* Current top of allocation stack.
                                     * This changes when the pool is pushed or popped. */
  MEM_POOL        *rest;            /* Used to keep a link of all initialized pools. */
  MEM_PURE_STACK  *pure_stack;      /* Stack of pointers, one for each push level,
                                     * each pointer pointing to first allocated piece
                                     * in that push-level. */
  mBOOL            bz;              /* To allocate zero'd memory or not. */
  mBOOL            frozen;          /* If frozen, subsequent Push/Pop ops disabled. */
  mUINT16          magic_num;       /* Set to 0xabcd when initialized, 0 when deleted. */
  MEM_STAT        *alloc_site_list; /* Used to report statistics about call sites. */
};
```

Supporting internal structures (from `memory.c`):

```c
struct mem_block {
  size_t     avail;         /* Bytes still available */
  MEM_PTR    ptr;           /* Next byte to allocate */
  MEM_BLOCK *rest;          /* Linked list of blocks */
};

struct mem_pool_blocks {
  MEM_BLOCK       *block;       /* list of memory blocks */
  MEM_LARGE_BLOCK *large_block; /* list of large memory blocks */
  MEM_BLOCK       *base_block;  /* block at push time */
  MEM_PTR         *base_ptr;    /* MEM_BLOCK_ptr at push time */
  size_t           base_avail;  /* MEM_BLOCK_avail at push time */
  MEM_POOL_BLOCKS *rest;        /* stack of allocation levels / free list */
};

struct mem_large_block {
  MEM_LARGE_BLOCK *next;        /* doubly-linked list */
  MEM_LARGE_BLOCK *prev;
  MEM_PTR          ptr;         /* user memory block */
  MEM_POOL_BLOCKS *base;        /* back-pointer to head of list */
  INT32            page_index;  /* index in Large_Block_Page_Table */
};

struct mem_pure_stack {
  MEM_PTR          last_alloc;  /* latest alloc'd storage */
  MEM_PURE_STACK  *prev_stack;  /* pointer to previous push level */
};
```

## Global State in MEM_POOL Implementation

Every global variable in `memory.c`, categorized by severity for per-context use:

### CRITICAL -- Shared mutable state used during normal allocation

| Variable | Type | Line | Description | Impact |
|----------|------|------|-------------|--------|
| `Large_Block_Page_Table` | `static LARGE_BLOCK_PAGE_TABLE_L2` | 204 | **Global page table for ALL large block allocations across ALL pools.** Two-level table mapping large block indices to virtual addresses. Used by `Add_Large_Block()`, `Update_Large_Block()`, `Validate_And_Delete_Large_Block()`. | **BLOCKER.** Every `Allocate_Large_Block()` (allocations > 0x800 = 2048 bytes) goes through this single global table. Two contexts allocating large blocks simultaneously will corrupt this table. |
| `free_mem_pool_blocks_list` | `static MEM_POOL_BLOCKS *` | 556 | **Global free list of MEM_POOL_BLOCKS structs.** When a pool is popped, its MEM_POOL_BLOCKS goes onto this list. When a pool is pushed, it takes from this list first. | **BLOCKER.** Shared across all pools. Two contexts pushing/popping simultaneously will corrupt this list. Also means freed blocks from one context can be reused by another (which may be acceptable but is still shared state). |
| `mem_overhead_pool` | `static MEM_POOL` | 564 | **Internal overhead pool** used to allocate MEM_POOL_BLOCKS when the free list is empty. The MEM_POOL system's own allocator. | **BLOCKER.** When `free_mem_pool_blocks_list` is empty, `MEM_POOL_Push_P()` allocates from this pool (line 1635). Multiple contexts will share this pool's allocation state. |
| `overhead_blocks` | `static MEM_POOL_BLOCKS` | 560 | Initial blocks for `mem_overhead_pool`. | Tied to `mem_overhead_pool`. |

### MODERATE -- Shared mutable state in non-default code paths

| Variable | Type | Line | Description | Impact |
|----------|------|------|-------------|--------|
| `The_Default_Mem_Pool` | `static MEM_POOL *` | 578 | Pointer set by `MEM_POOL_Set_Default()`. When code passes `Default_Mem_Pool` (which is `(MEM_POOL *)0`), it resolves to this. | **Problematic if any IPA code uses `Default_Mem_Pool`.** A single global pointer -- two contexts would need different defaults. |
| `purify_pools` | `BOOL` (global) | 528 | Flag for "purify" mode (all allocs use malloc/free for debugging). | Only matters if PURIFY_MEMPOOLS env var is set. Low risk in production. |
| `purify_pools_trace` | `static BOOL` | 529 | Trace flag for purify mode. | Same as above. |
| `purify_pools_trace_x` | `static BOOL` | 530 | Extended trace flag for purify mode. | Same as above. |

### LOW -- Debug/statistics only (compiled with `Is_True_On`)

| Variable | Type | Line | Description | Impact |
|----------|------|------|-------------|--------|
| `initialized_pools` | `static MEM_POOL *` | 590 | Linked list of all initialized pools (debug builds only). Traversed by `MEM_Trace()`. | Only in debug builds. Pools add themselves here on init, remove on delete. |
| `mem_tracing_enabled` | `static BOOL` | 587 | Whether memory tracing is active. | Debug only. |
| `sub_mem_tracing_enabled` | `static BOOL` | 588 | Sub-pool tracing control. | Debug only. |
| `call_site_hash_tab[503]` | `static MEM_STAT *[]` | 598 | Hash table for per-callsite allocation statistics. | Debug only. |
| `free_int_lists` | `static INT_LIST *` | 372 | Free list for INT_LIST nodes used by statistics tracking. | Debug only. |
| `special_address` | `const char *` | 1142 | Debug address tracking. | Debug only. |
| `special_address_owner` | `const char *` | 1143 | Debug address tracking. | Debug only. |

### TRIVIALLY SHARED -- Predefined pools (not a problem if per-context pools are separate)

| Variable | Type | Lines | Description |
|----------|------|-------|-------------|
| `MEM_local_pool` etc. (8 pools) | `MEM_POOL` | 134-156 | 8 predefined global pools (local, src, pu, phase x zeroed/non-zeroed). |
| `MEM_local_pool_ptr` etc. (8 ptrs) | `MEM_POOL *` | 139-156 | Pointers to above. Used by backward-compat macros (`L_Alloc`, `Src_Alloc`, etc.). |

### EFFECTIVELY CONSTANT -- Safe

| Variable | Type | Line | Description |
|----------|------|------|-------------|
| `redzone_size` | `static int` | 98 | Set once during Valgrind init. Effectively constant. |

## MEM_Initialize() Analysis

From `memory.c` lines 2220-2281:

```c
void MEM_Initialize(void)
{
  // 1. Parse PURIFY_MEMPOOLS env var (sets purify_pools global)
  // 2. Initialize 8 predefined global pools:
  MEM_POOL_Initialize(&MEM_local_pool, "Local", TRUE);
  MEM_POOL_Initialize(&MEM_src_pool, "Source", TRUE);
  MEM_POOL_Initialize(&MEM_pu_pool, "Program unit", TRUE);
  MEM_POOL_Initialize(&MEM_phase_pool, "Phase", TRUE);
  MEM_POOL_Push(&MEM_local_pool);
  MEM_POOL_Push(&MEM_src_pool);
  MEM_POOL_Push(&MEM_pu_pool);
  MEM_POOL_Push(&MEM_phase_pool);
  // ...same for nz variants...
}
```

**Can it be called multiple times?** No guard against re-initialization. Calling it twice would re-initialize the 8 predefined pools (triggering a DevWarn about double-init) and push them again, leaking the previous push level. It also re-parses the env var each time.

**Can it be called per-context?** It is **not** designed for per-context use. It operates exclusively on the 8 global predefined pools. However, the underlying `MEM_POOL_Initialize()` function can safely initialize any user-provided `MEM_POOL` struct -- `MEM_Initialize()` is just a convenience wrapper.

**Key insight:** `MEM_Initialize()` is irrelevant for per-context pools. What matters is `MEM_POOL_Initialize_P()`, which does work on arbitrary pool structs. The problem is not initialization -- it is the global state touched *during* allocation.

## MEM_POOL_Push/Pop Mechanism

### Push (lines 1572-1665)

The push/pop state is **per-pool** (good), but the mechanism relies on **global shared resources** (bad):

1. A new `MEM_POOL_BLOCKS` struct is needed to represent the new allocation level.
2. First, it checks the **global** `free_mem_pool_blocks_list` free list (line 1621).
3. If the free list is empty, it allocates from the **global** `mem_overhead_pool` (line 1635).
4. The new `MEM_POOL_BLOCKS` is linked into the pool's own `blocks` chain (per-pool state).

```c
if ( free_mem_pool_blocks_list != NULL ) {
    pb = free_mem_pool_blocks_list;                        // GLOBAL
    free_mem_pool_blocks_list = MEM_POOL_BLOCKS_rest(pb);  // GLOBAL
} else {
    pb = TYPE_MEM_POOL_ALLOC(MEM_POOL_BLOCKS, &mem_overhead_pool);  // GLOBAL
}
MEM_POOL_blocks(pool) = pb;  // per-pool
```

### Pop (lines 1710-1844)

1. Frees memory blocks allocated since the matching push (per-pool).
2. Frees large blocks, using the **global** `Large_Block_Page_Table` via `Validate_And_Delete_Large_Block()`.
3. Returns the popped `MEM_POOL_BLOCKS` to the **global** `free_mem_pool_blocks_list` (line 1819-1820).

```c
MEM_POOL_BLOCKS_rest(bsp) = free_mem_pool_blocks_list;  // GLOBAL
free_mem_pool_blocks_list = bsp;                          // GLOBAL
```

**Assessment:** The push/pop stack itself is per-pool, which is good. But the allocation of stack nodes and the large block bookkeeping goes through global state.

## MEM_POOL_Alloc Mechanism

From `MEM_POOL_Alloc_P()` (lines 1303-1364):

1. **Default pool resolution:** `if (pool == Default_Mem_Pool) pool = The_Default_Mem_Pool;` -- global.
2. **Malloc pool passthrough:** `if (pool == Malloc_Mem_Pool)` -- just calls `malloc()`, no globals.
3. **Purify mode:** if `purify_pools` is true, uses `calloc()` directly (global flag).
4. **Statistics:** if `mem_tracing_enabled`, calls `Site_Account_Alloc()` which uses `call_site_hash_tab[]` (global, debug only).
5. **Normal allocation path** (`Raw_Allocate`):
   - For small allocations (<= 0x800 bytes): bumps pointer in the pool's current `MEM_BLOCK`. If the block is full, calls `Allocate_Block()` which does a raw `malloc()`. **No global state touched** for small allocations with available space.
   - For large allocations (> 0x800 bytes): calls `Allocate_Large_Block()` which does `malloc()` AND calls `Add_Large_Block()` which modifies the **global** `Large_Block_Page_Table`.

**Key finding:** Small allocations (the common case, <= 2048 bytes) are almost entirely per-pool. Large allocations always touch the global `Large_Block_Page_Table`.

## Special Pools

### `Malloc_Mem_Pool` -- defined as `(MEM_POOL *) 1`

A sentinel value, not an actual pool. When passed to any MEM_POOL function, it causes a direct passthrough to `malloc()`/`realloc()`/`free()`. No global state beyond the standard C heap. Safe for concurrent use (as safe as malloc itself).

### `Default_Mem_Pool` -- defined as `(MEM_POOL *) 0`

A sentinel value (NULL pointer). When passed to any MEM_POOL function, it resolves to `The_Default_Mem_Pool`, a **global** pointer set by `MEM_POOL_Set_Default()`. This is a global mutable singleton -- only one "default" can exist at a time.

### `mem_overhead_pool` -- internal

A statically initialized `MEM_POOL` used by the MEM_POOL system itself to allocate `MEM_POOL_BLOCKS` structs. Shared across ALL pools in the process. This is the "bootstrap" pool.

### Predefined pools (8 total)

`MEM_local_pool`, `MEM_src_pool`, `MEM_pu_pool`, `MEM_phase_pool` and their `_nz` variants. These are global variables initialized by `MEM_Initialize()`. They are used directly by backward-compatibility macros (`L_Alloc`, `Src_Alloc`, `Pu_Alloc`, `P_Alloc`).

## Thread Safety

**There are NO thread-safety mechanisms whatsoever.** No mutexes, no atomic operations, no thread-local storage, no locks. The MEM_POOL system was designed for single-threaded use only.

A grep for `mutex`, `lock`, `atomic`, `thread`, `pthread`, `__thread`, and `thread_local` across the entire `memory.c` file returns zero results.

## Assessment: Can MEM_POOLs be per-context?

**PARTIALLY -- with targeted modifications to the implementation.**

### What already works per-pool (good news):

- The `MEM_POOL` struct itself is self-contained. Each pool has its own blocks, push/pop stack, name, and flags.
- Small allocations (the common case, <= 2048 bytes) only touch per-pool state when there is available space in the current block.
- `MEM_POOL_Initialize()` works on any user-provided `MEM_POOL` struct.
- `CXX_MEM_POOL` (C++ class) demonstrates that stack-allocated, locally-scoped pools already work.

### What blocks per-context use (the problems):

1. **`Large_Block_Page_Table` (CRITICAL):** Every allocation > 2048 bytes goes through this single global two-level page table. This table is used to validate large blocks during `MEM_POOL_FREE()` and `MEM_POOL_Pop()`. Two contexts doing large allocations simultaneously will corrupt this table.

2. **`free_mem_pool_blocks_list` (CRITICAL):** A global free list of `MEM_POOL_BLOCKS` structs shared across ALL pools. Every `Push` takes from it, every `Pop` returns to it. Two contexts pushing/popping simultaneously will corrupt this list.

3. **`mem_overhead_pool` (CRITICAL):** The internal "overhead" pool used to allocate `MEM_POOL_BLOCKS` when the free list is empty. Shared across all pools.

4. **`The_Default_Mem_Pool` (MODERATE):** A single global pointer. If any IPA code passes `Default_Mem_Pool` to allocation functions, it will resolve to the wrong pool in a multi-context scenario.

5. **Statistics globals (LOW):** `call_site_hash_tab`, `initialized_pools`, `free_int_lists`, `mem_tracing_enabled` -- only active in debug builds with `Is_True_On`.

### The bottom line:

You **cannot** simply instantiate multiple independent sets of MEM_POOLs in the same process today. Even if each context has its own `MEM_POOL` structs, the implementation shares critical global state behind the scenes. However, the fix is tractable because the globals are few and well-isolated.

## Recommendations for IPA Refactor

### Option A: Fix the MEM_POOL implementation (RECOMMENDED)

Make the MEM_POOL system truly independent per-pool by eliminating the three critical globals. This is the cleanest solution and benefits the entire compiler:

1. **Eliminate `Large_Block_Page_Table`:** This table exists only to validate that a pointer is a real large block during `MEM_POOL_FREE()`. The simpler alternative: each `MEM_POOL_BLOCKS` can maintain its own large block list (it already does via `large_block` field). The page table is redundant -- it was added as a safety check. Either:
   - Remove the page table entirely and rely on the existing per-pool `large_block` doubly-linked list for validation.
   - Or make the page table per-pool (add a `LARGE_BLOCK_PAGE_TABLE_L2` field to `MEM_POOL` or `MEM_POOL_BLOCKS`).

2. **Eliminate `free_mem_pool_blocks_list`:** Instead of a global free list, each pool (or context) should manage its own free list of `MEM_POOL_BLOCKS`. Alternatively, just use `malloc()`/`free()` for `MEM_POOL_BLOCKS` allocation -- they are small (48 bytes) and infrequent (only on push/pop). The overhead pool optimization is premature optimization from the 1990s.

3. **Eliminate `mem_overhead_pool`:** Once `free_mem_pool_blocks_list` is removed or made per-context, this pool is no longer needed. Replace with direct `malloc()`.

4. **Make `The_Default_Mem_Pool` per-context:** Either pass the default pool explicitly, or ensure IPA code never uses `Default_Mem_Pool`.

**Estimated effort:** ~100 lines of changes in `memory.c`. The three critical globals (`Large_Block_Page_Table`, `free_mem_pool_blocks_list`, `mem_overhead_pool`) can be replaced with per-pool or direct-malloc approaches. No API changes needed -- all changes are internal to the implementation.

### Option B: Give each IPA context its own isolated allocator

Instead of fixing the MEM_POOL globals, wrap MEM_POOL access so each context serializes through its own allocator. This avoids modifying `memory.c` but adds complexity:

- Each IPA context gets its own set of MEM_POOL variables.
- Guard all MEM_POOL operations with a context lock (if concurrent) or ensure contexts never run simultaneously.
- This does NOT fix the global state problem -- it merely works around it by preventing concurrent access.

**Not recommended** because it does not actually enable parallelism and adds fragile coupling.

### Option C: Don't share MEM_POOL code between contexts (fork-based)

If IPA contexts run in separate processes (via fork), each gets its own copy of all globals. This is the simplest option if fork-based parallelism is acceptable.

**Limited utility** -- this is already how Open64 works today with separate IPA child processes.

### Recommended Path Forward

Go with **Option A**. The changes are:

1. In `MEM_POOL_Push_P()`: replace the `free_mem_pool_blocks_list`/`mem_overhead_pool` allocation with `malloc(sizeof(MEM_POOL_BLOCKS))`.
2. In `MEM_POOL_Pop_P()` and `MEM_POOL_Delete()`: replace the free list return with `free(bsp)`.
3. In `Allocate_Large_Block()`: remove the `Add_Large_Block()` call (or make it per-pool).
4. In `MEM_POOL_FREE()`: remove `Validate_And_Delete_Large_Block()` validation (or use a per-pool scheme).
5. In `MEM_POOL_Pop_P()`: remove `Validate_And_Delete_Large_Block()` call (or use per-pool scheme).
6. Audit IPA code for use of `Default_Mem_Pool` and replace with explicit pool references.

After these changes, each `MEM_POOL` struct becomes fully self-contained, and multiple IPA contexts can each have their own pools without any shared mutable state.
