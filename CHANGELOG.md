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
