---
name: modern-c23
description: Use when writing modern C23 code for iogrpc, especially for gRPC framing, memory management (arenas), and core runtime logic. Covers required language features, checked arithmetic, and C23 attributes.
---

# Modern C23 for iogrpc

## Required Features

- Prefer `nullptr` over `NULL`.
- Use `[[nodiscard]]` on all status-returning and resource-allocating functions.
- Use `_Static_assert` for protocol invariants, structure sizes, and alignment requirements.
- Use `constexpr` for gRPC protocol constants (e.g., `IOGRPC_FRAME_HEADER_SIZE`).
- Use `<stdckdint.h>` for all arithmetic involving message lengths and offsets to prevent overflows.
- Use `bool`, `true`, `false` as native keywords.

## iogrpc-Specific Guidance

- **Memory**: Use Arena allocation for per-call data. Ensure proper alignment.
- **Protocol**: Strictly follow gRPC-over-HTTP/2 wire format.
- **Error Handling**: Always return `iogrpc_status_t` and use `[[nodiscard]]`.
- **Safety**: Use `memset_explicit` for sensitive data if needed.

## Patterns to Avoid

- Traditional `NULL` macro.
- Unchecked integer addition for buffer offsets.
- Variadic macros where `inline` functions or C23 features can suffice.
- Implicit type conversions that may lose precision.

## Workflow

1. Define protocol constants using `constexpr`.
2. Implement logic with checked arithmetic (`ckd_add`, `ckd_mul`).
3. Annotate public API with `[[nodiscard]]`.
4. Validate structure layouts with `_Static_assert`.
