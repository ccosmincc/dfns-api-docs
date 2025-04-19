# Keys & Multichain - Migration Guide

## TLDR

A Dfns managed key can now be used across different blockchains, as long as the key scheme and curve are compatible with the target chain. The primary use case is using the same key for the Ethereum ecosystem to have the same wallet address across L1 and all the L2 chains. But it doesn't stop there. You can also use the same ECDSA key for Bitcoin and TRON as well.

## What's New

### Keys API

Existing wallets are not impacted. The [Wallets API](../api-docs/wallets/) will remain mostly the same except the following endpoints, [delegate](../api-docs/keys/delegate-key.md), [import](../api-docs/keys/advanced-keys-apis/import-key.md), [export](../api-docs/keys/advanced-keys-apis/export-key.md) and [generate signature](../api-docs/keys/generate-signature/), are deprecated and migrated to equivalent endpoints in the new [Keys API](../api-docs/keys/).

Creating a new wallet automatically creates a signing key in the Dfns MPC cluster. This has always been the behavior except previously the details were hidden. Now the signing key linked to wallets are exposed as `signingKey.id`.

```json
{
  "id": "wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "network": "Ethereum",
  "address": "0x00e3495cf6af59008f22ffaf32d4c92ac33dac47",
  "signingKey": {
    "id": "key-6ece3-9l565-xxxxxxxxxxxxxxxx",
    "scheme": "ECDSA",
    "curve": "secp256k1",
    "publicKey": "e2375c8c9e87bfcd0be8f29d76c818cabacd51584f72cb2222d49a13b036d84d3d"
  },
  ...
}
```

This is the ID you use to invoke the keys endpoints where required.

### Reuse a Key for Multichain

Instead of creating a new signing key when creating a wallet, if you want to create additional wallets with an existing signing key, you can pass the key id in the request body,

```json
{
    "network": "Base",
    "signingKey": {
        "id": "key-6ece3-9l565-xxxxxxxxxxxxxxxx"
    }
}
```

This will create a new Base wallet, but the new wallet will reuse the same signing key and share the same address as the Ethereum wallet.

```json
{
  "id": "wa-19bcu-r0b9h-xxxxxxxxxxxxxxxx",
  "network": "Base",
  "address": "0x00e3495cf6af59008f22ffaf32d4c92ac33dac47",
  "signingKey": {
    "id": "key-6ece3-9l565-xxxxxxxxxxxxxxxx",
    ...
  },
  ...
}
```

When you bridge assets between the two blockchains and the balances will update accurately in both Dfns wallets. Note that you can only create one wallet per network with a given key.

### Deprecated Pseudo Networks

The [concept of pseudo networks](https://docs.dfns.co/d/api-docs/wallets#pseudo-networks), `KeyECDSA` and `KeyEdDSA` networks, was introduced as a way to use a raw signing key with the Wallets API. Now that Keys API is first class, this old workaround is no longer necessary. You should phase out the usage and migrate to [key's generate signature](../api-docs/keys/generate-signature/).

### Enhanced Generate Signature

The new generate signature endpoint with keys is nearly identical to the previous version for wallets, except it takes in one additional field `blockchainKind` in some situations. The reason is that without the wallet context, it can't always know how to interpret the data format. For example, to use a key to sign an Ethereum transaction, you will provide a hint by setting the `blockchainKind` to `Evm`.

```json
{
  "blockchainKind": "Evm",
  "kind": "Transaction",
  "transaction": "0x02e783aa36a71503850d40e49def82520894e5a2ebc128e262ab1e3bd02bffbe16911adfbffb0180c0"
}
```

And to sign the data as a Solana transaction instead, set `blockchainKind` to `Solana`.

One major enhancement is you can now sign complex formats with raw key signing. Before when you use a pseudo-network, the old equivalent of raw key signing, the only supported format is `Hash`. Now by declaring which blockchain you want to be compatible with, you can sign specific data formats with a raw key. For example, if you want to sign an EIP-712 typed message for a private blockchain with just a signing key, you can use

```json
{
  "blockchainKind": "Evm",
  "kind": "Eip712",
  "types": {
    ...
  },
  "domain": {
    "chainId": PRIVATE_CHAIN_ID_HERE,
  },
  "message": {
    ...
  }
}
```

Before you would have to compute the hash digest from the typed data message yourself before it's signable by Dfns.

### Changes to Wallets Permissions

The following new permission operations are added with the Keys API, some replaced the old wallets operations.

| Name                     | Description                                                                      | Note                                                                                                                                         |
| ------------------------ | -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `Keys:Create`            | Required for creating a new key or creating a new wallet with a new signing key. | Before you only need `Wallets:Create` when creating a new wallet, key creation is implicit. Now you will also need `Keys:Create` explicitly. |
| `Keys:Reuse`             | Required for creating a wallet with an existing signing key.                     |                                                                                                                                              |
| `Keys:Delegate`          | Required for delegating a key to an end user.                                    | `Wallets:Delegate` is deprecated.                                                                                                            |
| `Keys:Import`            | Required for importing a key.                                                    | `Wallets:Import` is deprecated.                                                                                                              |
| `Keys:Export`            | Required for exporting a key.                                                    | `Wallets:Export` is deprecated.                                                                                                              |
| `Keys:Signatures:Create` | Required for making a key generate signature request.                            | `Wallets:GenerateSignature` is deprecated.                                                                                                   |
| `Keys:Signatures:Read`   | Required for reading signature requests.                                         | `Wallets:ReadSignature` is deprecated.                                                                                                       |

Also the following operations have been renamed,

| Name                          | Replaced                       |
| ----------------------------- | ------------------------------ |
| `Wallets:Transfers:Create`    | `Wallets:TransferAsset`        |
| `Wallets:Transfers:Read`      | `Wallets:ReadTransfer`         |
| `Wallets:Transactions:Create` | `Wallets:BroadcastTransaction` |
| `Wallets:Transactions:Read`   | `Wallets:ReadTransaction`      |

All existing permissions are updated with the changes outlined above.
