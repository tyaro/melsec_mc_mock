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

---

## Checksums (SHA256)

Verify downloaded artifacts with sha256sum (Linux/macOS) or PowerShell's Get-FileHash (Windows):

```powershell
# Linux/macOS
sha256sum <file>

# Windows (PowerShell)
Get-FileHash -Algorithm SHA256 <PathToFile>
```

| File | Download (GitHub Release) | SHA256 |
| --- | --- | --- |
| [melsec_mc-windows-x86_64-v0.4.0.zip](https://github.com/tyaro/melsec_com/releases/download/v0.4.0/melsec_mc-windows-x86_64-v0.4.0.zip) | _Windows x86_64 (no PDB)_ | `0be4819168388a6b2f38c167f8a9f0cc143dd10945aa35060247bece672b2513` |
| [melsec_com-main-linux-x86_64.zip](https://github.com/tyaro/melsec_com/releases/download/v0.4.0/melsec_com-main-linux-x86_64.zip) | _Linux x86_64_ | `4ee463e7a722794a56c344f3c8541f79ecb6be9ca37991cf5b87d7f8658a98b6` |
| [melsec_com-main-linux-aarch64.zip](https://github.com/tyaro/melsec_com/releases/download/v0.4.0/melsec_com-main-linux-aarch64.zip) | _Linux aarch64_ | `9dcc62ece3602eba2bf3b18d1d68f64a23c044f4e093e9c1deb88c90f40655fc` |
| [melsec_com-main-macos-aarch64.zip](https://github.com/tyaro/melsec_com/releases/download/v0.4.0/melsec_com-main-macos-aarch64.zip) | _macOS aarch64_ | `0b980628e604e31bb5ebc30659b7fb7fa6284c9246dba82dc28fd7d49e71e9c5` |

Notes:
- The Windows ZIP was re-created with PDB files removed to reduce download size. If you need the original with debug symbols, contact the release manager.
- To verify on Linux/macOS: `sha256sum <file>`; on Windows PowerShell: `Get-FileHash -Algorithm SHA256 <PathToFile>`.

---

## Release artifact contents (summary)

### melsec_mc-windows-x86_64-v0.4.0.zip


### melsec_com-main-linux-x86_64.zip


### melsec_com-main-linux-aarch64.zip


### melsec_com-main-macos-aarch64.zip

