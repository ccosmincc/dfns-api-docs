# TRON

TRON supports the following signature `kinds`:

* `Transaction`: unsigned transaction.
* `Message`: an arbitrary message.

## Transaction <a href="#transaction-request-body" id="transaction-request-body"></a>

| Field            | Description                                          | Type - Optional |
| ---------------- | ---------------------------------------------------- | --------------- |
| `blockchainKind` | `Tron`                                               | String          |
| `kind`           | `Transaction`                                        | String          |
| `transaction`    | The unsigned hex encoded transaction as shown below. | String          |

```json
{
  "blockchainKind": "Tron",
  "kind": "Transaction",
  "transaction": "0x0a83010a0228222208b142ad939b228d784090a7eaa9cf315a65080112610a2d747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e5472616e73666572436f6e747261637412300a15419d31b91d72b58d7c8c02a7124410e168989f372d12154102a69d5d85c05864dc6fd74f57db3fa37aff7b94180170b0d2e6a9cf31"
}
```

### Typescript Example with TronWeb

First install Tronweb. You can find the full documentation here: [https://tronweb.network/docu/docs/intro/](https://tronweb.network/docu/docs/intro/)

Tron requires the transaction to be serialized using the protobuf format before it can be broadcast. As it's not trivial, you can use the functions exposed in Tronweb to generate the transaction in the right format (see below) and then sign via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
const TronWeb = require('tronweb')

const walletId = 'wa-6lbfv-9esgj-xxxxxxxxxxxxxxxx'
const wallet = await dfnsClient.wallets.getWallet({ walletId })

const transaction = await tronWeb.transactionBuilder.sendTrx('TADDx31pdCFfp3XrYxp6fQGbRxriYFLTrx', 1000, wallet.address)

const txPb = TronWeb.utils.transaction.txJsonToPb(transaction)

const res = await dfnsClient.wallets.generateSignature({
  walletId,
  body: {
    kind: 'Transaction',
    transaction: `0x${TronWeb.utils.bytes.byteArray2hexStr(txPb.serializeBinary())}`,
  },
})
```

## Message <a href="#message-request-body" id="message-request-body"></a>

Signs an arbitrary hex encoded message.

| Field            | Description                       | Type - Optional |
| ---------------- | --------------------------------- | --------------- |
| `blockchainKind` | `Tron`                            | String          |
| `kind`           | `Message`                         | String          |
| `message`        | An arbitrary hex encoded message. | String          |

```shell
{
  "blockchainKind": "Tron",
  "kind": "Message",
  "message": "0x49206c6f76652044666e73"
}
```
