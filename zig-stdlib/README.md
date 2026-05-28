# Zig Standard Library Audit Findings

Security audit of the Zig standard library. Each finding includes a detailed write-up and a patch.

This is the version of the audit that survived a second-pass review against current Zig master, applying a realistic threat model. Findings that were upstream-fixed, eliminated by refactors, or that rely on attacker control of a trust boundary that does not really exist (build script authors, self-debug paths, compiler-internal IPC) have been removed.

## Summary

**Total findings: 29** -- High: 14, Medium: 14, Low: 1

## Findings

### Async I/O, scheduling, and synchronization

| # | Finding | Severity |
|---|---------|----------|
| [004](004-empty-batch-initialization-reads-before-slice.md) | Empty batch initialization underflows storage index | Medium |
| [005](005-sleep-cancellation-callback-casts-wrong-waiter-type.md) | Sleep cancellation callback uses wrong waiter type | High |
| [019](019-zero-length-read-buffer-triggers-invalid-iovec-access.md) | Zero-length read buffer triggers invalid iovec access | Medium |

### Build system

| # | Finding | Severity |
|---|---------|----------|
| [015](015-double-free-of-step-names-trailing-during-teardown.md) | Double free of step_names_trailing during teardown | High |

### HTTP, WebSocket, and URI

| # | Finding | Severity |
|---|---------|----------|
| [001](001-proxy-mode-mutates-pooled-connection-after-lookup.md) | Proxy mode mutates pooled connection after lookup | Medium |
| [023](023-unvalidated-parsed-host-reaches-trusted-hostname-output.md) | Parsed URI host bypasses HostName validation | Medium |
| [026](026-websocket-upgrade-ignores-required-connection-header.md) | WebSocket upgrade ignores required Connection header | Medium |
| [027](027-response-header-crlf-validation-disappears-without-runtime-s.md) | Response header CRLF validation drops without runtime safety | Medium |
| [028](028-request-body-assertion-trusts-malformed-methods.md) | Request body assertion trusts malformed methods | Low |

### DNS

| # | Finding | Severity |
|---|---------|----------|
| [024](024-unchecked-search-directive-overflows-fixed-buffer.md) | Unchecked search directive overflows fixed buffer | High |
| [025](025-name-expansion-accepts-reserved-label-encodings-as-pointers.md) | Reserved DNS label encodings accepted as compression pointers | Medium |

### TLS

| # | Finding | Severity |
|---|---------|----------|
| [030](030-tls-1-2-record-length-subtraction-can-underflow.md) | TLS 1.2 short AEAD record triggers underflow and abort | High |
| [031](031-keyupdate-handshake-reads-byte-without-length-check.md) | KeyUpdate zero-length body causes out-of-bounds read | Medium |

### Cryptography

| # | Finding | Severity |
|---|---------|----------|
| [049](049-release-builds-permit-mismatched-buffer-lengths.md) | Release builds permit mismatched buffer lengths in ISAP | Medium |
| [055](055-associated-data-vector-length-can-overrun-fixed-stack-buffer.md) | Associated-data vector length can overrun fixed stack buffer | Medium |
| [066](066-der-parser-reads-past-input-before-length-validation.md) | DER header bounds check missing before parse reads | High |
| [077](077-block-checksums-are-parsed-but-never-verified.md) | xz block checksums are parsed but never verified | High |

### ELF

| # | Finding | Severity |
|---|---------|----------|
| [020](020-gnu-hash-bloom-size-zero-causes-division-by-zero.md) | GNU hash zero-sized tables trigger modulo-by-zero | Medium |
| [021](021-gnu-hash-empty-bucket-underflows-chain-index.md) | GNU hash empty bucket underflows chain index | High |
| [022](022-writable-pt-load-copy-ignores-segment-file-offset.md) | Writable PT_LOAD copy ignores segment file offset | High |

### Mach-O

| # | Finding | Severity |
|---|---------|----------|
| [058](058-unchecked-main-string-table-index-causes-out-of-bounds-slice.md) | Unchecked main string-table index causes out-of-bounds slice | High |

### PE / COFF

| # | Finding | Severity |
|---|---------|----------|
| [008](008-malformed-section-name-panics-parser.md) | Malformed COFF section name panics parser | Medium |
| [009](009-data-directory-count-slices-beyond-optional-header.md) | Data directory count slices beyond optional header | High |
| [010](010-section-data-length-uses-unchecked-file-controlled-bounds.md) | Section data length uses unchecked file-controlled bounds | High |

### Compression and archive extraction

| # | Finding | Severity |
|---|---------|----------|
| [037](037-symlink-extraction-permits-targets-outside-destination-root.md) | Symlink extraction permits targets outside destination root | Medium |
| [064](064-single-byte-literals-section-writes-past-empty-caller-buffer.md) | Single-byte literals section writes past empty caller buffer | High |
| [084](084-extraction-accepts-unlimited-compressed-input.md) | Zip extraction ignores declared compressed-size boundary | High |

### LLVM bitcode reader

| # | Finding | Severity |
|---|---------|----------|
| [095](095-record-name-allocation-underflows-on-empty-operands.md) | Bitcode record name allocation underflows on empty operands | High |
| [096](096-record-decoding-dereferences-missing-first-operand.md) | Bitcode record decoding rejects empty operand lists | Medium |
