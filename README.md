Tradervue API
=============

The Tradervue API is a REST-style API for [Tradervue](http://www.tradervue.com)
that uses JSON for serialization, and HTTP Basic authentication over 
SSL. The API is a work in progress; currently, the only endpoints available are for importing trade data.

Refer to the [change log](https://github.com/tradervue/api-docs/blob/master/CHANGELOG.md) for the history of API changes.

Making a request
----------------

All URLs start with `https://www.tradervue.com/api/v1`, and are accessible via SSL only.

Authentication
--------------

You use HTTP Basic authentication to authenticate with the Tradervue API.

Identify your app
-----------------

You must include a `User-Agent` header that identifies your application, including either a link
to your application or your email address, with each request. This allows us to get in touch with
you in case there is a problem (so we can warn you before you're blacklisted), or if we want to
talk to you about your app!  Something like:

```
User-Agent: TradeImporter (http://my-tradeimporter.com)
```

or

```
User-Agent: MyApp (yourname@example.com)
```

If you don't supply this header, you will get a HTTP 400 Bad Request response.

Samples
-------

- [Ruby sample application](https://github.com/tradervue/ruby-sample) which uses the Import API.
- [C# sample applications](https://github.com/tradervue/dotnet-sample) which use the Import API.
- Command line examples with curl are shown inline below.

Imports
-------

Importing trade data in Tradervue is an asynchronous operation, due to the potentially large amount of
data being processed and the time it might take. You make one API call to upload trade data and 
create a new import; you then make
another call to retrieve the status of that import. 

No new imports can be created until the previous import has completed; you can tell when this happens
by retrieving the status of the prior import. You are not required to retrieve the status, but it is
the only way to know if the prior import encountered errors or limits.

### Query import status

To read the import status from the previous import, you will use

`GET /api/v1/imports`

Querying the import status will clear it and reset it to "ready", unless the status indicates there 
is an import queued or processing. This call is not idempotent.

#### Request

There are no parameters in the request. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  https://www.tradervue.com/api/v1/imports
```

#### Response

If there was no prior import with unread status, the response will look like:

```json
{
  "status": "ready"
}
```

If there is currently an import in progress, the response will look like:

```json
{
  "status": "queued"
}
```

or

```json
{
  "status": "processing"
}
```

If the last import is complete and was successful, the response will look like:

```json
{
  "status": "succeeded",
  "info": {
    "exec_count": 2,
    "duplicate_count": 0,
    "overquota_count": 0,
    "skipped_futures": false,
    "skipped_options": false
  }
}
```

If an error occurred in the last import, the response will look like:

```json
{
  "status": "failed",
  "info": {
    "exec_count": 0,
    "duplicate_count": 0,
    "overquota_count": 0,
    "skipped_futures": false,
    "skipped_options": false,
    "error_description": "at least one data row had an invalid quantity",
    "error_execnumber": 1
  }
}
```

The `error_execnumber` will be the 1-based index of the execution in the import data where the
error occurred.

### Creating a new import

To import trade data into Tradervue, you will create a new import. The endpoint you will use is

`POST /api/v1/imports`

#### Request

This call expects a JSON payload with the execution data you wish to import, along with the
specific options you wish to use for the import.

Here is an example command line to import one execution:

```
curl -i \
  -X POST \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -H "Content-type: application/json" \
  -u example:password \
  -d '{"allow_duplicates":"false","overlay_commissions":"false","tags":["one","two"],"account_tag":"swing","executions":[{"datetime":"2013-02-7T09:53:34-05:00","symbol":"SPY","quantity":"100","price":"151.05","option":"","commission":"1.00","transfee":"0.04","ecnfee":"0.21"}]}' \
  https://www.tradervue.com/api/v1/imports
```

A sample JSON request looks like this:

```json
{
  "allow_duplicates": "false",
  "overlay_commissions": "false",
  "tags": ["one", "two"],
  "account_tag": "swing",
  "executions": [
    {
      "datetime": "2013-02-07T09:53:34-05:00",
      "symbol": "SPY",
      "quantity": "100",
      "price": "143.74",
      "option": "",
      "commission": "1.00",
      "transfee": "0.04",
      "ecnfee": "0.21"
    },
    {
      "datetime": "2013-02-07T09:54:01-05:00",
      "symbol": "SPY",
      "quantity": "-100",
      "price": "143.80",
      "option": "",
      "commission": "1.00",
      "transfee": "0.17",
      "ecnfee": "0.21"
    }
  ]
}
```

The datetime for each execution should be in ISO 8601 format (as shown above), but most date/time formats
supported by the Tradervue [generic import format](http://www.tradervue.com/help/generic) should work if 
combined into a single field (e.g. 2012-07-02 09:35:02).

The quantity for each execution should be positive for a buy, and negative for a sell.

For more details on each execution-related parameter, refer to the 
[Generic Import Format documentation](http://www.tradervue.com/help/generic).

The other fields in the request are:

- `allow_duplicates` - set this to true if you wish to disable Tradervue's automatic duplicate-detection
when importing this data.

- `overlay_commissions` - runs this import in commission-overlay mode; no new trades will be created, and
existing trades will be updated with commission and fee data. See the Tradervue 
[help article](http://www.tradervue.com/help/older_commissions) for more details.

- `tags` - this is an array of tags you want applied to each new trade created in this import.

- `account_tag` - allows you to specify an account tag to be used for the import; see
[Account Tags](http://www.tradervue.com/help/account_tags) for more details.

Some features may not be functional depending on the subscription of the user; the same restrictions found
in the Tradervue application are also enforced when using the API.

#### Response

The response will either be success, with a HTTP 200 response and

```json
{
  "status": "queued"
}
```

Or a failure, with a HTTP 4xx response and one of the following:

```json
{
  "status": "no executions"
}
```

or

```json
{
  "error": "Invalid request: error details here, here, here"
}
```

If an import is already in progress and not yet completed, the new
import creation will fail with a HTTP 424:

```json
{
  "error": "import already in progress"
}
```
