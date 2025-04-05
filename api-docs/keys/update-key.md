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

## Parameters <a href="#path-parameters" id="path-parameters"></a>

### Path parameters <a href="#path-parameters" id="path-parameters"></a>

| Path parameter | Description                   |
| -------------- | ----------------------------- |
| `keyId`        | Unique identifier of the key. |

## Request Body <a href="#request-body" id="request-body"></a>

| Field  | Description           | Type   |
| ------ | --------------------- | ------ |
| `name` | New name for the key. | String |

### Example

```shell
{
  "name": "new key name"
}
```

## Response Body <a href="#response" id="response"></a>

See [Create Key response](create-key.md#response).

### 200 Success <a href="#response-example" id="response-example"></a>

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
