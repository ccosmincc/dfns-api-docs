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

## Parameters <a href="#path-parameters" id="path-parameters"></a>

### Path parameters <a href="#path-parameters" id="path-parameters"></a>

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

## Request Body <a href="#native-currency-request-body" id="native-currency-request-body"></a>

| Field  | Description                        | Type           |
| ------ | ---------------------------------- | -------------- |
| `tags` | The tags to remove from the wallet | Array\<String> |

```shell
{
  "tags": ["deposit"]
}
```

## Response Body <a href="#native-currency-response-example" id="native-currency-response-example"></a>

### 200 Success <a href="#native-currency-response-example" id="native-currency-response-example"></a>

```json
{}
```
