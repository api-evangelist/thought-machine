# Thought Machine

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
