# ioplane Naming Standard

| Surface | Canonical form | Example |
|---|---|---|
| Public C function | `iogrpc_<module>_<verb>_<noun>` | `iogrpc_call_start` |
| Public type | `iogrpc_<name>_t` | `iogrpc_stream_t` |
| Macro/enum | `IOGRPC_<NAME>` | `IOGRPC_STATUS_OK` |
| Source/header | `lower_snake_case` | `grpc_frame.c` |
| Test | `test_<area>_<action>_<expected>.c` | `test_frame_rejects_short.c` |
| Plan | `YYYY-MM-DD-<scope>-plan.md` | `2026-08-06-iogrpc-plan.md` |
| RPC/config token | lowercase kebab-case / snake_case | `protocol-mode` / `protocol_mode` |

`protocol_mode=native` is the secure/performance default. Compatibility requires
an explicit versioned profile and is never inferred from HTTP/2 headers, peer
implementation, or retry behavior. Same canonical input means same names,
fixtures, generated code, and manifests; never use time, randomness, process
IDs, hostnames, branches, or collision counters. Collision is an error or a
documented deterministic lowercase hexadecimal digest. Extend this file after
auditing 20 names before adding a vocabulary.

The 2026-08-06 audit sampled the available CMake, plan, RFC, and scaffold names.
Because implementation files are absent, no symbol migration is claimed; the
`iogrpc_`/`IOGRPC_` convention is frozen before bootstrap implementation.
