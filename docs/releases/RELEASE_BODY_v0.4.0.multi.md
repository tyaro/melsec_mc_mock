# リリース v0.4.0 / Release v0.4.0

## 日本語 (Japanese)

このリリースでは、公開 API の堅牢化、型付き読み書き API の追加、ドキュメントとリリースフローの改善を行いました。

主な変更点

- 公開 API の堅牢化: パニックや `unwrap`/`expect`、`eprintln!` を排し、失敗は `Result<..., MelsecError>` で返すようにしました。ログはロギング仕組みで出力します。
- `McResponse::try_new` を導入し、呼び出し側を移行しました。
- 型付き読み書き API の追加: `FromWords` / `ToWords` トレイトと、`McClient::read_words_as` / `write_words_as` を導入しました。複数ワードにまたがる型（`f32`, `u32`, `[bool;16]` など）を直接扱えます。
- `error_codes` レジストリのマージ登録（register-or-merge）をサポートし、既存のエントリを上書きしない運用を可能にしました。
- 実機デバッグ用のサンプルとユーティリティを追加しました。
- CI: フォーマット、clippy（警告をエラー化）、テスト、およびタグ push による公開ワークフローを追加しました。

注意事項

- crates.io へは `v0.4.0` として公開済みです。
- GitHub Actions の `Build and attach release artifacts` ワークフローにより、Linux/macOS (arm/x86) 向けのバイナリ ZIP（PDB は除外）と SHA256 チェックサムを作成してリリースに添付しています。

詳細は `CHANGELOG.md` を参照してください。

貢献者の皆様、レビューしてくださった方々に感謝します。

---

## English

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
