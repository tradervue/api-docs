Comments API
=================

Comments for a specific trade, journal entry, or journal note can be listed via the API.

Comments
-------------

### Listing comments for trades

To list the comments for a particular trade, you will use

`GET /api/v1/trades/{id}/comments`

#### Request

There are no required parameters in the request. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://www.tradervue.com/api/v1/trades/421249/comments"
```

### Listing comments for journal entries

To list the comments for a particular journal entry, you will use

`GET /api/v1/journal/{id}/comments`

#### Request

There are no required parameters in the request. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://www.tradervue.com/api/v1/journal/11473/comments"
```

### Listing comments for journal notes

To list the comments for a particular journal note, you will use

`GET /api/v1/notes/{id}/comments`

#### Request

There are no required parameters in the request. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://www.tradervue.com/api/v1/notes/114712/comments"
```

### Response

The response will look like:

```json
{
  "comments": [
    {
      "id": 89,
      "username": "test1",
      "text": "Nice. Lorem ipsum dolor sit amet.",
      "datetime": "2014-06-04T21:49:56.265Z"
    },
    {
      "id": 90,
      "username": "example",
      "text": "Thanks for the feedback!",
      "datetime": "2014-06-04T21:50:17.227Z"
    }
  ]
}
```

Comments will be sorted by the time the comment was made, from oldest to newest.
