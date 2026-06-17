# Admin Reporting

Admin dashboard should let you answer:

- who is using the API
- how many requests each key makes
- which email owns each key
- which IPs are active
- which endpoints are being called
- when abuse or spikes happen

## Suggested views

### API key list

Columns:

- key name
- owner email
- status
- requests today
- requests this month
- rate limit
- created at
- last used at

### Usage detail

Filters:

- email
- API key
- IP address
- date range
- endpoint

### Abuse monitor

Show:

- 429 spikes
- repeated invalid key attempts
- unusual IP diversity
- requests outside quota
- suspended or revoked key attempts
