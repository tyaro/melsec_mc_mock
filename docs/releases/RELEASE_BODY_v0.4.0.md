# Release v0.4.0

This release prepares a set of API hardening and ergonomics improvements plus documentation updates.

Highlights
- Harden public APIs: replaced panics/unwrap/expect/eprintln! with fallible `Result` returns and logging.
- Introduced `McResponse::try_new` and updated call sites.
- Added typed read/write API: `FromWords` / `ToWords` traits and `McClient::read_words_as` / `write_words_as` helpers.
- `error_codes` registry now supports register-or-merge semantics to avoid overwriting previously-registered entries.
- Updated examples and added diagnostics helpers for real-device testing.
- CI: added GitHub Actions workflows for formatting, clippy (warnings-as-errors), tests, and publish-on-tag.

Notes
- Published as `melsec_mc v0.4.0` on crates.io.
- The GitHub Actions workflow `Publish to crates.io` triggers on tag pushes (`v*.*.*`) and uses the `CRATES_IO_TOKEN` secret.

Changelog
- See `CHANGELOG.md` for details.

Acknowledgements
- Thanks to contributors and reviewers for the changes and CI fixes.
