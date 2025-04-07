---
icon: diamond-exclamation
---

# \[deprecated] Delegate Wallet

`POST /wallets/:walletId/delegate`

{% hint style="danger" %}
Delegate Wallet is deprecated. Please use [Delegate Key](../keys/delegate-key.md) instead.
{% endhint %}

In most cases, when you want to implement [Delegated Signing](../../advanced-topics/delegated-signing.md), simply have the end-user create the wallet, in which case it will the non-custodial from the start.  There are some rare cases, however, where the wallet must be created before the user has accessed the system.  To accommodate this, we've added the ability to create a wallet from a service account, and then later delegate it (ie. transfer ownership of it) to an end user via this endpoint.

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name            | Conditions      |
| --------------- | --------------- |
| `Keys:Delegate` | Always Required |

## Parameters

### Path parameters

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

## Request Body

| Field    | Description                                       | Type   |
| -------- | ------------------------------------------------- | ------ |
| `userId` | The ID of the end user to delegate the wallet to. | String |

### Example

```shell
{
  "userId": "us-gk0i1-5bvju-xxxxxxxxxxxxxxxx"
}
```

## Response Body

The response indicates the status of the operation.

### 200 Success

```json
{
  "walletId": "wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "status": "Delegated"
}
```
