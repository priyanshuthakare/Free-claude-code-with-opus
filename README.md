# Claude Code via AgentRouter

![Node.js Version](https://img.shields.io/badge/Node.js-18%2B-brightgreen?style=flat-square)
[![npm version](https://img.shields.io/npm/v/@anthropic-ai/claude-code.svg?style=flat-square)](https://www.npmjs.com/package/@anthropic-ai/claude-code)

Claude Code is an agentic terminal-based coding assistant from Anthropic that can analyze codebases, execute development workflows, explain architecture, and assist with Git operations using natural-language commands.

> ⚠️ This setup uses a third-party endpoint ([AgentRouter](https://agentrouter.org)) and is **not** the official Anthropic configuration. Do not use this with sensitive or proprietary code.

---

## Prerequisites

**Node.js v18+** is required.

```bash
node -v
npm -v
```

Install Claude Code globally:

```bash
npm install -g @anthropic-ai/claude-code
```

> Use `sudo` on Linux/macOS if you encounter permission errors.

---

## Step 1 — Get AgentRouter Credits

1. Register at [agentrouter.org](https://agentrouter.org/register?aff=6GKO) *(referral link — new accounts receive ~$150 in credits)*
2. Go to `agentrouter.org/console/token` and copy your `sk-...` API key

> Keep your token secret. Never commit it to Git or share it publicly.

---

## Step 2 — Set Up the Client Bypass Proxy

AgentRouter restricts requests to specific recognized clients (Claude Code, Codex CLI, Roo Code). If you hit an **"unauthorized client detected"** error, you need to run a local proxy that spoofs the required headers.

> **This proxy step is required for most users.** Skip to Step 3 first — if it works without the proxy, you don't need this.

### Install Go

Download from [go.dev/dl](https://go.dev/dl/) and install.

### Create the Proxy

Save the following as `main.go` in any folder (e.g. `C:\tools\agentrouter-proxy\`):

```go
package main

import (
    "log"
    "net/http"
    "net/http/httputil"
    "net/url"
)

func main() {
    targetURL := "https://agentrouter.org"
    target, _ := url.Parse(targetURL)
    proxy := httputil.NewSingleHostReverseProxy(target)

    originalDirector := proxy.Director
    proxy.Director = func(req *http.Request) {
        originalDirector(req)
        req.Host = target.Host
        req.Header.Set("Originator", "codex_cli_rs")
        req.Header.Set("User-Agent", "codex_cli_rs/0.101.0 (Mac OS 26.0.1; arm64) Apple_Terminal/464")
        req.Header.Set("Version", "0.101.0")
    }

    log.Println("✅ Proxy running on http://localhost:8318")
    http.ListenAndServe(":8318", proxy)
}
```

### Build & Run

```bash
go build -ldflags="-s -w" -o agentrouter-proxy main.go   # Linux/macOS
go build -ldflags="-s -w" -o agentrouter-proxy.exe main.go  # Windows
```

```bash
./agentrouter-proxy       # Linux/macOS
.\agentrouter-proxy.exe   # Windows
```

Keep this running in a separate terminal.

### Auto-start on Windows (Optional)

Create `start-proxy.bat`:

```batch
@echo off
cd C:\tools\agentrouter-proxy
start /min agentrouter-proxy.exe
```

Press `Win + R` → type `shell:startup` → paste `start-proxy.bat` there. The proxy will now start silently on every boot.

### Auto-start on Linux (Optional)

```bash
# Create systemd user service
mkdir -p ~/.config/systemd/user
cat > ~/.config/systemd/user/agentrouter-proxy.service << EOF
[Unit]
Description=AgentRouter Proxy
After=network.target

[Service]
ExecStart=/path/to/agentrouter-proxy
Restart=always
RestartSec=3
MemoryMax=100M

[Install]
WantedBy=default.target
EOF

systemctl --user enable --now agentrouter-proxy.service
```

---

## Step 3 — Configure Environment Variables

Replace `sk-xxxx` with your actual token.

**If using the proxy** (Step 2), set `ANTHROPIC_BASE_URL` to `http://localhost:8318/`.  
**If not using the proxy**, set it to `https://agentrouter.org/`.

### Windows

Run in a new **PowerShell/CMD as Administrator**:

```powershell
setx ANTHROPIC_API_KEY "" /M
setx ANTHROPIC_BASE_URL "http://localhost:8318/" /M
setx ANTHROPIC_AUTH_TOKEN "sk-xxxx" /M
setx ANTHROPIC_MODEL "claude-opus-4-6" /M
setx CLAUDE_CODE_USE_AUTH_TOKEN "true" /M
```

Restart your terminal (or PC if variables don't apply).

### Linux / macOS

```bash
# Zsh (default on macOS, Kali, Ubuntu 24+)
echo 'export ANTHROPIC_API_KEY=""' >> ~/.zshrc
echo 'export ANTHROPIC_BASE_URL="http://localhost:8318/"' >> ~/.zshrc
echo 'export ANTHROPIC_AUTH_TOKEN="sk-xxxx"' >> ~/.zshrc
echo 'export ANTHROPIC_MODEL="claude-opus-4-6"' >> ~/.zshrc
echo 'export CLAUDE_CODE_USE_AUTH_TOKEN="true"' >> ~/.zshrc
source ~/.zshrc

# Bash — replace ~/.zshrc with ~/.bashrc above
```

---

## Step 4 — Launch Claude Code

```bash
cd your-project
claude
```

---

## Available Models

| Model | Type |
|---|---|
| `claude-opus-4-6` | Anthropic (recommended) |
| `claude-haiku-4-5-20251001` | Anthropic (faster, cheaper) |
| `deepseek-v3.1` | OpenAI-compatible |
| `deepseek-r1-0528` | OpenAI-compatible |

Check your account's model list at `agentrouter.org/console`.

---

## Troubleshooting

### `unauthorized client detected`
→ The proxy in Step 2 is required. Make sure it's running before launching `claude`.

### Auth conflict warning (both token and API key set)
→ Run `setx ANTHROPIC_API_KEY "" /M` and restart terminal.

### Retrying / hanging on requests
→ Verify the proxy is running (`http://localhost:8318` should be active).  
→ Confirm env vars are set correctly: `echo %ANTHROPIC_AUTH_TOKEN%` (Windows) or `echo $ANTHROPIC_AUTH_TOKEN` (Linux/macOS).

### `command not found: claude`
→ npm global binaries aren't in PATH. Check: `npm config get prefix` and add the `bin/` folder to your PATH.

### Model not found
→ Check `agentrouter.org/console` for your account's available models and update `ANTHROPIC_MODEL` accordingly.

---

## Security Notes

- Never commit tokens to Git
- Avoid sharing terminal history containing secrets
- Regenerate your token at `agentrouter.org/console/token` if accidentally exposed
- Don't use this setup with client or proprietary code — requests route through AgentRouter's servers

---

## What Claude Code Is Good For

- Refactoring and code cleanup
- Debugging with full codebase context
- Architecture explanation
- Test generation
- Git workflow assistance (`commit`, `diff`, `blame` in natural language)
