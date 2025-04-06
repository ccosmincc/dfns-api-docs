# Substrate (Polkadot)

Substrate based chains like Polkadot, Kusama and Polymesh support the following signature `kinds`:

* `SignerPayload`, serialized [generic signer payload](https://github.com/polkadot-js/api/blob/v15.0.1/packages/types/src/extrinsic/SignerPayload.ts#L47-L51).

## SignerPayload

Signs a generic signer payload. Note: converting the generic signer payload to a signable extrinsic requires fetching metadata from the targeted blockchain. Therefore it's tied to a specific `network` rather than the blockchain kind.

| Field     | Description                                             | Type - Optional |
| --------- | ------------------------------------------------------- | --------------- |
| `network` | A supported Substrate network.                          | String          |
| `kind`    | `SignerPayload`                                         | String          |
| `payload` | The unsigned hex encoded signer payload as shown below. | String          |

```shell
{
  "network": "Polymesh",
  "kind": "SignerPayload",
  "message": "0x0403007f87d29a4746b8e59e347c0598ad811a10c3cd8735d49cf96b75973864c8c98b0475000400386d0f0019000000e143f23803ac50e8f6f8e62695d1ce9e4e1d68aa36c1cd2cfd15340213f3423eb3b9c09f232a12c50f40e023a01f0b86d679b84748cc289534d96861ef611c67"
}
```

### Typescript Example with polkadot{.js}

First install the polkadot{.js} SDK. You can find the full documentation here: [https://polkadot.js.org/docs/](https://polkadot.js.org/docs/)

Here a code sample to generate a signature via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
import { ApiPromise, HttpProvider } from '@polkadot/api'
import { EXTRINSIC_VERSION } from '@polkadot/types/extrinsic/v4/Extrinsic'

const walletId = 'wa-6lbfv-9esgj-xxxxxxxxxxxxxxxx'
const wallet = await dfnsClient.wallets.getWallet({ walletId })

const httpProvider = new HttpProvider(process.env.POLKADOT_NODE_URL!)
const api = await ApiPromise.create({
  provider: httpProvider,
  signer: senderWallet,
  noInitWarn: true,
})

const transaction = api.tx.balances.transferKeepAlive('5EwvHZHrKd9WYc3LByzMZW5cmxJt9VMsfYiKg5jCJb8UBfbC', 10000)
const signerPayload: any = transaction.registry.createTypeUnsafe('SignerPayload', [
  transaction,
  { version: EXTRINSIC_VERSION },
])

const res = await dfnsClient.wallets.generateSignature({
  walletId,
  body: {
    kind: 'SignerPayload',
    message: signerPayload.toHex(),
  },
})
```
