# Legacy: mc3e definitions

このファイルは旧 `src/mc3e.rs` の内容をそのまま保存した参照用ドキュメントです。
`mc3e` と `mc4e` の定義や定数を比較・参照したい場合はこちらを参照してください。

> 注意: 現在のライブラリは `mc4e` を主要モジュール名として使っており、実装上も MC4E 形式を優先しています。

---

以下は元の `mc3e.rs` の内容（そのまま）:

```rust
//! MC3E/MC4E プロトコル定義モジュール
//!
//! このモジュールは MC3E/MC4E フレームの定数と簡易ユーティリティを提供します。
//! - サブヘッダ（要求/応答）の定義
//! - ヘッダサイズ、アクセス経路のデフォルト値
//! - コマンド／サブコマンド定数
//! - 簡易な end-code 名前解決
//!
//! NOTE: 低レイヤーのフレーム構築やパーサは別モジュールで実装されています。

// 要求電文の構築や応答電文の解析を行うモジュール
// NOTE: imports for read/write helpers were removed because submodules are not implemented.
// pub mod read;
// pub mod write;
// NOTE: read/write submodules are not implemented as separate files in this tree.

/// MC3E ヘッダサイズ（サブヘッダ等を含む）
pub const MC3E_HEADER_SIZE: usize = 11;

/// MC3E 要求サブヘッダ
pub const MC3E_SUBHEADER_REQUEST: [u8; 2] = [0x50, 0x00];

/// MC3E 応答サブヘッダ
pub const MC3E_SUBHEADER_RESPONSE: [u8; 2] = [0xD0, 0x00];

/// MC4E ヘッダサイズ（シリアル番号付きフレームを含む）
pub const MC4E_HEADER_SIZE: usize = 15;

/// MC4E 要求サブヘッダ
pub const MC4E_SUBHEADER_REQUEST: [u8; 2] = [0x54, 0x00];

/// MC4E 応答サブヘッダ
pub const MC4E_SUBHEADER_RESPONSE: [u8; 2] = [0xD4, 0x00];

/// アクセス経路（デフォルト）: ネットワーク0x00、PC番号0xFF、IO番号0x03FF、局番号0x00
pub const MC_ACCESS_PATH_DEFAULT: [u8; 5] = [0x00, 0xFF, 0xFF, 0x03, 0x00];

/// MC コマンド定義（プロトコル上は 2 バイトフィールド、wire はリトルエンディアン）
pub const MC_CMD_READ_WORD: u16 = 0x0104;
pub const MC_CMD_WRITE_WORD: u16 = 0x0114;
pub const MC_CMD_READ_BIT: u16 = 0x0104;
pub const MC_CMD_WRITE_BIT: u16 = 0x0114;

/// MC サブコマンド（Q/R 系列）
pub const MC_SUBCMD_Q_WORD: u16 = 0x0000;
pub const MC_SUBCMD_Q_BIT: u16 = 0x0100;
pub const MC_SUBCMD_R_WORD: u16 = 0x0200;
pub const MC_SUBCMD_R_BIT: u16 = 0x0300;

// MC end-code 定義（共通）
// Placeholder / canonical end-code constants. Add or replace with full table later.
pub const MC_END_OK: u16 = 0x0000;
pub const MC_END_ERROR_GENERAL: u16 = 0x0001;
pub const MC_END_ERROR_ILLEGAL_DATA: u16 = 0x0002;
pub const MC_END_ERROR_DEVICE_NOT_FOUND: u16 = 0x0003;

/// Return a short static name for a known end-code, or None if unknown.
pub fn end_code_name(code: u16) -> Option<&'static str> {
    match code {
        MC_END_OK => Some("OK"),
        MC_END_ERROR_GENERAL => Some("GENERAL_ERROR"),
        MC_END_ERROR_ILLEGAL_DATA => Some("ILLEGAL_DATA"),
        MC_END_ERROR_DEVICE_NOT_FOUND => Some("DEVICE_NOT_FOUND"),
        _ => None,
    }
}

// 再エクスポート: 詳細なエラーコード表を別ファイルで提供
// `src/error_codes.rs` に定義した説明付きコードを公開します。
pub use crate::error_codes::*;
```

---

保存場所: `docs/legacy_mc3e.md`。

このファイルはドキュメントとしてのみ利用され、ビルドには影響しません。
