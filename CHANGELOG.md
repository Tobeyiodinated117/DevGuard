# Changelog

All notable changes to Pentesterra DevGuard are documented here.

---

## v5.6.0 — 2026-03-25

### New: Python Runtime Execution Hook Detection

Triggered by the litellm 1.82.8 PyPI supply chain attack. New scan module detects three attack layers that most security tools miss:

- **`.pth` code execution** — Python auto-executes `.pth` files in `site-packages` at every interpreter startup, before any user code. A malicious package ships `packagename_init.pth` that imports a credential stealer.
- **`sitecustomize.py` implants** — foreign `sitecustomize.py`/`usercustomize.py` in `site-packages` that make outbound network calls or read sensitive env vars.
- **Credential-harvesting deps** — installed packages that read AWS/GCP/GitHub/OpenAI credentials AND make network calls in the same file.
- **Exec-at-import** — `subprocess` or `os.system` at module level in `__init__.py`.

Cross-references findings with `.env` file contents — if a malicious dep reads the exact same vars your project uses, the finding is flagged `confirmed_attack_surface: true`.

---

## v5.5.1 — 2026-03-18

### Attack Chain False Positive & Accepted Risk Management

- Mark attack chains as **False Positive** or **Accepted Risk** with a reason and scope (asset / project / org)
- Exceptions tab shows all marked chains with one-click restore
- Active chains view filters out marked entries

---

## v5.5.0 — 2026-03-10

### Global Auth Middleware Detection

DevGuard now automatically detects application-level auth middleware and applies it to all routes — reducing false positives on "unauthenticated endpoint" findings:

- Flask `@app.before_request` with auth logic
- Express `app.use(authMiddleware)`
- Django `AuthenticationMiddleware` in `MIDDLEWARE`
- NestJS `useGlobalGuards()` / `APP_GUARD`

Also extracts public route whitelists (`PUBLIC_ROUTES = [...]`) to further reduce noise.

---

## v5.4.0 — 2026-03-01

### Business Logic Risk Engine

New composite analysis layer that combines endpoint map + permission patterns + ORM models + workflow state machines into a unified risk score.

Detects:
- Missing authorization on sensitive operations
- IDOR patterns (resource ID in path, no ownership check)
- Bypassable workflow transitions
- Privileged operations exposed to unauthenticated routes
- Mass assignment vulnerabilities

---

## v5.3.0 — 2026-02-20

### Multi-Ecosystem Expansion

- **WordPress / Drupal / Joomla / Magento** — CMS-specific collector with plugin inventory and known-risky plugin detection
- **Go** — `go.mod` parsing, Gin/Echo/Chi route enumeration, `InsecureSkipVerify` and pprof detection
- **PHP / Laravel** — dangerous function detection (`eval`, `exec`, `unserialize`), debug config exposure

---

## v5.2.0 — 2026-02-10

### LLM / AI Integration Risk Scanning (Block 1)

- LLM gateway config scanning (LiteLLM, OpenRouter, Portkey, Helicone)
- Fine-tuning dataset PII risk detection
- MCP server threat intelligence — 40+ known-malicious MCP server patterns
- Prompt injection pattern detection (user input in `f"...{user_input}..."` → LLM call)
- AI agent dangerous tool inventory (ShellTool, BashTool, PythonREPLTool)

---

## v5.1.0 — 2026-02-01

### IDE Extension: VS Code / Cursor / Windsurf

- Scan-on-push via `FileSystemWatcher` on `.git/refs/remotes`
- Results webview with risk gauge, severity badges, per-category finding tables
- Graceful degradation: full / partial (no CLI) / setup-only (no key)
- Dep type badges: DIRECT / TRANSITIVE / DEV with dependency chain breadcrumb

---

## v5.0.0 — 2026-01-15

### 33-Module Scan Surface

Complete redesign of the scan surface to cover AI-era development environments. See [README → What DevGuard Scans](README.md#what-devguard-scans-33-scan-modules) for the full module list.
