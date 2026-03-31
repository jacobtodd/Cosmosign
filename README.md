# Cosmosign

A lightweight, client-side tool for signing and verifying messages using [Keplr Wallet](https://www.keplr.app) on any Cosmos chain.

**No backend. No server. Zero data leaves your browser.**

## Features

- **Connect Keplr Wallet** — supports Cosmos Hub, Osmosis, Juno, Stargaze, Injective, Celestia, dYdX, Noble
- **Sign Messages** — signs using ADR-036 (the Cosmos off-chain signing standard)
- **Verify Signatures** — full client-side secp256k1 verification, no wallet required
- **Ownership Proof** — derives the signer's bech32 address from the public key cryptographically and confirms it matches

## How it works

### Signing

Keplr's `signArbitrary` wraps your message in an ADR-036 amino document before signing:

```json
{
  "account_number": "0",
  "chain_id": "",
  "fee": { "amount": [], "gas": "0" },
  "memo": "",
  "msgs": [{ "type": "sign/MsgSignData", "value": { "data": "<base64(message)>", "signer": "cosmos1..." } }],
  "sequence": "0"
}
```

This document is serialized to canonical JSON (keys sorted alphabetically), then signed with the wallet's secp256k1 private key.

### Verification

1. The amino document is reconstructed from the original message and signer address
2. SHA-256 hashed
3. The secp256k1 signature is verified against the hash using the public key
4. The signer's bech32 address is derived from the public key: `SHA-256 → RIPEMD-160 → bech32`
5. The derived address is compared against the provided address to confirm ownership

## Supported Chains

| Chain | Chain ID | Prefix |
|---|---|---|
| Cosmos Hub | cosmoshub-4 | cosmos |
| Osmosis | osmosis-1 | osmo |
| Juno | juno-1 | juno |
| Stargaze | stargaze-1 | stars |
| Injective | injective-1 | inj |
| Celestia | celestia | celestia |
| dYdX | dydx-mainnet-1 | dydx |
| Noble | noble-1 | noble |

## Deployment (GitHub Pages)

1. Push to GitHub
2. Settings → Pages → Source: `main` branch `/root`
3. Live at `https://<username>.github.io/Cosmosign`

## Requirements

- [Keplr Wallet](https://www.keplr.app) browser extension

## Tech stack

- Vanilla HTML/CSS/JS — no build step, no framework
- [elliptic](https://github.com/indutny/elliptic) for secp256k1 verification
- [js-sha256](https://github.com/emn178/js-sha256) for SHA-256
- [hash.js](https://github.com/indutny/hash.js) for RIPEMD-160
- [Keplr Wallet](https://www.keplr.app) `window.keplr` API for signing
