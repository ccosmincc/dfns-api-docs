# Untag Wallet

`DELETE /wallets/{walletId}/tags`

Removes the specified tags from a wallet.&#x20;

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name                  | Conditions      |
| --------------------- | --------------- |
| `Wallets:Tags:Delete` | Always Required |

## Parameters

### Path parameters

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

## Request Body

| Field  | Description                        | Type           |
| ------ | ---------------------------------- | -------------- |
| `tags` | The tags to remove from the wallet | Array\<String> |

### Example

```shell
{
  "tags": ["deposit"]
}
```

## Response Body

### 200 Success

```json
{}
```
