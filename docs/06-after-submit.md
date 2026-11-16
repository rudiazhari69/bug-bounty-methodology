# Phase 6 — After Submit

Submitting is not the end. How you handle triage, rejections, and retests decides whether you get paid, whether you get invited back, and whether you get better.

## 1. Duplicates

Duplicates are the most common outcome, and they are not personal.

- **Before submitting,** search the program's disclosed reports, public writeups, and your own notes for the same issue. A 10-minute search saves a wasted report.
- **If marked duplicate,** read the original report if it is visible. Note what the first reporter did differently (earlier? better PoC? different asset?) and learn from it.
- **One root cause, one report.** If you find the same flaw on five endpoints, that is one report with five examples — not five reports competing with each other.
- Do not resubmit a duplicate with different wording. It will be marked duplicate again, and it costs you credibility.

## 2. Triage timelines

- Programs publish response and triage targets (for example: first response within 2 weeks, decision within a few weeks). Read them once, then be patient.
- Do not bump a report after two days. If the program's stated timeline has passed, one polite follow-up is fine.
- While waiting, move on to the next program. Do not sit on one report.
- Keep a small tracker: program, report title, date submitted, status, and outcome. Patterns in your own outcomes (which classes get accepted, which get closed) are worth more than any single payout.

## 3. Responding to triage questions

- Answer quickly and precisely. Triage usually asks one of three things: *can you reproduce it on this asset?*, *what is the impact?*, or *is this the same as report X?*
- Reproduce on demand: keep your test accounts and notes until the report is closed, so you can re-run the steps if asked.
- If triage cannot reproduce it, do not argue — re-check your steps from a clean state and send a corrected, minimal sequence. Most "cannot reproduce" outcomes are a missing setup step in the report.
- Be polite and concise. Triage teams remember hunters who are easy to work with.

## 4. Retesting fixes

- When a program says a fix is deployed, retest the exact reproduction steps.
- If it is fixed, confirm it and close the loop. If it is not fixed, say so with fresh evidence — do not just say "still works".
- Partial fixes are common (one endpoint fixed, a sibling endpoint not). Retest the siblings and report them in the same thread, not as a new report, unless the program asks otherwise.

## 5. Learning from rejections

Every closed-as-informative or not-applicable report is a lesson. For each one, write one line in your tracker:

```text
2026-11-16 — example.com — "missing header on /login" — closed N/A.
Lesson: headers alone are not findings; need demonstrated impact.
```

Common rejection patterns and what they teach:

| Rejection | Lesson |
| --------- | ------ |
| Duplicate | Search disclosed reports first; report earlier or better. |
| Informative / N/A | No demonstrated impact — reproduce the impact, not the observation. |
| Out of scope | Re-read the scope before testing, every time. |
| Needs more info | Reproduction steps were incomplete — test them from a clean state. |
| Self-XSS / social engineering needed | The class is excluded, or the chain is unrealistic — pick better targets. |

Review your tracker monthly. If the same rejection repeats three times, that is the thing to fix in your process — not the program.

## 6. Disclosure and writeups

- **Never disclose before the program allows it.** Most programs require private reporting first and set their own disclosure rules.
- If the program allows a writeup after the fix, write it: it builds your reputation more durably than a single payout.
- A good writeup teaches the method, not just the payload: scope, recon, mapping, the wrong turns, and the fix.
- Redact program-specific details the program asked you to keep private.

## 7. Reputation compounds

- Programs invite back hunters who submit clean, reproducible, in-scope reports and respond well to triage.
- A profile with a few well-written public reports and useful tooling (see the companion repos) is worth more than a long list of closed-as-duplicate submissions.
- Protect it: one sloppy report costs more reputation than five good ones earn.

## Phase 6 exit checklist

- [ ] Report tracked (date, status, outcome).
- [ ] Test accounts and notes kept until the report is closed.
- [ ] Triage questions answered promptly and precisely.
- [ ] Fix retested with the original steps; siblings checked.
- [ ] One lesson written down for every rejection.
- [ ] No public disclosure before the program allows it.
