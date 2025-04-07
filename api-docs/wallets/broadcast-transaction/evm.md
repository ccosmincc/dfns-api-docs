# EVM

EVM chains like Ethereum, Polygon, Base, etc support the use of templates to broadcast transactions. Select the following `kind`:

* `Evm`: Use this template if you don't want to worry about gas parameters.
* `Eip1559`: Use this template to interact with chains that support the [EIP-1559 ](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md)gas standard.
* `Transaction`: broadcasts a fully serialized EVM transaction.

## Basic Template

<table data-full-width="false"><thead><tr><th>Field</th><th>Description</th><th>Type - Optional</th></tr></thead><tbody><tr><td><code>kind</code></td><td><code>Evm</code></td><td>String</td></tr><tr><td><code>to</code></td><td>Blockchain address of target contract or payee.</td><td>String</td></tr><tr><td><code>value</code></td><td>Amount of the native currency to transfer denominated in WEI.</td><td>String <em>(optional)</em></td></tr><tr><td><code>data</code></td><td>Encoded hex string indicating which function in the smart contract to call with which parameters. Can also be an entire encoded contract in the case of contract deployment.</td><td>String <em>(optional)</em></td></tr><tr><td><code>nonce</code></td><td>The transaction number to guarantee idempotency. If omitted, it will be provided automatically. Note the same nonce can be submitted multiple times with a higher <code>maxFeePerGas</code> to "overwrite" existing transactions in the mempool.</td><td>Integer <em>(optional)</em></td></tr><tr><td><code>externalId</code></td><td>A unique ID from your system. It can be leveraged to be used as an idempotency key (read more <a href="../../../advanced-topics/api-idempotency.md">here</a>).</td><td>String <em>(optional)</em></td></tr></tbody></table>

```shell
{
    "kind": "Evm",
    "to": "0x00fb58432ef9d418bf6688bcf0a226d2fcaa18e2",
    "data": "0x40d097c3000000000000000000000000d2f77f85a50cdd650ca562f3a180284e1d5b4934",
}
```

## EIP-1559 Template

Use this template to adjust the `maxFeePerGas` and `maxPriorityFeePerGas` of an [EIP-1559](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md) type-2 transaction. Keep in mind that not all EVM compatible chains support this standard.

<table data-full-width="false"><thead><tr><th>Field</th><th>Description</th><th>Type - Optional</th></tr></thead><tbody><tr><td><code>kind</code></td><td><code>Eip1559</code></td><td>String</td></tr><tr><td><code>to</code></td><td>Blockchain address of target contract or payee.</td><td>String</td></tr><tr><td><code>value</code></td><td>Amount of the native currency to transfer denominated in WEI.</td><td>String <em>(optional)</em></td></tr><tr><td><code>data</code></td><td>Encoded hex string indicating which function in the smart contract to call with which parameters. Can also be an entire encoded contract in the case of contract deployment.</td><td>String <em>(optional)</em></td></tr><tr><td><code>nonce</code></td><td>The transaction number to guarantee idempotency. If omitted, it will be provided automatically. Note the same nonce can be submitted multiple times with a higher <code>maxFeePerGas</code> to "overwrite" existing transactions in the mempool.</td><td>Integer <em>(optional)</em></td></tr><tr><td><code>gasLimit</code></td><td>The maximum amount of gas that can be spent for executing the transaction. If omitted, it will be calculated automatically.</td><td>String <em>(optional)</em></td></tr><tr><td><code>maxPriorityFeePerGas</code></td><td>The maximum amount of gas to be included as a tip to the validator. If omitted, it will be calculated automatically.</td><td>String <em>(optional)</em></td></tr><tr><td><code>maxFeePerGas</code></td><td>The maximum amount for gas willing to be paid for the transaction. If omitted, it will be calculated automatically.</td><td>String <em>(optional)</em></td></tr><tr><td><code>externalId</code></td><td>A unique ID from your system. It can be leveraged to be used as an idempotency key (read more <a href="../../../advanced-topics/api-idempotency.md">here</a>).</td><td>String <em>(optional)</em></td></tr></tbody></table>

```shell
{
    "kind": "Eip1559",
    "to": "0x00fb58432ef9d418bf6688bcf0a226d2fcaa18e2",
    "data": "0x40d097c3000000000000000000000000d2f77f85a50cdd650ca562f3a180284e1d5b4934",
    "maxFeePerGas": "1626000000000",
    "maxPriorityFeePerGas": "1332000000000"
}
```

## Transaction

Signs an unsigned transaction and broadcasts it to chain.

| Property      | Description                                                                                                                                         | Type - Optional     |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `kind`        | `Transaction`                                                                                                                                       | String              |
| `transaction` | The unsigned hex encoded transaction as shown below                                                                                                 | String              |
| `externalId`  | A unique ID from your system. It can be leveraged to be used as an idempotency key (read more [here](../../../advanced-topics/api-idempotency.md)). | String _(optional)_ |

```json
{
  "kind": "Transaction",
  "transaction": "0x02e783aa36a71503850d40e49def82520894e5a2ebc128e262ab1e3bd02bffbe16911adfbffb0180c0"
}
```

### Typescript Example with Ethers <a href="#typescript-example" id="typescript-example"></a>

First install the Ethers JS. You can find the full documentation here: [https://docs.ethers.org/v6/](https://docs.ethers.org/v6/)

Here a code sample to broadcast a transaction via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
import { parseUnits, Transaction } from 'ethers'

const walletId = 'wa-6lbfv-9esgj-xxxxxxxxxxxxxxxx'

const transaction = Transaction.from({
  to: '0xa238b6008Bc2FBd9E386A5d4784511980cE504Cd',
  value: '1',
  gasLimit: '21000',
  maxPriorityFeePerGas: parseUnits('5', 'gwei'),
  maxFeePerGas: parseUnits('20', 'gwei'),
  nonce: 3,
  type: 2,
  chainId: 11155111,
})

const res = await dfnsClient.wallets.broadcastTransaction({
  walletId,
  body: { kind: 'Transaction', transaction: transaction.unsignedSerialized },
})
```
