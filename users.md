User Management API
===================

Users in an organization can be managed by the organization's manager using the API.

Making a request
----------------

All URLs start with `https://www.tradervue.com/api/v1`, and are accessible via SSL only. The
actual hostname should be your organization's URL; so for example, if you access your site
at `https://traders-r-us.tradervue.com`, then the API root will be 
`https://traders-r-us.tradervue.com/api/v1`.

Authentication
--------------

You use HTTP Basic authentication to authenticate with the Tradervue API. Authenticate with the
organization manager account to use the user management API.

Users
-----

### Listing users

To list the users in your organization, you will use

`GET /api/v1/users`

#### Request

There are no parameters in the request. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  https://www.tradervue.com/api/v1/users
```

#### Response

The response will look like:

```json
{
  "users": [
    {
      "id": 10000,
      "username": "test1",
      "email": "test1@example.com",
      "plan": "Free",
      "billing_mode": "Tradervue CC"
    },
    {
      "id": 10001,
      "username": "test2",
      "email": "test2@example.com",
      "plan": "Silver",
      "billing_mode": "Managed"
    }
  ]
}
```

### Displaying a single user

To retrieve information about a user in your organization, you will use

`GET /api/v1/users/{id}`

#### Request

The user's ID is part of the request URL. From the command line, you can use the following curl command:

```
curl -i \
  -H "Accept: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  https://www.tradervue.com/api/v1/users/10001
```

#### Response

The response will look like:

```json
{
  "id": 10001,
  "username": "test2",
  "email": "test2@example.com",
  "plan": "Silver",
  "billing_mode": "Managed"
}
```

### Updating a user

To update a user's information in your organization, you will use

`PUT /api/v1/users/{id}`

#### Request

The user's ID is part of the request URL. From the command line, you can use the following curl command:

```
curl -i \
  -X PUT \
  -H "Accept: application/json" \
  -H "Content-type: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  -d '{"plan":"Gold"}' \
  https://www.tradervue.com/api/v1/users/10001
```

A sample JSON request looks like this:

```json
{
  "username": "test1",
  "email": "test1@example.com",
  "plan": "Silver"
}
```

#### Response

If successful, you will get a HTTP 200 response and an empty body.

If the user wasn't found or isn't in your organization, you will get a HTTP 404.

If the update failed, you will get a HTTP 400 with a JSON response like

```json
{
  "error": "Invalid request: error details here, here, here"
}
```

### Creating a user

To create a new user in your organization, you will use

`POST /api/v1/users`

#### Request

From the command line, you can use the following curl command:

```
curl -i \
  -X POST \
  -H "Accept: application/json" \
  -H "Content-type: application/json" \
  -H "User-Agent: MyApp (yourname@example.com)" \
  -u example:password \
  -d '{"username":"test3","plan":"Gold","email":"test3@example.com","password":"the_password"}' \
  https://www.tradervue.com/api/v1/users
```

A sample JSON request looks like this:

```json
{
  "username": "test3",
  "plan": "Gold",
  "email": "test3@example.com",
  "password": "the_password"
}
```

The username, plan, email, and password fields shown are required.

To create a user with a trial period, include the `trial_end` parameter, as in the following example:

```json
{
  "username": "test3",
  "plan": "Gold",
  "email": "test3@example.com",
  "password": "the_password",
  "trial_end": "2014-06-30"
}
```

If you specify an invalid trial end date, or one that exceeds the maximum trial length for your
organization, you will get a 400 error and the user will not be created. 

You can also specify certain optional settings that the user will be created with, as in the following example:

```json
{
  "username": "test3",
  "plan": "Gold",
  "email": "test3@example.com",
  "password": "the_password",
  "settings": {
    "merge_mode": "force_split",
    "commission_stocks": "1.01",
    "commission_options": "1.25",
    "commission_futures": "2.05",
    "mentor": 124
  }
}
```

The available settings are as follows (all optional):

- `merge_mode` - this can be one of `normal`, `force_split`, or `force_merge`. If you omit this field, `normal` is assumed.

- `commission_stocks` - the default per-share commission to be used for stock executions in imports where no commission is specified.

- `commission_options` - the default per-contract commission to be used for option executions in imports where no commission is specified.

- `commission_futures` - the default per-contract commission to be used for futures executions in imports where no commission is specified.

- `mentor` - the ID of a user in your organization to be added as the new user's mentor. The mentor must exist in your organization, and be eligible to be a mentor (i.e. on the silver or gold plan). The new user will see that the mentor has been added in their activity feed.

#### Response

If successful, you will get a HTTP 201 response with a Location header containing the new user's URL:

```
Location: https://www.tradervue.com/api/v1/users/170
```

The JSON response will include the new user's ID:

```json
{
  "id":170
}
```

If the user could not be created, you will get a HTTP 400 with a JSON response like

```json
{
  "error": "Invalid request: error details here, here, here"
}
```
