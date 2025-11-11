# melsec_mc

[![crates.io](https://img.shields.io/crates/v/melsec_mc.svg)](https://crates.io/crates/melsec_mc) [![docs.rs](https://docs.rs/melsec_mc/badge.svg)](https://docs.rs/melsec_mc) [![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE) [![Publish Workflow](https://github.com/tyaro/melsec_com/actions/workflows/publish.yml/badge.svg)](https://github.com/tyaro/melsec_com/actions/workflows/publish.yml)

A lightweight Rust library that provides sending/receiving and a small client for Mitsubishi Electric PLCs using the MC protocol (Ethernet / MC4E compatible).

Version: 0.4.0

Main features
- Tokio-based asynchronous transport
- Raw MC frame send/receive and parser
- High-level one-shot helpers and a reusable `McClient`
- Typed read/write support via `FromWords` / `ToWords`
- Error registry merge semantics and other registry improvements

Important changes in this release
- Hardened public API (removed panic/unwrap/expect/eprintln!), errors are returned as `Result<..., MelsecError>`.
- Introduced `McResponse::try_new` and migrated call sites.
- Added typed read/write traits and client helpers: `FromWords` / `ToWords` and `McClient::read_words_as` / `write_words_as`.

Contents
- Quickstart
- Installation
- Usage (simple example)
- Advanced usage (Typed API, McClient)
- Releases & publishing
- Contributing and license

## Quickstart

1. Clone and build:

```powershell
git clone https://github.com/tyaro/melsec_com.git
cd melsec_com
cargo build
```

2. Run an example (adjust PLC address in `examples`):

```powershell
cargo run --example simple
```

## Installation

If published on crates.io, add to your `Cargo.toml`:

```toml
[dependencies]
melsec_mc = "0.4.0"
```

For development from the Git repository:

```toml
[dependencies]
melsec_mc = { git = "https://github.com/tyaro/melsec_com", branch = "main" }
```

## Usage (simple example)

Example using async Tokio runtime:

```rust
use melsec_mc::{McClient, ConnectionTarget};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Specify the target and construct a client
    let target = ConnectionTarget::direct("192.168.1.40", 4020);
    let client = McClient::new().with_target(target);

    // Read bits (method name is `read_bits`)
    let bits = client.read_bits("M", 10, 3).await?;
    println!("bits: {:?}", bits);

    // Read words (method name is `read_words`)
    let words = client.read_words("D", 100, 2).await?;
    println!("words: {:?}", words);

    Ok(())
}
```

## Typed read/write (Typed API)

This release adds `FromWords` / `ToWords` traits that let you read/write multi-word types such as `f32`, `u32`, or fixed-size boolean arrays directly.

```rust
// Reading (element count = 2 of f32; internally requests 4 words)
let floats: Vec<f32> = client.read_words_as::<f32>("D1000", 2).await?;

// Writing
client.write_words_as("D1010", &vec![1.23f32, 4.56f32]).await?;
```

Typed reads parse element-by-element. On parse failure the client logs a warning, skips one word to resynchronize, and continues; successfully parsed elements are returned (partial results allowed).

## Releases & publishing

- The project uses GitHub Releases: https://github.com/tyaro/melsec_com/releases
- To publish to crates.io use `cargo publish` (requires credentials and 2FA).

Verify package creation with:

```powershell
cargo publish --dry-run
```

## Contributing & contact

- Pull requests welcome. For major API changes, open an Issue first.
- Report bugs or request features via GitHub Issues.

## License

MIT

---
The Japanese README (`README.md`) was updated; this file provides an English counterpart as `README.en.md`.

## Payload field types (TOML)

In `commands.toml`, a request field that represents a payload can use the following kinds:

- `bytes`:
    - Raw byte payload. Parameters may be a numeric array (e.g. `[16, 32, 255]`) or a string which will be written as-is as bytes. Legacy aliases `rest` and `..` are supported for compatibility, but prefer `bytes` in new definitions.

- `ascii_hex`:
    - Treat the field as an ASCII hex string. The parameter must be a string containing only hex characters (`0-9`, `A-F`, `a-f`), e.g. `"0123AB"`. Each character is written as its ASCII byte value. This is useful for echo-style commands or when the payload is textual hex.

Example (echo command using ascii_hex):

```toml
[[command]]
id = "echo"
command_code = 0x0619
request_format = ["command:2be", "subcommand:2be", "payload:ascii_hex"]
response_format = ["payload:ascii_hex"]
device_family = "Any"
```
