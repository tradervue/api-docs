Journal API
==========

Journal entries can be listed, created, updated, and deleted via the API.

Journal Entries
---------------

### Listing journal entries

To list the entries, you will use

`GET /api/v1/journal`

#### Request

There are no required parameters in the request. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://www.tradervue.com/api/v1/journal"
```

#### Response

The response will look like:

```json
{
  "journal_entries": [
    {
      "id": 10680,
      "date": "2014-01-08",
      "notes": "",
      "comment_count": 0
    },
    {
      "id": 10118,
      "date": "2013-12-13",
      "notes": "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
      "comment_count": 3
    }
  ]
}
```

Journal entries will be sorted with the newest entry, based on the date for the entry.

While there are no required parameters to retrieve the list of entries, there are several optional parameters you can use:

Parameter | Description |
--------- | ----------- |
count     | Specifies how many entries you want back at a time. Defaults to 25, max is 100. |
page      | Specifies which page of results you want. For example, if you want results 26-50, use count=25&page=2. |
d         | Specifies a particular date to retrieve an entry for, formatted as mm/dd/yyyy (URL-encode the slashes in your request). This is effectively a shortcut for specifying a startdate and enddate of the same date. |
startdate | Filters results to entries on or after the start date, formatted as mm/dd/yyyy (URL-encode the slashes in your request) |
enddate   | Filters results to entries on or before the end date, formatted as mm/dd/yyyy (URL-encode the slashes in your request) |

### Displaying a single journal entry

You can retrieve details about a particular entry by using 

`GET /api/v1/journal/{id}`

#### Request

The entry's ID is part of the request URL. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://www.tradervue.com/api/v1/journal/12274"
```

#### Response

The response will look like:

```json
{
  "id": 9300,
  "date": "2013-09-06",
  "notes": "Not a bad trading day; however, I need to be more careful on the open.",
  "comment_count": 0,
  "trade_count": 2,
  "total_volume": 2400,
  "gross_pl": -1.0,
  "commfees": 20.57,
  "trade_ids": [
    35170,
    35176
  ]
}
```

The `trade_ids` field will be an array of the id's of all of the trades that were completed, opened, 
closed, or adjusted on that date. Details about the trades can be retrieved with the [Trades API](trades.md).

### Updating a journal entry

To update an entry, you will use

`PUT /api/v1/journal/{id}`

You can update the following fields for a journal entry:

- notes

#### Request

The entry's ID is part of the request URL. From the command line, you can use the following curl command:

```
curl -i \
  -X PUT \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -H "Content-type: application/json" \
  -u example:password \
  -d '{"notes":"here are my updated notes"}' \
  "https://www.tradervue.com/api/v1/journal/12274"
```

A sample JSON request looks like this:

```json
{
  "notes": "here are my updated notes"
}
```

#### Response

If successful, you will get a HTTP 200 response and an empty body.

If the entry wasn't found, you will get a HTTP 404.

If the update failed, you will get a HTTP 400 with a JSON response like

```json
{
  "error": "error details here, here, here"
}
```

### Creating a journal entry

To create a new entry, you will use

`POST /api/v1/journal`

This will create a new journal entry, on a specific date, similar to what happens when you click "Create" on the Journal page on the Tradervue
site.

#### Request

From the command line, you can use the following curl command:

```
curl -i \
  -X POST \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -H "Content-type: application/json" \
  -u example:password \
  -d '{"date":"2015-12-05", "notes":"My notes for the day."}' \
  "https://www.tradervue.com/api/v1/journal"
```

A sample JSON request looks like this:

```json
{
  "date": "2015-12-05",
  "notes": "My notes for the day."
}
```

The date field is required, and must be in yyyy-mm-dd format; all other fields are optional.

#### Response

If successful, you will get a HTTP 201 response with a Location header containing the new entry's URL:

```
Location: https://www.tradervue.com/api/v1/journal/13944
```

The JSON response will include the new entry's ID:

```json
{
  "id":13944
}
```

If the entry could not be created, you will get a HTTP 400 with a JSON response like

```json
{
  "error": "error details here, here, here"
}
```

### Deleting a journal entry

To delete a journal entry, you will use

`DELETE /api/v1/journal/{id}`

This will delete the entry, if there are no trades completed on that date.

#### Request

From the command line, you can use the following curl command:

```
  curl -i \
  -X DELETE \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -H "Content-type: application/json" \
  -u example:password \
  "https://www.tradervue.com/api/v1/journal/62245"
```

If the journal entry could not be deleted, because there were trades completed on that date, you will get a HTTP 400 with a JSON response like

```json
{
  "error": "error details here, here, here"
}
```