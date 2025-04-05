# Get Wallet by ID

`GET /wallets/{walletId}`

Retrieves a Wallet by its ID.

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

## Response Body <a href="#response" id="response"></a>

See [Create Wallet response](create-wallet/#response).

### 200 Success <a href="#response-example" id="response-example"></a>

```json
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
}
```
