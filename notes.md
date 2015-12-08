Journal Notes API
=================

Journal notes can be listed, created, updated, and deleted via the API. Journal notes are notes that
are not associated with a particular trade or trading day, and are described in this [Tradervue blog post](http://blog.tradervue.com/2014/05/01/saving-notes/).

Journal Notes
-------------

### Listing journal notes

To list the notes, you will use

`GET /api/v1/notes`

#### Request

There are no required parameters in the request. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://www.tradervue.com/api/v1/notes"
```

#### Response

The response will look like:

```json
{
  "journal_notes": [
    {
      "id": 11444,
      "notes": "These are my notes.",
      "comment_count": 0
    },
    {
      "id": 11433,
      "notes": "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\r\ntempor incididunt ut labore et dolore magna aliqua.",
      "comment_count": 0
    }
  ]
}
```

Journal entries will be sorted with the most recently updated note first.

While there are no required parameters to retrieve the list of notes, there are several optional parameters you can use:

Parameter | Description |
--------- | ----------- |
count     | Specifies how many notes you want back at a time. Defaults to 25, max is 100. |
page      | Specifies which page of results you want. For example, if you want results 26-50, use count=25&page=2. |

### Displaying a single journal note

You can retrieve details about a particular note by using 

`GET /api/v1/notes/{id}`

#### Request

The note's ID is part of the request URL. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  "https://www.tradervue.com/api/v1/notes/11433"
```

#### Response

The response will look like:

```json
{
  "id": 11433,
  "notes": "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\r\ntempor incididunt ut labore et dolore magna aliqua.",
  "comment_count": 0
}
```

### Updating a journal note

To update an note, you will use

`PUT /api/v1/notes/{id}`

You can update the following fields for a journal note:

- notes

#### Request

The note's ID is part of the request URL. From the command line, you can use the following curl command:

```
curl -i \
  -X PUT \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -H "Content-type: application/json" \
  -u example:password \
  -d '{"notes":"here are my updated notes"}' \
  "https://www.tradervue.com/api/v1/notes/12274"
```

A sample JSON request looks like this:

```json
{
  "notes": "here are my updated notes"
}
```

#### Response

If successful, you will get a HTTP 200 response and an empty body.

If the note wasn't found, you will get a HTTP 404.

If the update failed, you will get a HTTP 400 with a JSON response like

```json
{
  "error": "error details here, here, here"
}
```

### Creating a journal note

To create a new note, you will use

`POST /api/v1/notes`

This will create a new journal note, similar to what happens when you click "Create new note" on the Journal page on the Tradervue
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
  -d '{"notes":"My notes."}' \
  "https://www.tradervue.com/api/v1/notes"
```

A sample JSON request looks like this:

```json
{
  "notes": "My notes."
}
```

#### Response

If successful, you will get a HTTP 201 response with a Location header containing the new note's URL:

```
Location: https://www.tradervue.com/api/v1/notes/13922
```

The JSON response will include the new note's ID:

```json
{
  "id":13922
}
```

If the note could not be created, you will get a HTTP 400 with a JSON response like

```json
{
  "error": "error details here, here, here"
}
```

### Deleting a journal note

To delete a journal note, you will use

`DELETE /api/v1/notes/{id}`

This will delete the note.

#### Request

From the command line, you can use the following curl command:

```
curl -i \
  -X DELETE \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -H "Content-type: application/json" \
  -u example:password \
  "https://www.tradervue.com/api/v1/notes/22496"
```

If the note wasn't found, you will get a HTTP 404.
