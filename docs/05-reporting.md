# Phase 5 — Reporting

A report is judged on reproducible impact, not on length. Triage reads dozens of reports a day: the good ones make the problem obvious in the first paragraph and prove it in the next five lines.

## 1. Structure

Use this order — it matches how triage works:

1. **Title** — one line: vulnerability class + asset + impact. Example: `IDOR on order receipts allows any authenticated user to read another user's order details (api.example.com)`.
2. **Summary** — two or three sentences: what is wrong, who is affected, what an attacker gains. Put the impact first.
3. **Severity** — your assessment with a CVSS vector and a plain-language justification (see the severity guide in the companion templates repo).
4. **Affected asset** — exact host/endpoint, in scope, with version or build if relevant.
5. **Steps to reproduce** — numbered, from a clean state, with two accounts where needed. Every step must be something triage can do.
6. **Proof of concept** — the minimal requests/responses that demonstrate it. Redact all secrets and personal data (`REDACTED`); never include another real user's data.
7. **Impact** — who is affected and how badly, in concrete terms (how many users, what data, what money). Avoid worst-case fiction.
8. **Remediation** — one concrete fix (for example: enforce ownership checks server-side on every object access, not just in the UI).
9. **References** — links to the relevant docs or standards, if any.

A full template lives in the companion repo [bug-bounty-report-templates](https://github.com/rudiazhari69/bug-bounty-report-templates).

## 2. Title rules

- Name the class, the asset, and the impact — in that order.
- No clickbait, no "critical!!!" without evidence, no vague titles like "Security issue".
- If the same issue affects several assets, say so once and list them; do not file the same issue five times.

## 3. Reproduction steps that work

- Start from a clean state (fresh login, no leftover state).
- Number every step; one action per step.
- Include exact requests (method, path, parameters) and the exact responses that matter.
- Say which account did what, and what each account should and should not be able to see.
- If a step needs setup (two accounts, an order placed first), say so at the top.

Test your steps by following them yourself as if you were triage. If you cannot follow them, neither can they.

## 4. Minimal PoC

- The smallest sequence that proves the issue — not your whole testing session.
- Include the request and the relevant part of the response. Trim everything else.
- Use `REDACTED` for tokens, passwords, session values, and personal data. Never paste a live secret into a report.
- Screenshots or short recordings help for UI issues; for API issues, text is better.
- Never include another real user's data. If your PoC touched it, stop, delete it from the report, and re-do the PoC with test accounts only.

## 5. Severity justification

- Give a CVSS vector *and* a sentence in plain language explaining why the numbers fit.
- Base severity on demonstrated impact, not on the worst imaginable chain.
- If the program uses its own severity tiers, map your finding to them explicitly.
- If you are unsure between two severities, say so and give the reasoning — triage respects an honest range more than a confident overclaim.

## 6. Before you submit — the gate

Run the 7-question validation gate (full version in the templates repo):

1. Is it reproducible from a clean state?
2. Is the asset in scope?
3. Is it a duplicate of a known issue?
4. Is there real impact on a real user or the business?
5. Is the PoC minimal and free of secrets?
6. Is the severity justified by evidence, not by the worst case?
7. Would you pay for this report yourself?

If any answer is no, do not file yet. Fix the gap first.

## 7. Common report mistakes

- **Wall of text, no structure.** Triage skims; structure is how you survive the skim.
- **Scanner output pasted raw.** A scanner's claim is not evidence — your reproduction is.
- **Impact invented.** "An attacker could take over the whole company" needs a chain you demonstrated, not one you imagined.
- **Missing scope proof.** If the asset is not obviously in scope, say where it is listed.
- **Secrets in the report.** Redact before submitting, not after.
- **Five reports for one bug.** One root cause, one report — list all affected assets inside it.

## Phase 5 exit checklist

- [ ] Title names class + asset + impact.
- [ ] Summary states the impact in the first paragraph.
- [ ] Reproduction steps tested from a clean state.
- [ ] PoC minimal, secrets redacted, no other users' data.
- [ ] Severity justified with CVSS + plain language.
- [ ] Remediation is concrete.
- [ ] Validation gate passed (7/7).
