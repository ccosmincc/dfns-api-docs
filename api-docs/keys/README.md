# Keys

The Dfns Keys API enables you to sign complex transactions for any Bitcoin compatible, EVM compatible or other alternative L1 blockchains Dfns supports. This API enables integration with hundreds of blockchains and ecosystems, including the blockchains not officially supported by Dfns as long as they are compatible with one of the supported schemes.

Keys also support [Delegated Signing](../../advanced-topics/delegated-signing.md), enabling a non-custodial configuration. If you have feedback on the Wallets API, please send it to [docs@dfns.co](mailto:docs@dfns.co).

### Supported keys <a href="#supported-networks" id="supported-networks"></a>

| Scheme  | Curve     |
| ------- | --------- |
| ECDSA   | secp256k1 |
| ECDSA   | stark     |
| EdDSA   | ed25519   |
| Schnorr | secp256k1 |

