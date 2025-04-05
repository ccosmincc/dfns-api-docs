# Import Wallet

`POST /wallets/import`

{% hint style="info" %}
* This endpoint is not enabled by default. Contact Dfns to have it activated.
* User action signature required. See [User Action Signing](../../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

Dfns secures private keys by generating them as MPC key shares in our decentralized key management network.  This happens by default when you [create a wallet](../create-wallet/).&#x20;

In some circumstances, however, you may need to import an existing wallet (an existing private key) into Dfns infrastructure, instead of creating a brand new wallet with Dfns and transfer funds to it. As an example, you might want to keep an existing wallet if its address is tied to a smart contract which you don't want to re-deploy.

In such a case, Dfns exposes this wallet import API endpoint, which can be used in conjunction with our [import SDK](https://github.com/dfns/dfns-sdk-ts/tree/m/examples/sdk/import-wallet).   Note this is intended to be used only to migrate wallets when first onboarding onto the Dfns platform.&#x20;

{% hint style="danger" %}
Dfns can not guarantee the security of imported wallets, as we have no way to control who had access to the private key prior to import.  For this reason, this feature is restricted to Enterprise customers who have signed a contractual addendum limiting our liability for imported keys.  Please contact your sales representative for more information.&#x20;
{% endhint %}

## Required Permissions

| Name                  | Conditions      |
| --------------------- | --------------- |
| `Signers:ListSigners` | Always Required |
| `Keys:Import`         | Always Required |
| `Wallets:Create`      | Always Required |

## Wallet Import Flow

The wallet private key which you need to import will never be transmitted to Dfns API in one piece or in the clear (un-encrypted). The process is:

1. On your side (client-side), you call our `GET /signers` endpoint to get some information about your Signing Cluster. Your Signing Cluster is the network of nodes (also referred as "signers") the wallet key shares will be imported to. This will provide you with useful information for import (signer IDs, import encryption keys, etc.). This step corresponds to [this line](https://github.com/dfns/dfns-sdk-ts/blob/m/examples/sdk/import-wallet/index.ts#L32) in our SDK wallet import example.
2. With the help of our [import SDK libraries](https://github.com/dfns/dfns-sdk-ts/tree/m/packages/sdk-keyimport-utils), the private key is MPC-sharded on the client side, and each key share is then get encrypted with the corresponding signer encryption key it will get imported to. This step corresponds to [this line](https://github.com/dfns/dfns-sdk-ts/blob/m/examples/sdk/import-wallet/index.ts#L35-L39) in our SDK wallet import example.
3. You then call the Wallet Import endpoint, providing the API with each encrypted key share. This step corresponds to [this line](https://github.com/dfns/dfns-sdk-ts/blob/m/examples/sdk/import-wallet/index.ts#L42-L48) in our SDK wallet import example.
4. Each of those encrypted key shares is transmitted to the corresponding secure node in the Signing Cluster. Each node will then be able to securely decrypt its key share, validate that it is correct, secure it and store it the same way as any wallet in Dfns infrastructure.

## Request Body <a href="#request-body" id="request-body"></a>

<table data-full-width="false"><thead><tr><th>Property</th><th>Description</th><th>Type - Optional</th></tr></thead><tbody><tr><td><code>network</code></td><td>Network enum for the wallet.</td><td>String</td></tr><tr><td><code>name</code></td><td>A name for the key.</td><td>String</td></tr><tr><td><code>protocol</code></td><td><code>CGGMP21</code>, <code>FROST</code>, <code>FROST_BITCOIN</code></td><td>String</td></tr><tr><td><code>curve</code></td><td><code>secp256k1</code>, <code>edd25519</code>, <code>stark</code></td><td>String</td></tr><tr><td><code>minSigners</code></td><td>Always <code>3</code>. Mininum number of signers to complete a signature (TSS threshold).</td><td>Integer</td></tr><tr><td><code>encryptedKeyShares</code></td><td>An array of objects containing the encrypted keyshares.  See format below. </td><td>Array&#x3C;EncryptedKeyShare></td></tr></tbody></table>

#### EncryptedKeyShare

<table data-full-width="false"><thead><tr><th width="201">Property</th><th>Description</th><th width="174">Type - Required/Optional</th></tr></thead><tbody><tr><td><code>signerId</code></td><td>ID of the signer returned from List Signers.</td><td>String</td></tr><tr><td><code>encryptedKeyShare</code></td><td>The key share encrypted with the signer encryption key (public key, asymmetric encryption).</td><td>String</td></tr></tbody></table>

### Example

```json
{
  "network": "Ethereum",
  "name": "treasury wallet",
  "protocol": "CGGMP21",
  "curve": "secp256k1",
  "minSigners": 3,
  "encryptedKeyShares": [
    {
      "signerId": "EX5PdJFcutVTJCgAcSGGGy264JwnrOLLyrZIqMHG67I=",
      "encryptedKeyShare": "ilp3...yP4W"
    },
    {
      "signerId": "KaGnB8iWVpRKBRh+/sAJ0gz1cAZtjhHPufGRgkOXENo=",
      "encryptedKeyShare": "LtqC...r0vR"
    },
    {
      "signerId": "ZokM6nUhGXHYhtQYE/NTeBEz5udvx13Ympcd1raQ4Fc=",
      "encryptedKeyShare": "BFDF...lHMC"
    },
    {
      "signerId": "lGcHWQmdLtJ+4S+RIBFq704/Nox2bugUctVeLL0wPW8=",
      "encryptedKeyShare": "JH7L...sR8U"
    },
    {
      "signerId": "9R4OQb12f8PrEQwFmwZ58ZsNHs6EcGQPWF3fSzhXbVk=",
      "encryptedKeyShare": "R9p9...N+nX"
    }
  ]
}
```

## Response Body <a href="#response-example" id="response-example"></a>

See [Create Wallet Response](../create-wallet/#response).

### 200 Success

```json
{
  "id": "wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "network": "Ethereum",
  "address": "0x00e3495cf6af59008f22ffaf32d4c92ac33dac47",
  "name": "treasury wallet",
  "signingKey": {
    "id": "key-6ece3-9l565-xxxxxxxxxxxxxxxx",
    "scheme": "ECDSA",
    "curve": "secp256k1",
    "publicKey": "e2375c8c9e87bfcd0be8f29d76c818cabacd51584f72cb2222d49a13b036d84d3d"
  },
  "status": "Active",
  "dateCreated": "2023-04-14T20:41:28.715Z",
  "custodial": true,
  "tags": []
}
```
