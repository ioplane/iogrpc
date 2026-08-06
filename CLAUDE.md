# iogrpc - Project Instructions for Claude Code

## Quick Facts

- **Language**: C23 (`-std=c23`, `CMAKE_C_EXTENSIONS OFF`)
- **Project**: High-performance gRPC runtime for C23 using `io_uring` and `iohttp`
- **License**: MIT OR LGPL-2.1-or-later
- **Platform**: Linux-first development
- **Core Dependencies**: `liburing`, `nghttp2`, `wolfSSL`, `protobuf-c`, `iohttp`

## Build Commands

```bash
cmake --preset clang-debug
cmake --build --preset clang-debug
ctest --preset clang-debug
```

## Dev Container

- Build: `podman build -t iogrpc-dev:latest -f deploy/podman/Containerfile .`
- Run: `podman run --rm -it -v $(pwd):/workspace:Z iogrpc-dev:latest`
- Always develop inside the container for a consistent environment.

## Key Directories

```text
include/iogrpc/         # Public API headers
src/                    # gRPC framing, server, stream state machine, adapters
tests/                  # Unity tests
docs/plan/              # Implementation roadmap
tools/                  # protoc-gen-iogrpc-c generator
```

## Architectural Guidelines

- Use `iohttp` for the underlying HTTP/2 transport.
- Keep gRPC semantics strictly separate from general HTTP logic.
- Implement Unary RPC first, then streaming.
- Use arena-based allocation for per-call memory.
- Strictly follow gRPC over HTTP/2 protocol specification (headers, framing, trailers).

## Code Conventions

- Public symbols use `iogrpc_`; macros use `IOGRPC_`; typedefs end with `_t`.
- Use C23 features: `nullptr`, `[[nodiscard]]`, `constexpr`, `auto` (where appropriate).
- Match the style of `iohttpparser` and `iohttp`.

## Testing Rules

- Every new feature must have a corresponding test in `tests/`.
- Use `grpcurl` for integration testing.
- Validate malformed gRPC frames and protocol violations.
