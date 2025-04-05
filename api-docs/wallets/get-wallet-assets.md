# Get Wallet Assets

`GET /wallets/{walletId}/assets`

Retrieves a list of assets owned by the specified wallet.  Return values vary by chain as shown below.&#x20;

{% hint style="info" %}
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name           | Conditions      |
| -------------- | --------------- |
| `Wallets:Read` | Always Required |

## Parameters <a href="#request-example.1" id="request-example.1"></a>

### Path parameters <a href="#path-parameters" id="path-parameters"></a>

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

### Query parameters <a href="#path-parameters" id="path-parameters"></a>

| Query parameter | Description                                                                | Type - Optional     |
| --------------- | -------------------------------------------------------------------------- | ------------------- |
| `netWorth`      | Set to string value `true` to quote the wallet's total asset value in USD. | String _(optional)_ |

## Response Body <a href="#response" id="response"></a>

| Field      | Description                                                                                          | Type - Optional        |
| ---------- | ---------------------------------------------------------------------------------------------------- | ---------------------- |
| `walletId` | ID of the wallet.                                                                                    | String                 |
| `network`  | Network used for the wallet.                                                                         | String                 |
| `assets`   | A list of asset balances the wallet holds. Fields will vary depending on asset kind, see below.      | Array\<Asset>          |
| `netWorth` | Total net worth of the wallet converted to USD. The value will only include Coingecko listed assets. | Record<"USD", Decimal> |

### Native Asset Fields

| Field      | Description                                                                                                                                                 | Type - Optional                     |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| `kind`     | Asset kind is always `Native`.                                                                                                                              | String                              |
| `symbol`   | Asset symbol.                                                                                                                                               | String                              |
| `decimals` | The smallest unit the token can be divided.                                                                                                                 | Integer                             |
| `balance`  | The balance in the smallest token unit.                                                                                                                     | String                              |
| `verified` | All native assets are verified. For fungible tokens, whethher the token identities have been verified using the information provided by the token projects. | Boolean _(optional)_                |
| `quotes`   | For Coingecko listed assets, the value of holding converted to USD.                                                                                         | Record<"USD", Decimal> _(optional)_ |

```json
{
  "walletId": "wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "network": "Ethereum",
  "assets": [
    {
      "kind": "Native",
      "symbol": "ETH",
      "decimals": 18,
      "verified": true,
      "balance": "1000000000000000000"
    },
    ...
  ]
}
```

### Additional ERC-20 Asset Fields

| Field      | Description                                                                                                   | Type - Optional |
| ---------- | ------------------------------------------------------------------------------------------------------------- | --------------- |
| `kind`     | `Erc20`, the Ethereum fungible token [standard](https://github.com/ethereum/ercs/blob/master/ERCS/erc-20.md). | String          |
| `contract` | The ERC-20 smart contract address of the fungible token.                                                      | String          |

```json
{
  "walletId": "wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "network": "Ethereum",
  "assets": [
    {
      "kind": "Erc20",
      "contract": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
      "symbol": "USDC",
      "decimals": 6,,
      "verified": true,
      "balance": "100000000000000000000"
    },
    ...
  ]
}
```

### Additional Algorand Asset Fields <a href="#alogrand-asa" id="alogrand-asa"></a>

| Field     | Description                                                                                 | Type - Optional |
| --------- | ------------------------------------------------------------------------------------------- | --------------- |
| `kind`    | `Asa`, aka [Algorand standard asset](https://developer.algorand.org/docs/get-details/asa/). | String          |
| `assetId` | The asset ID.                                                                               | String          |

```json
{
  "walletId": "wa-341e6-12nj6-xxxxxxxxxxxxxxxx",
  "network": "Algorand",
  "assets": [
    {
       "kind": "Asa",
       "assetId": "31566704",
       "symbol": "USDC",
       "decimals": 6,
       "verified": true,
       "balance": "200000000"
    },
    ...
  ]
}
```

### Additional Aptos Asset Fields <a href="#evm" id="evm"></a>

| Field      | Description                                                                                                      | Type - Optional |
| ---------- | ---------------------------------------------------------------------------------------------------------------- | --------------- |
| `kind`     | `Aip21`, the Aptos fungible asset [standard](https://github.com/aptos-foundation/AIPs/blob/main/aips/aip-21.md). | String          |
| `metadata` | The asset's metadata address.                                                                                    | String          |

```json
{
  "walletId": "wa-4jbf7-s8lob-xxxxxxxxxxxxxxxx",
  "network": "Aptos",
  "assets": [
    {
       "kind": "Aip21",
       "metadata": "0xbae207659db88bea0cbead6da0ed00aac12edcdda169e591cd41c94180b46f3b",
       "symbol": "USDC",
       "decimals": 6,
       "verified": true,
       "balance": "200000000"
    },
    ...
  ]
}
```

### Additional Iota LockedCoin Asset Fields <a href="#evm" id="evm"></a>

| Field  | Description                                                                                | Type - Optional |
| ------ | ------------------------------------------------------------------------------------------ | --------------- |
| `kind` | `LockedCoin` for timelocked [coin assets](https://docs.iota.org/developer/standards/coin). | String          |
| `coin` | The coin object type of the timelocked coin.                                               | String          |

```json
{
  "walletId": "wa-4to1j-8tho9-xxxxxxxxxxxxxxxx",
  "network": "Iota",
  "assets": [
    {
       "kind": "LockedCoin",
       "coin": "0x2::iota::IOTA",
       "symbol": "IOTA",
       "decimals": 9,
       "verified": true,
       "balance": "100000000000"
    },
    ...
  ]
}
```

### Additional Solana Asset Fields <a href="#evm" id="evm"></a>

| Field  | Description                                                                                                                                                                           | Type - Optional |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| `kind` | Asset kind is either `Spl` for [Solana Program Library tokens](https://spl.solana.com/token), or `Spl2022` for the newly upgraded [token program](https://spl.solana.com/token-2022). | String          |
| `mint` | The token program's mint address.                                                                                                                                                     | String          |

```json
{
  "walletId": "wa-270b0-f5bng-xxxxxxxxxxxxxxxx",
  "network": "Solana",
  "assets": [
    {
      "kind": "Spl",
      "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
      "symbol": "USDC",
      "decimals": 6,
      "verified": true,
      "balance": "10000000"
    },
    ...
  ]
}
```

### Additional Stellar Asset Fields <a href="#solana-spl2022" id="solana-spl2022"></a>

| Field       | Description                                                                                              | Type - Optional |
| ----------- | -------------------------------------------------------------------------------------------------------- | --------------- |
| `kind`      | `Sep41` for [Classic Stellar Assets](https://developers.stellar.org/docs/tokens/stellar-asset-contract). | String          |
| `issuer`    | The asset issuer address.                                                                                | String          |
| `assetCode` | The asset code.                                                                                          | String          |

```json
{
  "walletId": "wa-1ho2b-20tfb-xxxxxxxxxxxxxxxx",
  "network": "Stellar",
  "assets": [
    {
        "kind": "Sep41",
        "issuer": "GA5ZSEJYB37JRC5AVCIA5MOP4RHTM335X2KGX3IHOJAPP5RE34K4KZVN",
        "assetCode": "USDC",
        "symbol": "USDC",
        "decimals": 7,
        "verified": true,
        "balance": "99999999999"
    },
    ...
  ]
}
```

### Additional TON Jetton Asset Fields <a href="#solana" id="solana"></a>

| Field    | Description                                                                                                             | Type - Optional |
| -------- | ----------------------------------------------------------------------------------------------------------------------- | --------------- |
| `kind`   | `Tep74` for [Jetton fungible tokens](https://github.com/ton-blockchain/TEPs/blob/master/text/0074-jettons-standard.md). | String          |
| `master` | The Jetton master smart contract address.                                                                               | String          |

```json
{
  "walletId": "wa-5iu4b-gp4pk-xxxxxxxxxxxxxxxx",
  "network": "Ton",
  "assets": [
    {
        "kind": "Tep74",
        "master": "EQCxE6mUtQJKFnGfaROTKOt1lZbDiiX1kCixRv7Nw2Id_sDs",
        "symbol": "USD₮",
        "decimals": 6,
        "verified": true,
        "balance": "99999999999999"
    },
    ...
  ]
}
```

### Additional TRON TRC-10 Asset Fields <a href="#tron" id="tron"></a>

| Field     | Description                                                                         | Type - Optional |
| --------- | ----------------------------------------------------------------------------------- | --------------- |
| `kind`    | `Trc10`, [native](https://developers.tron.network/docs/trc10) TRON fungible tokens. | String          |
| `tokenId` | The TRC-10 token ID.                                                                | String          |

```json
{
  "walletId": "wa-174tk-m918i-xxxxxxxxxxxxxxxx",
  "network": "Tron",
  "assets": [
    {
        "kind": "Trc20",
        "tokenId": "1004777",
        "symbol": "USDD",
        "decimals": 6,
        "verified": true,
        "balance": "99999000000"
    },
    ...
  ]
}
```

### Additional TRON TRC-20 Asset Fields

| Field      | Description                                                                                                     | Type - Optional |
| ---------- | --------------------------------------------------------------------------------------------------------------- | --------------- |
| `kind`     | `Trc20`, [smart contract based](https://developers.tron.network/docs/trc20-protocol-interface) fungible tokens. | String          |
| `contract` | The TRC-20 smart contract address.                                                                              | String          |

```json
{
  "walletId": "wa-174tk-m918i-xxxxxxxxxxxxxxxx",
  "network": "Tron",
  "assets": [
    {
        "kind": "Trc20",
        "contract": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t",
        "symbol": "USDT",
        "decimals": 6,
        "verified": true,
        "balance": "99999000000"
    },
    ...
  ]
}
```
