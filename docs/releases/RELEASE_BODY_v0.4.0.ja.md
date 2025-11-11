# リリース v0.4.0

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
