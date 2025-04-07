# Update Key

`PUT /keys/{keyId}`

Updates the name of an existing key.

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name          | Conditions      |
| ------------- | --------------- |
| `Keys:Update` | Always Required |

## Parameters

### Path parameters

| Path parameter | Description                   |
| -------------- | ----------------------------- |
| `keyId`        | Unique identifier of the key. |

## Request Body

| Field  | Description           | Type   |
| ------ | --------------------- | ------ |
| `name` | New name for the key. | String |

### Example

```shell
{
  "name": "new key name"
}
```

## Response Body

See [Create Key response](create-key.md#response).

### 200 Success

```json
{
  "id": "key-6ece3-9l565-xxxxxxxxxxxxxxxx",
  "name": "new key name",
  "scheme": "ECDSA",
  "curve": "secp256k1",
  "publicKey": "02660461d66a637ea2d2ee3565669ad794f51ca3e0812ff03a0fe4820a19754839",
  "status": "Active",
  "custodial": true,
  "dateCreated": "2025-03-26T20:25:52.909Z"
}
```
