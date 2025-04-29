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

#### Babylon

<table><thead><tr><th width="141">Property</th><th width="121">Required/Optional</th><th>Description</th><th width="80">Type</th></tr></thead><tbody><tr><td><code>protocol</code> <mark style="color:red;">*</mark></td><td>Required</td><td>Staking Protocol: Babylon</td><td>String</td></tr><tr><td><code>kind</code> <mark style="color:red;">*</mark></td><td>Required</td><td>The action to perform: Either Unbond or Withdraw</td><td>String</td></tr></tbody></table>

**Example**

```json
{
  "protocol": "Babylon",
  "kind": "Withdraw",
}
```

#### Ethereum

<table><thead><tr><th width="141">Property</th><th width="121">Required/Optional</th><th>Description</th><th width="80">Type</th></tr></thead><tbody><tr><td><code>protocol</code> <mark style="color:red;">*</mark></td><td>Required</td><td>Staking Protocol: Ethereum</td><td>String</td></tr><tr><td><code>kind</code> <mark style="color:red;">*</mark></td><td>Required</td><td>The action to perform: Only Withdraw available</td><td>String</td></tr></tbody></table>

**Example**

```json
{
  "protocol": "Ethereum",
  "kind": "Withdraw",
}
```



#### Iota

{% hint style="info" %}
The Deposit action and the partial withdrawing will be available in the next release
{% endhint %}

<table><thead><tr><th width="141">Property</th><th width="220.8203125">Required/Optional</th><th width="288.71484375">Description</th><th width="80">Type</th></tr></thead><tbody><tr><td><code>protocol</code> <mark style="color:red;">*</mark></td><td>Required</td><td>Staking Protocol: "Iota"</td><td>String</td></tr><tr><td><code>kind</code> <mark style="color:red;">*</mark></td><td>Required</td><td>The action to perform: Either Withdraw or Deposit</td><td>String</td></tr><tr><td>amount</td><td>Required for Withdraw / Update</td><td>Amount to withdraw or add to the stake.</td><td>String</td></tr><tr><td>lockedIotas</td><td>Required for Depost (Timelocked Stake only)</td><td>Locked Iotas to add to the stake.</td><td>String[]</td></tr></tbody></table>



**Example**

```json
{
  "protocol": "Iota",
  "kind": "Deposit",
  "amount": "1000000000"
  "lockedIotas": ["xxxxxx"] // Required when depositing new timelocked stakes
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
