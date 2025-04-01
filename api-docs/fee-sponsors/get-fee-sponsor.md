# Get Fee Sponsor

`GET /fee-sponsors/{feeSponsorId}`

Retrieves a Fee Sponsor by its ID.

{% hint style="info" %}
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name               | Conditions      |
| ------------------ | --------------- |
| `FeeSponsors:Read` | Always Required |

## Parameters <a href="#request-example.1" id="request-example.1"></a>

### Path parameters <a href="#path-parameters" id="path-parameters"></a>

| Path parameter | Description            |
| -------------- | ---------------------- |
| `feeSponsorId` | ID of the fee sponsor. |

## Response <a href="#response" id="response"></a>

See [Create Fee Sponsor response](create-fee-sponsor/#response).

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
