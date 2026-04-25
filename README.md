# Claude Code Custom Setup via AgentRouter

![Node.js Version](https://img.shields.io/badge/Node.js-18%2B-brightgreen?style=flat-square)
[![npm version](https://img.shields.io/npm/v/@anthropic-ai/claude-code.svg?style=flat-square)](https://www.npmjs.com/package/@anthropic-ai/claude-code)

Claude Code is an agentic terminal-based coding assistant from Anthropic that can analyze codebases, execute development workflows, explain architecture, and assist with Git operations using natural-language commands.
> This setup uses a third-party endpoint and is not the official Anthropic configuration.

---

## 🛠️ Prerequisites

Ensure you have **Node.js (v18 or higher)** installed on your system.
Check installed versions:
```bash
node -v
npm -v
```
Install Claude Code globally via NPM:
```bash
npm install -g @anthropic-ai/claude-code
```
*(Note: Use `sudo` on Linux/macOS if you encounter permission errors.)*

---


## Claim Credits
Create an [AgentRounter](https://agentrouter.org/register?aff=6GKO) account and claim the credits.
> Note: This link contains a referral parameter.

## Environment Configuration

The following variables configure Claude Code to use the custom endpoint:
```bash
ANTHROPIC_BASE_URL → custom API endpoint
ANTHROPIC_AUTH_TOKEN → authentication token
ANTHROPIC_MODEL → selected model
CLAUDE_CODE_USE_AUTH_TOKEN → enables token-based authentication
```
Replace YOUR_TOKEN_HERE with your own token.

### Windows (Command Prompt / PowerShell)
Run the following commands to set the environment variables persistently:

```powershell
setx ANTHROPIC_API_KEY ""
setx ANTHROPIC_BASE_URL "https://agentrouter.org"
setx ANTHROPIC_AUTH_TOKEN "sk-KUByfxxxxxxxxxxxxxxxxxxxxxxx"
setx ANTHROPIC_MODEL "claude-opus-4-6"
setx CLAUDE_CODE_USE_AUTH_TOKEN "true"
```
*Note: You may need to restart your terminal or computer for the changes to take effect.*

### Linux (Bash / Zsh)
To make the variables persistent, append them to your shell configuration file (`.bashrc` or `.zshrc`):

```bash
# For Zsh (Default on Kali/Ubuntu):
echo 'export ANTHROPIC_BASE_URL="https://agentrouter.org"' >> ~/.zshrc
echo 'export ANTHROPIC_AUTH_TOKEN="sk-KUByfxxxxxxxxxxxxxxxxxxxxxxx"' >> ~/.zshrc
echo 'export ANTHROPIC_MODEL="claude-opus-4-6"' >> ~/.zshrc
echo 'export CLAUDE_CODE_USE_AUTH_TOKEN="true"' >> ~/.zshrc
source ~/.zshrc

# For Bash:
# Replace ~/.zshrc with ~/.bashrc in the commands above and run 'source ~/.bashrc'.
```

### macOS (Zsh)
Run these commands in your terminal to configure your Zsh environment:

```bash
echo 'export ANTHROPIC_BASE_URL="https://agentrouter.org"' >> ~/.zshrc
echo 'export ANTHROPIC_AUTH_TOKEN="sk-KUByfxxxxxxxxxxxxxxxxxxxxxxx"' >> ~/.zshrc
echo 'export ANTHROPIC_MODEL="claude-opus-4-6"' >> ~/.zshrc
echo 'export CLAUDE_CODE_USE_AUTH_TOKEN="true"' >> ~/.zshrc
source ~/.zshrc
```

---

## Launching Claude Code

Once configured, simply run the following command in any project directory to start:

```bash
claude
```
## Troubleshooting

### Command not found

Ensure npm global binaries are available in PATH.

Check:

```bash
npm config get prefix
```

---

### Authentication failed

Verify:

* token is correct
* endpoint is reachable
* environment variables loaded

---

### Model not found

If `claude-opus-4-6` fails, check supported models from provider.

---

## Security Notes

* Never commit tokens to Git repositories
* Avoid sharing shell history containing secrets
* Prefer local environment variables over hardcoded credentials

---

## Recommended Usage

Use Claude Code for:

* refactoring
* debugging
* architecture explanation
* test generation
* Git workflow assistance
