# Contract-Driven Development (CDD)

A methodology where analysis comes first, then contracts, then implementation.

## What is CDD?

CDD enforces a disciplined development flow:

1. **Analyze the reference** — Before anything, get micro-detail perception of what you're building toward
2. **Write a contract** — Define requirements and tests grounded in the analysis
3. **Implement against it** — Build code that makes the tests pass
4. **Compare output to baseline** — Same analysis tool on output vs reference
5. **Iterate with precision** — Deviations are exact, fixes are targeted
6. **Freeze when stable** — Lock the contract, changes require version bumps

The analysis is the foundation. The contract references it. The implementation fulfills it.

---

## Phase 0: The Hard Gate

**Before any implementation begins, you must have:**

| Requirement | Question |
|-------------|----------|
| Reference artifact | What existing thing defines success? (PDF, mockup, audio file, API spec, sketch) |
| Analysis tool | What tool gives micro-detail perception of that artifact? |
| Baseline | What does the tool output when run on the reference? |
| Agreement | Human confirms the tool captures what matters |

**No reference + No tool = No implementation**

If a reference doesn't exist, create one (wireframe, sketch, example file). If a tool doesn't exist, build one first. CDD doesn't proceed without this foundation.

### Why the hard gate?

Human descriptions are imprecise:
- "The spacing looks off" → Which spacing? By how much?
- "Match the original form" → What are the exact dimensions?
- "Make it sound warm" → What frequencies? What characteristics?

Analysis tools provide precision:
- "Element R2_5 at (418, 523) is 127x21pt"
- "Spacing between rows is 15pt"
- "Spectral centroid at 1.2kHz, RMS at -18dB"

This precision eliminates guesswork and enables targeted iteration.

---

## The Development Loop

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   Reference ──► Analyze ──► Baseline                            │
│       │                         │                               │
│       │                         ▼                               │
│       │                   Write Contract                        │
│       │                         │                               │
│       │                         ▼                               │
│       │                    Implement                            │
│       │                         │                               │
│       │                         ▼                               │
│       │              Analyze Output ──► Compare                 │
│       │                                    │                    │
│       │                         ┌─────────┴─────────┐           │
│       │                         │                   │           │
│       │                    Deviations?          Match?          │
│       │                         │                   │           │
│       │                         ▼                   ▼           │
│       │                   Fix & Iterate         Done            │
│       │                         │                               │
│       └─────────────────────────┘                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Collaboration Model

CDD is a collaboration between human and AI:

| Role | Human | Claude |
|------|-------|--------|
| Reference | Provides or locates | Analyzes at micro-detail |
| Tooling | Signs off that it captures what matters | Creates/runs analysis |
| Calibration | Sets acceptable thresholds | Reports exact deviations |
| Direction | Decides what's important | Provides precision |
| Signoff | Final approval | Reports readiness |

**Neither works alone:**
- Claude without tooling = guessing from descriptions
- Human without Claude's analysis = imprecise feedback
- Together = spec emerges from dialogue

---

## Mandatory Gates

CDD enforces four gates. **You cannot proceed past a gate until it passes.**

| Gate | Command | Passes When | Blocks |
|------|---------|-------------|--------|
| G0: Analyze | `cdd analyze <ref>` | Baseline exists | Contract writing |
| G1: Lint | `cdd lint contracts/` | Exit 0 | Implementation start |
| G2: Test | `cdd test contracts/` | Exit 0 | Contract freeze |
| G3: Freeze | `status: frozen` | Human sets it | Deploy |

```
Reference ──► G0: Analyze ──► Contract ──► G1: Lint ──► Implement ──► G2: Test ──► G3: Freeze ──► Deploy
```

**Gates are not suggestions. They are hard stops.**

Deploying without passing all gates is a process violation. See [SPEC.md](SPEC.md#mandatory-gates) for details.

---

## Anti-Patterns

Avoid these common mistakes:

| Anti-Pattern | Wrong | Right |
|--------------|-------|-------|
| AP1: Visual verification | "Can you see the icon?" | `cdd analyze` + check JSON |
| AP2: Manual grep | `grep pattern file` | Contract test with `matches` |
| AP3: Skip gates | "Deploy now, test later" | Tests must pass before deploy |
| AP4: External tools | Standalone `analyze.py` | Add to `cdd analyze` |
| AP5: Draft in prod | Deploy with `status: draft` | Freeze before deploy |
| AP6: No source refs | Requirements without evidence | `source_ref: SRC001#E1_5` |

See [SPEC.md](SPEC.md#appendix-anti-patterns) for detailed examples.

---

## Contract Example

```yaml
# contracts/project.yaml
project: my-project
cdd_spec: 1.1.5
version: 1.0.0
status: draft

goal: |
  What you're building and why.

success_criteria:
  - Criterion 1 (reference: baseline analysis)
  - Criterion 2

components:
  - component_a
```

```yaml
# contracts/component_a.yaml
contract: component_a
version: 1.0.0
status: draft
description: What this component does.

runner:
  executor: python
  entry: src/component_a.py
  symbol: main_function

requirements:
  - id: R001
    priority: must
    description: Input fields match reference dimensions
    acceptance_criteria:
      - Field height 21pt (from baseline element R1_0)

tests:
  - id: T001
    name: field_dimensions_match
    requirement: R001
    type: unit
    steps:
      - action: call
        with: { input: "test" }
        save_as: result
    assert:
      - op: approx
        actual: $.result.field_height
        expected: 21
        tolerance: 1
```

---

## Project Structure

Recommended layout for CDD projects:

```
my-project/
├── analysis/
│   ├── baseline/           # Analysis of reference artifact
│   └── output/             # Analysis of generated output
├── reference/
│   └── original.pdf        # The reference artifact
├── contracts/
│   ├── project.yaml
│   └── component_a.yaml
├── src/
│   └── component_a.py
└── output/
    └── generated.pdf       # What you built
```

---

## Tooling

The reference implementation of CDD tooling is available at:

**[github.com/thegdyne/cdd-tooling](https://github.com/thegdyne/cdd-tooling)**

```bash
pip install cdd-tooling
```

---

## Documentation

- **[SPEC.md](SPEC.md)** — The normative specification (contract schema, assertion operators, report format)
- **[CHANGELOG.md](CHANGELOG.md)** — Spec version history

---

## Spec Version

Current: **1.1.5** (frozen)

The `cdd_spec` field in contracts declares which spec version they target. Tooling validates compatibility.

---

## License

MIT
