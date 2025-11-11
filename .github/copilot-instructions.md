# Copilot / AI agent guidance — melsec_mc

このリポジトリは Tokio ベースの Rust ライブラリ `melsec_mc` で、Mitsubishi PLC の MC プロトコル（Ethernet, MC3E 相当）の低レベル送受信と小さなクライアントラッパーを提供します。

要点（すぐ参照すると便利な箇所）
- 高レベル公開型: `McClient` (`src/transport.rs`, re-exported in `src/lib.rs`) — ソケット送受信、フレーム化、簡易リクエスト実行を持つ中心コンポーネント。
- リクエスト構築: `Mc3eRequest` と `Mc3eRequestBuilder` (`src/request.rs`) — サブヘッダ、アクセス経路、監視タイマ、要求データを組み立てる。使い方の一例: `Mc3eRequest::for_read_words_with_target(&target, "D", 0, 10)`。
-- 応答解析: `parse_mc_payload` (`src/parser.rs`) は生の MC3E/MC4E ペイロードを解析し `McResponse` を返す。`end_code` / `has_end_code` の扱いに注意。
- デバイス情報: `src/device.rs` はビルド時生成ファイル (`src/device_code_gen.rs`, `src/devices_gen.rs`) を `include!` して使う。これらは `build.rs` によって生成される（`cargo build` が自動で実行）。

重要な作法／プロジェクト固有の注意点
- フレーム長プレフィックス: 本ライブラリはデフォルトで 2 バイト長プレフィックスを使用せず、生の MC3E/MC4E ペイロードを扱います。送信も生ペイロードを送る想定です。
- 送受信 API:
  - 低レベル送信/受信: `send_raw` / `recv_raw`（`src/transport.rs`）
  - 高レベル交換: `fetch_and_extract` は受信後に長さプレフィックス除去と `parse_mc_payload` を実行し、エンドコードの検査を行う。
 - ConnectionTarget (`src/endpoint.rs`) を通して `access_route` や `monitor_timer` を一元管理する慣習がある。`McClient` に `target` を設定しておくと `read_words` 等の簡易 API が使える。
- Device コードは `sled` キャッシュで上書き可能: `sled_cache` モジュールを参照。CI/ビルドで生成される `device_*_gen.rs` を手で編集しないでください。

デバッグ／実行方法（ローカル）
- ビルド: `cargo build`
- ユニットテスト: `cargo test`（`src` 内のユニットテストと `tests/` 下の統合に注意）
- 例の実行: `cargo run --example simple`（`examples/` に複数サンプルあり。IP/ポートやバイト列を適切に編集して実行）

ログとデバッグ出力
- 初期ログは `env_logger` を使用。`RUST_LOG=info` や `RUST_LOG=debug` を環境変数で指定して起動してください。重要な手がかりは `transport.rs` の `println!` デバッグ行（`[MC SEND RAW]` / `[STREAM PARSER DEBUG]`）にも出ます。

拡張・変更時のチェックポイント（AI が行うべきこと）
- API 変更を加える場合、`src/parser.rs` と `src/transport.rs` の双方を確認。両者はフレーム構造と end-code の扱いを共有しているため、不整合が起きやすい。
- `device_*_gen.rs` と `device_code_gen.rs` は `build.rs` に依存。新しいデバイス定義を導入する場合は `build.rs` を編集し、`cargo build` で生成物を確認してからコミットする。
- `sled` を使う変更は `sled_cache.rs` を確認。ランタイムでの差し替え（override）動作があるため、既存の `DeviceCode::from_str` の挙動を壊さないこと。

参考ファイル（具体例）
- フレーム作成 / 送信: `src/transport.rs`（`send_raw`, `read_words`, `fetch_and_extract`）
- リクエスト組立: `src/request.rs`（`Mc3eRequestBuilder` の `start_address` は 3 バイト little-endian を生成する）
- 解析ロジック: `src/parser.rs`（複数フォーマットを判定して `Mc3Response` を返す）
- 接続ターゲット: `src/endpoint.rs`（`direct`, `local`, `from_parts` の使い分け）
- 例: `examples/simple.rs`, `examples/send_custom_hex.rs`

最後に
- 目的は「最短で安全に動くコードを編集／追加できること」です。変更前に上記ファイルを横断して該当する箇所を必ず確認してください。
- 不明点や不足している具体情報（例: PLC 実機の期待するアクセス経路、既存 Sled DB のキー設計など）があれば教えてください。ここを元に追補を作ります。

----
