---
name: Create and approve a disbursement
description: Idempotently create a grant disbursement, approve it, and track it to completion (with sandbox simulation).
api: openapi/chariot-openapi-original.yml
operations: [createDisbursement, getDisbursement, approveDisbursement, listDisbursements, cancelDisbursement, simulateDisbursementCompletion]
---

# Create and approve a disbursement

Use this flow to move grant money out to a nonprofit.

## Auth & environment
- HTTP bearer secret key (`Authorization: Bearer {TOKEN}`; test prefix `sk_test_`).
- Sandbox `https://sandboxapi.givechariot.com`, production `https://api.givechariot.com`.

## Steps
1. **Create the disbursement idempotently** — `createDisbursement`. Send a unique `Idempotency-Key` header so a retried request does not create a duplicate payment. To look up a prior create, call `listDisbursements` with the `idempotency_key` query parameter.
2. **Approve it** — `approveDisbursement` with the disbursement id (or `bulkApproveDisbursements` for many).
3. **Track state** — `getDisbursement`; list/paginate with `listDisbursements` (`limit` + `cursor`, `results` + `nextPageToken`).
4. **Cancel if needed** — `cancelDisbursement` before it settles.
5. **Sandbox testing** — drive terminal state with `simulateDisbursementCompletion` or `simulateDisbursementFailure`.

## Conventions
- Idempotency-Key is required discipline on money movement.
- Errors are RFC 7807 `application/problem+json`; reference `X-Request-Id` on support.
