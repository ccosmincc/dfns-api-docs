# Bitcoin / Litecoin

## PSBT

Signs a partially signed bitcoin transaction and broadcasts it to chain.

| Field        | Description                                                                                                                                         | Type - Optional     |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `kind`       | `Psbt`                                                                                                                                              | String              |
| `psbt`       | The hex encoded PSBT as shown below.                                                                                                                | String              |
| `externalId` | A unique ID from your system. It can be leveraged to be used as an idempotency key (read more [here](../../../advanced-topics/api-idempotency.md)). | String _(optional)_ |

```json
{
  "kind": "Psbt",
  "psbt": "0x70736274ff0100710200000001ca17431a33a13d3ef8bfb041c8546071f9d3a609abe3c91efbed83265e1426730100000000ffffffff02e803000000000000160014a40a65b46ff36c53f1afb8e35e25a4c0bcfc9979d6d1150000000000160014237ad8ba2ffd992f6ebc7ab388e77f00fc87d1c9000000000001011f54d6150000000000160014237ad8ba2ffd992f6ebc7ab388e77f00fc87d1c9000000"
}
```

### Typescript Example with BitcoinJS <a href="#typescript-example" id="typescript-example"></a>

First install the BitcoinJS SDK. You can find the full documentation here: [https://github.com/bitcoinjs/bitcoinjs-lib](https://github.com/bitcoinjs/bitcoinjs-lib)

Here a code sample to broadcast a transaction via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
import { networks, payments, Psbt } from 'bitcoinjs-lib'
import axios from 'axios'

const walletId = 'wa-6lbfv-9esgj-xxxxxxxxxxxxxxxx'
const wallet = await dfnsClient.wallets.getWallet({ walletId })
const publicKey = Buffer.from(wallet.signingKey.publicKey, 'hex')

const network = networks.testnet
const { address } = payments.p2wpkh({
  pubkey: publicKey,
  network,
})

const txid = '87872516c6e93f136fc6c493c7172596b11c695e27889de7532abffcac2a4b5e'
const n = 1
const utxo = (
  await axios.post(BITCOIN_NODE_URL, {
    jsonrpc: '2.0',
    id: 'gettxout',
    method: 'gettxout',
    params: [txid, n, false],
  })
).data.result

const balance = utxo.balance * 100000000
const amount = 1
const fee = 150

const psbt = new Psbt({ network })
psbt.addInput({
  hash: txid,
  index: n,
  witnessUtxo: {
    script: Buffer.from(utxo.scriptPubKey.hex, 'hex'),
    value: balance,
  },
})

psbt.addOutput({
  address: 'tb1q5s9xtdr07dk98ud0hr34ufdycz70exte2kehm2',
  value: amount,
})

psbt.addOutput({
  address,
  value: balance - amount - fee,
})

const res = await dfnsClient.wallets.broadcastTransaction({
  walletId,
  body: { kind: 'Psbt', psbt: `0x${psbt.toHex()}` },
})
```
