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

## Parameters <a href="#parameters.1" id="parameters.1"></a>

### Query parameters <a href="#path-parameters" id="path-parameters"></a>

| Query string parameter | Description                                                                                         | Type - Optional     |
| ---------------------- | --------------------------------------------------------------------------------------------------- | ------------------- |
| `owner`                | Get all delegated wallets owned by an end user, either by `userId` or `username`.                   | String _(optional)_ |
| `limit`                | Maximum number of items to return. Default to 50.                                                   | Number _(optional)_ |
| `paginationToken`      | Opaque token used to retrieve the next page. Returned as `nextPageToken` from the previous request. | String _(optional)_ |

## Response <a href="#response" id="response"></a>

| Field           | Description                                                                           | Type - Optional                                        |
| --------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| `items`         | List of wallets.                                                                      | See [Create Wallet response](create-wallet/#response). |
| `nextPageToken` | Opaque token used to retrieve the next page of items. `undefined` if end of the list. | String _(optional)_                                    |

### 200 Success <a href="#response-example" id="response-example"></a>

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
  "nextPageToken": "WszQXoE...wA1Sug=="
}
```
