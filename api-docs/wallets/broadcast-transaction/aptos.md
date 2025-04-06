# Aptos

## Transaction <a href="#transaction-request-body" id="transaction-request-body"></a>

Signs an unsigned transaction and broadcasts it to chain.

| Field         | Description                                                                                                                                         | Type - Optional     |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `kind`        | `Transaction`                                                                                                                                       | String              |
| `transaction` | The unsigned hex encoded transaction as shown below.                                                                                                | String              |
| `externalId`  | A unique ID from your system. It can be leveraged to be used as an idempotency key (read more [here](../../../advanced-topics/api-idempotency.md)). | String _(optional)_ |

```shell
{
  "kind": "Transaction",
  "transaction": "0xc5510ac408dcb78034b4fdcdaea6582c10f2e0d03dc684d32f9457b9d778e113020000000000000002000000000000000000000000000000000000000000000000000000000000000104636f696e087472616e73666572010700000000000000000000000000000000000000000000000000000000000000010a6170746f735f636f696e094170746f73436f696e0002205bdc24cb9033286ffe19f436145b9e2267dd03b0fd0d422459d381a6431d39ba080100000000000000400d0300000000006400000000000000cce1f2670000000002"
}
```

### Typescript Example with Aptos SDK

First install the Aptos Typescript SDK. You can find the full documentation here: [https://aptos.dev/en/build/sdks/ts-sdk](https://aptos.dev/en/build/sdks/ts-sdk)

Here a code sample to broadcast a transaction via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

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

const transaction = await client.transaction.build.simple({
    sender: wallet.address,
    data: {
      function: '0x1::coin::transfer',
      typeArguments: [APTOS_COIN],
      functionArguments: [
        "0x5bdc24cb9033286ffe19f436145b9e2267dd03b0fd0d422459d381a6431d39ba",
        "1",
      ],
    },
  })

const res = await dfnsClient.wallets.broadcastTransaction({
  walletId,
  body: {
    kind: 'Transaction',
    transaction: transaction.bcsToHex().toString(),
  },
})
```
