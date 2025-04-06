---
icon: diamond-exclamation
---

# \[deprecated] Generate Signature

`POST /wallets/{walletId}/signatures`

Request to generate a signature with the wallet key. **This process does not broadcast anything on-chain**, this is just an off-chain signature request.

{% hint style="danger" %}
Wallet Generate Signature is deprecated. Please use [Key Generate Signature](../keys/generate-signature/) instead.
{% endhint %}

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name                     | Conditions      |
| ------------------------ | --------------- |
| `Keys:Signatures:Create` | Always Required |

## Parameters <a href="#parameters.1" id="parameters.1"></a>

### Path parameters <a href="#path-parameters" id="path-parameters"></a>

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

## Request Body

The body of the request will depend on the chain you are targeting.   Please find the chain in question by expanding this section in the left hand navigation:

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

