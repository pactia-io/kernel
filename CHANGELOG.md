# Changelog

## 1.1.0 — 2026-06-30

### Added
- **4 new field modifiers**: `@@uuid`, `@@email`, `@@min`, `@@max` — format and value constraints on entity fields.
- **`@@min` and `@@max` support modifier shorthand**: `@@min(0)`, `@@max(255)` with inline numeric values.
- **Field specs on `@api`**: `method` and `path` required, `summary` optional. Redundant HTTP-specific fields removed.
- **Field spec on `@event`**: `payload` required, `version` optional (defaults to `"1"`).
- **Field specs on `@auth`**: `roles`, `scopes`, `actors` — all optional arrays. At least one expected.
- **Field specs on `@entity`**: `table` and `schema` — both optional strings.

### Changed
- **Prose tightened** — removed implementation details from `@entity`, `@@fk`, `@@index`, `@@pii`, `@@secret`, `@integration`, `@store`.
- **`@@modifier` scope expanded** from `field` to `service, field` — modifiers like `@@input`/`@@output` work in service context.
- **Surface/CLI tags moved** from `service` to `product` scope: `@screen`, `@nav`, `@bind`, `@command`, `@flag`, `@arg`.
- **`@event` and `@on` scopes** narrowed from `module, service, model` to `module, service` — events are behavioral, not data.
- **Whitespace normalized** on all `in` clauses.
- **`@tag` and `@@modifier`** now include prose descriptions.

## 1.0.0 — 2026-06-29

### Added
- **38 symbols**: 28 tags `@` + 10 modifiers `@@` — no macros.
- **`@tag`** — inline tag constructor; define and use a one-off tag anywhere without pre-declaring it in a package.
- **`@@modifier`** — inline modifier constructor; define and use a one-off modifier on the next host tag anywhere without pre-declaring it.
- **New tags**: `@relation`, `@states`, `@store`, `@on`, `@screen`, `@nav`, `@command`, `@flag`, `@arg`.
- **Modifier sigil `@@`**: field modifiers (`@@pk`, `@@fk`, `@@unique`, `@@index`, `@@nullable`, `@@pii`, `@@secret`) and service modifiers (`@@input`, `@@output`) now use `@@` instead of `@`.

### Removed
- **All `#` macros**: `#list`, `#detail`, `#create`, `#update`, `#delete`, `#paginated`, `#owner`, `#idempotent` — moved out of kernel; stacks define their own behavior macros.
- **Non-kernel symbols**: `@pactia`, `@stack`, `@tenancy`, `@security`, `@environment`, `@deploy`, `@gate`, `@policy`, `@observe`, `@public`, `@status`, `#database`, `#publish`, `#subscribe`, `@contract`, `@@paged`, `@@deprecated`, `@@readonly`, `@@computed`, `@@filterable`, `@@sortable`, `@@searchable`, `@@id`, `@@created_at`, `@@updated_at`, `@@soft_delete`, `@lifecycle`, `@dependencies`, `@client`.

### Changed
- **Single-package layout**: `pactia.toml` + `index.pactia` at repo root (was multi-slice: `core/`, `data/`, `ops/`, etc.).
- **Version bump**: `0.0.1` → `1.0.0`.
- **Scopes rationalized**: `@actor`, `@config`, `@rule`, `@must`, `@error` moved to `product, module` (cross-cutting); `@test` moved to `service`; `@surface` moved to `product`.
- **Provenance**: kernel now matches [enrichment roadmap](../../plans/library-enrichment/pactia-package-enrichment-roadmap.md) Phase 0 — Kernel Round 2.
