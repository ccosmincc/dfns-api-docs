# Deactivate Fee Sponsor

`PUT /fee-sponsors/{feeSponsorId}/deactivate`

Deactivate a Fee Sponsor: The fee sponsor won't be able to be used anymore when making a transfer. 

{% hint style="info" %}
* User action signature required. See [User Action Signing](../authentication/user-action-signing/) for more information.
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name                 | Conditions      |
| -------------------- | --------------- |
| `FeeSponsors:Update` | Always Required |

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
  "status": "Deactived",
  "dateCreated": "2023-04-14T20:41:28.715Z"
}
```
