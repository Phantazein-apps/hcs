# HCS Extension Namespace Registry

Extensions hold everything the core profile deliberately excludes. Each extension = one JSON Schema + one scope pair (`profile.<ext>:read` / `:write`) + one storage partition.

## Registered `hcs.*` namespaces (v0.1)

| Namespace | Status | Schema | Sensitivity class |
|---|---|---|---|
| `hcs.health` | Draft (reference example) | [`../schemas/hcs.health.schema.json`](../schemas/hcs.health.schema.json) | **Special category** — separate encryption key required |
| `hcs.residency` | Reserved | — | **Special category** — citizenship(s), visas, residency & tax status |
| `hcs.legal` | Reserved | — | High — matters, contracts of record, attorney contacts |
| `hcs.family` | Reserved | — | High — family structure, dependents, key dates, emergency contacts |
| `hcs.finance` | Reserved | — | High — account references (never credentials), obligations, advisors |
| `hcs.home` | Reserved | — | Medium — addresses, household, vehicles |

"Reserved" = the namespace and its sensitivity class are fixed by this registry; the schema lands in v0.2 via PR.

## Authoring rules

1. **One concern per namespace.** If a field could plausibly belong to two namespaces, it belongs to the more sensitive one.
2. **References, not secrets.** Extensions store facts and references (policy number *reference*, account *alias*) — never credentials, tokens, or full identifiers usable for fraud on their own.
3. **`additionalProperties: false`.** Extensions are strict schemas; free text goes in designated `notes` fields, unbounded personal narrative goes in memory, not the profile.
4. **Third-party namespaces** use reverse-DNS prefixes (`com.example.pets`). The `hcs.*` prefix is assigned only through this registry.
5. **Sensitivity class is normative.** Special-category namespaces inherit the §4.3/§9 encryption requirements of the spec.

## Proposing an extension

Open a PR adding a row to this registry + a schema in `schemas/`, with a short rationale: what user story requires an *assistant* to know this, and why memory pages aren't the right home for it.
