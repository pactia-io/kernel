# @pactia/kernel

Standard library of **domain-neutral software intent** for Pactia.

Kernel tags and macros describe *what* a product must satisfy across architecture, data, APIs, policy, quality, deployment, and operations — without naming a language, framework, wire format, or business domain.

Publish target: **`github.com/pactia-lang/kernel`** (git remote + semver tags, Go-style).  
Consume with: `import @pactia/kernel;` in `product.pactia` / package `index.pactia`.

Part of: [pactia-lang/spec](../spec/docs/packages.md) | [language-spec](../spec/docs/language-spec.md)

---

## Role in the stack

| Layer | Package | Owns |
| --- | --- | --- |
| **Kernel** | `@pactia/kernel` (+ optional `@pactia/kernel-*`) | Universal structure and engineering intent |
| **Stack** | `@pactia/rust-anb`, … | Platform law — language, framework, CI, deploy defaults |
| **Protocol** | `@pactia/protocol-rest`, … | Wire format — `method`, `path`, gRPC, GraphQL |
| **Vertical** | `@pactia/kyc-compliance`, … | Domain patterns — payments, catalog, compliance entities |
| **Surface** | `@pactia/surface-react`, … | UI framework registration |

Kernel answers **what**. Stack and protocol answer **how**.

---

## Design laws

1. **Domain-neutral** — useful for APIs, CLIs, batch jobs, mobile backends, and embedded services alike.
2. **Technology-neutral** — say `sql` or `cache`, not `sqlx` or `Redis`; stacks supply implementations.
3. **Prose-first** — every tag works with `>` guidance lines; structured fields are optional upgrades.
4. **Open extension** — tags accept extra fields; products extend without forking kernel.
5. **Small core, rich satellites** — one mandatory core package; optional `@pactia/kernel-*` slices for depth.
6. **Source of truth** — `index.pactia` only (`export def`); no generated JSON manifest.

**Inclusion test:** Would a marketplace API, a CLI tool, and a static site generator all plausibly use this tag? If no, it belongs in a stack, protocol, or vertical package.

---

## Planned repository layout

```text
kernel/
  README.md                 # this file
  pactia.toml               # [package] name = "@pactia/kernel"
  index.pactia              # core export defs (Phase 1)
  CHANGELOG.md

  # optional slices (Phase 2+) — separate index.pactia or separate published packages
  data/index.pactia         # @store, @cache, @schema, …
  ops/index.pactia            # @deploy, @observe, @slo, …
  quality/index.pactia        # @test, @integration_test, @gate, …
  governance/index.pactia     # @policy, @compliance, @audit, …
  engineering/index.pactia    # @pattern, @ownership, @documentation, …
```

Products import only what they need:

```pactia
import @pactia/kernel;
import @pactia/kernel-data;      // when persistence story matters
import @pactia/kernel-ops;       // when deploy / monitor matter
```

---

## Tag taxonomy

### Core (`@pactia/kernel`) — Phase 1

Structure every product shares.

| Scope | Tags / macros | Purpose |
| --- | --- | --- |
| **product** | `@topology`, `@tenancy`, `@guide`, `@stack` | System shape, isolation, guidance; `@stack` is optional profile block (stack binding is `#rust_anb` in stack packages) |
| **module** | `@actor`, `@rule`, `@security`, `@integration`, `@event`, `@environment`, `@deploy`, `@gate` | Roles, rules, integrations, events, env, deploy intent, quality gates |
| **model** | `@entity`, `@enum`, `@relation`, `@states`, `@pk`, `@fk`, `@unique`, `@index`, `@nullable`, `@pii` | Data model and field constraints |
| **service** | `@api`, `@auth`, `@input`, `@output`, `@public`, `@emit`, `@throws`, `@status`, `@test`, `@must` | Operations, contracts, acceptance scenarios, obligations |
| **service** | `#create`, `#idempotent`, `#database` | Common operation macros |
| **surface** | `@surface` | Generic UI attachment (platform, screen, route) |
| **module** | `@policy`, `@config`, `@errors`, `@observe` | Policy hooks, configuration, error catalog, observability intent |

### Data (`@pactia/kernel-data`) — Phase 2

Persistence and data architecture intent.

| Tag | Purpose |
| --- | --- |
| `@store` | Storage kind: `sql`, `document`, `kv`, `graph`, `column` (not a vendor name) |
| `@cache` | Cache layers, TTL, invalidation strategy |
| `@schema` | Migration and schema versioning intent |
| `@consistency` | Strong / eventual / session consistency |
| `@query` | Read-side / CQRS query patterns |
| `@retain`, `@encrypt` | Retention and encryption intent on fields |

### Operations (`@pactia/kernel-ops`) — Phase 2

Runtime, deployment, monitoring.

| Tag | Purpose |
| --- | --- |
| `@container` | OCI image, health / ready probes |
| `@orchestration` | Kubernetes, Compose, bare metal (intent only) |
| `@rollout` | Blue/green, canary, rolling |
| `@slo`, `@alert`, `@runbook` | SLI/SLO, alerting, operational docs |
| `@feature` | Feature-flag intent |
| `@schedule`, `@batch`, `@stream` | Jobs, batch, streaming |

### Quality (`@pactia/kernel-quality`) — Phase 2

Testing and conformance beyond unit code.

| Tag | Purpose |
| --- | --- |
| `@integration_test` | Real DB, wire boundaries, external systems |
| `@contract_test` | Consumer-driven contracts |
| `@load_test` | Performance and capacity validation |
| `@gate` | CI gates — coverage, lint, security scan (extends module scope) |

### Governance (`@pactia/kernel-governance`) — Phase 2

Policy, compliance, audit.

| Tag | Purpose |
| --- | --- |
| `@compliance` | Generic compliance flags (GDPR, SOC2, …) |
| `@audit` | Audit logging requirements |
| `@secrets` | Secret handling — never in repo, rotation |
| `@residency` | Data residency / region constraints |
| `@ratelimit` | Rate limiting intent |

### Engineering (`@pactia/kernel-engineering`) — Phase 2

Repository and practice intent (for humans and agents, not a replacement for git).

| Tag | Purpose |
| --- | --- |
| `@layout` | Monorepo / polyrepo intent |
| `@ownership` | Team ownership (CODEOWNERS intent) |
| `@documentation` | ADRs, API docs, README requirements |
| `@versioning` | Semver, API deprecation policy |
| `@pattern` | Design patterns — repository, CQRS, saga, outbox, … |
| `@context`, `@capacity`, `@resilience`, `@flow` | System design — context, scale, resilience, data flow |

### Architecture extensions — Phase 3

| Tag | Purpose |
| --- | --- |
| `@backup`, `@disaster_recovery` | DR and backup |
| `@multiregion` | Multi-region topology |
| `@i18n`, `@a11y` | Localization and accessibility intent |
| `@cost` | FinOps / cost constraints |
| `@incident` | Incident response hooks |
| `@deprecated` | Deprecation policy on APIs and fields |

---

## Explicitly not in kernel

| Concern | Where it lives |
| --- | --- |
| Language, framework, ORM, CI YAML | Stack packages (`@pactia/rust-anb`, …) |
| REST `method` / `path`, gRPC proto | Protocol packages |
| `Order`, `Invoice`, KYC rules | Product source or vertical packages |
| React / SwiftUI components | Surface packages |
| Pagination defaults (`#paginated`, `#list`) | Stack macros |

---

## Altitude model

Kernel tags support graded precision (see [overview](../spec/docs/overview.md)):

| Altitude | Kernel usage |
| --- | --- |
| **0** | Prose only — no kernel import required |
| **1** | Light tagging — `@api`, `@auth`, `@entity` with partial fields |
| **2** | Full enforcement — wire fields via protocol import, `@test` / `@must`, `@gate` |

---

## Roadmap

| Phase | Deliverable |
| --- | --- |
| **1** | `pactia.toml` + `index.pactia` with core tags (relay-compatible); publish `v1.0.0` |
| **2** | Satellite packages or submodules: data, ops, quality, governance, engineering |
| **3** | Architecture extensions; `@pattern` macro library; JSON Schema bodies in spec sync |

---

## Development

Kernel is a normal Pactia library crate:

```text
pactia.toml     # Cargo.toml — name, version, kind = "library"
index.pactia    # lib.rs — export def @… / #… / @@…
```

Validate by compiling products that import kernel (see [pactiac](../pactiac) relay fixture and [marketplace](../examples/marketplace) example).

```bash
# after index.pactia exists
cd ../pactiac && npm test
cd ../examples/marketplace && ../../pactia/dist/cli.js build
```

---

## License

MIT — see [LICENSE](LICENSE) when added.
