<div align="center">

<img src="https://www.pentesterra.com/devguard-logo.png" alt="Pentesterra DevGuard" width="120" />

# Pentesterra DevGuard

**Pre-push security scanner for AI-era developers**

Catch supply chain vulnerabilities, exposed secrets, malicious packages, and AI toolchain threats — before you `git push`.

[![PyPI version](https://img.shields.io/pypi/v/pentesterra-devguard?color=blue&label=CLI)](https://pypi.org/project/pentesterra-devguard/)
[![Python 3.9+](https://img.shields.io/badge/python-3.9%2B-blue)](https://pypi.org/project/pentesterra-devguard/)
[![VS Code](https://img.shields.io/badge/VS%20Code-extension-007ACC?logo=visualstudiocode)](https://www.pentesterra.com/devguard#install)
[![Cursor](https://img.shields.io/badge/Cursor-compatible-black)](https://www.pentesterra.com/devguard#install)
[![Windsurf](https://img.shields.io/badge/Windsurf-compatible-00C7B7)](https://www.pentesterra.com/devguard#install)
[![License](https://img.shields.io/badge/license-Proprietary-red)](LICENSE)

[**Get Started**](https://www.pentesterra.com/devguard) · [**Documentation**](https://www.pentesterra.com/devguard/docs) · [**Pricing**](https://www.pentesterra.com/pricing) · [**Dashboard**](https://app.pentesterra.com)

</div>

---

## What is DevGuard?

DevGuard is a **thin local agent** that scans your project before every push and sends metadata — never your source code — to the Pentesterra cloud for analysis.

```
Your Machine                    Pentesterra Cloud
─────────────────               ────────────────────────────
DevGuard CLI                →   Risk Engine + CVE/KEV KB
  • lockfile parsing              • Supply chain analysis
  • secret pattern matching       • Malicious package detection
  • config file scanning          • SAST scoring
  • MCP / AI toolchain scan  →   • AI threat intelligence
  • endpoint map extraction       • Business logic risk model
─────────────────               ────────────────────────────
No source code uploaded.        Results → Web Dashboard / IDE
```

**Philosophy:** Privacy-first. Your code stays local. DevGuard collects only structural metadata (dependency lists, file paths, secret fingerprints, config flags). Full source is never sent.

---

## Quick Start

```bash
# 1. Install
pip install pentesterra-devguard

# 2. Link to your Pentesterra account
pentesterra-devguard init

# 3. Scan your project
cd /path/to/your/project
pentesterra-devguard scan
```

Results appear in the [Pentesterra Dashboard](https://app.pentesterra.com) and as IDE notifications.

---

## IDE Extension

Install the VS Code extension for automatic scan-on-push, inline risk badges, and a full results panel — without leaving your editor.

**Works with:** VS Code · Cursor · Windsurf

```bash
# Download .vsix from your dashboard, then:
code    --install-extension pentesterra-devguard-*.vsix
cursor  --install-extension pentesterra-devguard-*.vsix
windsurf --install-extension pentesterra-devguard-*.vsix
```

<div align="center">
<img src="docs/images/ide-results.png" alt="DevGuard IDE results panel" width="700" />
<br/><em>Risk panel inside VS Code — Supply Chain, Secrets, AI Toolchain findings in one view</em>
</div>

**Extension features:**
- Sidebar with project status and last scan summary
- Auto-scan triggered on `git push` (configurable)
- Color-coded severity badges (Critical / High / Medium / Low)
- One-click "View Full Results" → Pentesterra Dashboard
- Works without any additional setup — uses the same CLI under the hood

---

## What DevGuard Scans (33 Scan Modules)

### Supply Chain & Dependencies
| Module | What it finds |
|--------|--------------|
| **Dependency CVE/KEV mapping** | Known CVEs across npm, pip, go, cargo, composer, rubygems lockfiles |
| **Malicious package detection** | event-stream, node-ipc, crewai incidents, 50+ known-malicious packages |
| **Typosquatting detection** | Packages with names ±1 char from popular libraries |
| **Python execution hook detection** *(new in v5.6)* | `.pth` files with executable code, credential-harvesting `__init__.py`, `sitecustomize.py` implants — the exact litellm 1.82.8 attack vector |
| **Transitive dependency chain** | DIRECT vs TRANSITIVE vs DEV — risk-weighted scoring |

### Secrets & Credentials
| Module | What it finds |
|--------|--------------|
| **Secrets exposure** | AWS keys, GCP credentials, GitHub tokens, Stripe, JWT secrets, private keys, `.env` values |
| **Cloud credential surface** | `.aws/`, `.gcloud/`, `.azure/`, kubeconfig, terraform state |
| **OS credential surface** | SSH keys, `.netrc`, `.npmrc` tokens, Docker registry auth, macOS Keychain references |

> **Privacy note:** Secret values are never transmitted. DevGuard sends only: type, file path, line number, and a SHA-256 fingerprint.

### AI Toolchain & MCP (Unique to DevGuard)
| Module | What it finds |
|--------|--------------|
| **MCP server threat intelligence** | Malicious MCP servers in `.cursor/`, `.windsurf/`, `.vscode/mcp.json`; typosquatted tool names; exfiltration-capable configs |
| **LLM integration risk** | Hardcoded API keys in LiteLLM/OpenRouter/Portkey configs; gateway credential exposure |
| **AI agent configuration** | `ShellTool`, `BashTool`, `PythonREPLTool` without sandboxing; insecure agentic loops |
| **Prompt injection risks** | String formatting of user input directly into prompts; LLM output piped to `exec()` / `subprocess()` |
| **Vector DB exposure** | Unauthenticated ChromaDB/Qdrant/Weaviate ports; API keys in source |
| **IDE extension threats** | Installed VS Code/Cursor/Windsurf extensions cross-referenced against malicious blocklist |

### Application Security (SAST-Lite + Business Logic)
| Module | What it finds |
|--------|--------------|
| **SAST Lite** | High-confidence OWASP Top 10 patterns in Python and JavaScript/TypeScript — no source sent, only `{type, file, line, snippet_hash}` |
| **Endpoint & auth map** | HTTP routes without authentication (FastAPI, Flask, Django, Express, NestJS, Next.js, Rails, Gin, Spring) |
| **Business logic risk** | IDOR patterns, missing authorization on sensitive operations, bypassable state machines |
| **ORM model inventory** | PII, financial, and health data fields — mapped to PCI-DSS, GDPR, HIPAA scope |

### Infrastructure & Platform
| Module | What it finds |
|--------|--------------|
| **Dev environment exposure** | `DEBUG=True`, `0.0.0.0` binds, Swagger/actuator endpoints in prod configs |
| **CMS security** (WordPress, Drupal, Joomla, Magento) | Risky plugins, debug mode, xmlrpc exposure, admin path disclosure |
| **Automation platform risk** | n8n dangerous nodes, webhook secret exposure (Zapier, Make, IFTTT) |
| **Go security misconfigs** | `InsecureSkipVerify`, pprof exposure, `math/rand` for crypto, SQL via `fmt.Sprintf` |
| **PHP / Laravel security** | `eval()`, `exec()`, `unserialize()` on user input, `.env` debug exposure |
| **Runtime & EOL detection** | Node.js, Python, Go version against EOL schedule; insecure Docker base images |
| **Crypto & TLS weaknesses** | TLS 1.0/1.1, RC4/DES, MD5/SHA-1, weak RSA keys, deprecated crypto libs |
| **Peer dependency conflicts** | Major-version mismatches that cause runtime crashes |
| **Deprecated API detection** | React legacy lifecycle methods, `ReactDOM.render`, deprecated Node.js built-ins |
| **Dev container security** | Privileged containers, host network mode, sensitive volume mounts |
| **Global tools & git hooks** | Malicious `curl`/`wget` in git hooks; outdated global npm/pip packages |

---

## Privacy-First Design

| What DevGuard collects | What DevGuard never sends |
|------------------------|--------------------------|
| Dependency names and versions | Your source code |
| File paths and line numbers | Secret values (only SHA-256 fingerprints) |
| Config flag presence (`DEBUG=True`) | Private keys or tokens |
| Structural metadata (route paths, ORM field names) | File contents |

Inspect exactly what will be sent before submitting:

```bash
pentesterra-devguard scan --dry-run
```

---

## Re-analysis Without Rescanning

When a new CVE drops, Pentesterra automatically re-scores your existing scans against the updated knowledge base. You get notified — no need to re-run the CLI.

---

## CI/CD Integration

```yaml
# GitHub Actions example
- name: DevGuard Security Scan
  run: |
    pip install pentesterra-devguard
    pentesterra-devguard scan --ci --wait --fail-on critical
  env:
    DEVGUARD_API_KEY: ${{ secrets.DEVGUARD_API_KEY }}
```

```bash
# Fail pipeline on critical findings or KEV-listed vulnerabilities
pentesterra-devguard scan --ci --wait --fail-on critical --fail-on kev
```

---

## Supported Ecosystems

| Language / Platform | Lockfile / Source |
|--------------------|-------------------|
| JavaScript / Node.js | `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml` |
| Python | `requirements.txt`, `poetry.lock`, `Pipfile.lock` |
| Go | `go.mod`, `go.sum` |
| PHP | `composer.lock` |
| Ruby | `Gemfile.lock` |
| Rust | `Cargo.lock` |
| WordPress / Drupal / Joomla / Magento | CMS-specific config files |
| AI / LLM | LiteLLM, LangChain, LlamaIndex, CrewAI, AutoGen, DSPy configs |

---

## CLI Reference

```
pentesterra-devguard <command> [options]

Commands:
  init              Configure API key and project
  scan [path]       Run security scan (default: current directory)
  status <scan_id>  Check scan status
  results <id>      View detailed findings
  projects          List your projects
  scans             List recent scans
  quota             Check usage quota
  update            Update CLI to latest version

Options:
  --json            Machine-readable JSON output (used by IDE extension)
  --dry-run         Show what would be collected, without sending
  --ci              Non-interactive mode for CI/CD
  --wait            Poll until results are ready
  --fail-on         Exit non-zero if findings match severity (critical/high/kev)
  --scan-mode       standard (default) or deep
```

---

## Pricing

| Tier | Projects | Scans/month | Scan modes |
|------|----------|-------------|-----------|
| **Free** | 1 | 20 | Standard |
| **Starter** | 5 | 100 | Standard + Deep |
| **Pro** | Unlimited | Unlimited | All + CI/CD |
| **Enterprise** | Custom | Custom | Custom SLA + On-prem |

[See full pricing →](https://www.pentesterra.com/pricing)

---

## Screenshots

<div align="center">
<img src="docs/images/devguard-dashboard.png" alt="DevGuard web dashboard" width="700" />
<br/><em>Scan results in Pentesterra Dashboard — risk score, severity breakdown, supply chain chains</em>

<br/><br/>

<img src="docs/images/devguard-ide-sidebar.png" alt="DevGuard IDE sidebar" width="400" />
<br/><em>Sidebar in VS Code / Cursor — status, projects, last scan summary</em>
</div>

---

## The Bigger Picture

DevGuard is the **shift-left entry point** to the Pentesterra platform:

```
DevGuard (pre-push)  →  Web App Pentest  →  Network Pentest  →  Continuous Monitoring
```

Start free with DevGuard. When you need a full penetration test, your project history, business logic model, and attack surface map are already in Pentesterra — ready to use.

---

## Get Started

1. [Sign up at pentesterra.com](https://www.pentesterra.com/devguard) — free tier, no credit card
2. Get your API key from the dashboard
3. `pip install pentesterra-devguard && pentesterra-devguard init`

---

## Links

- [Website](https://www.pentesterra.com)
- [DevGuard product page](https://www.pentesterra.com/devguard)
- [Documentation](https://www.pentesterra.com/devguard/docs)
- [Changelog](CHANGELOG.md)
- [Dashboard](https://app.pentesterra.com)
- [Report an issue or request a feature](https://github.com/pentesterra/devguard/issues)

---

<div align="center">

**Pentesterra DevGuard** — Write fast. Ship fast. Ship secure.

[pentesterra.com](https://www.pentesterra.com)

</div>
