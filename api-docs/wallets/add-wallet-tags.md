# Tag Wallet

`PUT /wallets/{walletId}/tags`

Tags are a way to add arbitrary metadata to wallets which can be used to [filter them in policy engine](https://docs.dfns.co/d/api-docs/policy-engine/policies#filters-for-wallets-sign-activity).  For example, you may want to create deposit wallets which are whitelisted to only send to an omnibus account.  In this case, you could add a tag "`deposit`" to each new wallet and then filter a whitelisting policy to just those wallets.

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name               | Conditions      |
| ------------------ | --------------- |
| `Wallets:Tags:Add` | Always Required |

## Parameters

### Path parameters

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

## Request Body

| Field  | Description                     | Type - Optional |
| ------ | ------------------------------- | --------------- |
| `tags` | The tags to apply to the wallet | Array\<String>  |

### Example

```shell
{
  "tags": ["deposit", "customer:xyz", "security/critical"]
}
```

## Response Body

### 200 Success

```json
{}
```
