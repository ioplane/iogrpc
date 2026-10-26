[![GitHub](https://img.shields.io/badge/GitHub-iogrpc-181717)](https://github.com/ioplane/iogrpc)

# iogrpc Agent Development Plan

Date: 2026-10-26. Audited default branch: `main` at
`854f8ee822a3aacb4623ae97d78d24f3b2588bc4`. Planning audit: `ngtun-i8s.42.2`.
Durable execution work order: `ngtun-4zy.7` under independent portfolio `ngtun-4zy`.

Historical plans and local uncommitted August plans remain preserved. This dated
plan supplies the next validation/implementation contracts; it does not rewrite past
measurements, sprint completion records or current publish decisions.

## Verified baseline

The GitHub `main` repository now exists. The default tree has no source/public-header/test
implementation; CMake lists seven absent `src/grpc_*.c` paths. The older bootstrap plan's
no-Git/no-remote observation is historical. The legacy CLAUDE contract declares MIT OR LGPL-2.1-or-later, but no LICENSE file
is present at this revision. Confirm that intended project license and the actual
iohttp/wolfSSL consumption route before implementation/distribution; no legal
compatibility conclusion is established here. The current
iohttp H2 interface at its audited base only dispatches completed END_STREAM requests,
so it is not yet an admitted gRPC streaming transport.

## Next functional slice

Keep `ngtun-4zy.7` at planned entry until `ngtun-4zy.5` supplies its reviewed H2
streaming contract and the owner selects the license and first consumer. Then produce
one buildable strict gRPC message-frame codec with a pure C ABI and focused tests.
Before code, freeze the five-byte header, checked length limit, incremental consumption,
compressed-flag rejection for the first profile, output lifetime and error behavior.
Remove scaffold references only as actual implemented targets replace them; do not create
empty source files or pretend all seven declared modules exist.

| Order | Owned paths | Exit evidence |
| --- | --- | --- |
| 1 | Existing plan, naming/engineering docs and actual first header contract | Owner license decision and reviewed ABI/frame vectors; exact dependency/build inputs |
| 2 | Actual frame codec, focused test and its CMake registration | Both compiler lanes; split-header/payload, overflow, max/max+1, unsupported flag and truncation controls |
| 3 | Future narrow iohttp adapter and unary integration | Proceed only after qualified H2 data/trailer/backpressure/cancel API; bounded unary success and error against an independent client |

## Dependencies and overlap

iohttp owns HTTP/2 and io_uring transport. Require its accepted incremental data, metadata,
trailers, pause/resume, flow-control and cancellation contract before RPC integration.
Resolve iohttp/wolfSSL and iogrpc licensing explicitly; this plan chooses no license.
Keep protobuf-c ownership explicit; a protoc plugin is later isolated host tooling and
must not add a C++ runtime. No custom H2 runtime, universal reactor, ngtun dependency or
shared-core extraction is introduced. No open PR was observed at the audit snapshot.

## Publication contract

Retain `docs/plan/` and the existing dated-name grammar. Preserve earlier plans as
historical design inputs. Each implementation leaf needs source, unit/malformed controls,
container sanitizer/fuzz evidence and scoped delivery. Independent grpcurl/native-client
proof belongs to the real unary slice, not the pure codec. Streaming, compatibility and
gateway work follow unary admission; no feature or throughput claim precedes execution.

## Execution and evidence rules

Read the target repository contract and applicable local skills before each leaf.
This independent portfolio does not gate ngtun release or require ngtun adoption.
Use an actually qualified project OCI; shared-base publication gates only its adoption.
Preserve dirty worktrees, unpublished plans, existing PRs and their path ownership.
Refresh exact default/PR identities before execution; keep compatibility explicit.
Record actual nonempty test/analyzer coverage and exact source/tool/image evidence.
Follow the repository task, changelog, review and delivery contract for each leaf.
This planning update claims no runtime success and authorizes no live mutation.
Repository quality gates remain pending until run; no host build or engine was used.

## Source baseline

- [Default README](https://github.com/ioplane/iogrpc/blob/854f8ee822a3aacb4623ae97d78d24f3b2588bc4/README.md).
- [Default repository contract](https://github.com/ioplane/iogrpc/blob/854f8ee822a3aacb4623ae97d78d24f3b2588bc4/AGENTS.md); applicable dirty local contracts were read without importing their changes.
- [CMakeLists.txt:66](https://github.com/ioplane/iogrpc/blob/854f8ee822a3aacb4623ae97d78d24f3b2588bc4/CMakeLists.txt#L66).
- [docs/plan/2026-08-06-iogrpc-plan.md:13](https://github.com/ioplane/iogrpc/blob/854f8ee822a3aacb4623ae97d78d24f3b2588bc4/docs/plan/2026-08-06-iogrpc-plan.md#L13).
- [docs/standards/engineering.md:3](https://github.com/ioplane/iogrpc/blob/854f8ee822a3aacb4623ae97d78d24f3b2588bc4/docs/standards/engineering.md#L3).
