# Generate Signature

`POST /keys/{keyId}/signatures`

Request to generate a signature with the key. **This process does not broadcast anything on-chain**, this is just an off-chain signature request.

Dfns is compatible with any blockchain that uses a supported [key format](../#supported-networks). If Dfns doesn't officially integrate with a blockchain, you can use hash signing to generate the signatures to interact with the chain.

{% hint style="info" %}
* User action signature required. See [User Action Signing](../../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name                     | Conditions      |
| ------------------------ | --------------- |
| `Keys:Signatures:Create` | Always Required |

## Parameters <a href="#parameters.1" id="parameters.1"></a>

### Path parameters <a href="#path-parameters" id="path-parameters"></a>

| Path parameter | Description                   |
| -------------- | ----------------------------- |
| `keyId`        | Unique identifier of the key. |

## Request Body

### Sign Hash

All cryptographic scheme support hash signing. Different blockchains will apply different hash functions to compute the hash.

<table data-full-width="false"><thead><tr><th>Field</th><th>Description</th><th>Type - Optional</th></tr></thead><tbody><tr><td><code>kind</code></td><td><code>Hash</code></td><td>String</td></tr><tr><td><code>hash</code></td><td>32-byte hash in hex encoded format.</td><td>String</td></tr><tr><td><code>taprootMerkleRoot</code></td><td>Required when signing with a <code>Schnorr</code> key. Specify the merkle root for tweaking the signing key, or the empty string <code>""</code> to tweak with the default merkle root.</td><td>String</td></tr></tbody></table>

#### ECDSA and EdDSA Example

```json
{
  "kind": "Hash",
  "hash": "0x031edd7d41651593c5fe5c006fa5752b37fddff7bc4e843aa6af0c950f4b9406"
}
```

#### Schnorr Example

```json
{
  "kind": "Hash",
  "hash": "0x031edd7d41651593c5fe5c006fa5752b37fddff7bc4e843aa6af0c950f4b9406",
  "taprootMerkleRoot": ""
}
```

### Sign Message

In addition to the `Hash` method shown above, `EdDSA` keys also support signing arbitrary length `Message` payload longer than 32 bytes.

<table data-full-width="false"><thead><tr><th>Field</th><th>Description</th><th>Type - Optional</th></tr></thead><tbody><tr><td><code>kind</code></td><td><code>Message</code></td><td>String</td></tr><tr><td><code>message</code></td><td>Hex encoded message.</td><td>String</td></tr></tbody></table>

#### EdDSA Example

```json
{
  "kind": "Message",
  "message": "0x01000507a824baef8cad745bb58148551728d245d6fc21679d1b8f3bbf6abed957f614719dca9b4fcc2b6a68aab9ef37b4db8dc5e99d2d803b577cc61c042453ddd525a6d215d4421860fc0e4a48255b2a6781a494e7ee3f055eeeda2233b590a07b6a2806a1d8179137542a983437bdfe2a7ab2557f535c8a78722b68a49dc00000000006a1d817a502050b680791e6ce6db88e1e5b7150f61fc6790a4eb4d10000000006a7d51718c774c928566398691d5eb68b5eb8a39b4b6d5c73555b210000000006a7d517193584d0feed9bb3431d13206be544281b57b8566cc5375ff40000001abbca65c30117367204561151b7660a672b5fc9fe3d2780d130ea30be604eea0103060102050604000402000000"
}
```

### Sign Chain Dependent Data

Keys can also be used to sign more specific formats for different blockchains. See the available supported options by expanding this section in the left hand navigation.

## Response Body

| Field                | Description                                                                                                                                                                                         | Type - Optional                |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| `id`                 | ID of the signature request.                                                                                                                                                                        | String                         |
| `keyId`              | ID of the signing key.                                                                                                                                                                              | String                         |
| `requester.userId`   | ID of the user made the signature request.                                                                                                                                                          | String                         |
| `requester.tokenId`  | ID of the token used to make the signature request.                                                                                                                                                 | String _(optional)_            |
| `requester.appId`    | Application ID used to make the signature request.                                                                                                                                                  | String _(optional)_            |
| `requestBody`        | The original request body.                                                                                                                                                                          | Object                         |
| `dateRequested`      | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the request was made.                                                                                                           | String                         |
| `status`             | The current status of the request. See table below for a list of possible statuses.                                                                                                                 | String                         |
| `signature`          | After successful signing, the signature separated into `r`, `s`, and `recid`. When signing a serialized transaction, `encoded` contains the complete signature formatted for the target blockchain. | Signature _(optional)_         |
| `signatures`         | When one request produces multiple signatures, for example signing a PSBT.                                                                                                                          | Array\<Signature> _(optional)_ |
| `signedData`         | Serialized signed transaction with encoded signature when signing a transaction.                                                                                                                    | String _(optional)_            |
| `dateSigned`         | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when request was signed.                                                                                                             | String _(optional)_            |
| `approvalId`         | ID of the approval when the request triggered a policy.                                                                                                                                             | String _(optional)_            |
| `datePolicyResolved` | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the triggered policy was either approved or denied.                                                                             | String _(optional)_            |
| `reason`             | The failure reason if the request failed to complete.                                                                                                                                               | String _(optional)_            |
| `txHash`             | The transaction hash if the signature is found on chain.                                                                                                                                            | String _(optional)_            |
| `fee`                | The transaction fee.                                                                                                                                                                                | String _(optional)_            |
| `network`            | The network of the transaction.                                                                                                                                                                     | String _(optional)_            |
| `dateConfirmed`      | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the transaction was confirmed on chain.                                                                                         | String _(optional)_            |

#### Request Statuses

<table><thead><tr><th width="150.0390625">Status</th><th>Definition</th></tr></thead><tbody><tr><td><code>Pending</code></td><td>The request is pending approval due to a <a href="https://docs.dfns.co/d/api-docs/policy-engine/policies#wallets-sign-activity">policy applied</a> to the wallet.</td></tr><tr><td><code>Executing</code></td><td>The request is approved and is in the process of being signed. Note this status is only set for a short time between pending and signed</td></tr><tr><td><code>Signed</code></td><td>The signature is complete and available in the response body.</td></tr><tr><td><code>Confirmed</code></td><td>The signature has been confirmed on-chain by our indexing pipeline.</td></tr><tr><td><code>Failed</code></td><td>Indicates an internal system failure to complete the request.</td></tr><tr><td><code>Rejected</code></td><td>The request has been rejected by a policy approval action.</td></tr></tbody></table>

```json
{
  "id": "sig-2ouaj-f4nq6-xxxxxxxxxxxxxxxx",
  "keyId": "key-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "requester": {
    "userId": "us-3v1ag-v6b36-xxxxxxxxxxxxxxxx",
    "tokenId": "to-7mkkj-c831n-xxxxxxxxxxxxxxxx",
    "appId": "ap-24vva-92s32-xxxxxxxxxxxxxxxx"
  },
  "requestBody": {
    "kind": "Hash",
    "hash": "0x031edd7d41651593c5fe5c006fa5752b37fddff7bc4e843aa6af0c950f4b9406"
  },
  "status": "Signed",
  "signature": {
    "r": "0x05e365d4304eaa78516eb309bff91f8c12e5445a431e3f2428239678d0150c6c",
    "s": "0x47e0765c439fb42d57767910865d240964b7b09f2b2f74d8f14a63da7ce5a1fe",
    "recid": 0
  },
  "dateRequested": "2023-05-15T20:21:11.576Z",
  "dateSigned": "2024-01-10T19:07:40.533Z"
}
```
