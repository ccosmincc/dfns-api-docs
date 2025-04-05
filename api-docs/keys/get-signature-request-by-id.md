# Get Signature Request by ID

`GET /keys/{keyId}/signatures/{signatureId}`

Get a signature request of a key.

{% hint style="info" %}
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name                   | Conditions      |
| ---------------------- | --------------- |
| `Keys:Signatures:Read` | Always Required |

## Parameters <a href="#parameters.1" id="parameters.1"></a>

### Path parameters <a href="#path-parameters" id="path-parameters"></a>

| Path parameter | Description                                 |
| -------------- | ------------------------------------------- |
| `keyId`        | Unique identifier of the key.               |
| `signatureId`  | Unique identifier of the signature request. |

## Response Body <a href="#response" id="response"></a>

See [Generate Signature Response](generate-signature.md#response-body).

### 200 Success <a href="#response-example" id="response-example"></a>

```json
{
  "id": "sig-2ouaj-f4nq6-xxxxxxxxxxxxxxxx",
  "keyId": "key-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "requester": {
    "userId": "us-3v1ag-v6b36-xxxxxxxxxxxxxxxx",
    "tokenId": "to-7mkkj-c831n-xxxxxxxxxxxxxxxx",
    "appId": "ap-24vva-92s32-xxxxxxxxxxxxxxxx"
  },
  "requestBody": {
    "kind": "Hash",
    "hash": "0x031edd7d41651593c5fe5c006fa5752b37fddff7bc4e843aa6af0c950f4b9406"
  },
  "status": "Signed",
  "signature": {
    "r": "0x05e365d4304eaa78516eb309bff91f8c12e5445a431e3f2428239678d0150c6c",
    "s": "0x47e0765c439fb42d57767910865d240964b7b09f2b2f74d8f14a63da7ce5a1fe",
    "recid": 0
  },
  "dateRequested": "2023-05-15T20:21:11.576Z",
  "dateSigned": "2024-01-10T19:07:40.533Z"
}
```
