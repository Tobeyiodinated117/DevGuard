# Installation Guide

## CLI

### Requirements

- Python 3.9+
- Git (for scan-on-push)

### Install via pip

```bash
pip install pentesterra-devguard
```

### Install from archive (offline / air-gapped)

```bash
curl -LO https://www.pentesterra.com/devguard.tar.gz
pip install devguard.tar.gz
```

### Verify installation

```bash
pentesterra-devguard --version
```

---

## Initial Setup

```bash
pentesterra-devguard init
```

This will:
1. Ask for your API key (get it from [app.pentesterra.com → Settings → DevGuard](https://app.pentesterra.com))
2. Let you register the current project with an alias
3. Store config in `~/.pentesterra/config.yaml`

---

## IDE Extension

The extension is distributed as a `.vsix` file — download it from your [Pentesterra dashboard](https://app.pentesterra.com).

### VS Code

```bash
code --install-extension pentesterra-devguard-*.vsix
```

Or: Extensions sidebar → `...` menu → Install from VSIX

### Cursor

```bash
cursor --install-extension pentesterra-devguard-*.vsix
```

### Windsurf

```bash
windsurf --install-extension pentesterra-devguard-*.vsix
```

### Extension settings

| Setting | Default | Description |
|---------|---------|-------------|
| `devguard.apiUrl` | `https://api.pentesterra.com` | API endpoint |
| `devguard.cliPath` | auto-detect | Path to CLI binary |
| `devguard.scanOnPush` | `true` | Auto-scan on git push |
| `devguard.scanMode` | `standard` | `standard` or `deep` |
| `devguard.blockPushOnCritical` | `false` | Block push if critical findings |

Configure in VS Code Settings (`Ctrl+,`) → search for `devguard`.

---

## Uninstall

```bash
pip uninstall pentesterra-devguard
rm -rf ~/.pentesterra/
```
