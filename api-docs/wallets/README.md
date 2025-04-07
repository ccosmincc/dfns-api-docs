# Wallets

The Dfns Wallets API enables you to create wallets across a wide variety of chains. APIs are exposed at a high level to view and transfer native cryptocurrencies, fungible tokens and NFTs. Additionally we've exposed low-level transaction broadcast APIs enabling integrations with all available features of the supported chains. All the Tier-1 chains listed below are fully indexed to provide accurate asset reporting and transaction history.

Wallets also support [Delegated Signing](../../advanced-topics/delegated-signing.md), enabling a non-custodial configuration and an "Apple Pay for Crypto" UX via our WebAuthn/Passkeys integrations. If you have feedback on the Wallets API, please send it to [docs@dfns.co](mailto:docs@dfns.co).

### Supported networks

You can use any of the follow enumerated types in the `network` field of [Create Wallet](https://docs.dfns.co/dfns-docs/api-docs/wallets/create-wallet#request-body):

| Mainnets                 |      Testnets      | Tier |            Standards           | Confirmation Delay\* |
| ------------------------ | :----------------: | :--: | :----------------------------: | -------------------- |
| `Algorand`               |  `AlgorandTestnet` |   1  |               ASA              | 7                    |
| `Aptos`                  |   `AptosTestnet`   |   1  |             AIP-21             | 30                   |
| `ArbitrumOne`            |  `ArbitrumSepolia` |   1  |         ERC-20, ERC-721        | 50                   |
| `AvalancheC`             |  `AvalancheCFuji`  |   1  |         ERC-20, ERC-721        | 50                   |
| `Base`                   |    `BaseSepolia`   |   1  |         ERC-20, ERC-721        | 50                   |
| `Berachain`              | `BerachainBepolia` |   1  |         ERC-20, ERC-721        | 5                    |
| `Bitcoin`                |   `BitcoinSignet`  |   1  |               N/A              | 2                    |
| `Bsc`                    |    `BscTestnet`    |   1  |         ERC-20, ERC-721        | 50                   |
| `Cardano`                |  `CardanoPreprod`  |   2  |               N/A              | N/A                  |
| `Dogecoin`               | No testnet support |   1  |               N/A              | 40                   |
| `Ethereum`               |  `EthereumSepolia` |   1  |         ERC-20, ERC-721        | 12                   |
| `FantomOpera`            |   `FantomTestnet`  |   1  |         ERC-20, ERC-721        | 5                    |
| `ICP` (aka Dfinity) \*\* | No testnet support |   1  | OGY (ICRC support per request) | 2                    |
| `Ion`                    |    `IonTestnet`    |   1  |          TEP-74/Jetton         | 15                   |
| `Iota` \*\*              |    `IotaTestnet`   |   1  |               N/A              | N/A                  |
| `Kaspa` \*\*             | No testnet support |   1  |               N/A              | 20                   |
| `Kusama`                 |      `Westend`     |   2  |               N/A              | N/A                  |
| `Litecoin`               | No testnet support |   1  |               N/A              | 12                   |
| `Optimism`               |  `OptimismSepolia` |   1  |         ERC-20, ERC-721        | 50                   |
| `Polkadot`               |      `Westend`     |   2  |               N/A              | N/A                  |
| `Polygon`                |    `PolygonAmoy`   |   1  |         ERC-20, ERC-721        | 50                   |
| `Polymesh` \*\*          |  `PolymeshTestnet` |   1  |               N/A              | 4                    |
| `Race`                   |    `RaceSepolia`   |   1  |         ERC-20, ERC-721        | 50                   |
| `SeiPacific1`            |   `SeiAtlantic2`   |   1  |               N/A              | 150                  |
| `Solana`                 |   `SolanaDevnet`   |   1  |           SPL/SPL2022          | 8                    |
| `Stellar`                |  `StellarTestnet`  |   1  |         SEP-41/Classic         | 2                    |
| `Tezos`                  |   `TezosGhostnet`  |   2  |               N/A              | N/A                  |
| `Ton`                    |    `TonTestnet`    |   1  |          TEP-74/Jetton         | 15                   |
| `Tron`                   |     `TronNile`     |   1  |     TRC-10, TRC-20, TRC-721    | 19                   |
| `XrpLedger` (aka Ripple) | `XrpLedgerTestnet` |   2  |               N/A              | N/A                  |

{% hint style="info" %}
\* Confirmation Delay refers to the number of blocks that must be validated or mined after a transaction has been included in a block for that transaction to be indexed by Dfns. Tier-2 chains are not indexed so finality doesn't apply.

\*\* Specialty networks are not available by default. Please contract your sales representative for additional information.
{% endhint %}

### Tier-1 vs Tier-2 support

We plan to add support for more blockchain networks over time. The supported features will vary depending on popularity and market demand.

Tier-1 blockchain networks will support all wallet features, including automatic detection of wallet [asset](get-wallet-assets.md) and [NFT](get-wallet-nfts.md) balances if applicable, and on-chain asset transfer [history](get-wallet-history.md). Tier-1 support also include [transfer asset](transfer-asset.md), [broadcast transaction](broadcast-transaction/) and [generate signature](generate-signature.md). Tier-1 chains also support [Webhooks](../webhooks/) driven by chain indexing.

Tier-2 blockchain networks do not track tokens or on-chain history. Only the [balance](get-wallet-assets.md) of the native token, which is used to pay transaction fees, is returned. Tier-2 support includes [Broadcast Transaction](broadcast-transaction/), [Generate Signature](generate-signature.md), and Transfer Asset for native chain cryptocurrency only. Webhooks are not available for Tier-2 chains.

### Pseudo Networks

{% hint style="danger" %}
Pseudo-network based unbounded wallet creation is deprecated. For raw key signing, please use the [Keys API](../keys/) instead.
{% endhint %}
