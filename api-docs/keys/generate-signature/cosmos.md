# Cosmos Appchain

Cosmos Appchains like Osmosis and Sei support the following signature `kinds`:

* `SignDocDirect`, serialized sign doc for [sign mode direct](https://docs.cosmos.network/main/learn/advanced/transactions#sign_mode_direct-preferred).

## SignDocDirect

Signs an unsigned transaction using SIGN\_MODE\_DIRECT.

| Field            | Description                                        | Type - Optional |
| ---------------- | -------------------------------------------------- | --------------- |
| `blockchainKind` | `Cosmos`                                           | String          |
| `kind`           | `SignDocDirect`                                    | String          |
| `signDoc`        | The hex encoded `SignDoc` Protobuf as shown below. | String          |

```json
{
  "blockchainKind": "Cosmos",
  "kind": "SignDocDirect",
  "signDoc": "0x0a89010a86010a1c2f636f736d6f732e62616e6b2e763162657461312e4d736753656e6412660a2b6f736d6f313366796d6d61797430727a72366d6130716439686a6d71747433706b37787737736871666b70122b6f736d6f313378396b70343573366e6539686a6738356b333867346d7a687067337435766a666c30356b6c1a0a0a05756f736d6f12013112640a4e0a460a1f2f636f736d6f732e63727970746f2e736563703235366b312e5075624b657912230a210286a1b7ce6ae2b5b33c47d0b6c91bb28e4cdb786d15f4497adcdaa69640ca0db512040a02080112120a0c0a05756f736d6f120337353010e0a7121a0b6f736d6f2d746573742d3520e39f06"
}
```

### Typescript Example with CosmJS SDK <a href="#typescript-example" id="typescript-example"></a>

First install the CosmJS SDK. You can find the full documentation here: [https://cosmos.github.io/cosmjs/](https://cosmos.github.io/cosmjs/)

Here a code sample to generate a signature via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
const client = await Comet38Client.create(new HttpClient(process.env.OSMOSIS_NODE_URL!))
const queryClient = QueryClient.withExtensions(cometClient, setupAuthExtension)

const body: TxBodyEncodeObject = {
  typeUrl: '/cosmos.tx.v1beta1.TxBody',
  value: {
    messages: [
      {
        typeUrl: '/cosmos.bank.v1beta1.MsgSend',
        value: {
          fromAddress: address,
          toAddress: to,
          amount: coins(1, 'uosmo'),
        },
      },
    ],
    memo,
  },
}

const account = await context.queryClient.auth.account(address)
const { sequence, accountNumber } = accountFromAny(account)

const authInfoBytes = makeAuthInfoBytes(
  [
    {
      pubkey: encodePubkey(encodeSecp256k1Pubkey(Buffer.from(publicKey, 'hex'))),
      sequence,
    },
  ],
  '100',
  Int53.fromString(fee.gas).toNumber(),
  undefined,
  undefined
)

const signDoc = makeSignDoc(context.registry.encode(body), authInfoBytes, 'osmo-test-5', accountNumber)

const res = await dfnsClient.wallets.generateSignature({
  walletId,
  body: {
    kind: 'SignDocDirect',
    signDoc: `0x${Buffer.from(makeSignBytes(doc)).toString('hex')}`,
  },
})
```
