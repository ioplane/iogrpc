---
name: iogrpc-buffer-management
description: Rules for handling gRPC Length-Prefixed Messages (LPM), zero-copy buffering, and io_uring provided buffers. Use when implementing frame parsers or message serializers.
---

# iogrpc Buffer Management

## gRPC Wire Format (LPM)

Each gRPC message is prefixed by a 5-byte header:
- **Compressed-Flag**: 1 byte (0 = uncompressed, 1 = compressed).
- **Message-Length**: 4 bytes (big-endian integer).
- **Data**: `Message-Length` bytes of protobuf-encoded message.

## Parsing Strategy

1.  **Fragmentation**: Data frames from HTTP/2 may not align with gRPC message boundaries.
2.  **Accumulation**: Accumulate bytes until at least 5 bytes are available for the header.
3.  **Slicing**: Use `io_uring` provided buffers or arena-allocated spans to avoid `memcpy`.
4.  **Limits**: Strictly enforce `max_message_bytes` to prevent OOM/DoS.

## Memory Safety

- Use `ckd_add` and `ckd_mul` for all offset and length calculations.
- Validate that `5 + Message-Length` does not exceed available buffer space before reading.
- Use `iogrpc_arena_alloc()` for temporary decoding buffers.

## Zero-Copy Goals

- Pass raw data pointers to `protobuf-c` where possible.
- Use `grpc_slice`-like abstractions for reference-counted or owner-less memory spans.
