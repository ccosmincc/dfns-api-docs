# List Fee Sponsors

`GET /fee-sponsors`

Retrieves a lit of fee sponsors.

{% hint style="info" %}
* Request headers required. See [Request Headers](../../getting-started/request-headers.md) for more information.
* Authentication required. See [Authentication Headers](../../getting-started/request-headers.md#authentication-headers) for more information.
{% endhint %}

## Required Permissions

| Name               | Conditions      |
| ------------------ | --------------- |
| `FeeSponsors:Read` | Always Required |

## Response <a href="#response" id="response"></a>

| Field           | Description                                                                           | Type - Optional                                                  |
| --------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| `items`         | List of fee sponsors.                                                                 | See [Create Fee Sponsor response](create-fee-sponsor/#response). |
| `nextPageToken` | Opaque token used to retrieve the next page of items. `undefined` if end of the list. | String _(optional)_                                              |

### 200 Success <a href="#response-example" id="response-example"></a>

```json
{
  "items": [
    {
      "id": "fs-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
      "walletId": "wa-1f04s-lqc9q-xxxxxxxxxxxxxxxx",
      "network": "Solana",
      "status": "Active",
      "dateCreated": "2023-04-14T20:41:28.715Z"
    }
  ]
}
```
