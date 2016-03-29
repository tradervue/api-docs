Trades API
==========

Trades can be listed, created, updated, and deleted via the API.

Trades
------

### Listing trades

To list the trades, you will use

`GET /api/v1/trades`

#### Request

There are no required parameters in the request. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://www.tradervue.com/api/v1/trades"
```

#### Response

The response will look like:

```json
{
  "trades": [
    {
      "id": 643888,
      "symbol": "BBY",
      "volume": 160,
      "open": false,
      "gross_pl": -5.592,
      "native_pl": -5.592,
      "native_currency": "USD",
      "commission": 3.0,
      "fees": 0.06,
      "side": "long",
      "entry_price": 40.75,
      "exit_price": 40.62,
      "start_datetime": "2013-10-15T13:07:22.000-04:00",
      "end_datetime": "2013-10-15T15:59:16.000-04:00",
      "notes_excerpt": "Second try on pullback trade here, using 15m chart.\r\n\r\nThis one really started to roll around...",
      "tags": [
        "pb15",
        "pullback",
        "try2"
      ],
      "exec_count": 3,
      "comment_count": 0,
      "shared": false
    },
    {
      "id": 643749,
      "symbol": "BBY",
      "volume": 200,
      "open": false,
      "gross_pl": -29.0,
      "native_pl": -29.0,
      "native_currency": "USD",
      "commission": 2.0,
      "fees": 0.54,
      "side": "long",
      "entry_price": 41.1,
      "exit_price": 40.81,
      "start_datetime": "2013-10-15T11:07:32.000-04:00",
      "end_datetime": "2013-10-15T11:37:14.000-04:00",
      "notes_excerpt": "Strong opening drive, then a lower volume pullback.  Entered as a pullback trade on the 15-min as...",
      "tags": [
        "pb15",
        "pullback",
        "try1"
      ],
      "exec_count": 2,
      "comment_count": 0,
      "shared": false
    }
  ]
}
```

Trade results will be sorted with the newest trade first, by trade open date/time. For trades with no executions,
which have no open time, the creation time of the trade will be used instead.

The exit_price field will be omitted if the trade is open.

While there are no required parameters to retrieve the list of trades, there are several optional parameters you can use:

Parameter | Description |
--------- | ----------- |
count     | Specifies how many trades you want back at a time. Defaults to 25, max is 100. |
page      | Specifies which page of results you want. For example, if you want results 26-50, use count=25&page=2. |
symbol    | Filter results to a specific symbol, e.g. symbol=AAPL |
tag       | Filter results to trades with a specific tag, e.g. tag=MyTag |
startdate | Filter results to trades opened on or after the start date, formatted as mm/dd/yyyy (URL-encode the slashes in your request) |
enddate   | Filter results to trades opened on or before the enddate date, formatted as mm/dd/yyyy (URL-encode the slashes in your request) |
side      | Filter results to long or short trades only. Use side=L for long, and side=S for short. |
duration  | Filter results by intraday or multiday trades. Use duration=I for intraday, and duration=M for multi-day trades. |

There are a number of other filter options that can be used; if you build a filter on the Tradervue web site using the drill-down
interactive reports, and examine the URL immediately after narrowing the filter, you will see some of the available filter parameters.

### Displaying a single trade

You can retrieve details about a particular trade by using 

`GET /api/v1/trades/{id}`

#### Request

The trade's ID is part of the request URL. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://www.tradervue.com/api/v1/trades/643888"
```

#### Response

The response will look like:

```json
{
  "id": 643888,
  "symbol": "BBY",
  "volume": 160,
  "open": false,
  "gross_pl": -5.592,
  "native_pl": -5.592,
  "native_currency": "USD",
  "commission": 3.0,
  "fees": 0.06,
  "side": "long",
  "entry_price": 40.75,
  "exit_price": 40.62,
  "start_datetime": "2013-10-15T13:07:22.000-04:00",
  "end_datetime": "2013-10-15T15:59:16.000-04:00",
  "exec_count": 3,
  "comment_count": 0,
  "shared": false,
  "notes": "Second try on pullback trade here, using 15m chart.\r\n\r\nThis one really started to roll around 2pm, when the market was rolling over at the same time. About 3:30, things weren't really looking up for either BBY or the market, so I decided to sell half at roughly break-even; holding the remainder to either targets or market close (don't want to hold overnight with the government shutdown still unresolved).",
  "tags": [
    "pb15",
    "pullback",
    "try2"
  ],
  "initial_risk": 20.0,
  "position_mfe": 43.2,
  "position_mfe_datetime": "2013-10-15T14:00:00.000-04:00",
  "position_mae": -17.6,
  "position_mae_datetime": "2013-10-15T14:48:00.000-04:00",
  "price_mfe": 0.54,
  "price_mfe_datetime": "2013-10-15T14:00:00.000-04:00",
  "price_mae": -0.22,
  "price_mae_datetime": "2013-10-15T14:48:00.000-04:00",
  "best_exit_pl": 1.204,
  "best_exit_pl_datetime": "2013-10-15T15:36:00.000-04:00"
}
```

Note that the exit_price field will be omitted if the trade is open.

### Updating a trade

To update a trade, you will use

`PUT /api/v1/trades/{id}`

You can update the following fields for a trade:

- notes
- shared
- initial risk
- tags

#### Request

The trade's ID is part of the request URL. From the command line, you can use the following curl command:

```
curl -i \
  -X PUT \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -H "Content-type: application/json" \
  -u example:password \
  -d '{"notes":"here are my notes"}' \
  "https://www.tradervue.com/api/v1/trades/659080"
```

A sample JSON request looks like this:

```json
{
  "notes": "here are my notes",
  "shared": true,
  "initial_risk": 15.00,
  "tags": [
    "one",
    "two",
    "three"
  ]
}
```

Only the fields you wish to update need to be included; all other fields will be left unmodified.

#### Response

If successful, you will get a HTTP 200 response and an empty body.

If the trade wasn't found, you will get a HTTP 404.

If the update failed, you will get a HTTP 400 with a JSON response like

```json
{
  "error": "error details here, here, here"
}
```

### Creating a trade

To create a new trade, you will use

`POST /api/v1/trades`

This will create a new trade with no executions, similar to what happens when you click "New Trade" on the Tradervue
site and fill in the form there.

#### Request

From the command line, you can use the following curl command:

```
curl -i \
  -X POST \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -H "Content-type: application/json" \
  -u example:password \
  -d '{"symbol":"AAAA","notes":"foo","initial_risk":25.0,"shared":false,"tags":["tag1","tag2"]}' \
  "https://www.tradervue.com/api/v1/trades"
```

A sample JSON request looks like this:

```json
{
  "symbol": "AAAA",
  "notes": "foo",
  "initial_risk": 25.0,
  "shared": false,
  "tags": [
    "tag1",
    "tag2"
  ]
}
```

The symbol field is required; all other fields are optional.

#### Response

If successful, you will get a HTTP 201 response with a Location header containing the new trade's URL:

```
Location: https://www.tradervue.com/api/v1/trades/12345
```

The JSON response will include the new trade's ID:

```json
{
  "id":12345
}
```

If the trade could not be created, you will get a HTTP 400 with a JSON response like

```json
{
  "error": "error details here, here, here"
}
```

### Deleting a trade

To delete a trade, you will use

`DELETE /api/v1/trades/{id}`

This will delete the trade, including any executions that make up the trade, and any comments on the trade.

#### Request

From the command line, you can use the following curl command:

```
curl -i \
  -X DELETE \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -H "Content-type: application/json" \
  -u example:password \
  "https://www.tradervue.com/api/v1/trades/46525"
```
