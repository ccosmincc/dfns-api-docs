# Create Key

`POST /keys`

Creates a key for the given scheme and curve. Returns a new key entity.&#x20;

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name            | Conditions                             |
| --------------- | -------------------------------------- |
| `Keys:Create`   | Always Required.                       |
| `Keys:Delegate` | Required if `delegateTo` is specified. |

## Request Body <a href="#request-body" id="request-body"></a>

| Field             | Description                                                                                                                                                    | Type - Optional      |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| `scheme`          |  A supported `scheme`.                                                                                                                                         | String               |
| `curve`           | A supported curve that's compatible with the chosen `scheme`.                                                                                                  | String               |
| `name`            | Name given to the key.                                                                                                                                         | String _(optional)_  |
| `delegateTo`      | ID of the end user to delegate this key to upon creation. The key will be non-custodial and can only be used by the end user.                                  | String _(optional)_  |
| `delayDelegation` | Specify this if you want to create the key from a service account and [later delegate it to an end user](../wallets/delegate-wallet.md).  Defaults to `false`. | Boolean _(optional)_ |

### Example

```json
{
  "scheme": "ECDSA",
  "curve": "secp256k1"
}
```

## Response Body <a href="#response" id="response"></a>

| Field          | Description                                                                                      | Type - Optional      |
| -------------- | ------------------------------------------------------------------------------------------------ | -------------------- |
| `id`           | ID of the key.                                                                                   | String               |
| `scheme`       | Key scheme.                                                                                      | String               |
| `curve`        | Key curve.                                                                                       | String               |
| `publicKey`    | Hex-encoded value of public key.                                                                 | String               |
| `status`       | Status of the key, can be one of `Active`, `Archived`.                                           | String               |
| `custodial`    | Whether the wallet is owned by an end user (non-custodial), or by your organization (custodial). | Boolean              |
| `dateCreated`  | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when wallet was created.          | String               |
| `imported`     | `true` if the key is imported.                                                                   | Boolean _(optional)_ |
| `exported`     | `true` if the key was already exported at least once.                                            | Boolean _(optional)_ |
| `dateExported` | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when wallet was first exported.   | String _(optional)_  |

### 200 Success <a href="#response-example" id="response-example"></a>

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
