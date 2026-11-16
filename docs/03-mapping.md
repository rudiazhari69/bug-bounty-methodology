# Phase 3 — Mapping the Attack Surface

Mapping turns your recon inventory into a model of how the target works: who can do what, where data flows, and where trust lives. You test against the model — not against random URLs.

## 1. Why map before testing

Testing without a map is guessing. With a map you can:

- See which endpoints matter (the ones that touch money, identity, or other users' data).
- See which roles exist and what each role is allowed to do.
- Pick the smallest set of tests most likely to find real issues.
- Explain a finding in terms of the model ("a user of role A reached data of role B through endpoint X") — which is exactly what triage wants to read.

## 2. Map the auth flows

For each application, trace:

- **Sign-up:** what is verified (email? phone? nothing?), what can be reused (can two accounts share an email?), what happens on re-registration.
- **Login:** session handling (cookies vs tokens), expiry, concurrent sessions, "remember me".
- **Password reset / recovery:** token format and lifetime, whether the token leaks in responses or logs, rate limiting.
- **Social/OAuth login:** which providers, what scopes, what happens if the provider account email changes.
- **Logout:** does the session actually die server-side?

Write each flow as a short numbered list in your notes, with the requests involved. Gaps in your understanding of a flow are test ideas.

## 3. Map the roles

List every role the system has (for example: anonymous visitor, registered user, merchant, support agent, admin) and, for each role:

- What can it create, read, update, and delete?
- Which of those objects belong to *other* users?
- Which actions are supposed to be impossible for this role?

The most valuable sentence in bug bounty is: *"Role A should not be able to do X, but it can."* You can only write it if you know what each role should and should not do.

## 4. Map the API surface

From your phase 2 JS endpoints and archived URLs, plus the app's own network traffic while you use it normally:

- Group endpoints by resource: `/users/...`, `/orders/...`, `/payments/...`, `/addresses/...`.
- For each endpoint, note: method, auth required (yes/no), parameters, and what object it returns or changes.
- Mark the interesting ones: anything with an ID in the path or body (`/orders/{id}`, `userId=...`), anything that changes state, anything that touches money or files.
- Note the API style (REST, GraphQL, RPC) — it changes how you test, not what you test.

Do not test yet. Mapping is reading and using the app as a normal user, watching what it calls.

## 5. Map the data flows

Trace one piece of sensitive data end to end. For example, an order:

```text
user creates order → order stored with user id → order visible in "my orders"
→ order receipt generated → receipt emailed → receipt viewable at a URL
```

At each step ask: who can see this, with what identifier, and is that identifier guessable or enforced? Receipts, invoices, export files, and shared links are classic places where an ID check is missing.

Do the same for: user profile data, uploaded files, messages/notifications, and payment data.

## 6. Map the trust boundaries

A trust boundary is where data crosses from one trust level to another:

- Browser → API server.
- API server → third-party service (payment gateway, shipping provider, chatbot backend).
- One tenant/user → another tenant/user.
- Client-side code → server-side decision (never trust the client: if the UI hides a button, the API must still refuse).

Every boundary is a place to test later. Write them down as a list.

## 7. Pick what to test first

Rank your map by expected value:

1. **Money and identity flows** — checkout, payment, refunds, coupons, account takeover paths.
2. **Cross-user access** — any endpoint that returns or changes another user's object.
3. **State-changing endpoints with IDs** — the classic authorization-test surface.
4. **File handling** — uploads, downloads, receipts, exports.
5. **Everything else** — in the order your map suggests.

Write the test plan as a short list before phase 4. A plan keeps you from drifting into random clicking.

## 8. Notes structure

Keep one notes file per program, roughly:

```text
# program: example.com
## Scope summary (from phase 1)
## Inventory (from phase 2)
## Auth flows
## Roles
## API surface (grouped by resource)
## Data flows
## Trust boundaries
## Test plan (ranked)
## Findings (one section each, with reproduction notes)
## Dead ends (what you tried and ruled out — saves you re-testing it)
```

The "dead ends" section is underrated: it stops you from re-testing the same thing next week.

## Phase 3 exit checklist

- [ ] Auth flows traced and written down.
- [ ] Roles listed with what each may and may not do.
- [ ] API surface grouped by resource, interesting endpoints marked.
- [ ] At least one sensitive data flow traced end to end.
- [ ] Trust boundaries listed.
- [ ] Ranked test plan written before any testing starts.
