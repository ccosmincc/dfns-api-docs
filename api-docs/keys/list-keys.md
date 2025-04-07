# List Keys

`GET /keys?owner={userId}&limit={limit}&paginationToken={token}`

Retrieves a list of keys.

{% hint style="info" %}
* Request headers required. See [Request Headers](https://app.gitbook.com/o/puStYG2QYnebEAexXqmt/s/tnSPOZGQ2hBmgoVWX5H6/advanced-topics/authentication/request-headers) for more information.
* CommentAuthentication required. See [Authentication Headers](https://app.gitbook.com/o/puStYG2QYnebEAexXqmt/s/tnSPOZGQ2hBmgoVWX5H6/advanced-topics/authentication/request-headers#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name        | Conditions      |
| ----------- | --------------- |
| `Keys:Read` | Always Required |

## Parameters

### Query parameters

| Query parameter   | Description                                                                                         | Type - Optional      |
| ----------------- | --------------------------------------------------------------------------------------------------- | -------------------- |
| `owner`           | Get all delegated keys owned by an end user, either by `userId` or `username`.                      | String _(optional)_  |
| `limit`           | Maximum number of items to return. Default to 100.                                                  | Integer _(optional)_ |
| `paginationToken` | Opaque token used to retrieve the next page. Returned as `nextPageToken` from the previous request. | String _(optional)_  |

## Response Body

| Field           | Description                                                                                                  | Type - Optional                                    |
| --------------- | ------------------------------------------------------------------------------------------------------------ | -------------------------------------------------- |
| `items`         | List of keys.                                                                                                | See [Create Key response](create-key.md#response). |
| `nextPageToken` | Opaque token used to retrieve the next page of items by setting as `paginationToken` in the query parameter. | String _(optional)_                                |

### 200 Success

```json
{
  "items": [
    {
      "id": "key-6ece3-9l565-xxxxxxxxxxxxxxxx",
      "scheme": "ECDSA",
      "curve": "secp256k1",
      "publicKey": "02660461d66a637ea2d2ee3565669ad794f51ca3e0812ff03a0fe4820a19754839",
      "status": "Active",
      "custodial": true,
      "dateCreated": "2025-03-26T20:25:52.909Z"
    },
    ...
  ],
  "nextPageToken": "WszQXoENUIYyoBQjJm4DE6QhCk2sB7WAh9kykUMaTQcD25SToKbuXkgf3td8ZYb2LrtopPLo35u407gwwA1Sug=="
}
```
