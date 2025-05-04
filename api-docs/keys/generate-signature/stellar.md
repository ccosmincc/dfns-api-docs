# Stellar

Stellar supports the following signature `kinds`:

* `Transaction`, unsigned transaction.

## Transaction

Signs an unsigned transaction. Note: Stellar signing needs a network dependent [passphrase](https://developers.stellar.org/docs/learn/encyclopedia/network-configuration/network-passphrases). Therefore it's tied to a specific `network` rather than the blockchain kind.

| Field         | Description                                          | Type - Optional |
| ------------- | ---------------------------------------------------- | --------------- |
| `network`     | A supported Stellar network.                         | String          |
| `kind`        | `Transaction`                                        | String          |
| `transaction` | The unsigned hex encoded transaction as shown below. | String          |

```json
{
  "network": "Stellar",
  "kind": "Transaction",
  "transaction": "0x0000000500000000d6319d6ad33ba335eb96bf1355a5d930d95f2894067023e15d6046eb698de33700000000000001900000000200000000d6319d6ad33ba335eb96bf1355a5d930d95f2894067023e15d6046eb698de337000000640013442f00000039000000010000000000000000000000006630fac3000000010000001054657374205472616e73616374696f6e00000001000000000000000100000000d6319d6ad33ba335eb96bf1355a5d930d95f2894067023e15d6046eb698de3370000000000000000000000640000000000000001698de33700000040d3c6d585afb419d91e35576327b084515a4e38cf80b1614923b60c09161db46d7624cdc5fcbde7077db82121f421f1d1ab5365c2d949680fe0decbd2c5b7a9080000000000000000"
}
```

### Typescript Example with Stellar SDK <a href="#typescript-example" id="typescript-example"></a>

First install the Stellar SDK. You can find the full documentation here: [https://stellar.github.io/js-stellar-sdk/](https://stellar.github.io/js-stellar-sdk/)

Here a code sample to generate a signature via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
import { Asset, BASE_FEE, Horizon, Networks, Operation, TransactionBuilder } from '@stellar/stellar-sdk'

const walletId = 'wa-6lbfv-9esgj-xxxxxxxxxxxxxxxx'
const wallet = await dfnsClient.wallets.getWallet({ walletId })

const provider = new Horizon.Server(process.env.HORIZON_API_URL!)

const account = await provider.loadAccount(senderWallet.address)
const transaction = new TransactionBuilder(account, {
  fee: BASE_FEE,
  networkPassphrase: Networks.TESTNET,
})
  .addOperation(
    Operation.payment({
      destination: 'GAZWLHTNAOJWW52GZCUJAS5MSXK7LAWCUC5TFOFFVDQ7CDTNFODJ37GB',
      asset: Asset.native(),
      amount: '1',
    })
  )
  .setTimeout(180)
  .build()

const res = await dfnsClient.wallets.generateSignature({
  walletId,
  body: {
    kind: 'Transaction',
    transaction: `0x${transaction.toEnvelope().toXDR('hex')}`,
  },
})
```
