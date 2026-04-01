# CI/CD Integration

DevGuard can fail a pipeline when critical vulnerabilities or KEV-listed CVEs are found.

## GitHub Actions

```yaml
name: Security Scan

on: [push, pull_request]

jobs:
  devguard:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install DevGuard
        run: pip install pentesterra-devguard

      - name: Run security scan
        run: pentesterra-devguard scan --ci --wait --fail-on critical
        env:
          DEVGUARD_API_KEY: ${{ secrets.DEVGUARD_API_KEY }}
          DEVGUARD_PROJECT: my-project
```

### Fail conditions

| Flag | Behavior |
|------|----------|
| `--fail-on critical` | Exit 1 if any Critical findings |
| `--fail-on high` | Exit 1 if Critical or High findings |
| `--fail-on kev` | Exit 1 if any CVE is in CISA KEV catalog |

Combine multiple flags: `--fail-on critical --fail-on kev`

---

## GitLab CI

```yaml
devguard-scan:
  stage: test
  image: python:3.11
  script:
    - pip install pentesterra-devguard
    - pentesterra-devguard scan --ci --wait --fail-on critical
  variables:
    DEVGUARD_API_KEY: $DEVGUARD_API_KEY
```

---

## Environment Variables

| Variable | Description |
|----------|-------------|
| `DEVGUARD_API_KEY` | Your DevGuard API key |
| `DEVGUARD_PROJECT` | Project alias (overrides config) |
| `DEVGUARD_API_URL` | Custom API URL (Enterprise) |

---

## Non-interactive mode

```bash
# Submit scan and exit immediately (async)
pentesterra-devguard scan --ci

# Submit and wait for results (blocks until done)
pentesterra-devguard scan --ci --wait

# Dry run — show what would be collected
pentesterra-devguard scan --dry-run
```

---

## Branch Protection

For teams using branch protection rules, combine DevGuard with a required status check:

1. Add the GitHub Actions workflow above
2. In repository Settings → Branches → Require status checks → add `devguard`
3. Pushes to `main`/`master` will be blocked if DevGuard finds critical issues

---

## Support

- [Dashboard](https://app.pentesterra.com)
- [Documentation](https://www.pentesterra.com/devguard/docs)
- [Issues](https://github.com/pentesterra/devguard/issues)
