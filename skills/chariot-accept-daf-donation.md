---
name: Accept a DAF donation with Chariot Connect
description: Start a Chariot Connect session so a donor can give from their donor-advised fund, then confirm the resulting donation.
api: openapi/chariot-openapi-original.yml
operations: [create-connect, get-connect, listDonations, getDonation]
---

# Accept a DAF donation with Chariot Connect

Use this flow to let a donor give directly from their donor-advised fund (DAFpay).

## Auth & environment
- Authenticate with an HTTP bearer secret key: `Authorization: Bearer {TOKEN}`. Test keys are prefixed `sk_test_`.
- Sandbox base URL `https://sandboxapi.givechariot.com`; production `https://api.givechariot.com`. HTTPS TLS 1.2+ only.

## Steps
1. **Create a Connect session** — `create-connect`. Provide the receiving organization and gift context. The response is a `Connect` object your front end hands to the `react-chariot-connect` component to open the donor's DAF flow.
2. **Read session status** — `get-connect` with the returned id to check whether the donor completed the Connect experience.
3. **Confirm the donation** — once the grant settles, find it with `listDonations` (filter/paginate with `limit` + `cursor`, read `results` + `nextPageToken`) and read details with `getDonation`.
4. **React to events instead of polling** — subscribe to `donation.created` / `donation.updated` (see the events skill) rather than polling.

## Conventions
- Every response carries `X-Request-Id`; log it for support.
- Errors are RFC 7807 `application/problem+json` (`type`/`title`/`status`/`detail`).
