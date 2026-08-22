# Thought Machine

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Thought Machine is a London-headquartered cloud-native core banking technology company founded in 2014 by Paul Taylor, with offices in New York and Singapore. It builds **Vault Core**, a universal product engine in which every banking product is expressed as a developer-written smart contract executed against a real-time ledger, and **Vault Payments**, a cloud-native payments processing platform that natively represents payments as ISO 20022 messages.

- Website — https://www.thoughtmachine.net/
- Vault Core — https://www.thoughtmachine.net/vault-core
- Vault Payments — https://www.thoughtmachine.net/vaultpayments
- Documentation (partner login required) — https://docs.thoughtmachine.net/
- GitHub — https://github.com/thought-machine

## API surface

| API | Product | Style |
|---|---|---|
| Core API | Vault Core | REST — external integrations (channels, CRM, operator UI) |
| Posting API | Vault Core | financial movements against the real-time ledger |
| Streaming API | Vault Core | Kafka — accounting, balance and customer events |
| Migration API | Vault Core | Kafka — bulk legacy core data loads |
| Contracts API | Vault Core | smart contract functions |
| Vault Payments APIs | Vault Payments | REST + real-time streaming |

Base URLs are per-deployment (SaaS tenant or bank-hosted cluster) and are not published publicly.

## What the enrichment pass found (2026-08-02)

**No public machine-readable contract.** Probed `/openapi.json`, `/openapi.yaml`, `/swagger.json`, `/api-docs`, `/docs`, `/redoc` and `/asyncapi.yaml` against `www.`, `docs.`, `portal.` and `api.` hosts. Every path on the documentation host answers `302` to the `auth.thoughtmachine.net` SSO flow; `api.thoughtmachine.net` does not resolve. No GraphQL endpoint, **no MCP server**, and **no A2A agent card** (`/.well-known/agent-card.json` and `/.well-known/agent.json` miss on every host — no `a2a/` artifact was written).

What *is* anonymously observable and captured here:

- `well-known/` — full probe record; the OIDC discovery document for the Vault portal identity provider (Authentik) is real and saved verbatim. The only `security.txt` found is the upstream Authentik product default (expired, points at `security@goauthentik.io`) — it is **not** a Thought Machine disclosure policy and no `SecurityTxt`/`Security` pointer was wired from it.
- `authentication/` + `scopes/` — the OIDC/OAuth 2.0 profile of the portal SSO (authorization_code + PKCE S256, client_credentials, device_code; scopes `openid`, `email`, `profile`, `entitlements`, `role`). Scoped explicitly to the portal, **not** the Vault data APIs.
- `sandbox/` — the TM Sandbox APIs / TM Sandbox Environment programme, access by application form under published terms.
- `conformance/` — ISO 27001, AICPA SOC 2 and ISO 22301 accreditation badges published in the site footer (each verified visually); ISO 20022 native in Vault Payments; OAuth 2.0 / OIDC / PKCE probed.
- `lifecycle/` — annual major-release cadence and the Contracts Language API v3→v4 deprecation path (third-party partner evidence; Thought Machine's own release notes are gated). **No public status page and no published deprecation policy** — absence recorded, so no `StatusPage`/`Deprecation` pointers.
- `asyncapi/` — the Kafka streaming event surface. No AsyncAPI document and no HTTP webhooks, so the `Webhooks` pointer was deliberately withheld.
- `security/` — probed TLS/HSTS/DNSSEC/CAA/SPF/DMARC. No vulnerability disclosure programme and no trust centre found.
- `llms/` — generated (no provider-published `/llms.txt`).

No public first-party client SDKs exist. The `thought-machine` GitHub org publishes engineering tooling (the Please build system, Dracon, Prometheus exporters, a Go ISO 20022 message library) rather than Vault API clients, so no `packages/`/`SDKs` pointer was emitted.
