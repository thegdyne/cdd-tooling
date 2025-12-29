# Changelog

Spec changes only. For tooling changes, see [cdd-tooling CHANGELOG](https://github.com/thegdyne/cdd-tooling/blob/main/CHANGELOG.md).

## [1.1.5] - 2025-12-29

### Added
- Mandatory Gates section (G0-G3 with sequence diagram)
- Process Checkpoints table
- Anti-Patterns appendix (AP1-AP6)

### Compatibility
- **No behavior change** — clarifies process enforcement without changing contract schema

## [1.1.4] - 2025-12-29

### Added
- "Analysis precedes contracts" as first principle
- Reference, Analysis, Baseline terms added to Glossary
- Reference Analysis and Compare phases added to The Model diagram

### Compatibility
- **No behavior change** — clarifies process without changing contract schema

## [1.1.3] - 2025-12-28

### Fixed
- Clarified variable interpolation in shell command arguments

### Compatibility
- **No behavior change**

## [1.1.2] - 2025-12-28

### Fixed
- Clarified repo_root detection behavior

### Compatibility
- **No behavior change**

## [1.1.1] - 2025-12-27

### Added
- Native static file scanning specification

### Compatibility
- **Additive, backwards compatible**

## [1.1.0] - 2025-12-27

### Added
- `cdd_spec` field in project contracts — required for `status: frozen`, optional for `draft`
- Version compatibility checking semantics
- `not_matches` operator — inverse of `matches`
- `pattern` field on assertions — alternative to `expected` for regex operators
- `message` field on assertions — user-provided context for failure reporting
- `{var}` interpolation in `files:` globs

### Compatibility
- **Additive, backwards compatible** — existing contracts continue to work

## [1.0.14] - 2025-12-27

### Added
- Normative Step Fields table
- Clarified `contains` on arrays uses exact element equality

### Compatibility
- **No behavior change**

## [1.0.13] - 2025-12-27

### Fixed
- `±` encoding issue in spec
- Added `$.env.os_family` for platform skip logic

### Compatibility
- **No behavior change**

## [1.0.12] - 2025-12-27

### Added
- Compatibility Promise with table format
- Normative Core definition
- "Reference runner is arbiter" rule

### Compatibility
- **No behavior change**

## [1.0.0] - 2025-12-27

### Added
- Initial frozen release
- Complete schema with field tables
- Assertion DSL with 12 operators
- JSONPath resolution rules
- Step/action vocabulary with executor validity matrix
- Parameterisation via CLI and matrix
- Report format specification

---

## Version Semantics

| Bump | When | Example |
|------|------|---------|
| Patch (1.0.x) | Clarifications, typos | 1.0.10 → 1.0.11 |
| Minor (1.x.0) | New optional features, backwards compatible | 1.0.11 → 1.1.0 |
| Major (x.0.0) | Breaking changes to normative sections | 1.1.0 → 2.0.0 |
