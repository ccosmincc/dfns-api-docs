# Algorand

Algorand supports the following signature `kinds`:

* `Transaction`, unsigned transaction.

## Transaction

Signs an unsigned transaction.

| Field            | Description                           | Type - Optional |
| ---------------- | ------------------------------------- | --------------- |
| `blockchainKind` | `Algorand`                            | String          |
| `kind`           | `Transaction`                         | String          |
| `transaction`    | The unsigned hex encoded transaction. | String          |

```json
{
  "blockchainKind": "Algorand",
  "kind": "Transaction",
  "transaction": "0x81a374786e89a3616d7401a3666565cd03e8a26676ce0233fc92a367656eac746573746e65742d76312e30a26768c4204863b518a4b3c84ec810f22d4f1081cb0f71f059a7ac20dec62f7f70e5093a22a26c76ce0234007aa3726376c4202c72fe6b78fb1ac99b8e72c9224a6f114c63e598fc1bcf6b048012ae9fc4730aa3736e64c4201256a859b39429ee178e0a65056fb33d51c5139044f6a2603c144278010c7684a474797065a3706179"
}
```

### Typescript Example with AlgoSDK <a href="#typescript-example" id="typescript-example"></a>

First install the AlgoSDK. You can find the full documentation here: [https://github.com/algorand/js-algorand-sdk](https://github.com/algorand/js-algorand-sdk)

Here a code sample to generate a signature via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
import { Algodv2, encodeObj, makePaymentTxnWithSuggestedParamsFromObject } from 'algosdk'

const walletId = 'wa-6lbfv-9esgj-xxxxxxxxxxxxxxxx'
const wallet = await dfnsClient.wallets.getWallet({ walletId })

const algod = new Algodv2(ALGORAND_TOKEN, ALGORAND_NODE_URL)

const suggestedParams = await algod.getTransactionParams().do()
const transaction = makePaymentTxnWithSuggestedParamsFromObject({
  from: wallet.address,
  suggestedParams,
  to: 'CJLKQWNTSQU64F4OBJSQK35THVI4KE4QIT3KEYB4CRBHQAIMO2CD6JWBCY',
  amount: 10000,
})

const bytes = encodeObj({ txn: transaction.get_obj_for_encoding() })

const res = await dfnsClient.wallets.generateSignature({
  walletId,
  body: {
    kind: 'Transaction',
    transaction: `0x${Buffer.from(bytes).toString('hex')}`,
  },
})
```
