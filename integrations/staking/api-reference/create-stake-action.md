# Create Stake Action

`POST /staking/stakes/:stakeId/actions`

Creates a new stake action.

{% hint style="info" %}
* User action signature required. See [User Action Signing](../../../api-docs/authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name            | Conditions      |
| --------------- | --------------- |
| `Stakes:Update` | Always Required |

## Path Parameters <a href="#parameters.1" id="parameters.1"></a>

| Name                                        | Conditions                |
| ------------------------------------------- | ------------------------- |
| `stakeId`<mark style="color:red;">\*</mark> | Id of the stake to update |

## Body <a href="#request-body" id="request-body"></a>

<table><thead><tr><th width="141">Property</th><th width="121">Required/Optional</th><th>Description</th><th width="80">Type</th></tr></thead><tbody><tr><td><code>protocol</code> <mark style="color:red;">*</mark></td><td>Required</td><td>Staking Protocol, Eg "Babylon", "Ethereum", "Iota"</td><td>String</td></tr><tr><td><code>kind</code> <mark style="color:red;">*</mark></td><td>Required</td><td>The action to perform</td><td>String</td></tr></tbody></table>



Possible value for kind depending on the protocol:

<table><thead><tr><th width="146.0703125">Protocol</th><th>Kind</th></tr></thead><tbody><tr><td>Babylon</td><td>Unbond | Withdraw</td></tr><tr><td>Ethereum</td><td>Withdraw</td></tr></tbody></table>



**Example**

```json
{
  "protocol": "Babylon",
  "kind": "Withdraw",
}
```

## Response <a href="#response" id="response"></a>

### Response example <a href="#response-example" id="response-example"></a>

```json
{
  "stake": {
    "id": "stk-5q230-nl4b0-xxxxxxxxxxxxxxxx",
    "provider": "Figment",
    "providerStakeId": "1dd3b430-729e-4935-8da1-bc7af56a4e7a",
    "walletId": "wa-fd328-9v5a8-xxxxxxxxxxxxxxxx",
    "protocol": "Babylon",
    "status": "Active",
    "requester": {
      "userId": "us-3htce-s75t3-xxxxxxxxxxxxxxxx",
      "tokenId": "to-72305-jh38s-xxxxxxxxxxxxxxxx",
      "appId": "ap-3g5ir-mt688-xxxxxxxxxxxxxxxx"
    },
    "requestBody": {
      "kind": "Native",
      "amount": "50000",
      "walletId": "wa-fd328-9v5a8-xxxxxxxxxxxxxxxx",
      "provider": "Figment",
      "protocol": "Babylon",
      "duration": 150
    },
    "dateCreated": "2024-11-27T19:05:33.551Z"
},
  "stakeAction": {
    "id": "stka-5q230-nl4b0-xxxxxxxxxxxxxxxx",
    "stakeId": "stk-5q230-nl4b0-xxxxxxxxxxxxxxxx",
    "transactionId": "1dd3b430-729e-4935-8da1-bc7af56a4e7a",
    "kind": "Withdraw",
    "protocol": "Babylon",
    "requester": {
      "userId": "us-3htce-s75t3-xxxxxxxxxxxxxxxx",
      "tokenId": "to-72305-jh38s-xxxxxxxxxxxxxxxx",
      "appId": "ap-3g5ir-mt688-xxxxxxxxxxxxxxxx"
    },
    "requestBody": {
      "kind": "Withdraw",
      "protocol": "Babylon",
    },
    "dateCreated": "2024-11-27T19:05:33.551Z"
  },
}
```
