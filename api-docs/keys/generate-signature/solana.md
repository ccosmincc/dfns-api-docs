# Solana

Solana supports the following signature `kinds`:

* `Transaction`, unsigned transaction.

## Transaction <a href="#transaction-request-body" id="transaction-request-body"></a>

Signs an unsigned transaction.

| Field            | Description                                          | Type - Optional |
| ---------------- | ---------------------------------------------------- | --------------- |
| `blockchainKind` | `Solana`                                             | String          |
| `kind`           | `Transaction`                                        | String          |
| `transaction`    | The unsigned hex encoded transaction as shown below. | String          |

```json
{
  "blockchainKind": "Solana",
  "kind": "Transaction",
  "transaction": "0x01000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008001000103b25c8c464080ab2835a166d2b3f13195c2ff3c8f281c7ebe492f0d45d830ff4824a8b38a94b73d2756f2be68655a49706be9b1dc900978984d6eeaf65ab62e900000000000000000000000000000000000000000000000000000000000000000ed589eed2559d935c834cd6d6cbee12970423ad37853618d39e632032aa4c51201020200010c02000000010000000000000000"
}
```

### Typescript Example with Solana web3.js

First install the Solana web3.js SDK. You can find the full documentation here: [https://docs.solana.com/developing/clients/javascript-api](https://docs.solana.com/developing/clients/javascript-api)

Here a code sample to generate a signature via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
import {
  clusterApiUrl,
  Connection,
  PublicKey,
  SystemProgram,
  TransactionMessage,
  VersionedTransaction,
} from '@solana/web3.js'

const walletId = 'wa-6lbfv-9esgj-88s80c0qsih0a393'
const wallet = await dfnsClient.wallets.getWallet({ walletId })

const myAddress = new PublicKey(wallet.address)
const toAddress = new PublicKey('3U6stgsD1FmA7o3omUguritCU8iWmUM7Rs6KqAHHxHVZ')

const connection = new Connection(clusterApiUrl('devnet'), 'confirmed')
const latestBlockhash = await connection.getLatestBlockhash()

const message = new TransactionMessage({
  payerKey: myAddress,
  recentBlockhash: latestBlockhash.blockhash,
  instructions: [
    SystemProgram.transfer({
      fromPubkey: myAddress,
      toPubkey: toAddress,
      lamports: 1n,
    }),
  ],
}).compileToV0Message()
const transaction = new VersionedTransaction(message)

const res = await dfnsClient.wallets.generateSignature({
  walletId,
  body: {
    kind: 'Transaction',
    transaction: `0x${Buffer.from(transaction.serialize()).toString('hex')}`,
  },
})
```
