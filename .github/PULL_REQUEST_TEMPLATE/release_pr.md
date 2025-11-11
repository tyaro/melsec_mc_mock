<!--
Use this template for release PRs that bump versions, update CHANGELOG, and contain breaking changes.
-->
# Release PR: {{pr_title}}

## Summary

This PR prepares a new release for `melsec_mc`.

- Version: `0.3.0`
- Branch: `chore/replace-mcresponse-new`
- Type: breaking change (see notes below)

## What changed

- Hardened public APIs, replaced `McResponse::new` with `McResponse::try_new` (fallible API).
- `error_codes` registry now merges entries instead of overwriting.
- Added typed read/write helpers: `FromWords`/`ToWords` and `McClient::read_words_as` / `write_words_as`.
- Added examples and diagnostic tools for real-device testing.
- Centralized logging/config and removed direct `eprintln!` usage in public flows.

See `CHANGELOG.md` for full details.

## Checklist (required)

- [ ] Confirm CI (fmt/clippy/tests) passed on the PR.
- [ ] Bump version in `Cargo.toml` is correct and `CHANGELOG.md` entry exists.
- [ ] Update `README.md` / examples if they reference removed compatibility shims.
- [ ] Prepare release artifacts (tag, build, sign) after merge.
- [ ] Notify downstream consumers of the breaking change (if any).

## Testing notes

Local checks run before creating this PR:

- `cargo fmt` ✅
- `cargo clippy --all-targets --all-features -- -D warnings` ✅
- `cargo test` ✅

If CI fails, re-run the relevant step locally and attach failing output here.

## Reviewers / Assignees

- Suggested reviewers: @tyaro

## Release steps (post-merge)

1. Create a signed tag `v0.3.0`.
2. Build release artifacts: `cargo build --release` and produce packaging as needed.
3. Publish to crates.io: `cargo publish` (ensure you have permissions and 2FA ready).
4. Create GitHub release with notes from `CHANGELOG.md`.

---
*Auto-generated PR template for release.*
