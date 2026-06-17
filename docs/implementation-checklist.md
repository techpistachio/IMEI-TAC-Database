# Implementation Checklist

## Backend

- [ ] Hash API keys before storage
- [ ] Add owner email to API key records
- [ ] Add separate API usage log collection
- [ ] Capture IP address, key id, and owner email for every request
- [ ] Add per-key rate limit enforcement
- [ ] Add per-IP rate limit enforcement
- [ ] Add daily quota enforcement
- [ ] Add admin endpoints to approve or reject access requests
- [ ] Add admin endpoints to revoke and rotate keys
- [ ] Add reporting endpoints for keys and usage

## Public site

- [ ] Add access request form
- [ ] Add API documentation page
- [ ] Add sample code snippets
- [ ] Add terms of use and acceptable use policy
- [ ] Add support contact instructions

## Operations

- [ ] Set up SMTP provider
- [ ] Set up Redis for distributed rate limits
- [ ] Set up log retention policy
- [ ] Set up abuse alerts
- [ ] Set up backup and restore for MongoDB
