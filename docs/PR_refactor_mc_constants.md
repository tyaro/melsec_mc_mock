# PR: Refactor MC constant names (MC3E_ → MC_)

この PR は公開 API の定数名を整理する目的で、内部 API とドキュメントを更新します。

## 概要

- 新しい汎用名を追加しました: `MC_HEADER_SIZE`, `MC_SUBHEADER_REQUEST`, `MC_SUBHEADER_RESPONSE` など（`mc4e.rs` に追加）。
- 既存の `MC3E_` 系の定数は互換性のために `#[deprecated]` 属性付きのエイリアスとして残しました。
  - 例: `pub const MC3E_SUBHEADER_REQUEST: [u8;2] = MC_SUBHEADER_REQUEST;`
- ライブラリ内部の参照は新名に切り替えました（`src/request.rs`, `examples/read_4e.rs` など）。
- 旧ソース `src/mc3e.rs` はファイルを残しつつ中身を shim（短い注釈）に置き換え、元の定義は `docs/legacy_mc3e.md` に保管しています。
- README の説明も古い "長さプレフィックス" の言及を除去し、サンプルの説明を整理しました。

## 互換性

- 既存のコードが `MC3E_` 名を直接参照していても、コンパイルは引き続き可能です（ただし警告が出ます）。
- 将来的なメジャーリリースで `MC3E_` 名を削除する予定です。消去は破壊的変更となるため、事前に移行猶予を設けます。

## 移行手順（ユーザ向け）

1. コンパイル警告を確認しつつ、`MC3E_` の使用を `MC_` 系に置換してください。
   - 例: `MC3E_SUBHEADER_REQUEST` → `MC_SUBHEADER_REQUEST`
2. テストや CI を実行して動作を確認してください。

## 開発者向けメモ

- 実施済み:
  - ブランチ: `refactor/mc-consts` にて変更を実施・ push 済み
  - 全テスト（ユニット + examples dry-run + doctest）をローカルで実行して全て成功
- 今後の作業候補:
  - ドキュメント中の例（外部リポジトリや README）で `MC3E_` を残している箇所を掃除
  - Deprecation 警告の期間を決め、次のメジャーリリースで削除

## CI / テスト
- ローカルで `cargo test` を実行済み（全テスト通過）

---

レビュアーの方へ: 互換性を意図的に残すため `#[deprecated]` を使用しています。PR をマージする前に CI の確認とレビューをお願いします。