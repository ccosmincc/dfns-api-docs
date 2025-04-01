# List Sponsored Fees

`GET /fee-sponsors/{feeSponsorId}/fees`

Retrieves the list of all fees paid by a fee sponsor.

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

| Field           | Description                                                                           | Type - Optional                      |
| --------------- | ------------------------------------------------------------------------------------- | ------------------------------------ |
| `items`         | List of sponsored fees.                                                               | See [Sponsored Fee](#sponsored-fee). |
| `nextPageToken` | Opaque token used to retrieve the next page of items. `undefined` if end of the list. | String _(optional)_                  |

### Sponsored Fee <a href="#sponsored-fee" id="sponsored-fee"></a>

| Field           | Description                                                                                    | Type - Optional |
| --------------- | ---------------------------------------------------------------------------------------------- | --------------- |
| `id`            | ID of the sponsored fee.                                                                       | String          |
| `sponsoreeId`   | Id of the entity being sponsored, e.g. `walletId`.                                             | String          |
| `requestId`     | Id of the request that was sponsored                                                           | String          |
| `status`        | `Pending` or `Confirmed`                                                                       | String          |
| `fee   `        | Fee amount that was paid for the request                                                       | String          |
| `dateRequested` | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the request was created.   | String          |
| `dateConfirmed` | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) date string when the request was confirmed. | String          |

### 200 Success <a href="#response-example" id="response-example"></a>

```json
{
  "items": [
    {
      "id": "sf-xxxxx-xxxxx-xxxxxxxxxxxxxxx",
      "sponsoreeId": "wa-xxxxx-xxxxx-xxxxxxxxxxxxxxx",
      "requestId": "xfr-xxxxx-xxxxx-xxxxxxxxxxxxxxx",
      "status": "Confirmed",
      "fee": "10050",
      "dateRequested": "2025-03-26T13:55:53.218Z",
      "dateConfirmed": "2025-03-26T13:55:54.000Z"
    }
  ]
}
```
