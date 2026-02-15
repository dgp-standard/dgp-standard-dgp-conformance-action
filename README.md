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
        uses: dgp-standard/dgp-conformance-action@v1
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
        uses: dgp-standard/dgp-conformance-action@v1
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
| `conformant` | `true` if all required vectors passed and canonical hash matches, `false` otherwise |
| `vectors_passed` | Number of canonical vectors passed |

---

## Requirements

Your implementation must:

1. **Pass all 8 canonical vectors** from [canonical-v1.json](https://github.com/dgp-standard/dgp-standard-dgp-spec/blob/main/conformance/canonical-v1.json)
2. **Match expected outputs bit-for-bit** (no approximations)
3. **Include the canonical vectors** in your test suite
4. **Emit machine-readable conformance artifact** at `.dgp/conformance-result.json` (or `DGP_RESULT_PATH`)
5. **Include pinned canonical hash** `b551d99d6c6ac9dfebd660ad1be2e326ce31aef372a659fa7a6a5af1bf33b9c2`

Required artifact shape:

```json
{
  "dgp_version": "1.0.0",
  "engine": "node",
  "vectors_passed": 8,
  "vectors_total": 8,
  "canonical_hash": "b551d99d6c6ac9dfebd660ad1be2e326ce31aef372a659fa7a6a5af1bf33b9c2",
  "timestamp": "2026-02-15T10:00:00Z"
}
```

Schema reference:
[conformance-result.schema.json](https://github.com/dgp-standard/dgp-standard-dgp-spec/blob/main/schemas/conformance-result.schema.json)

### Why grep-based conformance is banned

The action does not trust console text patterns like `8 passed` or `8/8` because they are spoofable.
Conformance is granted only when the JSON artifact exists, contains required typed fields, matches the pinned canonical hash, and reports full vector pass totals.

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

- Check `vectors_passed` and `vectors_total` in `.dgp/conformance-result.json`
- Confirm `canonical_hash` equals pinned DGP v1.0 hash
- Verify bit-for-bit match (no rounding errors, no string changes)

### "Missing conformance result file"

- Ensure tests write `.dgp/conformance-result.json`
- If custom path is used, set `DGP_RESULT_PATH` to that location

### "Canonical hash mismatch"

- Recompute hash using DGP non-self-referential algorithm
- Verify vectors file is canonical and unmodified

### "Tests failed"

- Check that `conformance/canonical-v1.json` exists in your repo
- Verify your test command is correct
- Ensure dependencies are installed

---

## License

MIT — See [LICENSE](./LICENSE)

## Contributing

Issues and PRs welcome at [dgp-conformance-action](https://github.com/dgp-standard/dgp-conformance-action)
