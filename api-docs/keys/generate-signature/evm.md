# EVM

EVM chains like Ethereum, Polygon, Base etc support the following signature `kinds`:

* `Transaction`: unsigned transaction.
* `Message`: an arbitrary message.
* `Eip712`: typed structured data defined in [EIP-712](https://eips.ethereum.org/EIPS/eip-712).

## Transaction <a href="#transaction-request-body" id="transaction-request-body"></a>

Signs an unsigned transaction.

| Field            | Description                                          | Type - Optional |
| ---------------- | ---------------------------------------------------- | --------------- |
| `blockchainKind` | `Evm`                                                | String          |
| `kind`           | `Transaction`                                        | String          |
| `transaction`    | The unsigned hex encoded transaction as shown below. | String          |

```shell
{
  "blockchainKind": "Evm",
  "kind": "Transaction",
  "message": "0x02e783aa36a71503850d40e49def82520894e5a2ebc128e262ab1e3bd02bffbe16911adfbffb0180c0"
}
```

### Typescript Example with EthersJs

First install the Ethers JS. You can find the full documentation here: [https://docs.ethers.org/v6/](https://docs.ethers.org/v6/)

Here a code sample to generate a signature via [the Dfns TypeScript SDK](https://github.com/dfns/dfns-sdk-ts):

```typescript
import { parseUnits, Transaction } from 'ethers'

const walletId = 'wa-6lbfv-9esgj-88s80c0qsih0a393'

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

const res = await dfnsClient.wallets.generateSignature({
  walletId,
  body: { kind: 'Transaction', transaction: transaction.unsignedSerialized },
})
```

## Message <a href="#message-request-body" id="message-request-body"></a>

Signs an arbitrary hex encoded message.

| Field            | Description                       | Type - Optional |
| ---------------- | --------------------------------- | --------------- |
| `blockchainKind` | `Evm`                             | String          |
| `kind`           | `Message`                         | String          |
| `message`        | An arbitrary hex encoded message. | String          |

```shell
{
  "blockchainKind": "Evm",
  "kind": "Message",
  "message": "0x49206c6f76652044666e73"
}
```

## EIP-712 TypedData <a href="#eip712-request-body" id="eip712-request-body"></a>

Signs a typed structured data defined in [EIP-712](https://eips.ethereum.org/EIPS/eip-712).

| Field            | Description                 | Type - Optional                 |
| ---------------- | --------------------------- | ------------------------------- |
| `blockchainKind` | `Evm`                       | String                          |
| `kind`           | `Eip712`                    | String                          |
| `types`          | Type definitions.           | Map\<String, TypedDataField\[]> |
| `domain`         | Domain separator.           | Eip712Domain                    |
| `message`        | Structured message to sign. | Object                          |

**TypedDataField**

| Field  | Description | Type - Optional |
| ------ | ----------- | --------------- |
| `name` | Field name. | String          |
| `type` | Field type. | String          |

**Eip712Domain**

| Field               | Description                                                 | Type - Optional |
| ------------------- | ----------------------------------------------------------- | --------------- |
| `name`              | Name of the signing domain.                                 | String          |
| `version`           | Current major version of the signing domain.                | String          |
| `chainId`           | Chain ID.                                                   | Integer         |
| `verifyingContract` | The address of the contract that will verify the signature. | String          |
| `salt`              | 32-byte value as a last-resort domain separator.            | String          |

```shell
{
  "blockchainKind": "Evm",
  "kind": "Eip712",
  "types": {
    "Person": [
      { "name": "name", "type": "string" },
      { "name": "wallet", "type": "address" }
    ],
    "Mail": [
      { "name": "from", "type": "Person" },
      { "name": "to", "type": "Person" },
      { "name": "contents", "type": "string" }
    ]
  },
  "domain": {
    "name": "Ether Mail",
    "version": "1",
    "chainId": 1,
    "verifyingContract": "0x1b352de7a926ebd1bf52194dab487c2cb0793a9b",
    "salt": "0xf2d857f4a3edcb9b78b4d503bfe733db1e3f6cdc2b7971ee739626c97e86a558"
  },
  "message": {
    "from": {
      "name": "Chris",
      "wallet": "0x00e3495cf6af59008f22ffaf32d4c92ac33dac47"
    },
    "to": {
      "name": "Bob",
      "wallet": "0xcc0ee1a1c5e788b61916c8f1c96c960f9a9d3db7"
    },
    "contents": "Hello, Bob!"
  }
}
```
