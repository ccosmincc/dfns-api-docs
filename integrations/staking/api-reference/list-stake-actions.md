# List Stake Actions

`GET /staking/stakes/:stakeId/actions`

Retrieves a list of exchanges.

{% hint style="info" %}
* Request headers required. See [Request Headers](../../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name          | Conditions      |
| ------------- | --------------- |
| `Stakes:Read` | Always Required |

## Parameters <a href="#parameters.1" id="parameters.1"></a>

### Path parameters <a href="#path-parameters" id="path-parameters"></a>

| Path parameter                              | Description                                   |
| ------------------------------------------- | --------------------------------------------- |
| `stakeId`<mark style="color:red;">\*</mark> | The stake Id for which we want to get rewards |

## Response <a href="#native-currency-request-body" id="native-currency-request-body"></a>

### Query parameters <a href="#path-parameters" id="path-parameters"></a>

<table><thead><tr><th>Query string parameter</th><th width="196">Required/Optional</th><th>Description</th><th>Type</th></tr></thead><tbody><tr><td><code>limit</code></td><td>Optional</td><td>Maximum number of items to return. Default to 50.</td><td>Number</td></tr><tr><td><code>paginationToken</code></td><td>Optional</td><td>Opaque token used to retrieve the next page. Returned as <code>nextPageToken</code> from the previous request.</td><td>String</td></tr></tbody></table>

## Response <a href="#response" id="response"></a>

### 200 Response example <a href="#response-example" id="response-example"></a>

```json
{
  "items": [
    {
      "id": "stka-6q2kv-838tf-8nbofcddhf3a13hu",
      "stakeId": "stk-5mgj6-kkogb-8fhplpfsnjik3u3m",
      "transactionId": "tx-3434j-tl8ou-94obe8il3bi1krcv",
      "kind": "Stake",
      "requester": {
        "userId": "us-gob8o-mm189-4hbgrpl4fjgcoqh",
        "tokenId": "to-2hebo-3ga68-9h09mhc1lborpnot",
        "appId": "ap-604pj-npt53-92np1nrefcqbc7hn"
      },
      "requestBody": {
        "walletId": "wa-2l3v7-kiba0-8gs9cj4m8eteph0u",
        "protocol": "Iota",
        "validator": "0x392316417a23198afeeb80d9fec314c65162ab5ad18f8a4c3375d31deab29670",
        "objectIds": [
          "0xd33ab1cfc6ace10d9b79461b299cfd1f658a56e7b94ad18976a4860c6bc60ac1"
        ]
      },
      "dateCreated": "2025-03-20T17:52:55.116Z"
      "data": {}
    }
  ],
  "nextPageToken": "eJyrVspMUbJSKi7J1jUtNDI20M3LMUky0LVIK7VMSkktS81LLTE2TMpUqgUABv8NBA"
}
```

### Remarks <a href="#response-example" id="response-example"></a>

In the stake action information, the data can give useful information on the staking transaction.

For example, for Iota, data shows the stakeObject Ids and the expiration (in case of vested staking)
