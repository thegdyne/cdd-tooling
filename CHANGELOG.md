# Changelog

Tooling implementation changes. For spec changes, see [CDD Spec CHANGELOG](https://github.com/thegdyne/cdd/blob/main/CHANGELOG.md).

## [0.1.1] - 2025-12-29

### Fixed
- `cdd compare` now correctly routes HTML analyses to `compare_html_analyses()`
- Previously all comparisons used PDF comparison logic regardless of file type

### Changed
- Compare command help text updated to reflect HTML support
- ROADMAP updated to reflect actual implementation status

### Implements
- CDD Spec 1.1.5

## [0.1.0] - 2025-12-29

### Added
- Initial release extracted from cdd repo
- PDF analyzer (`cdd analyze *.pdf`) — requires `[pdf]` extra
- HTML analyzer (`cdd analyze *.html`)
- Analysis comparison (`cdd compare`)
- Contract linting (`cdd lint`)
- Contract testing (`cdd test`)
- Coverage reporting (`cdd coverage`)
- Python executor with `call`, `call_n` actions
- Shell executor with `shell` action
- Static executor for file content assertions (`matches`, `not_matches`)
- SuperCollider executor (stub — `render_nrt` returns "not_implemented")

### Executors
| Executor | Status | Actions |
|----------|--------|---------|
| `python` | ✅ Full | `call`, `call_n` with timing |
| `shell` | ✅ Full | Command execution, stdout/stderr capture |
| `static` | ⚠️ Partial | File scanning works; AST parsing is stub |
| `sclang` | ⚠️ Stub | Returns "not_implemented" |

### Implements
- CDD Spec 1.1.x

---

## Version Numbering

Tooling versions are independent of spec versions:

| Tooling | Spec | Notes |
|---------|------|-------|
| 0.1.x | 1.1.x | Initial implementation |

Tooling uses semver:
- **Patch** (0.1.x): Bug fixes, no API changes
- **Minor** (0.x.0): New features, backwards compatible
- **Major** (x.0.0): Breaking changes to CLI or output format
