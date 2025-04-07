# XRP Ledger (Ripple)

XRP Ledger supports the following signature `kinds`:

* `Transaction`, unsigned transaction.

## Transaction

| Field            | Description                                          | Type   |
| ---------------- | ---------------------------------------------------- | ------ |
| `blockchainKind` | `XrpLedger`                                          | String |
| `kind`           | `Transaction`                                        | String |
| `transaction`    | The unsigned hex encoded transaction as shown below. | String |

```json
{
  "blockchainKind": "XrpLedger",
  "kind": "Transaction",
  "transaction": "0x120000220000000024029a62a82e0001e240201b02a6243661400000000000000168400000000000000c8114860184b4f4c6cc17ae9c2a77cfcd328b43ec2aac8314543aba55a3bede29c5d512ff0cb17db626b9ed9a"
}
```

### Typescript Example with xrpl.js <a href="#typescript-example" id="typescript-example"></a>

First install xrpl.js. You can find the full documentation here: [https://js.xrpl.org/](https://js.xrpl.org/)

Here a code sample to generate a signature via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
import { Client, encode } from 'xrpl'

const walletId = 'wa-6lbfv-9esgj-xxxxxxxxxxxxxxxx'
const wallet = await dfnsClient.wallets.getWallet({ walletId })

const client = new Client(RIPPLE_NODE_URL)
await client.connect()

const transaction = await client.autofill({
  TransactionType: 'Payment',
  Account: wallet.address,
  Destination: 'rBYtCQKxGTfFuob3hxSc8pEYddetT9CdDZ',
  Amount: '1',
})

const res = await dfnsClient.wallets.generateSignature({
  walletId,
  body: {
    kind: 'Transaction',
    transaction: `0x${encode(transaction).toLowerCase()}`,
  },
})
```
