# Estimate fees

`GET /networks/fees?network={network}`

Gets real-time fee details for a given network, allowing users to make decisions based on their preferences for transaction speed/priority. Three levels of priority will be displayed: `slow`, `standard`, `fast`.

{% hint style="info" %}
Note: Get Fee only works on EVM chains currently.  We will add support for additional L1s incrementally.&#x20;
{% endhint %}

{% hint style="info" %}
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

{% hint style="warning" %}
**only supported for EVM chains are supported, for now**
{% endhint %}

## Required Permissions

No permission is required for this call

## Parameters <a href="#parameters.1" id="parameters.1"></a>

### Query parameters <a href="#path-parameters" id="path-parameters"></a>

<table><thead><tr><th width="248">Query parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>network</code></td><td>Enumerated type representing the Blockchain network from the list found <a href="https://docs.dfns.co/dfns-docs/api-docs/wallets#supported-networks">here</a>.  <strong>⚠️ only EVM chains are supported for now</strong>.</td></tr></tbody></table>

## Response <a href="#response" id="response"></a>

### Response example <a href="#response-example" id="response-example"></a>

```json
{
    "kind": "Eip1559",
    "network": "Ethereum",
    "estimatedBaseFee": "1594798936",
    "blockNumber": 7838295,
    "slow": {
        "maxPriorityFeePerGas": "308373420",
        "maxFeePerGas": "3497971292"
    },
    "standard": {
        "maxPriorityFeePerGas": "1006011766",
        "maxFeePerGas": "4195609638"
    },
    "fast": {
        "maxPriorityFeePerGas": "1809702937",
        "maxFeePerGas": "4999300809"
    }
}
```



## Strategies <a href="#response" id="response"></a>

### EIP-1559 <a href="#response-example" id="response-example"></a>

For EIP 1559, we  provide 2 different fields for each strategy: maxFee (per Gas) and maxPriorityFee (per Gas). To compute these estimations, we look at the block history, compute three different percentiles for the rewards offered by the transactions in the blocks, and then calculate the average for each strategy.



