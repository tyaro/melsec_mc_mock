Release preparation: v0.3.0

概要
- バージョンを v0.3.0 に更新します（breaking change）。
- パブリック API を堅牢化（パニック/unwrap/expect/eprintln! を除去し、Result/エラーを返す設計へ）。
- `McResponse::new` を廃止し、`McResponse::try_new` に移行しました（呼び出し側を更新済み）。
- `error_codes` レジストリは上書きではなくマージするように動作を変更しました（回帰テスト追加）。
- ジェネリックな型付き読み書き API を追加しました：`FromWords`/`ToWords` + `McClient::read_words_as` / `write_words_as`。
- 実機用の例・デバッグ例を追加しました（`examples/*`）。
- ログ/設定の集約と CI（fmt / clippy / tests）を追加しました。

変更点のハイライト
- `src/` の API 安全化（panic 等の除去）
- `src/error_codes.rs` に `register_or_merge` ロジックを導入
- `src/mc_client.rs` に `FromWords`/`ToWords` と `read_words_as`/`write_words_as` を追加
- ドキュメント: `CHANGELOG.md`, `docs/registry_policy.md`
- CI: `.github/workflows/ci.yml`
- examples: `read_f32_test.rs`, `read_f32_range.rs`, `dump_words.rs`, `write_various_types.rs`

ローカルでの検証（実行済み）
- `cargo fmt` ✅
- `cargo clippy --all-targets --all-features -- -D warnings` ✅
- `cargo test` ✅

注意（breaking change）
- 公開 API の一部を落としているため、外部で当該 API を使っている場合は修正が必要です。特に `McResponse::new` → `McResponse::try_new` の置換を行ってください。
- `read_words_as<T>` は `count` を「要素数」として扱います（内部で T::WORDS に基づき要求ワード数を計算します）。既存の `read_words`（ワード数ベース）は互換です。

チェックリスト（マージ前に確認してください）
- [ ] CI (fmt/clippy/tests) が成功している
- [ ] `Cargo.toml` と `CHANGELOG.md` のバージョン表記が正しい
- [ ] README / examples に互換切れを示す注記を追加（必要に応じて）
- [ ] 署名付きタグ `v0.3.0` を作成し、release 作業を行う準備があること

マージ後の手順（推奨）
1. マージ後にローカルでタグを作成・プッシュ:
   - `git tag -s v0.3.0 -m "melsec_mc v0.3.0"`
   - `git push origin v0.3.0`
2. crates.io へ publish（権限と 2FA を確認）:
   - `cargo publish`
3. GitHub の Release を作成（CHANGELOG の該当節を使う）

レビュワー候補
- @tyaro

補足
- PR に CI の通過ログを添えてください。もし CI で問題が出たらログを共有すると対処します.
