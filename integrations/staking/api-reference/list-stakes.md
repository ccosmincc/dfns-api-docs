# List Stakes

`GET /staking/stakes`

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

### Query parameters <a href="#path-parameters" id="path-parameters"></a>

<table><thead><tr><th>Query string parameter</th><th width="196">Required/Optional</th><th>Description</th><th>Type</th></tr></thead><tbody><tr><td><code>limit</code></td><td>Optional</td><td>Maximum number of items to return. Default to 50.</td><td>Number</td></tr><tr><td><code>paginationToken</code></td><td>Optional</td><td>Opaque token used to retrieve the next page. Returned as <code>nextPageToken</code> from the previous request.</td><td>String</td></tr></tbody></table>

## Response <a href="#response" id="response"></a>

### 200 Response example <a href="#response-example" id="response-example"></a>

```json
{
  "items": [
    {
      "id": "stk-5q230-nl4b0-xxxxxxxxxxxxxxxx",
      "provider": "Figment",
      "providerStakeId": "1dd3b430-729e-4935-8da1-xxxxxxxxxxx",
      "walletId": "wa-fd328-9v5a8-xxxxxxxxxxxxxxxx",
      "protocol": "Babylon",
      "status": "Active",
      "requester": {
        "userId": "us-3htce-s75t3-xxxxxxxxxxxxxxxx",
        "tokenId": "to-72305-jh38s-xxxxxxxxxxxxxxxx",
        "appId": "ap-3g5ir-mt688-xxxxxxxxxxxxxxxx"
      },
      "requestBody": {
        "kind": "Native",
        "amount": "50000",
        "walletId": "wa-fd328-9v5a8-xxxxxxxxxxxxxxxx",
        "provider": "Figment",
        "protocol": "Babylon",
        "duration": 150
      },
      "dateCreated": "2024-11-27T19:05:33.551Z"
      "data": {
        "stakedObjectId": "0x4efd89d885701106f732b79b837fd2fe92a692da9f2291822dcef7fc59a1ec59",
        "expirationDate": "2025-04-19T06:49:04.793Z"
      }}
    }
  ],
  "nextPageToken": "eJyrVspMUbJSKi7J1jUtNDI20M3LMUky0LVIK7VMSkktS81LLTE2TMpUqgUABv8NBA"
}
```

In the `ListStake` response, a `data` field is included. This field contains detailed information about each stake. The structure and contents of `data` vary depending on the protocol used:

#### Iota Data example



```
{
  stakedObjects: [{
     id: string,
     amount: string,
     expirationDate?: string // For Timelock
  }],
  amount: string, // total amount
  validator: string,
}
```

{% hint style="info" %}


This new format will be available on the next release.\
Currently it's still:\
\
&#x20;   {

```
  stakedObjectId: string,
  amount: string, // total amount
  validator: string,
  expirationDate?: string // for timelocked
}
```
{% endhint %}

**Ethereum Data example**

```
{
  validators : [
    pubkey: string,
    withdrawalAddress: string
  ]
}
```



**Babylon Data example**

```
{
  finalityProviders: string[],
  covenantPubkeys: string[],
  magicBytes: string,
  covenantThreshold: number,
  minUnbondingTime: number,
  lockHeight: number,
}
```
