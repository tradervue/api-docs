Executions API
=================

Executions for a specific trade can be listed via the API.

Executions
-------------

### Listing executions

To list the executions for a particular trade, you will use

`GET /api/v1/trades/{id}/executions`

#### Request

There are no required parameters in the request. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://app.tradervue.com/api/v1/trades/659082/executions"
```

#### Response

The response will look like:

```json
{
  "executions": [
    {
      "id": 4480554,
      "datetime": "2013-10-21T09:59:02.000-04:00",
      "symbol": "HAS",
      "quantity": 100,
      "price": 51.51,
      "commission": 1,
      "trans_fee": 0,
      "ecn_fee": 0.3
    },
    {
      "id": 4480555,
      "datetime": "2013-10-21T09:59:38.000-04:00",
      "symbol": "HAS",
      "quantity": -100,
      "price": 51.31,
      "commission": 1,
      "trans_fee": 0.1,
      "ecn_fee": 0.3
    }
  ]
}
```

Executions will be sorted by execution time, from oldest to newest.
