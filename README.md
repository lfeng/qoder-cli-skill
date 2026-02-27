# Qoder CLI Skill for OpenClaw

🤖 Delegate coding tasks to Qoder CLI with full feature support.

## Features

- ✅ **TUI Mode** - Interactive terminal UI with PTY support
- ✅ **Print Mode** - Non-interactive automation
- ✅ **Subagents** - Specialized AI agents for specific tasks
- ✅ **Worktrees** - Parallel development with Git worktrees
- ✅ **MCP Servers** - Extend with Model Context Protocol tools
- ✅ **Quest Mode** - Spec-driven delegated development
- ✅ **Memory System** - AGENTS.md for project context
- ✅ **Custom Commands** - Extendable slash commands
- ✅ **Hooks** - Notification and integration hooks
- ✅ **Granular Permissions** - Fine-grained access control
- ✅ **Model Selection** - Auto-routing or manual model selection
- ✅ **All-Sessions Ready** - Works in group chats, DMs, and shared workspaces

## Installation

This skill is designed for [OpenClaw](https://github.com/openclaw/openclaw).

### Option 1: Clone to Skills Directory

```bash
cd ~/.openclaw/workspace/skills
git clone https://github.com/lfeng/qoder-cli-skill.git qoder-agent
```

### Option 2: Manual Installation

1. Download this repository
2. Place in `~/.openclaw/workspace/skills/qoder-agent`
3. The skill will be automatically loaded by OpenClaw

## Prerequisites

- **OpenClaw** installed and configured
- **Qoder CLI** installed: `curl -fsSL https://qoder.com/install | bash`
- **Authentication**: Set `QODER_PERSONAL_ACCESS_TOKEN` environment variable

## Configuration

### 1. Set Up Authentication

Add your Qoder Personal Access Token to your shell configuration:

```bash
# Add to ~/.zshrc or ~/.bashrc
export QODER_PERSONAL_ACCESS_TOKEN="your_personal_access_token_here"

# Reload shell
source ~/.zshrc
```

Get your token from: https://qoder.com/account/integrations

### 2. Verify Installation

```bash
# Check Qoder CLI
qodercli --version

# Check authentication
qodercli status
```

## Usage

Once installed, the skill is automatically available in OpenClaw. Just ask:

### Basic Examples

```
"Use Qoder CLI to add error handling to the API calls"
"Refactor the authentication module using ultimate model"
"Review the code in src/ for security issues"
```

### Advanced Usage

```
"Use Qoder CLI with worktree to fix issue #78 in parallel"
"Create a subagent for code review and use it to analyze src/"
"Run quest mode to build a REST API with authentication"
```

### Model Selection

- **Default**: `auto` (automatic model routing)
- **Best Quality**: `--model=ultimate` (refactoring, architecture)
- **Quick Tasks**: `--model=efficient` (simple fixes)
- **Code Review**: `--model=performance`

## Skill Structure

```
qoder-agent/
├── SKILL.md          # Main skill documentation and instructions
├── README.md         # This file
├── LICENSE           # MIT License
└── examples/         # Usage examples (optional)
```

## Documentation

See [SKILL.md](SKILL.md) for complete usage documentation including:

- TUI mode and input modes
- Print mode flags and options
- Subagent creation and usage
- Worktree parallel jobs
- MCP server integration
- Permission configuration
- Memory system (AGENTS.md)
- Custom commands
- Hooks setup
- Troubleshooting

## Examples

### Quick Task (Print Mode)

```bash
# One-shot task
qodercli -p "Add error handling to src/api.ts"

# With ultimate model
qodercli --model=ultimate -p "Refactor the authentication module"

# JSON output
qodercli --output-format=json -p "Analyze src/"
```

### Interactive Session (TUI Mode)

```bash
# Start TUI (requires PTY)
qodercli

# In TUI, use slash commands:
/login      # Authenticate
/init       # Initialize AGENTS.md
/quest      # Spec-driven development
/review     # Code review
/agents     # Manage subagents
```

### Subagents

Create `agents/code-review.md`:

```markdown
---
name: code-review
description: Code review expert for security and quality
tools: Read, Grep, Glob, Bash
---

You are a senior code reviewer. Check for:
1. Security vulnerabilities
2. Code quality
3. Best practices
```

Use it:

```bash
qodercli -p "Use code-review subagent to analyze src/"
```

### Worktrees (Parallel Development)

```bash
# Create parallel jobs
qodercli --worktree -p "Fix issue #78"
qodercli --worktree -p "Fix issue #99"

# List jobs
qodercli jobs --worktree

# Remove job
qodercli rm <jobId>
```

### MCP Servers

```bash
# Add browser automation
qodercli mcp add playwright -- npx -y @playwright/mcp@latest

# Add knowledge base
qodercli mcp add deepwiki -- npx -y mcp-deepwiki@latest

# List servers
qodercli mcp list
```

## Permissions

Configure in `~/.qoder/settings.json` or `${project}/.qoder/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "Read(/Users/demo/projects/myproject/**)",
      "Edit(/Users/demo/projects/myproject/**)"
    ],
    "ask": [
      "Read(!/Users/demo/projects/myproject/**)"
    ],
    "deny": [
      "Bash(rm -rf *)"
    ]
  }
}
```

## Troubleshooting

### Not Logged In

```bash
# Set environment variable
export QODER_PERSONAL_ACCESS_TOKEN="your_token"

# Or use TUI login
qodercli
/login
```

### Permission Denied

```bash
# Use yolo mode (caution)
qodercli --yolo -p "task"

# Or configure permissions
```

### TUI Not Working

```bash
# Ensure PTY mode in OpenClaw
# The skill automatically uses pty:true for TUI mode
```

## Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

MIT License - See [LICENSE](LICENSE) file for details.

## Support

- **OpenClaw Docs**: https://docs.openclaw.ai
- **Qoder CLI Docs**: https://docs.qoder.com/cli
- **Issues**: https://github.com/lfeng/qoder-cli-skill/issues

## Acknowledgments

- Built for [OpenClaw](https://github.com/openclaw/openclaw)
- Uses [Qoder CLI](https://qoder.com) by Alibaba Cloud
