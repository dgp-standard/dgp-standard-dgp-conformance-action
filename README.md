# DGP Conformance GitHub Action

Verify that your DGP v1.0 implementation passes all 8 canonical vectors.

**Use this action in your CI to prove conformance and earn the "DGP v1.0 Conformant" badge.**

---

## Usage

### Python Implementation

```yaml
name: DGP Conformance
on: [push, pull_request]

jobs:
  conformance:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Verify DGP v1.0 Conformance
        uses: Jtjr86/dgp-standard-dgp-conformance-action@v1
        with:
          language: python
          test_command: pytest tests/test_contract_compliance.py -v
```

### JavaScript/Node.js Implementation

```yaml
name: DGP Conformance
on: [push, pull_request]

jobs:
  conformance:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Verify DGP v1.0 Conformance
        uses: Jtjr86/dgp-standard-dgp-conformance-action@v1
        with:
          language: node
          test_command: npm test
```

### Auto-detect Test Command

If your test command follows standard patterns, you can omit `test_command`:

```yaml
- uses: dgp-standard/dgp-conformance-action@v1
  with:
    language: python  # Auto-runs: pytest tests/test_contract_compliance.py -v
```

```yaml
- uses: dgp-standard/dgp-conformance-action@v1
  with:
    language: node  # Auto-runs: npm test
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `language` | Implementation language (`python` or `node`) | ✅ Yes | - |
| `test_command` | Command to run conformance tests | ❌ No | `auto` |
| `working_directory` | Working directory for tests | ❌ No | `.` |

---

## Outputs

| Output | Description |
|--------|-------------|
| `conformant` | `true` if 8/8 vectors passed, `false` otherwise |
| `vectors_passed` | Number of canonical vectors passed |

---

## Requirements

Your implementation must:

1. **Pass all 8 canonical vectors** from [canonical-v1.json](https://github.com/dgp-standard/dgp-standard-dgp-spec/blob/main/conformance/canonical-v1.json)
2. **Match expected outputs bit-for-bit** (no approximations)
3. **Include the canonical vectors** in your test suite

---

## Conformance Badge

Add this badge to your README after passing:

```markdown
[![DGP v1.0 Conformant](https://img.shields.io/badge/DGP-v1.0%20Conformant-brightgreen.svg)](https://github.com/dgp-standard/dgp-standard-dgp-spec)
```

Result: [![DGP v1.0 Conformant](https://img.shields.io/badge/DGP-v1.0%20Conformant-brightgreen.svg)](https://github.com/dgp-standard/dgp-standard-dgp-spec)

---

## Example Implementations

| Language | Repository | Status |
|----------|------------|--------|
| **JavaScript** | [dgp-standard-dgp-js](https://github.com/dgp-standard/dgp-standard-dgp-js) | ✅ 8/8 |
| **Python** | [dgp-standard-dgp-py](https://github.com/dgp-standard/dgp-standard-dgp-py) | ✅ 8/8 |

---

## Troubleshooting

### "Not all canonical vectors passed"

- Ensure your test output contains `8 passed` or `8/8`
- Check that all 8 vectors from `canonical-v1.json` are tested
- Verify bit-for-bit match (no rounding errors, no string changes)

### "Tests failed"

- Check that `conformance/canonical-v1.json` exists in your repo
- Verify your test command is correct
- Ensure dependencies are installed

---

## License

MIT — See [LICENSE](./LICENSE)

## Contributing

Issues and PRs welcome at [dgp-conformance-action](https://github.com/dgp-standard/dgp-conformance-action)
