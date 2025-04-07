# Create Wallet

`POST /wallets`

Creates a new `Wallet` associated with the given chain (such as `Bitcoin` or `Ethereum` ). Returns a new wallet entity.&#x20;

{% hint style="info" %}
* User action signature required. See [User Action Signing](../../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name               | Conditions                                                                                                           |
| ------------------ | -------------------------------------------------------------------------------------------------------------------- |
| `Wallets:Create`   | Always Required                                                                                                      |
| `Keys:Create`      | Required if wallet creation also creates a new [Key entity](../../keys/create-key.md). This is the default behavior. |
| `Keys:Reuse`       | Required if `signingKey.id` is specified. Wallet will reuse an existing key instead of creating a new one.           |
| `Keys:Delegate`    | Required if `delegateTo` is specified.                                                                               |
| `Wallets:Tags:Add` | Required if `tags` are specified.                                                                                    |

## Request Body

| Field               | Description                                                                                                                                                                                                                                                                                                                                                             | Type - Optional           |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| `network`           | Network used for the wallet. See [Supported Networks](../#supported-networks) for possible values.                                                                                                                                                                                                                                                                      | String                    |
| `name`              | Name given to the wallet                                                                                                                                                                                                                                                                                                                                                | String _(optional)_       |
| `signingKey.id`     | Create a wallet from an existing key. This enables one key to be used across multiple networks and have the same address if networks share the same address format, ex. `Ethereum` and `Polygon`. If specified, requires the `Keys:Reuse` permission. If the key is delegated to an end user, then the new wallet will be automatically delegated to the same end user. | String _(optional)_       |
| `signingKey.scheme` | For networks that support multiple key formats, specify the scheme of the key to create. ex. use `Schnorr` to create a `Bitcoin Taproot` wallet.                                                                                                                                                                                                                        | String _(optional)_       |
| `signingKey.curve`  | For networks that support multiple key formats, specify the curve of the key to create.                                                                                                                                                                                                                                                                                 | String _(optional)_       |
| `tags`              | List of tags to be created for this wallet. If specified, requires the `Wallets:Tags:Add` permission, like the [Tag Wallet](../add-wallet-tags.md) endpoint.                                                                                                                                                                                                            | Array\<String> (optional) |
| `delegateTo`        | ID of the end user to delegate this wallet to upon creation. The wallet will be non-custodial and can only be used by the end user.                                                                                                                                                                                                                                     | String _(optional)_       |
| `delayDelegation`   | Specify if you want to create the wallet from a service account and [later delegate it to an end user](../delegate-wallet.md).  Defaults to `false`.                                                                                                                                                                                                                    | Boolean _(optional)_      |

### Example

```shell
{
  "network": "Ethereum",
  "name": "trading hot wallet"
}
```

## Response Body

| Field                    | Description                                                                                      | Type - Optional     |
| ------------------------ | ------------------------------------------------------------------------------------------------ | ------------------- |
| `id`                     | ID of the wallet.                                                                                | String              |
| `network`                | Network used for the wallet.                                                                     | String              |
| `address`                | Wallet address on its corresponding network.                                                     | String              |
| `name`                   | Name given to the wallet.                                                                        | String _(optional)_ |
| `signingKey.id`          | ID of the key for the wallet.                                                                    | String              |
| `signingKey.scheme`      | Key scheme.                                                                                      | String              |
| `signingKey.curve`       | Key curve.                                                                                       | String              |
| `signingKey.publicKey`   | Hex-encoded value of the public key.                                                             | String              |
| `signingKey.delegatedTo` | The end user ID the key (and wallet) is delegated to.                                            | String _(optional)_ |
| `status`                 | Status of the wallet, can be one of `Active`, `Archived`.                                        | String              |
| `custodial`              | Whether the wallet is owned by an end user (non-custodial), or by your organization (custodial). | Boolean             |
| `dateCreated`            | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when wallet was created.          | String              |
| `tags`                   | List of tags.                                                                                    | Array\<String>      |

### 200 Success

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
