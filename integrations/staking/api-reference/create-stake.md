# Create Stake

`POST /staking/stakes`

Creates a new stake.

{% hint style="info" %}
* User action signature required. See [User Action Signing](../../../api-docs/authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name            | Conditions      |
| --------------- | --------------- |
| `Stakes:Create` | Always Required |

## Parameters <a href="#parameters.1" id="parameters.1"></a>

## Body <a href="#request-body" id="request-body"></a>

<table><thead><tr><th width="141">Property</th><th width="121">Required/Optional</th><th width="258.01953125">Description</th><th width="80">Type</th></tr></thead><tbody><tr><td><code>walletId</code> <mark style="color:red;">*</mark></td><td>Required</td><td>Id of the Dfns wallet making the deposit.</td><td>String</td></tr><tr><td><code>amount</code> <mark style="color:red;">*</mark></td><td>Required</td><td>Transaction amount denominated in min units</td><td>String</td></tr><tr><td><code>provider</code> </td><td>Optional</td><td>Staking Provider, Eg "Figment" </td><td>String</td></tr><tr><td><code>protocol</code> <mark style="color:red;">*</mark></td><td>Required</td><td>Staking Protocol, Eg "Babylon", "Iota",..</td><td>String</td></tr></tbody></table>

Currently, only two kind are possible: `LockedNative`  for vested staking on IOTA and `Native`  otherwise.

Depending on the protocol, some arguments are required when creating a stake:\


#### Babylon

| fields                                       | Type   |
| -------------------------------------------- | ------ |
| `duration`<mark style="color:red;">\*</mark> | number |

#### Iota

You need to choose between amount and lockedIotas to indicate if you want to stake vested coins or native coins

| fields                                        | Type      | Remarks            |
| --------------------------------------------- | --------- | ------------------ |
| `validator`<mark style="color:red;">\*</mark> | string    |                    |
| `amount`                                      | string    | for native staking |
| `lockedIotas`                                 | string\[] | for vested staking |

**Example**

```json
{
  "walletId": "wa-fd328-9v5a8-xxxxxxxxxxxxxxxx",
  "provider": "Figment",
  "protocol": "Babylon",
  "duration": 150,
  "amount": "50000"
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
      "amount": "50000",
      "walletId": "wa-fd328-9v5a8-xxxxxxxxxxxxxxxx",
      "provider": "Figment",
      "protocol": "Babylon",
      "duration": 150
    },
    "dateCreated": "2024-11-27T19:05:33.551Z",
    "actions": [
      {
        "id": "stktx-7cmu5-u8pct-xxxxxxxxxxxxxxxx",
        "stakeId": "stk-5q230-nl4b0-xxxxxxxxxxxxxxxx",
        "transactionId": "tx-4j1md-uperm-xxxxxxxxxxxxxxxx",
        "kind": "Stake",
        "requester": {
          "userId": "us-3htce-s75t3-xxxxxxxxxxxxxxxx",
          "tokenId": "to-72305-jh38s-xxxxxxxxxxxxxxxx",
          "appId": "ap-3g5ir-mt688-xxxxxxxxxxxxxxxx"
        },
        "requestBody": {
          "amount": "50000",
          "walletId": "wa-fd328-9v5a8-xxxxxxxxxxxxxxxx",
          "provider": "Figment",
          "protocol": "Babylon",
          "duration": 150
        },
        "dateCreated": "2024-11-27T19:05:33.551Z"
      }
    ] 
  }
}
```

## Remarks <a href="#response" id="response"></a>

When creating a stake it will implicitly create a `StakeAction` with kind `Stake`
