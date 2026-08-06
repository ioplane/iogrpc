# iogrpc Bootstrap and Completion Plan

> **For agentic workers:** REQUIRED: Use `superpowers:executing-plans` or `superpowers:subagent-driven-development`. This plan starts with repository bootstrap; no implementation claim may precede source and test receipts.

**Goal:** Turn the current iogrpc scaffold into an independent, buildable C23 repository with a secure native unary runtime before adding compatibility or streaming features.

**Architecture:** iohttp owns HTTP/2 transport and io_uring integration. iogrpc owns gRPC framing, metadata, status, deadlines, cancellation, flow control, protobuf-c codec boundaries, and service dispatch. C++ is restricted to isolated `protoc` host tooling behind a C ABI; the runtime has no C++ dependency.

**Tech Stack:** C23, CMake/Ninja, Clang 22, GCC 15, iohttp, nghttp2, protobuf-c, wolfSSL, Unity, LibFuzzer, ASan/UBSan/TSan/MSan, grpcurl, Podman.

---

## Audit baseline

- This directory is not a Git repository: no `.git`, remote, default branch, commit history, tags, or GitHub project were found.
- `AGENTS.md` explicitly states that only CMake/planning scaffolding exists; no `src/`, public implementation headers, tests, examples, or Git metadata exist.
- `README.md` is absent.
- `CMakeLists.txt` declares seven source files under `src/` that do not exist and has no registered test (`# iogrpc_add_test(...)` is commented out).
- Measured implementation surface: 0 C/C++ source files, 0 test files, 0 workflows.
- `CMakeLists.txt` uses directory-wide compile flags and links dependencies before any runtime target can build.

## Findings

### P0 — there is no repository implementation

**Impact:** the current CMake target is a scaffold and cannot establish an ABI, security contract, or compatibility claim.

**Plan:** bootstrap GitHub ownership/default branch/README/AGENTS and a minimal source/test tree before expanding the architecture.

### P0 — no frozen gRPC wire/API contract

**Plan:** specify the 5-byte compressed flag/message-length prefix, maximum message size, metadata/trailer grammar, status mapping, deadline/cancellation semantics, flow-control ownership, arena lifetime, and native/compatibility profile inputs before coding.

### P1 — dependency boundary is ambiguous

**Evidence:** the CMake file comments out `pkg_check_modules(IOHTTP ...)` and calls transport dependencies directly, while the contract says iohttp owns HTTP/2 transport.

**Plan:** define an adapter interface that accepts iohttp HTTP/2 events or a narrow transport-neutral stream API; do not duplicate sockets/io_uring in iogrpc.

## Implementation sequence

### Task 1: Bootstrap the repository

**Files:** create `.gitignore`, `README.md`, `AGENTS.md`, `docs/README.md`, `docs/en/README.md`, `docs/ru/README.md`, `docs/standards/naming.md`, `docs/standards/engineering.md`, `.github/workflows/ci.yml`.

- [ ] Initialize Git and create the GitHub repository under the agreed ioplane namespace.
- [ ] Record C23, pure-C runtime, explicit native/compatibility modes, naming, ABI, and dependency policies.
- [ ] Add rootless Podman build instructions based on the immutable ioplane-base digest.
- [ ] Add CI that proves source/test discovery is non-empty and fails on a deliberate broken fixture.

### Task 2: Freeze public ABI and frame contracts

**Files:** `include/iogrpc/iogrpc.h`, `include/iogrpc/frame.h`, `include/iogrpc/metadata.h`, `include/iogrpc/status.h`, `docs/en/01-architecture.md`, `docs/en/02-wire-contract.md`.

- [ ] Define opaque handles, ownership, `[[nodiscard]]` results, error/status mapping, message limits, and versioning.
- [ ] Define native mode and explicit compatibility profiles; reject unknown profiles and implicit selection.
- [ ] Add compile-time ABI/version identifiers and deterministic generated names.

### Task 3: Implement strict unary runtime

**Files:** `src/grpc_frame.c`, `src/grpc_metadata.c`, `src/grpc_status.c`, `src/grpc_server.c`, `src/grpc_h2_adapter.c`, `src/grpc_codec_protobuf_c.c`, `tests/unit/`, `tests/integration/`.

- [ ] Implement bounded frame encode/decode with overflow, truncation, compression-flag, and maximum-size rejection.
- [ ] Implement metadata/trailer validation, canonical ordering where required, status mapping, deadlines, and cancellation.
- [ ] Implement protobuf-c codec ownership and arena cleanup; no runtime C++ dependency.
- [ ] Add malformed-frame, timeout, cancellation, backpressure, and grpcurl/native-client tests.

### Task 4: Add code generation as an isolated tool

**Files:** `tools/protoc-gen-iogrpc-c/`, `docs/en/03-code-generation.md`, `tests/golden/`.

- [ ] Define the generator input/output contract and deterministic output naming.
- [ ] Keep protobuf C++ dependencies in the host-tool image only; generated runtime code links against the C ABI.
- [ ] Add golden regeneration and diff checks that are idempotent.

### Task 5: Expand only after unary evidence

**Files:** `src/`, `include/`, `tests/integration/`, `tests/fuzz/`, `docs/en/`.

- [ ] Add server streaming and flow control after unary lifecycle is green.
- [ ] Add health/reflection/interceptors and metrics as separate contracts.
- [ ] Add liboas gateway and JSON transcoding only after ownership and security limits are specified.
- [ ] Add compatibility profiles only with external wire fixtures and explicit parameters.

## Definition of done

- The directory is a real GitHub repository with a README, governance, naming, CI, and reproducible container build.
- A strict native unary RPC passes unit, malformed-input, sanitizer/fuzz, grpcurl, and native-client tests.
- HTTP/2 transport remains owned by iohttp; iogrpc has no hidden socket or C++ runtime dependency.
- Streaming, compatibility, reflection, and gateway claims remain absent until independently evidenced.
