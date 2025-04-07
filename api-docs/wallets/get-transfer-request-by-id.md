# Get Transfer Request by ID

`GET /wallets/{walletId}/transfers/{transferId}`

Retrieves a Wallet Transfer Request by its ID.

{% hint style="info" %}
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name                     | Conditions      |
| ------------------------ | --------------- |
| `Wallets:Transfers:Read` | Always Required |

## Parameters

### Path parameters

| Path parameter | Description                                |
| -------------- | ------------------------------------------ |
| `walletId`     | Unique identifier of the wallet.           |
| `transferId`   | Unique identifier of the transfer request. |

## Response Body

See [Transfer Asset Response](transfer-asset.md#response-body).

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
