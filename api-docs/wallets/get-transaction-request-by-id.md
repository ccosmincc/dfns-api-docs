# Get Transaction Request by ID

`GET /wallets/{walletId}/transactions/{transactionId}`

Retrieves a Wallet Transaction Request by its ID.

{% hint style="info" %}
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name                        | Conditions      |
| --------------------------- | --------------- |
| `Wallets:Transactions:Read` | Always Required |

## Parameters

### Path parameters

| Path parameter  | Description                                   |
| --------------- | --------------------------------------------- |
| `walletId`      | Unique identifier of the wallet.              |
| `transactionId` | Unique identifier of the transaction request. |

## Response Body

See [Broadcast Transaction Response](broadcast-transaction/#response-body).

### 200 Success

```json
{
  "id": "tx-1jbko-fmk8d-xxxxxxxxxxxxxxxx",
  "walletId": "wa-6lbvd-hjdu1-xxxxxxxxxxxxxxxx",
  "network": "Ethereum",
  "requester": {
    "userId": "us-3v1ag-v6b36-xxxxxxxxxxxxxxxx",
    "tokenId": "to-7mkkj-c831n-xxxxxxxxxxxxxxxx",
    "appId": "ap-24vva-92s32-xxxxxxxxxxxxxxxx"
  },
  "requestBody": {
    "kind": "Transaction",
    "transaction": "0x02e783aa36a71503850d40e49def82520894e5a2ebc128e262ab1e3bd02bffbe16911adfbffb0180c0"
  },
  "status": "Confirmed",
  "txHash": "0x1e62ce5cf14b026d8fe3b5fa6195857049ec22e55fe932c74598c21866c07f14",
  "fee": "31500000147000",
  "dateRequested": "2023-05-09T19:51:33.628Z",
  "dateBroadcasted": "2023-05-09T19:51:39.983Z",
  "dateConfirmed": "2023-05-09T19:51:48.000Z"
}
```
