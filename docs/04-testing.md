# Phase 4 — Testing

Testing is where you verify the model from phase 3: does the system actually enforce what it claims to enforce? Work from the test plan, one area at a time, and keep notes as you go.

## 1. Ground rules

- **Test only what is in scope**, with your own accounts only.
- **One variable at a time.** Change one thing per request; if you change three, you will not know which one mattered.
- **Record everything.** Request, response, timestamp, account used, and what you expected vs what happened. A finding without notes is a finding you will have to re-derive.
- **Stop when it is proven or dead.** A test ends in one of two states: reproduced (write it up) or ruled out (write it in "dead ends" and move on).

## 2. Authorization (the highest-value area)

Test whether the server enforces who may access what — never trust the UI to do it.

- **Object access:** take an object that belongs to your second account (an order, address, receipt, file) and request it with your first account's session. Use its real ID. If the server returns it, you have a finding; if it returns 403/404, note it and move on.
- **Function access:** call an action your role should not have (for example a merchant or admin action) as a normal user. The UI hiding the button proves nothing — the API must refuse.
- **ID manipulation:** where IDs appear (path, body, query), try the same request with another user's ID, with no ID, and with an ID of a different object type. Predict the expected response first; a mismatch is a lead.
- **Role escalation:** can a normal user set a parameter (role, plan, isAdmin, discount) that the server trusts?

Keep the test minimal and reversible. Never modify another user's real data — if a write succeeds where it should not, stop, note it, and do not repeat it.

## 3. Authentication

- **Session handling:** does logout kill the session server-side? Does a password change kill other sessions? Can a session token be reused after expiry?
- **Reset flows:** is the reset token tied to the account, single-use, and short-lived? Does the response leak whether an account exists?
- **Rate limiting:** is login, reset, and code verification actually limited — or only the UI?
- **OAuth/social login:** what happens if the provider email is unverified, or if two provider accounts share one email?

Report only what you can demonstrate with your own accounts.

## 4. Input handling

- **Injection classes:** test inputs the way the application handles them — but keep payloads harmless and non-destructive (no data deletion, no stored payload that affects other users).
- **File uploads:** what types are accepted, where files are stored, who can fetch them, and whether the filename or content is trusted.
- **Business parameters:** prices, quantities, discounts, and currencies are inputs too. If a price can be set client-side, does the server re-check it? (See business logic below.)
- **Mass assignment:** if an endpoint accepts a JSON object, does it accept fields it should not (role, price, owner)? Try adding one unexpected field at a time and observe whether it takes effect.

## 5. Business logic

Business-logic issues are the most program-specific and often the most valuable. Look for:

- **Checkout/order flows:** can a price, discount, or quantity be changed between selection and payment? Can an order be paid twice, refunded twice, or completed without payment?
- **Coupons and promotions:** can a single-use coupon be reused, stacked, or applied to an ineligible order?
- **State machines:** can an order jump states (for example from "created" to "shipped" without payment)? Can an action be repeated after it should be final?
- **Race conditions:** only test where the program allows it, and never against payment or inventory in a way that creates real orders.

Frame every business-logic test as: *"the system assumes X; I will check whether X actually holds."*

## 6. When a "weird behavior" is worth chasing

Chase it when it has at least one of these:

- It crosses a trust boundary (one user reaching another user's data or action).
- It changes money, identity, or access.
- It contradicts the program's own documented behavior.
- It is reproducible from a clean state with a minimal sequence.

Do not chase it when:

- It only affects your own account and your own data.
- It requires an unrealistic chain (five unlikely preconditions, victim cooperation, and a misconfiguration).
- It is a scanner finding with no demonstrated impact.

A useful question: *"If I explain this to triage in two sentences, does it sound like a real problem?"* If not, keep it in the notes and move on.

## 7. Keeping notes while testing

For each test, record:

```text
Test: order receipt access, second account
Expected: 403 (receipt belongs to account B)
Tried: GET /api/v2/receipts/12345 as account A, 14:32 WIB
Got: 200, full receipt JSON incl. name, address, order total
Verdict: REPRODUCED — write up (phase 5)
```

For dead ends, one line is enough: `Test: coupon stacking on example.com — server rejects second coupon, 400. Ruled out 14:40 WIB.`

## Phase 4 exit checklist

- [ ] Test plan worked through in ranked order.
- [ ] Every test recorded with expected vs actual.
- [ ] Reproduced findings have minimal reproduction steps written down.
- [ ] Ruled-out tests recorded in "dead ends".
- [ ] No out-of-scope testing, no other users' data touched, no destructive payloads.
- [ ] At least one finding reproduced twice from a clean state before reporting.
