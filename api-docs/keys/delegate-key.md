# Delegate Key

`POST /keys/:keyId/delegate`

{% hint style="warning" %}
Only keys created with "`delayDelegation: true`" can then be delegated to an end-user. It means you need to know ahead of time that you're creating a wallet meant to be delegated to an end-user later. This is a safety to prevent, for example, a treasury wallet from being unintentionally delegated to an end-user.&#x20;
{% endhint %}

{% hint style="info" %}
When a key is delegated to an end user, all wallets using this key as the signing key are also automatically delegated to the same end user. Key and wallet ownerships are guaranteed to be always consistent.
{% endhint %}

{% hint style="danger" %}
This operation is irreversible. The key ownership will be transferred to the end-user
{% endhint %}

In most cases, when you want to implement [Delegated Signing](../../advanced-topics/delegated-signing.md), simply create the wallet by directly delegating it to an end user, in which case it will the non-custodial from the start.  There are some rare cases, however, where the key or wallet must be created before the user has accessed to the system.  To accommodate this, we've added the ability to create a key or wallet in delay delegation mode, and then later delegate it (ie. transfer ownership of it) to an end user via this endpoint.

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

| Path parameter | Description                   |
| -------------- | ----------------------------- |
| `keyId`        | Unique identifier of the key. |

## Request Body

| Field    | Description                                    | Type   |
| -------- | ---------------------------------------------- | ------ |
| `userId` | The ID of the end user to delegate the key to. | String |

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
  "keyId": "us-gk0i1-5bvju-xxxxxxxxxxxxxxxx",
  "status": "Delegated"
}
```
