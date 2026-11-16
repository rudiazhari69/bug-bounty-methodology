# Phase 2 — Recon (Passive)

Recon answers one question: *what exists?* Done well, it is mostly passive — gathering what is already public — and it ends with an inventory you can test against, not a pile of raw tool output.

## 1. Principles

- **Passive first.** Certificate transparency, DNS records, archived URLs, and public JS bundles tell you most of what you need before you send a single request to the target.
- **Scope-filter everything.** Every host you find gets checked against your phase 1 scope summary. Out-of-scope hosts go on the do-not-touch list, not in the test plan.
- **Write the inventory down.** A list in your notes beats 10,000 lines in a tool's output file. Raw output is evidence; the inventory is what you work from.

## 2. Subdomain enumeration

Goal: the full list of hostnames under the in-scope domains.

- Certificate Transparency logs (crt.sh and equivalents) — the most reliable passive source.
- Public DNS datasets and passive DNS where available.
- Search engines and code search for references to the target's domains.
- Brute-forcing subdomains is *active* — only do it if the program allows it, and keep it slow.

Practical notes:

- CT logs are rate-limited. Query serially with pauses (for example 10+ seconds between requests) and retry on failure.
- Deduplicate and sort. One hostname appearing in five sources is still one hostname.
- Separate the list into: in-scope web, in-scope API, in-scope wildcard, out-of-scope, and unclear (ask before testing the unclear ones).

## 3. Live-host check

Goal: which of those hostnames actually answer.

- One polite request per host (HEAD or a small GET), with a timeout, following redirects one step.
- Record: host, status code, response size, response time, and redirect target.
- A redirect to an out-of-scope domain is a stop sign: do not follow it into testing.
- Hosts that answer 000/timeout are dead for now — note them and move on; do not hammer them.

## 4. Technology fingerprinting

Goal: what the target is built with, so you know which bug classes apply.

- HTTP headers (server, framework, CDN/WAF markers), cookies, and error pages.
- HTML/JS hints: framework markers, build hashes, asset paths.
- TLS certificate details (issuer, SANs — sometimes reveals sibling domains).
- Never fingerprint by sending attack payloads. Headers and error pages are enough at this stage.

Record the stack per asset: for example `example.com — React SPA, API at api.example.com, Akamai edge, Hybris-style /api/v2/ paths`. The stack tells you which testing areas matter (see phase 3).

## 5. JS endpoints and archived URLs

Two of the highest-value passive sources:

- **JS bundles:** download the site's JavaScript and extract candidate API paths (`/api/...`, `/oauth/...`, `/users/...`, `/orders/...`). These paths are the API surface you will map in phase 3. Do this from the site's own public bundles — no credentials needed.
- **Archived URLs (Wayback Machine CDX API):** harvest known URLs for the domain. Old URLs reveal retired endpoints, old parameters, admin paths, and forgotten subdomains. This touches the archive, not the target.

Both are passive. Both regularly produce the lead that becomes the finding.

## 6. Interesting-host triage

After enumeration, sort the live hosts into test priorities:

| Priority | Examples (placeholders) |
| -------- | ----------------------- |
| High | `api.example.com`, `app.example.com`, `pay.example.com`, `account.example.com` |
| Medium | `media.example.com`, `cdn.example.com`, `queue.example.com` |
| Low | static marketing hosts, documentation hosts |
| Do not touch | anything out of scope, redirect targets outside the program |

Also note anomalies: hosts returning 500s, stub bodies, or unexpected tech — anomalies are leads, but verify them passively first (is the 500 a generic error page, or a stack trace?).

## 7. When to stop recon

Stop and move to mapping when:

- You have a deduplicated inventory of in-scope live hosts.
- You know the stack per major asset.
- You have a list of candidate API paths and archived URLs.
- New sources are returning only hosts you already have.

Recon has diminishing returns. Two focused hours beats two unfocused days. If you catch yourself enumerating the same hosts a third time, you are done — move on.

## Phase 2 exit checklist

- [ ] Deduplicated subdomain list, scope-filtered.
- [ ] Live-host results recorded (status, size, redirect target).
- [ ] Tech stack noted per major asset.
- [ ] JS endpoint candidates extracted.
- [ ] Archived URLs harvested.
- [ ] Priority list written: what to map first in phase 3.
- [ ] No out-of-scope host touched.

## Tooling

The companion repo [bug-bounty-recon-kit](https://github.com/rudiazhari69/bug-bounty-recon-kit) implements this phase as scripts: crt.sh enumeration, live-host checks, JS endpoint extraction, and Wayback harvest — all passive, all rate-limited.
