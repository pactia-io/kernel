# @pactia/kernel

**The default package.** Universal across every surface Pactia targets.

Kernel tags and modifiers describe *what* a product must satisfy — without naming a language, framework, wire format, or business domain.

Publish target: **`github.com/pactia-io/kernel`** (git remote + semver tags, Go-style).  
Consume with: `import @pactia/kernel;` in `product.pactia` / package `index.pactia`.

Stack, protocol, surface, and vertical packages live in **[pactia-lang/pactia-io](https://github.com/pactia-lang/pactia-io)** — not in this repo.

Part of: [pactia-lang/spec](../spec/docs/packages.md) | [language-spec](../spec/docs/language-spec.md)

---

## Scope

Pactia describes any software product built around **actors, operations, and data** — backend services, mobile apps, desktop apps, frontends, and CLI tools alike. The kernel is the minimal, platform-agnostic vocabulary every one of those paradigms needs.

The test for kernel inclusion: does every paradigm need this, is it declarative, and is it inexpressible by combining other kernel primitives? If any answer is no, it belongs in a package, not the kernel.

---

## Design laws

1. **Domain-neutral** — useful for APIs, CLIs, mobile backends, frontends, and CLI tools alike.
2. **Technology-neutral** — describe structure, operations, and contracts; stacks supply implementation.
3. **Prose-first** — every symbol works with `>` guidance lines; structured fields are optional upgrades.
4. **Open extension** — tags accept extra fields; products extend without forking kernel.
5. **Single package** — one `pactia.toml` + `index.pactia` at repo root. No slices, no sub-packages.
6. **Source of truth** — `index.pactia` only (`export def`); no generated JSON manifest.

---

## Repository layout

```text
kernel/
  README.md
  pactia.toml                  # @pactia/kernel v1.0.0
  index.pactia                 # all kernel exports
```

Former kernel slices (`kernel-data`, `kernel-ops`, `kernel-quality`, `kernel-governance`, `kernel-engineering`, `kernel-architecture`, `kernel-privacy`, `kernel-security`) are community packages — see the [enrichment roadmap](../../plans/library-enrichment/pactia-package-enrichment-roadmap.md).

---

## Kernel symbol reference (v1.1.0 — 42 symbols: 28 @ tags + 14 @@ modifiers)

### Meta — inline tag & modifier constructors

| Symbol | Purpose |
|---|---|
| `@tag` | Inline tag constructor — define and use a one-off tag anywhere without pre-declaring it in a package. Cross-cutting (`product, module, service, model`). |
| `@@modifier` | Inline modifier constructor — define and use a one-off modifier on the next host tag anywhere without pre-declaring it. Cross-cutting (`product, module, service, model`). |

### Structure & shape — `model`-scoped

| Symbol | Purpose |
|---|---|
| `@entity` | A data shape with identity (has a primary key) |
| `@enum` | A closed set of named values |
| `@relation` | Connects two entities (owns, has, references) |
| `@states` | Legal state transitions on a field |
| `@store` | Device-local / client-only persistence (mobile/desktop offline data) |

### Field modifiers — `model`-scoped, `@@`

| Symbol | Purpose |
|---|---|
| `@@pk` | Primary key — uniquely identifies an entity instance |
| `@@fk` | Foreign key — references another entity |
| `@@unique` | Uniqueness constraint — no two instances may share this value |
| `@@index` | Indexed for fast lookup |
| `@@nullable` | Optional — may be absent |
| `@@pii` | Personally identifiable — triggers privacy and retention policies |
| `@@secret` | Sensitive — never logged, stored in a secure vault |
| `@@uuid` | UUID v4 format constraint |
| `@@email` | Email format constraint |
| `@@min` | Minimum value/length — `@@min(0)` |
| `@@max` | Maximum value/length — `@@max(255)` |

### Operations — `service`-scoped

| Symbol | Purpose |
|---|---|
| `@api` | A callable operation — protocol-agnostic, binds to any surface |
| `@auth` | Who may call this operation |
| `@throws` | Declared error outcomes for this operation |
| `@emit` | Event(s) produced by this operation |
| `@@input` | Request/input shape modifier |
| `@@output` | Response/output shape modifier |

### Identity & access — cross-cutting

| Symbol | Purpose |
|---|---|
| `@actor` | Defines a role and its capabilities — product or module scope |

### Events — cross-cutting

| Symbol | Purpose |
|---|---|
| `@event` | Declares an event name + payload shape |
| `@on` | Wiring: event → consumer |

### Errors — cross-cutting

| Symbol | Purpose |
|---|---|
| `@error` | Declares an error code + meaning, referenced by `@throws` |

### Rules & obligations — cross-cutting

| Symbol | Purpose |
|---|---|
| `@rule` | An enforceable invariant — declarative, always true |
| `@must` | An obligation: "when X happens, Y must follow" |

### Configuration — cross-cutting

| Symbol | Purpose |
|---|---|
| `@config` | Required/optional externally-supplied value (env var, API key, license key) |

### Product structure

| Symbol | Purpose |
|---|---|
| `@topology` | Product shape: backend, client, hybrid, and sub-shapes |

### Testing

| Symbol | Purpose |
|---|---|
| `@test` | An acceptance scenario — given/when/then shape |

### Surfaces & binding

| Symbol | Purpose |
|---|---|
| `@surface` | UI or interaction surface — web, mobile, desktop, or cli |
| `@bind` | Connects a surface element to an operation |
| `@screen` | Names a UI view/screen within a surface |
| `@nav` | How a screen is reached — tab, stack push, modal, deep-link route |
| `@command` | Names a CLI command, binds to an `@api` |
| `@flag` | Named option on a CLI command — maps to `@@input` fields |
| `@arg` | Positional argument on a CLI command — maps to `@@input` fields |

### Integration

| Symbol | Purpose |
|---|---|
| `@integration` | Binding to an external system — third-party API, webhook, device sensor |

### Guidance

| Symbol | Purpose |
|---|---|
| `@guide` | AI-facing advisory text — never enforced, never gated by conformance |

---

## Coverage by paradigm

| Paradigm | Status | Notes |
|---|---|---|
| Backend / API service | Full | Original design target — `service`, `@api`, `@auth`, `@entity`, `@event` |
| Mobile (Flutter, SwiftUI, React Native) | Full | `@surface mobile` + `@screen` + `@nav` + `@store` cover navigation and offline data |
| Desktop (Electron, Tauri, native) | Full | Same surface mechanism as mobile |
| Frontend / SPA | Full | `@bind` targeting `@integration` covers binding to external or non-Pactia backends |
| CLI tools | Full | `@surface cli` + `@command` + `@flag` + `@arg`, same `@bind` mechanism |
| Embedded / firmware | Out of scope | No actors, no operations, no surface — below the contract line |
| Batch / ML pipelines | Out of scope | Algorithmic, not contract-shaped |

---

## Explicitly excluded from kernel — these are packages

| Concept | Lives in |
|---|---|
| Language, framework, runtime specifics | Stack packages (`@pactia/rust-stack`, …) |
| Deploy specifics (replicas, regions, gates) | Community ops packages |
| Security specifics (OWASP, encryption detail) | `@pactia/kernel-security` |
| Privacy/compliance specifics (GDPR, retention) | `@pactia/kernel-privacy`, `@pactia/kernel-governance` |
| Observability (SLOs, metrics, alerts) | Community ops packages |
| Protocol wire formats (REST, gRPC, GraphQL) | Protocol packages |
| Database specifics (Postgres, Mongo, SQLite) | Database packages |
| `function`, `class`, `interface` as code | Nowhere — below the contract line, in any paradigm |

---

## Development

Kernel is a normal Pactia library package:

```text
pactia.toml     # name, version, kind = "library"
index.pactia    # export def @… / @@… / #…
```

Validate by compiling products that import kernel (see [pactiac](../pactiac) relay fixture).

```bash
cd ../pactiac && npm test
```

---

## License

MIT — see [LICENSE](LICENSE) when added.
