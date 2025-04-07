# Export Key

`POST /keys/{keyId}/export`

{% hint style="info" %}
* This endpoint is not enabled by default. Contact Dfns to have it activated.
* User action signature required. See [User Action Signing](../../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

Dfns secures private keys by generating them as MPC key shares in our decentralized key management network.  Our goal is to eliminate all single points of failure (SPOFs) associated with blockchain private keys.

In certain circumstances, however, customers require Dfns to export a private key. In this case, Dfns exposes the following endpoint which can be used in conjunction with our [export SDK](https://github.com/dfns/dfns-sdk-ts/tree/m/examples/sdk/export-wallet).

{% hint style="danger" %}
Dfns can not guarantee the security of exported keys as we have no way to control blockchain transactions once the single point of failure has been reconstituted.  For this reason, this feature is restricted to customers who have signed a contractual addendum limiting our liability for exported keys.  Additionally, by default exported keys can no longer be used to sign within the Dfns platform. Please contact your sales representative for more information.&#x20;
{% endhint %}

## Required Permissions

| Name          | Conditions      |
| ------------- | --------------- |
| `Keys:Export` | Always Required |

## Key Export Flow

The private key which you need to export, will never be transmitted through Dfns system in one piece, or in clear (un-encrypted). The process follows this flow:

1. On your side (client-side), with the help of our [export SDK library](https://github.com/dfns/dfns-sdk-ts/tree/m/packages/sdk-keyexport-utils), you create an "export context" locally. This will generate an encryption/decryption key pair to perform the export in a secure way. This step corresponds to [this line](https://github.com/dfns/dfns-sdk-ts/blob/m/examples/sdk/export-wallet/index.ts#L26) in our SDK key export example.
2. You then call the Key Export endpoint, providing the API with the previous encryption key for secure export. This step corresponds to [this line](https://github.com/dfns/dfns-sdk-ts/blob/m/examples/sdk/export-wallet/index.ts#L29) in our SDK key export example.
3. On Dfns side, the export encryption key gets transmitted to each node of your Signing Cluster (Your Signing Cluster is the network of nodes, also referred as "signers", where your private key shares are securely stored). Each signer node will encrypt the corresponding key share to be exported. All encrypted key shares are then transmitted back to you.
4. On your side (client-side), with the help of our [export SDK library](https://github.com/dfns/dfns-sdk-ts/tree/m/packages/sdk-keyexport-utils), you will then decrypt each encrypted key share, and re-compose the key shares into a single private key. This step corresponds to [this line](https://github.com/dfns/dfns-sdk-ts/blob/m/examples/sdk/export-wallet/index.ts#L35) in our SDK key export example.

## Request Body

<table data-full-width="false"><thead><tr><th>Property</th><th>Description</th><th>Type - Optional</th></tr></thead><tbody><tr><td><code>encryptionKey</code></td><td>The public key of an asymmetric key pair used to encrypt the key shares prior to transmission.</td><td>String</td></tr><tr><td><code>supportedSchemes</code></td><td>An object with the format shown below. </td><td>Array&#x3C;SupportedScheme></td></tr><tr><td><code>delete</code></td><td>Whether the key should be deleted after export. Defaults to <code>true</code> when not specified.</td><td>Boolean <em>(optional)</em></td></tr></tbody></table>

#### SupportedScheme

<table data-full-width="false"><thead><tr><th>Property</th><th>Description</th><th>Type - Optional</th></tr></thead><tbody><tr><td><code>protocol</code></td><td><code>CGGMP21</code>, <code>FROST</code>, <code>FROST_BITCOIN</code></td><td>String</td></tr><tr><td><code>curve</code></td><td><code>secp256k1</code>, <code>edd25519</code>, <code>stark</code></td><td>String</td></tr></tbody></table>

### Example

```json
{
    "encryptionKey": "AQNiFCgqtXFvRdNVciLzZ0hjZxumwtP0hfCrUDsymzWU5A==",
    "supportedSchemes": [
        {
            "protocol": "CGGMP21",
            "curve": "secp256k1"
        }
    ]
}
```

## Response Body

<table data-full-width="false"><thead><tr><th>Property</th><th>Description</th><th>Type - Optional</th></tr></thead><tbody><tr><td><code>publicKey</code></td><td>Public key of the exported key.</td><td>String</td></tr><tr><td><code>protocol</code></td><td><code>CGGMP21</code>, <code>FROST</code>, <code>FROST_BITCOIN</code></td><td>String</td></tr><tr><td><code>curve</code></td><td><code>secp256k1</code>, <code>edd25519</code>, <code>stark</code></td><td>String</td></tr><tr><td><code>minSigners</code></td><td>Always <code>3</code>. Mininum number of signers to complete a signature (TSS threshold).</td><td>Integer</td></tr><tr><td><code>encryptedKeyShares</code></td><td>An array of objects containing the encrypted keyshares.  See format below. </td><td>Array&#x3C;EncryptedKeyShare></td></tr></tbody></table>

#### EncryptedKeyShare

<table data-full-width="false"><thead><tr><th>Property</th><th>Description</th><th>Type - Optional</th></tr></thead><tbody><tr><td><code>signerId</code></td><td>ID of the signer returned from List Signers.</td><td>String</td></tr><tr><td><code>encryptedKeyShare</code></td><td>The key share encrypted with the signer encryption key (public key, asymmetric encryption).</td><td>String</td></tr></tbody></table>

### 200 Success

```json
{
  "publicKey": "0363fd944987c22382c2f34f8ffc53e1fc1e2def96baacd9cbaa5ff51bfb308e2b",
  "curve": "secp256k1",
  "protocol": "CGGMP21",
  "minSigners": 3,
  "encryptedKeyShares": [
    {
      "signerId": "9R4OQb12f8PrEQwFmwZ58ZsNHs6EcGQPWF3fSzhXbVk=",
      "encryptedKeyShare": "Op1j...4tQY"
    },
    {
      "signerId": "lGcHWQmdLtJ+4S+RIBFq704/Nox2bugUctVeLL0wPW8=",
      "encryptedKeyShare": "617Q...p7Yy"
    },
    {
      "signerId": "EX5PdJFcutVTJCgAcSGGGy264JwnrOLLyrZIqMHG67I=",
      "encryptedKeyShare": "YvUd...5t8y"
    },
    {
      "signerId": "ZokM6nUhGXHYhtQYE/NTeBEz5udvx13Ympcd1raQ4Fc=",
      "encryptedKeyShare": "W8pF...Nu7h"
    },
    {
      "signerId": "KaGnB8iWVpRKBRh+/sAJ0gz1cAZtjhHPufGRgkOXENo=",
      "encryptedKeyShare": "7ZZM...Xgm3"
    }
  ]
}
```
