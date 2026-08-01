---
name: Retrieve a credential from Securden
description: Fetch a password for an account from a self-hosted Securden PAM/Password Vault server using the token-authenticated Password Retrieval API.
api: openapi/securden-password-retrieval-openapi.yml
operations: [get_password]
method: generated
source: openapi/securden-password-retrieval-openapi.yml
---

# Retrieve a credential from Securden

Use this skill to fetch a stored credential from a Securden server for a script,
pipeline, or agent — eliminating hardcoded passwords.

## Prerequisites
- Your Securden server URL: `https://{your_api_url}` (e.g. `https://company.securden.com:5454`).
- An API Authentication Token (`authtoken`). Either a static token issued in the admin
  console (Admin >> API Access >> Authentication Token for APIs), or a dynamic token
  minted at request time.

## Steps

1. (Optional) Mint a dynamic token:
   ```
   GET /api/get_auth_token?login_name={user}&password={pass}&domain_name={domain}
   ```
   Skip this if you already hold a static `authtoken`.

2. Retrieve the password with `get_password`, identifying the account by any of
   `account_id`, `account_name`, `account_title`, or `account_type`, and passing your
   token as `authtoken`:
   ```
   GET /api/get_password?authtoken={authtoken}&account_name={name}
   ```
   A `200` returns `{ "password": "..." }`.

## Rules
- Auth: pass the token as the `authtoken` parameter (see `authentication/securden-authentication.yml`).
- Errors: `400` = malformed/missing parameters; `500` = server error (see `errors/securden-problem-types.yml`).
- Never log the returned password or the `authtoken`; treat both as secrets.
- The API is read-only credential retrieval — there is no idempotency-key or pagination contract.
