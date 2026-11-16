# Bug Bounty Methodology

An end-to-end methodology playbook for web bug bounty hunting — from reading the scope to learning from the triage result. It is a way of working, not a list of payloads: the same six phases apply whether the target is a small shop or a large platform.

## The 6-phase flow

| Phase | Question it answers | Output |
| ----- | ------------------- | ------ |
| [1. Scope & Rules](docs/01-scope-and-rules.md) | What am I allowed to touch, and under what rules? | A written scope summary + a do-not-touch list |
| [2. Recon](docs/02-recon.md) | What exists? | An asset inventory: hosts, tech, endpoints, URLs |
| [3. Mapping](docs/03-mapping.md) | How does it work, and where is the trust? | A map of auth flows, roles, APIs, and data flows |
| [4. Testing](docs/04-testing.md) | Where does it break? | Reproduced findings with notes and evidence |
| [5. Reporting](docs/05-reporting.md) | What did I find, and why does it matter? | A submitted report with minimal PoC |
| [6. After Submit](docs/06-after-submit.md) | What happens next? | Triage handling, retests, lessons learned |

Most beginners jump straight to phase 4. Most professionals spend most of their time in phases 1–3 — because a finding you cannot place in scope, reproduce, or explain is not a finding, it is a note.

## Golden rules

1. **Scope first.** Read the full program scope before touching anything. Program rules always win over this playbook.
2. **Impact over noise.** One reproducible issue with real impact beats ten scanners' worth of low-value observations.
3. **Reproduce twice.** If you cannot reproduce it from a clean state, do not report it yet.
4. **Never test out of scope.** Not "a little bit", not "just to check". Out-of-scope testing is how hunters get banned.
5. **Passive before active.** Recon that does not touch the target comes before anything that does.
6. **Notes or it did not happen.** Every test, every odd response, every dead end — written down with a timestamp.

## Who this is for

- Hunters who have done a few programs and want a repeatable process instead of wandering.
- Hunters returning after a break who want a checklist to get back up to speed.
- Anyone who has ever had a report closed as "informative" and wondered why.

## How to use this playbook

Work the phases in order on each new program. Keep one notes file per program (see [03-mapping](docs/03-mapping.md) for a suggested structure). When you finish a program, read [06-after-submit](docs/06-after-submit.md) and write down one lesson before moving on.

## What this is not

- Not a payload collection. Payloads go stale; method does not.
- Not a scanner manual. Automated tools are covered only where they fit the phase.
- Not legal advice. Read the program's safe-harbour terms yourself; if a program has none, be extra careful.
- Not a shortcut to payouts. Methodology raises the quality of your reports; it does not guarantee triage outcomes.

## Companion repos

- [bug-bounty-recon-kit](https://github.com/rudiazhari69/bug-bounty-recon-kit) — scripts for phase 2 (subdomain enum, live-host checks, JS endpoints, Wayback harvest).
- [bug-bounty-report-templates](https://github.com/rudiazhari69/bug-bounty-report-templates) — report template, severity guide, and a 7-question validation gate for phase 5.

## License

[MIT](LICENSE)
