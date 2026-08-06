# iogrpc

C23 gRPC runtime scaffold for the ioplane ecosystem.

The planned architecture keeps HTTP/2 transport and `io_uring` ownership in
`iohttp`; `iogrpc` owns gRPC framing, metadata, status, deadlines,
cancellation, flow control, protobuf-c codec boundaries, and service dispatch.
The runtime is intended to expose a pure C ABI. C++ is limited to isolated
`protoc` host tooling.

## Current status

This repository is at the bootstrap/scaffold stage. The source, public headers,
tests, examples, and code generator are not implemented yet. Do not interpret
the CMake source list as a built feature.

The implementation plan is
[`docs/plan/2026-08-06-iogrpc-plan.md`](docs/plan/2026-08-06-iogrpc-plan.md).

## Planned build workflow

Build and test inside the Podman development container:

```bash
podman build -t iogrpc-dev:latest -f deploy/podman/Containerfile .
podman run --rm -it -v "$(pwd):/workspace:Z" iogrpc-dev:latest
cmake --preset clang-debug
cmake --build --preset clang-debug
ctest --preset clang-debug
```

See [`AGENTS.md`](AGENTS.md) for the repository contract and
[`docs/standards/engineering.md`](docs/standards/engineering.md) for the
cross-project engineering rules.
