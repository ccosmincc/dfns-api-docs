# Keys

The Dfns Keys API enables you to sign complex transactions for any Bitcoin compatible, EVM compatible or other alternative L1 blockchains that Dfns supports. This API enables integration with hundreds of blockchains and ecosystems, for example

* Use Dfns keys with hundreds of blockchains in the EVM, Cosmos, or Polkadot ecosystems that Dfns doesn't directly integrate with.
* Use Dfns keys with blockchains Dfns doesn't natively support, like Starknet, as long as they use one of the supported key formats.
* Use Dfns keys with private blockchains that Dfns doesn't have access to, such as Polygon Supernets or Avalanche Subnets.

Keys also support [Delegated Signing](../../advanced-topics/delegated-signing.md), enabling a non-custodial configuration. If you have feedback on the Keys API, please send it to [docs@dfns.co](mailto:docs@dfns.co).

## Supported Key Formats <a href="#supported-networks" id="supported-networks"></a>

| Scheme  | Elliptical Curve |
| ------- | ---------------- |
| ECDSA   | secp256k1        |
| ECDSA   | stark            |
| EdDSA   | ed25519          |
| Schnorr | secp256k1        |
