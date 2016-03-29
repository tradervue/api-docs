### Mar 29, 2016

- added exit_price field to trades

### Jan 6, 2016

- Added support for retrieving comments on trades, journal entries, and journal notes
- Added documentation for executions API, for retrieving the executions attached to a trade

### Dec 8, 2015

- Added documentation for journal notes API, for listing, creating, updating, and deleting journal notes

### Dec 7, 2015

- Added documentation for journal API, for listing, creating, updating, and deleting journal entries
- Cleaned up some smart quotes in curl samples, which would cause problems when copying/pasting examples

### Dec 2, 2015

- Added documentation for trades API, for listing, creating, updating, and deleting trades
- Added public documentation for user management API, used for managing users using
  Tradervue for trading firms. This documentation
  was previously available privately, but is now moving to the repository.
- Reorganized documentation to separate each API group into its own page, and removed
  redundant information on each page.

### Dec 19, 2013

- Added support for the optional Tradervue-UserId header, for organizations to use to
  issue API calls on behalf of their users


### Apr 9, 2013

- Added `skipped_forex` to import status response, indicating the user is not allowed to
  import forex trades on his current subscription plan.

### Mar 28, 2013

- You are no longer required to retrieve the prior import status before starting a new
  import. As long as the prior import is not "queued" or "processing", the new import
  can be started. Note, however, that starting a new import will clear the prior status -
  if there was an error or other information available about the prior import, that will
  be permanently cleared and you will not be able to access it.

  The recommended flow is still to create an import, and then retrieve its status periodically
  until you see that it has completed. Or alternatively, retrieve the prior status before starting a
  new import.

### Mar 6, 2013

- User-agent is now required for all API calls

### Feb 8, 2013

- Initial version of API released
