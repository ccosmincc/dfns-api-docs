# Get Wallet NFTs

`GET /wallets/{walletId}/nfts`

Retrieves a list of NFTs owned by the specified Wallet.

{% hint style="info" %}
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name           | Conditions      |
| -------------- | --------------- |
| `Wallets:Read` | Always Required |

## Parameters

### Path parameters

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

## Response Body

| Field      | Description                                                                             | Type - Optional |
| ---------- | --------------------------------------------------------------------------------------- | --------------- |
| `walletId` | ID of the wallet.                                                                       | String          |
| `network`  | Network used for the wallet.                                                            | String          |
| `nfts`     | A list of NFTs the wallet holds. Fields will vary depending on the NFT kind, see below. | Array\<NFT>     |

### Algorand NFT Fields

| Field      | Description             | Type - Optional     |
| ---------- | ----------------------- | ------------------- |
| `kind`     | `ASA`.                  | String              |
| `assetId`  | The NFT's asset ID.     | String              |
| `symbol`   | The NFT's symbol.       | String _(optional)_ |
| `tokenUri` | The NFT's metadata URI. | String _(optional)_ |

```json
{
  "walletId": "wa-341e6-12nj6-xxxxxxxxxxxxxxxx",
  "network": "Algorand",
  "nfts": [
    {
      "kind": "Asa",
      "assetId": "2844799928",
      "symbol": "HUNT013"
    },
    ...
  ]
}
```

### ERC-721 NFT Fields

| Field      | Description                                                 | Type - Optional     |
| ---------- | ----------------------------------------------------------- | ------------------- |
| `kind`     | `Erc721`, smart contract based non fungible token standard. | String              |
| `contract` | ERC-721 contract address.                                   | String              |
| `tokenId`  | The NFT's token ID.                                         | String              |
| `symbol`   | The NFT's symbol.                                           | String _(optional)_ |
| `tokenUri` | The NFT's metadata URI.                                     | String _(optional)_ |

```json
{
  "walletId": "wa-19lns-o74qn-xxxxxxxxxxxxxxxx",
  "network": "Ethereum",
  "nfts": [
    {
      "kind": "Erc721",
      "contract": "0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d",
      "tokenId": "8500",
      "symbol": "BAYC",
      "tokenUri": "ipfs://QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq/8500"
    },
    ...
  ]
}
```

### TRC-721 NFT Fields

| Field      | Description                                                 | Type - Optional     |
| ---------- | ----------------------------------------------------------- | ------------------- |
| `kind`     | `Trc721`, smart contract based non fungible token standard. | String              |
| `contract` | TRC-721 contract address.                                   | String              |
| `tokenId`  | The NFT's token ID.                                         | String              |
| `symbol`   | The NFT's symbol.                                           | String _(optional)_ |
| `tokenUri` | The NFT's metadata URI.                                     | String _(optional)_ |

```json
{
  "walletId": "wa-174tk-m918i-xxxxxxxxxxxxxxxx",
  "network": "Tron",
  "nfts": [
    {
      "kind": "Trc721",
      "contract": "TUyxyj7sjk5ShsFo1oyNZ2b5uuwcqSctd2",
      "tokenId": "178",
      "symbol": "BAYC"
    },
    ...
  ]
}
```
