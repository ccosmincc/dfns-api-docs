# Transfer Asset

`POST /wallets/{walletId}/transfers`

Transfer an asset out of the specified wallet to a destination address. For all fungible token transfers, the transfer amount must be specified in the minimum denomination of that token. For example, use the amount in `Satoshi` for a Bitcoin transfer, or the amount in `Wei` for an Ethereum transfer etc.

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name                       | Conditions      |
| -------------------------- | --------------- |
| `Wallets:Transfers:Create` | Always Required |

## Parameters

### Path parameters

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

## Request Body

### Native Token

Transfer the native token of the network. All networks support the native token type.

| Field          | Description                                                                                                                                      | Type - Optional     |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------- |
| `kind`         | `Native`                                                                                                                                         | String              |
| `to`           | The destination address.                                                                                                                         | String              |
| `amount`       | The amount of native tokens to transfer in minimum denomination.                                                                                 | String              |
| `priority`     | The priority that determines the fees paid for the transfer. \[1]                                                                                | String _(optional)_ |
| `memo`         | The memo or destination tag. \[2]                                                                                                                | String _(optional)_ |
| `externalId`   | A unique ID from your system. It can be leveraged to be used as an idempotency key. (read more [here](../../advanced-topics/api-idempotency.md)) | String _(optional)_ |
| `feeSponsorId` | A fee sponsor id to sponsor the transaction fee by another wallet. (read more [here](../fee-sponsor/))                                           | String _(optional)_ |

1. All EVM compatible networks and Bitcoin support `priority`. Not supported for other networks. The accepted values are `Slow`, `Standard` and `Fast`. It uses the [estimate fees](../networks/estimate-fees.md) API to calculate the transfer fees. When not specified, defaults to `Standard` priority.
2. `Stellar`, `TON` and `XrpLedger` support `memo`. Not valid for other networks.

```json
{
  "kind": "Native",
  "to": "0xb282dc7cde21717f18337a596e91ded00b79b25f",
  "amount": "1000000000"
}
```

### Algorand Standard Asset <a href="#asa" id="asa"></a>

Transfer Algorand standard assets, or [ASAs](https://developer.algorand.org/docs/get-details/asa/).

| Field     | Description                                               | Type - Optional |
| --------- | --------------------------------------------------------- | --------------- |
| `kind`    | `Asa`                                                     | String          |
| `assetId` | The asset ID of the token.                                | String          |
| `to`      | The destination address.                                  | String          |
| `amount`  | The amount of tokens to transfer in minimum denomination. | String          |

```json
{
  "kind": "Asa",
  "assetId": "31566704",
  "to": "FRZP423Y7MNMTG4OOLESESTPCFGGHZMY7QN462YEQAJK5H6EOMFHZG73UA",
  "amount": "1000000"
}
```

### Aptos Fungible Asset (AIP-21) <a href="#aip-21" id="aip-21"></a>

Transfer Aptos fungible asset that implement the [AIP-21 specification](https://github.com/aptos-foundation/AIPs/blob/main/aips/aip-21.md).

| Field      | Description                                               | Type - Optional |
| ---------- | --------------------------------------------------------- | --------------- |
| `kind`     | `Aip21`                                                   | String          |
| `metadata` | The asset metadata address.                               | String          |
| `to`       | The destination address.                                  | String          |
| `amount`   | The amount of tokens to transfer in minimum denomination. | String          |

```json
{
  "kind": "Aip21",
  "metadata": "0xbae207659db88bea0cbead6da0ed00aac12edcdda169e591cd41c94180b46f3b",
  "to": "0x470a59fbbaaf5dc6bd06eea917245f025a8024d23c4364acec0f377282ee269a",
  "amount": "1000000"
}
```

### EVM Fungible Token (ERC-20) <a href="#erc-20" id="erc-20"></a>

Transfer fungible tokens that implement the [ERC-20 specification](https://eips.ethereum.org/EIPS/eip-20).

| Field      | Description                                                  | Type - Optional     |
| ---------- | ------------------------------------------------------------ | ------------------- |
| `kind`     | `Erc20`                                                      | String              |
| `contract` | The ERC-20 contract address.                                 | String              |
| `to`       | The destination address.                                     | String              |
| `amount`   | The amount of tokens to transfer in minimum denomination.    | String              |
| `priority` | The priority that determines the fees paid for the transfer. | String _(optional)_ |

```json
{
  "kind": "Erc20",
  "contract": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
  "to": "0xb282dc7cde21717f18337a596e91ded00b79b25f",
  "amount": "1000000"
}
```

### EVM Non Fungible Token (ERC-721) <a href="#erc-721" id="erc-721"></a>

Transfer non-fungible tokens that implement the [ERC-721 specification](https://eips.ethereum.org/EIPS/eip-721)

| Field      | Description                                                  | Type - Optional     |
| ---------- | ------------------------------------------------------------ | ------------------- |
| `kind`     | `Erc721`                                                     | String              |
| `contract` | The ERC-721 contract address.                                | String              |
| `to`       | The destination address.                                     | String              |
| `tokenId`  | The token to transfer.                                       | String              |
| `priority` | The priority that determines the fees paid for the transfer. | String _(optional)_ |

```shell
{
  "kind": "Erc721",
  "contract": "0x00fb58432ef9d418bf6688bcf0a226d2fcaa18e2",
  "to": "0xb282dc7cde21717f18337a596e91ded00b79b25f",
  "tokenId": "1"
}
```

### Solana Program Library Token (SPL and SPL 2022) <a href="#spl" id="spl"></a>

Transfer [SPL tokens](https://spl.solana.com/token) or [SPL 2022 tokens](https://spl.solana.com/token-2022).

| Field                      | Description                                                                                                      | Type                 |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------- | -------------------- |
| `kind`                     | `Spl` or `Spl2022`                                                                                               | String               |
| `mint`                     | The mint account address.                                                                                        | String               |
| `to`                       | The destination address.                                                                                         | String               |
| `amount`                   | The amount of tokens to transfer in minimum denomination.                                                        | String               |
| `createDestinationAccount` | If `true`, pay to create the associated token account of the recipient if it doesn't exist. Defaults to `false`. | Boolean _(optional)_ |
| `feeSponsorId`             | A fee sponsor id to sponsor the transaction fee by another wallet. (read more [here](../fee-sponsor/))           | String _(optional)_  |

```json
{
  "kind": "Spl",
  "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
  "to": "3U6stgsD1FmA7o3omUguritCU8iWmUM7Rs6KqAHHxHVZ",
  "amount": "1000000"
}
```

### Stellar Classic Assets (SEP-41) <a href="#sep-41" id="sep-41"></a>

Transfer classic [Stellar Assets](https://developers.stellar.org/docs/issuing-assets/anatomy-of-an-asset). They all implement the [SEP-41 token interface](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0041.md).

| Field       | Description                                               | Type                |
| ----------- | --------------------------------------------------------- | ------------------- |
| `kind`      | `Sep41`                                                   | String              |
| `issuer`    | The asset issuer address.                                 | String              |
| `assetCode` | The asset code.                                           | String              |
| `to`        | The destination address.                                  | String              |
| `amount`    | The amount of tokens to transfer in minimum denomination. | String              |
| `memo`      | The memo.                                                 | String _(optional)_ |

```json
{
  "kind": "Sep41",
  "issuer": "GA5ZSEJYB37JRC5AVCIA5MOP4RHTM335X2KGX3IHOJAPP5RE34K4KZVN",
  "assetCode": "USDC",
  "to": "GAZWLHTNAOJWW52GZCUJAS5MSXK7LAWCUC5TFOFFVDQ7CDTNFODJ37GB",
  "amount": "1000000"
}
```

### TON Jetton (TEP-74) <a href="#tep-74" id="tep-74"></a>

Transfer [Jetton](https://github.com/ton-blockchain/TEPs/blob/master/text/0074-jettons-standard.md) tokens.

| Field    | Description                                               | Type - Optional |
| -------- | --------------------------------------------------------- | --------------- |
| `kind`   | `Tep74`                                                   | String          |
| `master` | The Jetton master contract address.                       | String          |
| `to`     | The destination address.                                  | String          |
| `amount` | The amount of tokens to transfer in minimum denomination. | String          |

```json
{
  "kind": "Tep74",
  "master": "EQAIZUJZxUgjovq8C6P5tRGwSsydiCtKiwRnycPnN1k4WpFo",
  "to": "EQBfYLuQwjbBd-LAZ6eNC26XmVVxEl86MQPKG981hdTSicL_",
  "amount": "1000000"
}
```

### TRON Native Fungible Token (TRC-10) <a href="#trc-10" id="trc-10"></a>

Transfer TRON's TRC-10 fungible tokens

| Field     | Description                                               | Type - Optional |
| --------- | --------------------------------------------------------- | --------------- |
| `kind`    | `Trc10`                                                   | String          |
| `tokenId` | The token ID.                                             | String          |
| `to`      | The destination address.                                  | String          |
| `amount`  | The amount of tokens to transfer in minimum denomination. | String          |

```json
{
  "kind": "Trc10",
  "tokenId": "1005273",
  "to": "TADDx31pdCFfp3XrYxp6fQGbRxriYFLTrx",
  "amount": "10000"
}
```

### TRON Smart Contract Fungible Token (TRC-20) <a href="#trc-20" id="trc-20"></a>

Transfer fungible tokens that implement the TRC-20 smart contract specification.

| Field      | Description                                               | Type - Optional |
| ---------- | --------------------------------------------------------- | --------------- |
| `kind`     | `Trc20`                                                   | String          |
| `contract` | The smart contract address.                               | String          |
| `to`       | The destination address.                                  | String          |
| `amount`   | The amount of tokens to transfer in minimum denomination. | String          |

```json
{
  "kind": "Trc20",
  "contract": "TXLAQ63Xg1NAzckPwKHvzw7CSEmLMEqcdj",
  "to": "TQJNezrbfJ3akrGgR7eM2fWyFpsKeM8wzN",
  "amount": "1000000"
}
```

### TRON Non Fungible Token (TRC-721) <a href="#trc-721" id="trc-721"></a>

Transfer non-fungible tokens that implement the TRC-721 smart contract specification.

| Field      | Description                 | Type - Optional |
| ---------- | --------------------------- | --------------- |
| `kind`     | `Trc721`                    | String          |
| `contract` | The smart contract address. | String          |
| `to`       | The destination address.    | String          |
| `tokenId`  | The token to transfer.      | String          |

```json
{
  "kind": "Trc721",
  "contract": "TKgnDMWHYmwH24REe9XnrnwcNCvtb53n8Q",
  "to": "TQJNezrbfJ3akrGgR7eM2fWyFpsKeM8wzN",
  "tokenId": "1"
}
```

## Response Body

| Field                | Description                                                                                                             | Type - Optional     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `id`                 | ID of the transfer request.                                                                                             | String              |
| `walletId`           | ID of the wallet.                                                                                                       | String              |
| `network`            | The network of the transfer.                                                                                            | String              |
| `requester.userId`   | ID of the user made the transfer request.                                                                               | String              |
| `requester.tokenId`  | ID of the token used to make the transfer request.                                                                      | String _(optional)_ |
| `requester.appId`    | Application ID used to make the transfer request.                                                                       | String _(optional)_ |
| `requestBody`        | The original request body.                                                                                              | Object              |
| `externalId`         | External ID specified in the request.                                                                                   | String _(optional)_ |
| `feeSponsorId`       | The ID of the fee sponsor.                                                                                              | String _(optional)_ |
| `dateRequested`      | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the request was made.                               | String              |
| `status`             | The current status of the request. See table below for a list of possible statuses.                                     | String              |
| `txHash`             | The on-chain transaction hash.                                                                                          | String _(optional)_ |
| `dateBroadcasted`    | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when transaction was broadcasted to the blockchain.      | String _(optional)_ |
| `approvalId`         | ID of the approval when the request triggered a policy.                                                                 | String _(optional)_ |
| `datePolicyResolved` | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the triggered policy was either approved or denied. | String _(optional)_ |
| `reason`             | The failure reason if the request failed to complete.                                                                   | String _(optional)_ |
| `fee`                | The transaction fee.                                                                                                    | String _(optional)_ |
| `dateConfirmed`      | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the transaction was confirmed on chain.             | String _(optional)_ |

#### Request Statuses

<table><thead><tr><th width="149.91796875">Status</th><th>Definition</th></tr></thead><tbody><tr><td><code>Pending</code></td><td>The request is pending approval due to a <a href="https://docs.dfns.co/d/api-docs/policy-engine/policies#wallets-sign-activity">policy applied</a> to the wallet.</td></tr><tr><td><code>Executing</code></td><td>The request is approved and is in the process of being executed. note this status is only set for a short time between pending and broadcasted.</td></tr><tr><td><code>Broadcasted</code></td><td>The transaction has been successfully written to the mempool.</td></tr><tr><td><code>Confirmed</code></td><td>The transaction has been confirmed on-chain by our indexing pipeline.</td></tr><tr><td><code>Failed</code></td><td>Indicates either system failure to complete the request or the transaction failed on chain.</td></tr><tr><td><code>Rejected</code></td><td>The request has been rejected by a policy approval action.</td></tr></tbody></table>

### 200 Success

```json
{
  "id": "xfr-6ulmv-sa183-xxxxxxxxxxxxxxxx",
  "walletId": "wa-40f4f-51gpm-xxxxxxxxxxxxxxxx",
  "network": "Ethereum",
  "requester": {
    "userId": "us-4vu4v-kud3l-xxxxxxxxxxxxxxxx",
    "appId": "ap-7c2pm-avfsr-xxxxxxxxxxxxxxxx"
  },
  "requestBody": {
    "kind": "Erc20",
    "contract": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
    "amount": "1000000",
    "to": "0xb282dc7cde21717f18337a596e91ded00b79b25f"
  },
  "metadata": {
    "asset": {
      "symbol": "USDC",
      "decimals": 6,
      "verified": true,
      "quotes": {
        "USD": 1.000804849917271,
        "EUR": 0.9201529894769885
      }
    }
  },
  "status": "Confirmed",
  "fee": "1542993669053672",
  "txHash": "0x8e88793607610a83798eb5ec6dde861f3e459c7e4a22e78b0d2e675b86d0d1e7",
  "dateRequested": "2024-01-18T23:03:53.739Z",
  "dateBroadcasted": "2024-01-18T23:03:55.685Z",
  "dateConfirmed": "2024-01-18T23:03:59.000Z"
}
```
