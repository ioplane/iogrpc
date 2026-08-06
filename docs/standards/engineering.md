# ioplane Engineering Rules — iogrpc

- The runtime is pure C23 with a stable C ABI. Protobuf/C++ code generation is
  an isolated host-tool concern; it must not add a C++ runtime to the client or
  server.
- Native gRPC client/server behavior is strict, secure, bounded, and measured
  by default. Compatibility profiles are explicit parameters with separate
  fixtures and deprecation metadata.
- Keep gRPC semantics separate from iohttp transport. Preserve 5-byte message
  framing, HTTP/2 metadata/trailers, deadlines, cancellation, flow control,
  protobuf-c ownership, and arena lifetime invariants.
- The implementation is not present in this checkout yet; CMake declarations
  are not evidence of a built feature. Every phase must add source, tests,
  integration evidence, and a reproducible artifact.
- Use deterministic `iogrpc_` names from [naming.md](naming.md), and keep
  English stable documentation authoritative.
