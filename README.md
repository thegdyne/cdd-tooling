# CDD Tooling

Reference implementation of [Contract-Driven Development](https://github.com/thegdyne/cdd) tooling.

Implements **CDD Spec 1.1.5**

## Installation

```bash
# Via pip
pip install cdd-tooling

# With PDF analysis support
pip install cdd-tooling[pdf]

# Via pipx (recommended for CLI use)
pipx install cdd-tooling[pdf]

# From source (editable)
git clone https://github.com/thegdyne/cdd-tooling.git
cd cdd-tooling
pip install -e ".[dev,pdf]"
```

## Quick Start

```bash
# Analyze a reference artifact
cdd analyze reference.pdf -o analysis/baseline/

# Write contracts grounded in the analysis
# ... edit contracts/project.yaml ...

# Verify paths resolve
cdd paths contracts/

# Validate contracts
cdd lint contracts/

# Run tests
cdd test contracts/

# Compare output to baseline
cdd analyze output.pdf -o analysis/output/
cdd compare analysis/baseline/ analysis/output/
```

## Commands

| Command | Description |
|---------|-------------|
| `cdd spec --version` | Show spec/tool version |
| `cdd paths <contracts/>` | Verify file paths in contracts resolve |
| `cdd analyze <file> -o <dir>` | Extract baseline from PDF/HTML |
| `cdd compare <baseline> <output>` | Compare two analyses |
| `cdd lint <contracts/>` | Validate contract schema + coverage |
| `cdd test <contracts/>` | Run contract tests |
| `cdd coverage <contracts/>` | Requirement coverage report |

### Paths

```bash
# Verify all file paths in contracts resolve correctly
cdd paths contracts/

# Single contract
cdd paths contracts/feature.yaml

# JSON output
cdd paths contracts/ --json
```

Pre-gate check that catches path errors before running tests. Extracts paths from `files:` fields and shell command arguments, verifies they resolve relative to the contract directory.

### Analyze

```bash
# PDF analysis (requires [pdf] extra)
cdd analyze form.pdf -o analysis/

# HTML analysis
cdd analyze wireframe.html -o analysis/

# Output includes:
#   - structure.json (element catalog)
#   - page images (for PDF)
#   - elements.md (human-readable summary)
#   - layout.md (form field associations)
```

### Compare

```bash
# Compare baseline to output (PDF or HTML)
cdd compare analysis/baseline/ analysis/output/

# Shows exact deviations:
#   ✗ Element spacing: baseline 15pt, output 12pt (deviation: 3pt)
#   ✓ Field dimensions: match within tolerance

# JSON output for CI
cdd compare analysis/baseline/ analysis/output/ --json
```

### Lint

```bash
# Basic validation
cdd lint contracts/

# Strict mode (warnings become errors)
cdd lint contracts/ --strict

# JSON output for CI
cdd lint contracts/ --json
```

### Test

```bash
# Run all tests
cdd test contracts/

# With variables
cdd test contracts/ --var target=my_component

# Filter by test ID
cdd test contracts/ --only T001

# JSON output
cdd test contracts/ --json
```

### Coverage

```bash
# Show requirement coverage
cdd coverage contracts/

# Fail if uncovered requirements exist
cdd coverage contracts/ --strict
```

## Executors

| Executor | Status | Actions | Use Case |
|----------|--------|---------|----------|
| `python` | ✅ Full | `call`, `call_n` | Python functions |
| `shell` | ✅ Full | `shell` | CLI tools, scripts |
| `static` | ⚠️ Partial | (file assertions) | Regex/content checks |
| `sclang` | ⚠️ Stub | `render_nrt` | SuperCollider audio |

**Note:** The `sclang` executor currently returns "not_implemented". Full NRT rendering support is planned for Phase 3.

## Known Issues

| Issue | Status | Workaround |
|-------|--------|------------|
| `cdd test file.yaml` runs all contracts in directory | Open | Use `--only T001` to filter, or test in isolated directory |
| Static executor `$.file.content` returns null | Open | Use shell executor with grep instead |

### Isolation Workaround

When testing a single contract in a project with multiple contracts:

```bash
# Create isolated test environment
mkdir -p /tmp/cdd-work/contracts
cp contracts/feature.yaml /tmp/cdd-work/contracts/

# Symlink source directories
ln -s ~/project/src /tmp/cdd-work/src

# Test in isolation
cd /tmp/cdd-work
cdd paths contracts/feature.yaml
cdd test contracts/feature.yaml

# Cleanup
rm -rf /tmp/cdd-work
```

## Spec Compatibility

This tooling implements [CDD Spec](https://github.com/thegdyne/cdd) version **1.1.5**.

| Tooling Version | Spec Version | Status |
|-----------------|--------------|--------|
| 0.1.x | 1.1.x | Current |

The `cdd_spec` field in your contracts declares which spec version you're targeting. Tooling validates compatibility at runtime.

## Documentation

- **[CDD Spec](https://github.com/thegdyne/cdd)** — The methodology and normative specification
- **[CHANGELOG.md](CHANGELOG.md)** — Tooling version history
- **[ROADMAP.md](ROADMAP.md)** — Implementation status and future plans
- **[CDD_ANALYSIS_AND_IMPROVEMENTS.md](CDD_ANALYSIS_AND_IMPROVEMENTS.md)** — Detailed implementation notes

## Development

```bash
# Clone
git clone https://github.com/thegdyne/cdd-tooling.git
cd cdd-tooling

# Install in dev mode
pip install -e ".[dev,pdf]"

# Run tests
pytest

# Lint
ruff check src/
```

## License

MIT
