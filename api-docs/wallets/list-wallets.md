# List Wallets

`GET /wallets/?owner={userId}&limit={limit}&paginationToken={token}`

Retrieves a list of wallets.

{% hint style="info" %}
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name           | Conditions      |
| -------------- | --------------- |
| `Wallets:Read` | Always Required |

## Parameters

### Query parameters

| Query parameter   | Description                                                                                         | Type - Optional     |
| ----------------- | --------------------------------------------------------------------------------------------------- | ------------------- |
| `owner`           | Get all delegated wallets owned by an end user, either by `userId` or `username`.                   | String _(optional)_ |
| `limit`           | Maximum number of items to return. Default to 100.                                                  | Number _(optional)_ |
| `paginationToken` | Opaque token used to retrieve the next page. Returned as `nextPageToken` from the previous request. | String _(optional)_ |

## Response Body

| Field           | Description                                                                                                  | Type - Optional                                        |
| --------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------ |
| `items`         | List of wallets.                                                                                             | See [Create Wallet response](create-wallet/#response). |
| `nextPageToken` | Opaque token used to retrieve the next page of items by setting as `paginationToken` in the query parameter. | String _(optional)_                                    |

### 200 Success

```json
{
  "items": [
    {
      "id": "wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
      "network": "Ethereum",
      "address": "0x00e3495cf6af59008f22ffaf32d4c92ac33dac47",
      "name": "trading hot wallet",
      "signingKey": {
        "id": "key-6ece3-9l565-xxxxxxxxxxxxxxxx",
        "scheme": "ECDSA",
        "curve": "secp256k1",
        "publicKey": "e2375c8c9e87bfcd0be8f29d76c818cabacd51584f72cb2222d49a13b036d84d3d"
      },
      "status": "Active",
      "dateCreated": "2023-04-14T20:41:28.715Z",
      "custodial": true,
      "tags": []
    },
    ...
  ],
  "nextPageToken": "WszQXoENUIYyoBQjJm4DE6QhCk2sB7WAh9kykUMaTQcD25SToKbuXkgf3td8ZYb2LrtopPLo35u407gwwA1Sug=="
}
```
