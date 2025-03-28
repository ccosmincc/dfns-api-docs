# Create Fee Sponsor

`POST /fee-sponsors`

Creates a new `FeeSponsor` associated with a wallet. Returns a new fee sponsor entity.&#x20;

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

<table><thead><tr><th width="374">Name</th><th>Conditions</th></tr></thead><tbody><tr><td><code>FeeSponsors:Create</code></td><td>Always Required</td></tr></tbody></table>

## Request <a href="#request-body" id="request-body"></a>

<table><thead><tr><th width="203">Field</th><th>Description</th><th width="184">Type - Optional</th></tr></thead><tbody><tr><td><code>walletId</code></td><td>Id of the wallet that will be used to sponsor the fee for other wallets</td><td>String</td></tr></tbody></table>

### Example

```shell
{
  "walletId": "wa-759uq-tlr7k-9ljofi7pijl99tfg"
}
```

## Response <a href="#response" id="response"></a>

| Field         | Description                                                                             | Type - Optional |
| ------------- | --------------------------------------------------------------------------------------- | --------------- |
| `id`          | ID of the wallet.                                                                       | String          |
| `network`     | Network used for the wallet.                                                            | String          |
| `walletId`    | Id of the wallet that will be used to sponsor the fee for other wallets                 | String          |
| `status`      | `Active` or `Deactivated`                                                               | String          |
| `dateCreated` | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when wallet was created. | String          |

### 200 Success <a href="#response-example" id="response-example"></a>

```json
{
  "id": "fs-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "walletId": "wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
  "network": "Solana",
  "status": "Active",
  "dateCreated": "2023-04-14T20:41:28.715Z"
}
```
