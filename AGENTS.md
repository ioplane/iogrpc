# iogrpc — Agent Contract

This is the canonical contract for the iogrpc scaffold. The existing
`CLAUDE.md` is retained for compatibility; this file governs new
cross-repository work.

## Current status

The checkout contains CMake and planning scaffolding but no implementation
files under `src/`, `include/`, tests, examples, or Git metadata. Never report a
CMake source list as a built feature. The bootstrap plan is
`docs/plan/2026-08-06-iogrpc-plan.md`.

## Architecture and build

- Use C23 with extensions disabled and a pure C runtime ABI.
- Do not use C++-style `auto` type inference; in C23 it is not type inference.
- Use `iohttp` for HTTP/2 transport, `nghttp2` for HTTP/2 semantics,
  `protobuf-c` for runtime messages, and wolfSSL for TLS/ALPN.
- Keep gRPC framing, metadata, trailers, deadlines, cancellation, flow
  control, arena ownership, and generated-code tooling as explicit boundaries.
- Build and test in the repository Podman container. Every feature needs unit,
  malformed-input, sanitizer/fuzz, and grpcurl/native-client evidence.

## Secure native and compatibility modes

- `protocol_mode=native` is the default: strict validation, maximum security,
  bounded allocation, and measured performance.
- `protocol_mode=compatibility` is explicit and requires a registered,
  versioned `compatibility_profile`. Never infer it from HTTP/2 metadata,
  peer identity, or retries.
- C++ is allowed only for isolated `protoc` host tooling behind a C ABI; it
  must not add a C++ runtime dependency to the client or server.

## Naming and planning

- Follow `docs/standards/naming.md`: `iogrpc_` public symbols, deterministic
  generated names, and `YYYY-MM-DD-<scope>-plan.md` plans.
- Stable names cannot contain time, randomness, hostnames, process IDs,
  branches, or collision counters. Same canonical input must produce the same
  name and manifest on every run.
- Follow `docs/standards/engineering.md`; update the plan and English stable
  documentation with every contract change.
