# Phase 1 — Scope and Rules

Everything in this playbook depends on this phase. A perfect technical finding on an out-of-scope asset is worth nothing — and testing it can end your relationship with the program.

## 1. Read the whole scope, twice

Read the program page end to end before you open a single tool:

- **In scope:** which assets (domains, apps, APIs), which vulnerability classes, which severity tiers.
- **Out of scope:** which assets and which classes are explicitly excluded. Out-of-scope lists are long and specific — read all of them.
- **Rules:** rate limits, automation limits, account rules (do you need a special test email?), disclosure rules, and whether duplicate reports are paid.
- **Safe harbour:** what legal protection the program offers, and what it requires from you (usually: stay in scope, do not exfiltrate data, do not harm availability, report privately first).

Write a one-page scope summary in your notes before you start:

```text
Program: example.com (placeholder)
In scope: *.example.com web, api.example.com, Android app
Out of scope: *.internal.example.com, third-party services, physical security
Tiers: web app Tier 1, API Tier 1, wildcards Tier 5
Rate limit: max 5 req/s, no automated scanners on login/checkout
Test account: use your own account only; a second account needs a program email
Focus: payment & order flows, authorization flaws in APIs
```

If anything in the scope is ambiguous, ask the program before testing that area — a 10-minute question is cheaper than a ban.

## 2. Program rules win

If this playbook and a program disagree, the program wins. Examples:

- The program says "no automated tooling" → do not run a scanner, even a polite one.
- The program says "max 5 req/s" → that is a hard ceiling, not a suggestion.
- The program says "test accounts only" → do not test with a real customer account.
- The program says "no social engineering" → no phishing, no pretexting, no exceptions.

## 3. Safe-harbour basics

Safe harbour is not a blanket permission. It typically covers you only when you:

- Stay strictly within the published scope.
- Do not access, copy, or retain other users' data.
- Do not degrade availability (no DoS, no load testing, no resource exhaustion).
- Do not disclose the issue publicly before the program allows it.
- Report through the program's channel, with enough detail to reproduce.

If a program has no safe-harbour statement, treat that as a reason to be *more* careful, not less.

## 4. What "in scope" really means

- **An asset is in scope** only if it is listed. A subdomain you discovered is not automatically in scope — check the list (wildcards count if listed).
- **A vulnerability class is in scope** only if it is not excluded. "Anything goes" is rare; most programs exclude whole classes (for example self-XSS, missing headers, or brute force).
- **A finding is in scope** only if it is on an in-scope asset *and* in an in-scope class *and* demonstrated with real impact.

## 5. Rate limits and politeness

- Default to **max 5 requests per second** against any target unless the program says otherwise.
- Add delays between requests in scripts; do not parallelize aggressively.
- Never run load, stress, or fuzzing campaigns against login, checkout, or payment flows unless the program explicitly invites it.
- If a host starts returning 429 or 403, stop, back off, and note it. Pushing through a rate limit is how accounts get flagged.

## 6. Accounts and identity

- Test with your own accounts only.
- If testing authorization between users, you usually need a second account — check whether the program allows it and whether it requires a program-specific email domain.
- Never use another real person's account, session, or data — even if you "only read" it.
- Never create accounts at scale (no mass registration) unless the program explicitly allows it.

## Phase 1 exit checklist

- [ ] Scope summary written in your notes, including tiers and focus areas.
- [ ] Out-of-scope list copied in full — including the classes, not just the assets.
- [ ] Rate limit and automation rules noted.
- [ ] Test-account rules understood (one account or two, special email or not).
- [ ] Safe-harbour terms read; anything unclear asked about first.
- [ ] Do-not-touch list written down before any tool runs.

Do not start phase 2 until every box is checked.
