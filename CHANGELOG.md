# Changelog

## v0.4.0 - 2025-11-07

### Changed

- Harden public APIs and remove panics/unwrap/expect/eprintln! from public paths. Prefer returning `Result<..., MelsecError>` and logging via the `log` facade.
- Centralized runtime configuration and logging initialization.
- Migration: replaced `McResponse::new` with fallible `McResponse::try_new` and updated call sites.
- `error_codes` registry: registration now merges new error code entries instead of overwriting the registry; added regression tests for merge behavior.
- Added generic typed read/write helpers:
  - `FromWords` trait + `McClient::read_words_as<T: FromWords>` which interprets `count` as element-count (internally multiplies by `T::WORDS`).
  - `ToWords` trait + `McClient::write_words_as<T: ToWords>` for typed writes and write/read verification.
  - Implementations provided for common types: `u16`, `i16`, `u32`, `i32`, `f32`, and `[bool; 16]`.
- Read API is tolerant: when parsing element-by-element, parse failures for some elements are logged and the reader continues to collect successfully parsed elements when possible.

### Added

- Examples for real-device testing and debugging:
  - `examples/read_f32_test.rs`, `examples/read_f32_range.rs`, `examples/dump_words.rs`, `examples/write_various_types.rs`.
- `docs/registry_policy.md` describing registry merge/set-once policies.
- GitHub Actions CI workflow to run fmt / clippy (warnings as errors) / tests on PRs and pushes.

### Fixed

- Clippy and minor lint fixes across examples and core code (use `is_empty`, iterator idioms, replace approximate float with `std::f32::consts::PI`, etc.).

### Notes

- Local verification performed:
  - `cargo test` — all tests passed locally.
  - `cargo clippy --all-targets --all-features -- -D warnings` — no warnings locally.

See the `Unreleased` section below for prior notes.
# Changelog

## v0.2.0 - YYYY-MM-DD

### Breaking
- Remove deprecated `MC3E_*` aliases and migrate public constants to `MC_*` names:
  - `MC_CMD_*`, `MC_SUBCMD_*`, `MC_ACCESS_PATH_DEFAULT`, `MC_END_*` are now the canonical names.
  - Remaining MC3E/MC4E-specific values (subheaders and header sizes) kept as `MC3E_*` / `MC4E_*`.

### Changed
- Repository-wide refactor to use common constant names for MC protocol values.
- Examples and internal code updated to use new names.

### Fixed
- Remove unused mut warnings in `src/transport.rs`.
- Ensure test suite passes after migration.


