# Update Wallet

`PUT /wallets/{walletId}`

Updates the name of an existing wallet.

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name             | Conditions      |
| ---------------- | --------------- |
| `Wallets:Update` | Always Required |

## Path parameters <a href="#path-parameters" id="path-parameters"></a>

| Path parameter | Description                                             |
| -------------- | ------------------------------------------------------- |
| `walletId`     | ID of the wallet. ex. `wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx` |

## Request <a href="#request-body" id="request-body"></a>

<table><thead><tr><th width="203">Field</th><th>Description</th><th width="184">Type - Optional</th></tr></thead><tbody><tr><td><code>name</code></td><td>New name for the wallet</td><td>String</td></tr></tbody></table>

#### Example

```shell
{
  "name": "new wallet name"
}
```

## Response <a href="#response" id="response"></a>

See [Create Wallet response](create-wallet/#response).

### 200 Success <a href="#response-example" id="response-example"></a>

```json
{
  "id": "wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "network": "Ethereum",
  "address": "0x00e3495cf6af59008f22ffaf32d4c92ac33dac47",
  "name": "new wallet name",
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
