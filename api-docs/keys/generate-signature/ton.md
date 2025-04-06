# TON

TON supports the following signature `kinds`:

* `Message`, unsigned external-in message.

## Message <a href="#transaction-request-body" id="transaction-request-body"></a>

Signs an unsigned external-in message in BoC format. Note: successful signing doesn't return `signedData`. Constructing the broadcast data depends on the target smart contract.

| Field            | Description                             | Type - Optional |
| ---------------- | --------------------------------------- | --------------- |
| `blockchainKind` | `Ton`                                   | String          |
| `kind`           | `Message`                               | String          |
| `message`        | The serialized external-in message BoC. | String          |

```json
{
  "blockchainKind": "Ton",
  "kind": "Message",
  "message": "0xb5ee9c7241010201004400011c29a9a317671ba91000000004000301006242000b3fa8f8f4c8abf5f1086d9d8ca76c23ac7186a03cd4142b77428237abb8bfb7880800000000000000000000000000bc0d3854"
}
```

### Typescript Example with ton-core

First install ton-core. You can find the full documentation here: [https://github.com/ton-org/ton](https://github.com/ton-org/ton)

Ton requires a specific message format when interacting with the network. We need to create a signing message payload BoC. Then, we can sign the BoC via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
import { beginCell, internal, SendMode, storeMessageRelaxed, TonClient, WalletContractV4 } from '@ton/ton'

const walletId = 'wa-6lbfv-9esgj-xxxxxxxxxxxxxxxx'
const wallet = await dfnsClient.wallets.getWallet({ walletId })

const client = new TonClient({ endpoint })

const contract = client.open(
  WalletContractV4.create({
    workchain: 0,
    publicKey: Buffer.from(wallet.signingKey.publicKey, 'hex'),
  })
)

const seqno = await opened.getSeqno()

const builder = beginCell().storeUint(contract.walletId, 32)
if (seqno === 0) {
  for (let i = 0; i < 32; i++) {
    builder.storeBit(1)
  }
} else {
  builder.storeUint(Math.floor(Date.now() / 1e3) + 60, 32)
}

builder.storeUint(seqno, 32)
builder.storeUint(0, 8)
builder.storeUint(SendMode.PAY_GAS_SEPARATELY, 8)

const message = internal({
  value: '1',
  to: '0QDXET3_xkWbJwXv16hU402QY3hS3cCelF5Ax3cvUpEbzG5A',
  body: 'Dfns SDK Example',
})

const cell = builder.storeRef(beginCell().store(storeMessageRelaxed(message))).endCell()

const res = await dfnsClient.wallets.generateSignature({
  walletId,
  body: {
    kind: 'Message',
    message: `0x${cell.toBoc().toString('hex')}`,
  },
})
```
