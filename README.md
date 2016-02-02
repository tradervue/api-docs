Tradervue API
=============

The Tradervue API is a REST-style API for [Tradervue](http://www.tradervue.com)
that uses JSON for serialization, and HTTP Basic authentication over 
SSL.

Refer to the [change log](CHANGELOG.md) for the history of API changes.

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

Multiple Users
--------------

If you are the administrator of an organization in Tradervue, and this feature has been enabled
for your organization, you can issue API calls on behalf of users in your organization. You could
use this to auto-import trades for your users, for example.

To issue an API call on behalf of another user, you should authenticate as the organization
administrator, and include a Tradervue-UserId HTTP header that identifies the target user. Something
like:

```
Tradervue-UserId: 78914
```

If you do not have permission to issue API calls for the specified user, you will get a HTTP 403
Not Authorized response.

*Note: the "X-" prefix for non-standard headers has been deprecated 
in [RFC 6648](http://tools.ietf.org/html/rfc6648), which is why it is not used here.*

Samples
-------

- [Ruby sample application](https://github.com/tradervue/ruby-sample) which uses the Import API.
- [C# sample applications](https://github.com/tradervue/dotnet-sample) which use the Import API.
- Command line examples with curl are shown inline on the API detail pages.
- [tv-industrify](https://github.com/tradervue/tv-industrify) - sample to add industry tags to trades

Community
---------

These are some open-source projects that use the Tradervue API.

- [py-tradervue-api](https://github.com/nall/py-tradervue-api) - Python wrapper and command line client example for Tradervue API
- [tv-importofx](https://github.com/nall/tv-importofx) - Utility to download OFX files from a broker and import them into Tradervue

If there are additional projects we're missing, let us know!

Specific API Documentation
--------------------------

- [Import API](imports.md) - for importing trades
- [Trades API](trades.md) - for creating and managing trades
- [Journal API](journal.md) - for creating and managing journal entries
- [Journal Notes API](notes.md) - for creating and managing journal notes
- [Executions API](executions.md) - for retrieving the executions for a trade
- [Comments API](comments.md) - for retrieving the comments on a trade, journal entry, or journal note
- [User Management API](users.md) - for managing users who belong to an organization
