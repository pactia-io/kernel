# @pactia/kernel

Standard library of **domain-neutral software intent** for Pactia.

Kernel tags and macros describe *what* a product must satisfy across architecture, data, APIs, policy, quality, deployment, and operations — without naming a language, framework, wire format, or business domain.

Publish target: **`github.com/pactia-lang/kernel`** (git remote + semver tags, Go-style).  
Consume with: `import @pactia/kernel;` in `product.pactia` / package `index.pactia`.

Stack, protocol, surface, and vertical packages live in **[pactia-lang/pactia-io](https://github.com/pactia-lang/pactia-io)** — not in this repo.

Part of: [pactia-lang/spec](../spec/docs/packages.md) | [language-spec](../spec/docs/language-spec.md)

---

## Role in the stack

| Layer | Package | Publish | Owns |
| --- | --- | --- | --- |
| **Kernel** | `@pactia/kernel` (+ `@pactia/kernel-*`) | [pactia-lang/kernel](https://github.com/pactia-lang/kernel) | Universal structure and engineering intent |
| **Stack** | `@pactia/*` stack crates | [pactia-lang/pactia-io](https://github.com/pactia-lang/pactia-io) | Platform law — language, framework, CI, deploy defaults |
| **Protocol** | `@pactia/protocol-*` | [pactia-lang/pactia-io](https://github.com/pactia-lang/pactia-io) | Wire format — REST, gRPC, GraphQL, … |
| **Vertical** | `@pactia/*` domain patterns | [pactia-lang/pactia-io](https://github.com/pactia-lang/pactia-io) | Payments, catalog, compliance entities, … |
| **Surface** | `@pactia/surface-*` | [pactia-lang/pactia-io](https://github.com/pactia-lang/pactia-io) | UI framework registration |

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

## Repository layout

```text
kernel/
  README.md
  core/
    pactia.toml               # @pactia/kernel
    index.pactia              # core tags + @pactia
  data/pactia.toml + index.pactia       # @pactia/kernel-data
  ops/pactia.toml + index.pactia        # @pactia/kernel-ops
  quality/pactia.toml + index.pactia    # @pactia/kernel-quality
  governance/pactia.toml + index.pactia # @pactia/kernel-governance
  engineering/pactia.toml + index.pactia
  architecture/pactia.toml + index.pactia
```

Each slice is a separate published package at v0.0.1 — `pactia.toml` + `index.pactia` only, prose-first export defs.

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
| **product** | `@topology`, `@tenancy`, `@guide`, `@stack`, `@pactia` | System shape, isolation, guidance; `@stack` is optional profile block after a stack-package macro imported in source |
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
| `@unit_test` | Isolated logic — mocks, no real wire or database |
| `@load_test` | Performance and capacity validation under stress |
| `@benchmark` | Timed baselines for a specific path — latency and regression tracking |
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
| Language, framework, ORM, CI YAML | Stack packages in [pactia-io](https://github.com/pactia-lang/pactia-io) |
| REST `method` / `path`, gRPC proto | Protocol packages in [pactia-io](https://github.com/pactia-lang/pactia-io) |
| `Order`, `Invoice`, KYC rules | Product source or vertical packages in [pactia-io](https://github.com/pactia-lang/pactia-io) |
| React / SwiftUI components | Surface packages in [pactia-io](https://github.com/pactia-lang/pactia-io) |
| Pagination defaults, list helpers | Stack macros |

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
| **1** | Core + satellite packages published from this repo (`core/`, `data/`, `ops/`, …) at v0.0.1 |
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
