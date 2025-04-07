# Aptos

Aptos supports the following signature `kinds`:

* `Transaction`, unsigned transaction.

## Transaction

Signs an unsigned transaction in BCS format.

| Field            | Description                                                               | Type - Optional |
| ---------------- | ------------------------------------------------------------------------- | --------------- |
| `blockchainKind` | `Aptos`                                                                   | String          |
| `kind`           | `Transaction`                                                             | String          |
| `transaction`    | The unsigned BCS format transaction in hex encoding. More details bellow. | String          |

```json
{
  "blockchainKind": "Aptos",
  "kind": "Transaction",
  "transaction": "0xc5510ac408dcb78034b4fdcdaea6582c10f2e0d03dc684d32f9457b9d778e113020000000000000002000000000000000000000000000000000000000000000000000000000000000104636f696e087472616e73666572010700000000000000000000000000000000000000000000000000000000000000010a6170746f735f636f696e094170746f73436f696e0002205bdc24cb9033286ffe19f436145b9e2267dd03b0fd0d422459d381a6431d39ba080100000000000000400d0300000000006400000000000000cce1f2670000000002"
}
```

### Transaction Format

We accept two transaction kinds to be signed:

* [RawTransaction](https://aptos.web3doc.top/guides/creating-a-signed-transaction/#raw-transaction), this is the simplest transaction, no external fee payer, no secondary signers.
* [SignedTransaction](https://aptos.web3doc.top/guides/creating-a-signed-transaction/#signed-transaction), for more complex transaction, you can pass a `SignedTransaction` that will contains the RawTransaction and an authenticator depending on the transaction. Accepted authenticator are:
  * &#x20;TransactionAuthenticatorFeePayer for sponsored transactions (single/multi sig)
  * TransactionAuthenticatorMultiAgent for multi signature transactions (not sponsored)

All the passed transactions should be [BCS](https://aptos.dev/en/build/sdks/ts-sdk/building-transactions/bcs-format) serialized and in hex encoding.

You can find advanced transaction constructs in our [Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts).

### Typescript Example with Aptos SDK <a href="#typescript-example" id="typescript-example"></a>

First install the Aptos SDK. You can find the source code here: [https://github.com/aptos-labs/aptos-ts-sdk](https://github.com/aptos-labs/aptos-ts-sdk)

Here a code sample to generate a signature for a basic transaction via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript

import {
  Aptos, 
  APTOS_COIN, 
  AptosConfig, 
  MimeType, 
  Network, 
  PendingTransactionResponse, 
  postAptosFullNode
} from '@aptos-labs/ts-sdk'


const walletId = 'wa-6lbfv-9esgj-88s80c0qsih0a393'
const wallet = await dfnsClient.wallets.getWallet({ walletId })

const myAddress = new PublicKey(wallet.address)
const toAddress = new PublicKey('0x5bdc24cb9033286ffe19f436145b9e2267dd03b0fd0d422459d381a6431d39ba')

const aptosConfig = new AptosConfig({
    network: Network.TESTNET,
  })
const client = new Aptos(aptosConfig)

const tx = await client.transaction.build.simple({
    sender: wallet.address,
    data: {
      function: '0x1::coin::transfer',
      typeArguments: [APTOS_COIN],
      functionArguments: [
        "0x5bdc24cb9033286ffe19f436145b9e2267dd03b0fd0d422459d381a6431d39ba", // Receiver
        "1", // Amount
      ],
    },
  })

const signed = (await wallet.signTransaction(tx))

const { data } = await postAptosFullNode<Uint8Array, PendingTransactionResponse>({
  aptosConfig: client.config,
  body: signed.bcsToBytes(),
  path: 'transactions',
  originMethod: 'submitTransaction',
  contentType: MimeType.BCS_SIGNED_TRANSACTION,
})

// Wait for the transaction to be included
await client.waitForTransaction({transactionHash: data.hash})

```
