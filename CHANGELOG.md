# Changelog

All notable changes to Pentesterra DevGuard are documented here https://pentesterra.com/changelog


## 2026-04-18 · DevGuard 1.3.59 · minor
### Highlights:
- The only secrets scanner that blocks commits at the shell level - works for SVN, Mercurial, Perforce, and any other VCS, not just git. One `devguard shell-install` and you are protected everywhere, forever.
- SVN history scan: leaked secrets do not disappear when you rotate them - they stay in every old revision. DevGuard now scans SVN repository history and tells you exactly which revision a secret appeared in, whether it was ever deleted, and how many hours it was exposed.
- SVN → Git migration safety check: if you are migrating from SVN to GitHub, all SVN history becomes publicly indexed via GitHub Code Search. Run `devguard migration-check` before you push and find every secret that would become searchable on the internet.
- Entropy scoring: a second filter that eliminates ~60% of false alarms before they reach you - short passwords, placeholder values, and test strings are now silently skipped based on information-theoretic analysis. Only high-confidence secrets produce a finding.

---

## 2026-04-15 · DevGuard v1.3.50 · minor
### Highlights:
- Works with SVN, Mercurial, and any version control system - not just git
- Token liveness check: when a secret is found, instantly know if it is still active and what it can access
- Shell history scanning: credentials accidentally typed in the terminal are found before they spread
- Commit message scanning: secrets in commit messages and SVN `-m` arguments are caught
- Fewer false alarms: entropy-based filtering removes low-confidence guesses, so only real findings reach you
- GitHub classic PAT detection: 40-character legacy tokens - the format used in many open source projects - are now reliably detected

---


## v1.3.50 - 2026-04-06

### CLI bundled with IDE extension

The VS Code / Cursor / Windsurf extension now ships with the DevGuard CLI embedded. On first activation the CLI installs from the bundled package - no internet connection required, no separate `pip install` step for end users.

### Accurate installed dependency versions

DevGuard now runs the package manager directly to get actually-installed versions:

- **Node.js**: `npm list --json`, `yarn list --json`, `pnpm list --json`
- **Python**: venv `pip list`, `poetry show`, `pipenv run pip list`

Previously, version resolution relied on parsing lockfiles and `package.json` ranges, which could produce false positives when a patched version was installed but the lockfile entry still referenced an older specifier. No more.

### GraphQL Security Analysis (new module)

Detects GraphQL APIs and analyzes authorization coverage without transmitting source code:

- Supports SDL (`.graphql` / `.gql`), Graphene, Strawberry (Python), Apollo, and Nexus (TypeScript/JS)
- Flags unauthenticated mutations and unprotected sensitive queries
- Detects open introspection endpoints (production risk)
- Reports per-resolver auth coverage: query / mutation / subscription

### Multi-branch CVE matching fix

CVEs affecting multiple major version series with different fix versions (e.g. CVE-2025-29927 - Next.js 11.x through 15.x, each with a separate patched release) are now correctly matched in every branch. Previously a vulnerable package on one branch could slip through if only another branch's fix version was used as the upper bound.

### Global auth middleware recognized on all routes

Flask `before_request`, Express `app.use(authMiddleware)`, Django `MIDDLEWARE`, and NestJS global guards are now correctly propagated to every route they protect. Routes secured exclusively via a global gate no longer appear as "unprotected" in API Route Security findings.

### Improvements

- Self-hosted and on-prem deployments: report links in CLI output and IDE extension panels now derive the correct URL from the configured API endpoint - no longer hardcoded to `app.pentesterra.com`.
- Extension update check interval reduced from 24 hours to 1 hour.

---

## 2026-03-25

### New: Python Runtime Execution Hook Detection

Triggered by the litellm 1.82.8 PyPI supply chain attack. New scan module detects three attack layers that most security tools miss:

- **`.pth` code execution** - Python auto-executes `.pth` files in `site-packages` at every interpreter startup, before any user code. A malicious package ships `packagename_init.pth` that imports a credential stealer.
- **`sitecustomize.py` implants** - foreign `sitecustomize.py`/`usercustomize.py` in `site-packages` that make outbound network calls or read sensitive env vars.
- **Credential-harvesting deps** - installed packages that read AWS/GCP/GitHub/OpenAI credentials AND make network calls in the same file.
- **Exec-at-import** - `subprocess` or `os.system` at module level in `__init__.py`.

Cross-references findings with `.env` file contents - if a malicious dep reads the exact same vars your project uses, the finding is flagged `confirmed_attack_surface: true`.

---

## 2026-03-18

### Attack Chain False Positive & Accepted Risk Management

- Mark attack chains as **False Positive** or **Accepted Risk** with a reason and scope (asset / project / org)
- Exceptions tab shows all marked chains with one-click restore
- Active chains view filters out marked entries

---

## 2026-03-10

### Global Auth Middleware Detection

DevGuard now automatically detects application-level auth middleware and applies it to all routes - reducing false positives on "unauthenticated endpoint" findings:

- Flask `@app.before_request` with auth logic
- Express `app.use(authMiddleware)`
- Django `AuthenticationMiddleware` in `MIDDLEWARE`
- NestJS `useGlobalGuards()` / `APP_GUARD`

Also extracts public route whitelists (`PUBLIC_ROUTES = [...]`) to further reduce noise.

---

## 2026-03-01

### Business Logic Risk Engine

New composite analysis layer that combines endpoint map + permission patterns + ORM models + workflow state machines into a unified risk score.

Detects:
- Missing authorization on sensitive operations
- IDOR patterns (resource ID in path, no ownership check)
- Bypassable workflow transitions
- Privileged operations exposed to unauthenticated routes
- Mass assignment vulnerabilities

---

## 2026-02-20

### Multi-Ecosystem Expansion

- **WordPress / Drupal / Joomla / Magento** - CMS-specific collector with plugin inventory and known-risky plugin detection
- **Go** - `go.mod` parsing, Gin/Echo/Chi route enumeration, `InsecureSkipVerify` and pprof detection
- **PHP / Laravel** - dangerous function detection (`eval`, `exec`, `unserialize`), debug config exposure

---

## 2026-02-10

### LLM / AI Integration Risk Scanning (Block 1)

- LLM gateway config scanning (LiteLLM, OpenRouter, Portkey, Helicone)
- Fine-tuning dataset PII risk detection
- MCP server threat intelligence - 40+ known-malicious MCP server patterns
- Prompt injection pattern detection (user input in `f"...{user_input}..."` → LLM call)
- AI agent dangerous tool inventory (ShellTool, BashTool, PythonREPLTool)

---

## 2026-02-01

### IDE Extension: VS Code / Cursor / Windsurf

- Scan-on-push via `FileSystemWatcher` on `.git/refs/remotes`
- Results webview with risk gauge, severity badges, per-category finding tables
- Graceful degradation: full / partial (no CLI) / setup-only (no key)
- Dep type badges: DIRECT / TRANSITIVE / DEV with dependency chain breadcrumb

---

## 2026-01-15

### 33-Module Scan Surface

Complete redesign of the scan surface to cover AI-era development environments. See [README → What DevGuard Scans](README.md#what-devguard-scans-33-scan-modules) for the full module list.
