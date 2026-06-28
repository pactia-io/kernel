# Changelog

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
