# List Transaction Requests

`GET /wallets/{walletId}/transactions?paginationToken={token}`

Retrieves a list of transactions requests for the specified wallet.

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

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

### Query parameters

| Query parameter   | Description                                                                                         | Type - Optional      |
| ----------------- | --------------------------------------------------------------------------------------------------- | -------------------- |
| `limit`           | Maximum number of items to return. Default to 100.                                                  | Integer _(optional)_ |
| `paginationToken` | Opaque token used to retrieve the next page. Returned as `nextPageToken` from the previous request. | String _(optional)_  |

## Response Body

| Field           | Description                                                                                                  | Type - Optional                                                             |
| --------------- | ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| `walletId`      | ID of the wallet.                                                                                            | String                                                                      |
| `items`         | List of transaction requests.                                                                                | See [Broadcast Transaction Response](broadcast-transaction/#response-body). |
| `nextPageToken` | Opaque token used to retrieve the next page of items by setting as `paginationToken` in the query parameter. | String _(optional)_                                                         |

### 200 Success

```json
{
  "walletId": "wa-6lbvd-hjdu1-xxxxxxxxxxxxxxxx",
  "items": [
    {
      "id": "tx-214gn-efbru-xxxxxxxxxxxxxxxx",
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
      "txHash": "0x192948dae1bb4cd5765f46417fbfbe500c413f9947dab89184ef3ecd16117640",
      "fee": "93636000499392",
      "dateRequested": "2023-05-10T22:23:44.742Z",
      "dateBroadcasted": "2023-05-10T22:23:51.887Z",
      "dateConfirmed": "2023-05-10T22:24:00.000Z"
    },
    ...
  ],
  "nextPageToken": "WszQXoENUIYyoBQjJm4DE6QhCk2sB7WAh9kykUMaTQcD25SToKbuXkgf3td8ZYb2LrtopPLo35u407gwwA1Sug=="
}
```
