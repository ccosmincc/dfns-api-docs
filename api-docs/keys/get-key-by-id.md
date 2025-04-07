# Get Key by ID

`GET /keys/{keyId}`

Retrieves a key by its ID.

{% hint style="info" %}
* Request headers required. See [Request Headers](https://app.gitbook.com/o/puStYG2QYnebEAexXqmt/s/tnSPOZGQ2hBmgoVWX5H6/advanced-topics/authentication/request-headers) for more information.
* CommentAuthentication required. See [Authentication Headers](https://app.gitbook.com/o/puStYG2QYnebEAexXqmt/s/tnSPOZGQ2hBmgoVWX5H6/advanced-topics/authentication/request-headers#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name        | Conditions      |
| ----------- | --------------- |
| `Keys:Read` | Always Required |

## Parameters

### Path parameters

| Path parameter | Description                   |
| -------------- | ----------------------------- |
| `keyId`        | Unique identifier of the key. |

## Response Body

See [Create Key response](create-key.md#response).

### 200 Success

```json
{
  "id": "key-6ece3-9l565-xxxxxxxxxxxxxxxx",
  "scheme": "ECDSA",
  "curve": "secp256k1",
  "publicKey": "02660461d66a637ea2d2ee3565669ad794f51ca3e0812ff03a0fe4820a19754839",
  "status": "Active",
  "custodial": true,
  "dateCreated": "2025-03-26T20:25:52.909Z"
}
```
