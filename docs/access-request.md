# Access Request Flow

Users should not receive API keys automatically.

## Separate request page

The request flow should live on the main website, not inside the API route set.

Recommended public location:

- `imeicheckpro.com/contact#tac-access`

## Recommended flow

1. User opens the request form.
2. User submits:
   - full name
   - email
   - company name
   - use case
   - estimated requests per day
   - website or app URL
3. System stores the request as `pending`.
4. Admin reviews the request.
5. Admin approves or rejects.
6. If approved, the system:
   - creates the API key
   - stores it against the request owner
   - emails the key and usage instructions
   - marks the request as approved

## Denied or suspended access

Admins must be able to:

- suspend a key immediately
- revoke a key immediately
- delete a key record if needed
- rotate a key without changing the owner's account

## Email content

Approval email should include:

- API key
- base URL
- sample request
- rate limit summary
- support contact

## Admin review fields

Admin should see:

- email
- company
- use case
- traffic estimate
- approval status
- notes
- generated API key status
