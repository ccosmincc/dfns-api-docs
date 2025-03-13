# get Rewards

`GET /staking/stakes/:stakeId/rewards`

Retrieves the rewards linked to a specific stake

{% hint style="info" %}
* Request headers required. See [Request Headers](../../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name          | Conditions      |
| ------------- | --------------- |
| `Stakes:Read` | Always Required |

## Path Parameters <a href="#parameters.1" id="parameters.1"></a>

<table><thead><tr><th>Query string parameter</th><th width="196">Required/Optional</th><th>Description</th><th>Type</th></tr></thead><tbody><tr><td><code>stakeId</code><mark style="color:red;">*</mark></td><td>Optional</td><td>The id of the stake for which we want to get rewards</td><td>string</td></tr></tbody></table>



## Response <a href="#response" id="response"></a>

### 200 Response example <a href="#response-example" id="response-example"></a>

```json
{
  symbol: "ETH",
  balance: 0.1324
}
```
