# tk-rs

A Rust library for interacting with the Turnkey API, providing secure token signing and verification capabilities. This library supports both general-purpose signing and Solana-specific signing operations.

## Features

- ECDSA signing using Turnkey's secure infrastructure
- Solana transaction signing support
- Secure API authentication with stamps
- Async/await support
- Error handling with anyhow

## Installation

Add this to your `Cargo.toml`:

```toml
[dependencies]
tk-rs = { git = "https://github.com/pkxro/tk-rs.git" }
```

## Usage

First, set up your Turnkey credentials:

```rust
use tk_rs::TurnkeySigner;

let signer = TurnkeySigner::new(
    env::var("TURNKEY_API_PUBLIC_KEY").unwrap(),
    env::var("TURNKEY_API_PRIVATE_KEY").unwrap(),
    env::var("TURNKEY_ORGANIZATION_ID").unwrap(),
    env::var("TURNKEY_PRIVATE_KEY_ID").unwrap(),
    env::var("TURNKEY_PUBLIC_KEY").unwrap(),
)?;
```

### General Purpose Signing

```rust
let message = b"Hello, World!";
let signature = signer.sign(message).await?;
```

### Solana Signing

```rust
let message = b"Solana transaction data...";
let signature = signer.sign_solana(message).await?;

// Get the associated Solana public key
let pubkey = signer.solana_pubkey();
```

## API Reference

### `TurnkeySigner`

The main struct for interacting with the Turnkey API.

#### Methods

- `new(api_public_key: String, api_private_key: String, organization_id: String, private_key_id: String, public_key: String) -> Result<Self, anyhow::Error>`
  - Creates a new TurnkeySigner instance with the provided credentials.

- `async sign(&self, message: &[u8]) -> Result<Vec<u8>, anyhow::Error>`
  - Signs a message using ECDSA and returns the signature as a byte vector.

- `async sign_solana(&self, message: &[u8]) -> Result<solana_sdk::signature::Signature, anyhow::Error>`
  - Signs a message and returns a Solana-compatible signature.

- `solana_pubkey(&self) -> solana_sdk::pubkey::Pubkey`
  - Returns the associated Solana public key.

## Environment Variables

Create a `.env` file with your Turnkey credentials:

```env
TURNKEY_API_PUBLIC_KEY=your_api_public_key
TURNKEY_API_PRIVATE_KEY=your_api_private_key
TURNKEY_ORGANIZATION_ID=your_organization_id
TURNKEY_PRIVATE_KEY_ID=your_private_key_id
TURNKEY_PUBLIC_KEY=your_public_key
```

## License

This project is licensed under the MIT License.
