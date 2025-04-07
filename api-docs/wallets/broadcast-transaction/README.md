# Sign and Broadcast Transaction

`POST /wallets/{walletId}/transactions`

Sign & Broadcast transaction enables communication with any arbitrary smart contract of the target blockchain. You can construct a transaction that performs a complex task and this endpoint will sign the transaction, add the signature and broadcast it to chain. It can be used to call smart contract functions like mint tokens and even deploy new smart contracts.

Note: for reading from a "view" function on EVM chains, please use [Read Contract](../../networks/read-contract.md).

{% hint style="info" %}
* User action signature required. See [User Action Signing](../../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name                          | Conditions      |
| ----------------------------- | --------------- |
| `Wallets:Transactions:Create` | Always Required |

## Parameters

### Path parameters

| Path parameter | Description                      |
| -------------- | -------------------------------- |
| `walletId`     | Unique identifier of the wallet. |

## Request Body

The body of the request will depend on the chain you are targeting. Please find the chain in question by expanding this section in the left hand navigation:

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

## Response Body

| Field                | Description                                                                                                             | Type - Optional     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `id`                 | ID of the transaction request.                                                                                          | String              |
| `walletId`           | ID of the wallet.                                                                                                       | String              |
| `network`            | The network of the transaction.                                                                                         | String              |
| `requester.userId`   | ID of the user made the transfer request.                                                                               | String              |
| `requester.tokenId`  | ID of the token used to make the transfer request.                                                                      | String _(optional)_ |
| `requester.appId`    | Application ID used to make the transfer request.                                                                       | String _(optional)_ |
| `requestBody`        | The original request body.                                                                                              | Object              |
| `externalId`         | External ID specified in the request.                                                                                   | String _(optional)_ |
| `dateRequested`      | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the request was made.                               | String              |
| `status`             | The current status of the request. See table below for a list of possible statuses.                                     | String              |
| `txHash`             | The on-chain transaction hash.                                                                                          | String _(optional)_ |
| `dateBroadcasted`    | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when transaction was broadcasted to the blockchain.      | String _(optional)_ |
| `approvalId`         | ID of the approval when the request triggered a policy.                                                                 | String _(optional)_ |
| `datePolicyResolved` | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the triggered policy was either approved or denied. | String _(optional)_ |
| `reason`             | The failure reason if the request failed to complete.                                                                   | String _(optional)_ |
| `fee`                | The transaction fee.                                                                                                    | String _(optional)_ |
| `dateConfirmed`      | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the transaction was confirmed on chain.             | String _(optional)_ |

#### Request Statuses

<table><thead><tr><th width="149.74609375">Status</th><th>Definition</th></tr></thead><tbody><tr><td><code>Pending</code></td><td>The request is pending approval due to a <a href="https://docs.dfns.co/d/api-docs/policy-engine/policies#wallets-sign-activity">policy applied</a> to the wallet.</td></tr><tr><td><code>Executing</code></td><td>The request is approved and is in the process of being executed. note this status is only set for a short time between pending and broadcasted.</td></tr><tr><td><code>Broadcasted</code></td><td>The transaction has been successfully written to the mempool.</td></tr><tr><td><code>Confirmed</code></td><td>The transaction has been confirmed on-chain by our indexing pipeline.</td></tr><tr><td><code>Failed</code></td><td>Indicates either a system failure to complete the request or the transaction failed on chain.</td></tr><tr><td><code>Rejected</code></td><td>The request has been rejected by a policy approval action.</td></tr></tbody></table>

### 200 Success

```json
{
  "id": "tx-1jbko-fmk8d-xxxxxxxxxxxxxxxx",
  "walletId": "wa-6lbvd-hjdu1-xxxxxxxxxxxxxxxx",
  "network": "Ethereum",
  "requester": {
    "userId": "us-3v1ag-v6b36-xxxxxxxxxxxxxxxx",
    "tokenId": "to-7mkkj-c831n-xxxxxxxxxxxxxxxx",
    "appId": "ap-24vva-92s32-xxxxxxxxxxxxxxxx"
  },
  "requestBody": {
    "kind": "Transaction",
    "transaction": "0x02e783aa36a71503850d40e49def82520894e5a2ebc128e262ab1e3bd02bffbe16911adfbffb0180c0"
  },
  "status": "Confirmed",
  "txHash": "0x1e62ce5cf14b026d8fe3b5fa6195857049ec22e55fe932c74598c21866c07f14",
  "fee": "31500000147000",
  "dateRequested": "2023-05-09T19:51:33.628Z",
  "dateBroadcasted": "2023-05-09T19:51:39.983Z",
  "dateConfirmed": "2023-05-09T19:51:48.000Z"
}
```
