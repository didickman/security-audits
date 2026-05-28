# Zig ziglibc Audit Findings

Security audit of the Zig standard library's C compatibility layer at `lib/c/`. Each finding includes a detailed write-up and a patch.

This is the post-second-pass version of the audit, with findings re-verified against current Zig master. Findings that describe C-language UB inherited unchanged from the C standard (for example `abs(INT_MIN)` or `lrint(NaN)`) have been removed, since those are caller obligations rather than Zig-introduced defects. What remains are Zig-introduced defects in the shim layer itself.

## Summary

**Total findings: 7** -- High: 3, Medium: 4

## Findings

### Memory allocation

| # | Finding | Severity |
|---|---------|----------|
| [001](001-malloc-integer-overflow.md) | malloc integer overflow on near-`maxInt(usize)` requests | High |
| [003](003-posix-memalign-missing-alignment-validation.md) | posix_memalign missing alignment validation | Medium |

### C library shims

| # | Finding | Severity |
|---|---------|----------|
| [002](002-memccpy-omits-matched-byte-and-never-returns-null.md) | memccpy drops terminator byte and violates NULL-on-miss semantics | High |
| [003](003-strtok-r-leaves-save-state-stale-when-input-is-all-delimiter.md) | strtok_r leaves save state stale on delimiter-only input | Medium |
| [012](012-wcsnlen-slices-with-maxint-usize-on-sentinel-terminated-poin.md) | wcsnlen slices with maxInt(usize) on sentinel-terminated pointer | Medium |

### Linux syscall layer

| # | Finding | Severity |
|---|---------|----------|
| [001](001-getgroupslinux-intcast-of-negative-size-causes-panic.md) | getgroupsLinux @intCast of negative size causes panic | Medium |
| [011](011-mprotectlinux-aligns-len-without-accounting-for-addr-alignme.md) | mprotectLinux aligns len without accounting for addr alignment delta | High |
