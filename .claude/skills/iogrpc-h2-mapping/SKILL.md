---
name: iogrpc-h2-mapping
description: Rules for mapping HTTP/2 streams to gRPC semantics according to the official spec. Use when implementing the H2 adapter, metadata handling, or status/trailers logic.
---

# iogrpc HTTP/2 to gRPC Mapping

## Required Request Headers

- `:method`: Must be `POST`.
- `:path`: `/<package>.<Service>/<Method>`. Case-sensitive.
- `content-type`: Must start with `application/grpc`.
- `te`: Must contain `trailers`.

## Method Dispatch

1.  Extract service and method from `:path`.
2.  Perform exact match lookup in the `iogrpc_server_t` registry.
3.  If not found, return `UNIMPLEMENTED` (status 12) in trailers.

## Status and Trailers

- **Outcome**: The result of an RPC is always sent in HTTP/2 trailers as `grpc-status`.
- **Status Codes**: Map to `iogrpc_status_code_t`. `0` is OK.
- **Trailers-Only**: For early errors (e.g., auth failure), send a single HEADERS frame with `END_STREAM` containing both headers and trailers.

## Deadlines

- Parse `grpc-timeout` header (e.g., `100m`, `5S`).
- Convert to monotonic absolute time.
- Integrate with `iohttp` timer events or `io_uring` timeout SQEs.
- If timeout expires, terminate stream with `DEADLINE_EXCEEDED` (status 4).
