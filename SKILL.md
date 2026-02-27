---
name: qoder-agent
description: 'Delegate coding tasks to Qoder CLI. Use when: (1) building/creating new features or apps, (2) code reviews, (3) refactoring, (4) iterative coding that needs file exploration. Supports TUI mode, print mode, subagents, worktrees, MCP servers, quest mode, and hooks. Works in all session types (direct chat, group chat, Discord, etc.). NOT for: simple one-liner fixes (just edit), reading code (use read tool). Requires qodercli installed.'
metadata:
  {
    "openclaw": { "emoji": "🤖", "requires": { "anyBins": ["qodercli"] } },
  }
---

# Qoder Agent (bash-first, all-sessions)

Use **bash** (with optional background mode) for all Qoder CLI work. Simple and effective.

**✅ All-Sessions Ready:** This skill works in:
- Direct 1:1 chats
- Group chats (DingTalk, Discord, Slack, etc.)
- Shared workspace sessions
- Private sessions

---

## ⚠️ PTY Mode Required!

Qoder CLI TUI mode is an **interactive terminal application** that needs a pseudo-terminal (PTY) to work correctly. Without PTY, you'll get broken output, missing colors, or the CLI may hang.

**Always use `pty:true`** when running Qoder CLI in TUI mode:

```bash
# ✅ Correct - with PTY
bash pty:true command:"qodercli -w /path/to/project"

# ❌ Wrong - no PTY, TUI may break
bash command:"qodercli -w /path/to/project"
```

### Bash Tool Parameters

| Parameter    | Type    | Description                                                                 |
| ------------ | ------- | --------------------------------------------------------------------------- |
| `command`    | string  | The shell command to run                                                    |
| `pty`        | boolean | **Use for TUI mode!** Allocates a pseudo-terminal for interactive CLI       |
| `workdir`    | string  | Working directory (Qoder sees only this folder's context)                   |
| `background` | boolean | Run in background, returns sessionId for monitoring                         |
| `timeout`    | number  | Timeout in seconds (kills process on expiry)                                |
| `elevated`   | boolean | Run on host instead of sandbox (if allowed)                                 |

### Process Tool Actions (for background sessions)

| Action      | Description                                          |
| ----------- | ---------------------------------------------------- |
| `list`      | List all running/recent sessions                     |
| `poll`      | Check if session is still running                    |
| `log`       | Get session output (with optional offset/limit)      |
| `write`     | Send raw data to stdin                               |
| `submit`    | Send data + newline (like typing and pressing Enter) |
| `send-keys` | Send key tokens or hex bytes                         |
| `paste`     | Paste text (with optional bracketed mode)            |
| `kill`      | Terminate the session                                |

---

## 🔐 Environment Setup (Auto-Detected)

Qoder CLI authentication is automatically available via:

```bash
# Environment variable (set in ~/.zshrc)
QODER_PERSONAL_ACCESS_TOKEN="your_token_here"

# Or check if already authenticated
qodercli status
```

**In any session type**, the environment variable is inherited from the shell, so Qoder CLI works seamlessly.

---

## 🚀 Quick Start

### Print Mode (Non-Interactive) - Recommended for Most Tasks

```bash
# Basic one-shot task (no PTY needed)
bash workdir:~/project command:"qodercli -p 'Add error handling to the API calls'"

# With ultimate model for best quality
bash workdir:~/project command:"qodercli --model=ultimate -p 'Refactor this module'"

# With JSON output
bash workdir:~/project command:"qodercli --output-format=json -p 'Analyze this code'"

# Yolo mode (skip permission checks)
bash workdir:~/project command:"qodercli --yolo -p 'Make the changes'"

# Continue last session
bash workdir:~/project command:"qodercli -c -p 'Continue the refactoring'"

# Max turns limit
bash workdir:~/project command:"qodercli --max-turns=10 -p 'Fix the bug'"
```

### TUI Mode (Interactive)

```bash
# Start TUI in project directory (with PTY!)
bash pty:true workdir:~/project command:"qodercli"

# Continue last session
bash pty:true workdir:~/project command:"qodercli -c"

# Resume specific session
bash pty:true workdir:~/project command:"qodercli -r <session-id>"

# Background for longer work
bash pty:true workdir:~/project background:true command:"qodercli"
```

---

## 📋 TUI Mode Details

### Input Modes

| Mode | Command | Description |
|------|---------|-------------|
| `>` | Default | Dialog mode - chat with the CLI |
| `!` | Type `!` | Bash mode - run shell commands directly |
| `/` | Type `/` | Slash mode - built-in commands |
| `#` | Type `#` | Memory mode - edit AGENTS.md |
| `\` + `⏎` | Type `\` then Enter | Multiline input mode |

### Built-in Tools

Qoder CLI ships with tools for file/directory operations:
- **Grep** - Search code
- **Read** - Read files
- **Write** - Write/edit files
- **Bash** - Execute shell commands
- **Glob** - Pattern-based file matching
- **LS** - List directory contents

### Slash Commands (Complete List)

| Command | Description |
|---------|-------------|
| `/login` | Log in to Qoder account |
| `/help` | Show TUI help |
| `/init` | Initialize or update `AGENTS.md` memory file |
| `/memory` | Edit AGENTS.md (user or project level) |
| `/quest` | **Quest Mode**: Spec-driven delegated task |
| `/review` | Code review for local changes |
| `/resume` | List and resume sessions |
| `/clear` | Clear current session context history |
| `/compact` | Summarize current session's context history |
| `/usage` | Show current credit usage |
| `/status` | Show CLI status: version, model, account, API connectivity, tool status |
| `/config` | Show system configuration |
| `/agents` | Subagent commands: list, create, manage |
| `/bashes` | List running background Bash jobs |
| `/release-notes` | Show Qoder CLI release notes |
| `/vim` | Open external editor to edit input |
| `/feedback` | Send feedback about Qoder CLI |
| `/quit` | Exit TUI |
| `/logout` | Log out of Qoder account |

---

## 🎯 Print Mode Flags

| Flag | Description | Example |
|------|-------------|---------|
| `-p` | Run non-interactively (print mode) | `qodercli -p "task"` |
| `-q` | Quiet mode (hide spinner) | `qodercli -q -p "task"` |
| `--output-format` | Output format: text, json, stream-json | `qodercli --output-format=json` |
| `--input-format` | Input format: text, stream-json | `qodercli --input-format=stream-json` |
| `-w` | Specify workspace directory | `qodercli -w /path/to/project` |
| `-c` | Continue last session | `qodercli -c -p "continue"` |
| `-r` | Resume specific session | `qodercli -r <session-id>` |
| `--model` | Model tier selection | `qodercli --model=ultimate` |
| `--max-turns` | Maximum dialog turns (0 = unlimited) | `qodercli --max-turns=10` |
| `--max-output-tokens` | Max tokens: 16k, 32k | `qodercli --max-output-tokens=32k` |
| `--yolo` | Skip permission checks | `qodercli --yolo` |
| `--dangerously-skip-permissions` | Same as --yolo | `qodercli --dangerously-skip-permissions` |
| `--allowed-tools` | Allow only specified tools | `qodercli --allowed-tools=READ,WRITE` |
| `--disallowed-tools` | Disallow specified tools | `qodercli --disallowed-tools=Bash` |
| `--agents` | JSON object defining custom agents | `qodercli --agents='{"reviewer":{...}}'` |
| `--with-claude-config` | Load Claude Code configs from .claude folders | `qodercli --with-claude-config` |
| `--attachment` | Attach image files (repeatable) | `qodercli --attachment=img.png` |

---

## 🧠 Model Selection

Qoder CLI uses **automatic model routing** - it selects the globally optimal model based on task characteristics. However, you can override this:

| Model Value | Use Case | Speed | Quality | Cost |
|-------------|----------|-------|---------|------|
| `auto` | **Default** - automatic routing | ⚡⚡⚡ | ⭐⭐⭐ | 💰💰💰 |
| `efficient` | Quick tasks, simple queries | ⚡⚡⚡⚡ | ⭐⭐ | 💰💰 |
| `lite` | Very simple tasks | ⚡⚡⚡⚡⚡ | ⭐ | 💰 |
| `performance` | Complex tasks needing depth | ⚡⚡ | ⭐⭐⭐⭐ | 💰💰💰💰 |
| `ultimate` | **Best quality** - refactoring, architecture, code review | ⚡ | ⭐⭐⭐⭐⭐ | 💰💰💰💰💰 |
| `qmodel` | Qwen model family | ⚡⚡⚡ | ⭐⭐⭐⭐ | 💰💰💰 |
| `q35model` | Qwen 3.5 specific | ⚡⚡⚡ | ⭐⭐⭐⭐ | 💰💰💰 |
| `mmodel` | MiniMax model | ⚡⚡⚡ | ⭐⭐⭐⭐ | 💰💰💰 |
| `gmodel` | GPT model family | ⚡⚡ | ⭐⭐⭐⭐⭐ | 💰💰💰💰 |
| `kmodel` | Other specialized model | ⚡⚡⚡ | ⭐⭐⭐ | 💰💰💰 |

**Recommendations:**
- **Default**: Use `--model=auto` (let Qoder choose)
- **Refactoring/Architecture**: `--model=ultimate`
- **Quick fixes**: `--model=efficient`
- **Code review**: `--model=performance` or `ultimate`
- **Simple queries**: `--model=lite` or `efficient`

---

## 🎯 Quest Mode (Spec-Driven Development)

Quest Mode allows you to write specifications while AI automatically completes development tasks using subagents.

```bash
# Start quest mode
bash workdir:~/project command:"qodercli -p '/quest'"

# Or directly specify quest
bash workdir:~/project command:"qodercli --model=ultimate -p 'Build a REST API with authentication, rate limiting, and logging'"
```

Quest Mode automatically:
1. Analyzes requirements
2. Routes to appropriate subagents
3. Coordinates multi-step development
4. Ensures consistency across files

---

## 🤖 Subagents

Subagents are specialized AI agents for specific tasks with their own context windows and tool permissions.

### Create a Subagent (Manual)

Create markdown files in:
- `~/.qoder/agents/<agentName>.md` - User-level (all projects)
- `${project}/agents/<agentName>.md` - Project-level

**Example: code-review agent**
```markdown
---
name: code-review
description: Code review expert for quality and security checks
tools: Read, Grep, Glob, Bash
---

You are a senior code reviewer responsible for ensuring code quality.

Checklist:
1. Readability and code style
2. Naming conventions
3. Error handling
4. Security checks
5. Test coverage
6. Performance considerations
```

### Create a Subagent (Automatic)

```bash
# In TUI: /agents -> User or Project -> Create new agent
bash pty:true workdir:~/project command:"qodercli"
# Then type: /agents -> Create new agent -> "code reviewer for security"
```

### Use Subagents

```bash
# Explicit invocation
bash workdir:~/project command:"qodercli -p 'Use code-review subagent to check code issues'"

# Implicit invocation
bash workdir:~/project command:"qodercli -p 'Analyze this code for potential performance issues'"

# Chained subagents
bash workdir:~/project command:"qodercli -p 'First use design subagent for system design, then use code-review subagent'"

# Custom agents inline
bash workdir:~/project command:"qodercli --agents='{\"reviewer\":{\"description\":\"Reviews code\",\"prompt\":\"You are a code reviewer\"}}' -p 'Review this'"
```

---

## 🌳 Worktree (Parallel Jobs)

Worktree jobs are concurrent jobs that use Git worktrees to run tasks in parallel, avoiding read/write conflicts.

**Requirements:** Git installed and usable locally.

### Commands

| Command | Description |
|---------|-------------|
| `qodercli --worktree "job description"` | Create and start new worktree job |
| `qodercli jobs --worktree` | List existing worktree jobs |
| `qodercli rm <jobId>` | Remove a job (delete worktree) |

### Create a Job

```bash
# Basic worktree job
bash workdir:~/project command:"qodercli --worktree 'Fix issue #78'"

# Non-interactive (print mode)
bash workdir:~/project command:"qodercli --worktree -p 'Build feature X'"

# With branch specification
bash workdir:~/project command:"qodercli --worktree --branch=main -p 'Implement feature'"

# With additional options
bash workdir:~/project command:"qodercli --worktree --max-turns=20 -p 'Complex refactoring'"
```

### View Jobs

```bash
bash workdir:~/project command:"qodercli jobs --worktree"
```

**Output example:**
```
Qoder jobs for workspace: /Users/demo/project

Worktree Jobs:
ID              INIT PROMPT    PATH                                STATUS      CREATED
11758283139787  [I] hello      ~/.qoder/worktrees/11758283139787   running     5 minutes ago
11758283382928  [N] hello      ~/.qoder/worktrees/11758283382928   exited      1 minute ago

Total: 3 worktree job(s)
```

**Field descriptions:**
- **ID**: Unique job ID
- **INIT PROMPT**: Initial job description
- **PATH**: Git worktree directory
- **STATUS**: running, exited, etc.
- **CREATED**: Job creation time

### Delete Jobs

```bash
bash workdir:~/project command:"qodercli rm <jobId>"
```

⚠️ **Warning:** Deletion is irreversible. Proceed with caution.

### Parallel Issue Fixing Example

```bash
# Multiple worktrees for parallel work
bash workdir:~/project background:true command:"qodercli --worktree -p 'Fix issue #78'"
bash workdir:~/project background:true command:"qodercli --worktree -p 'Fix issue #99'"

# Monitor progress
process action:list
process action:log sessionId:XXX

# Create PRs after fixes complete
cd /tmp/worktree-78 && git push -u origin fix/issue-78
```

---

## 🔌 MCP Servers

Qoder CLI integrates with any standard MCP (Model Context Protocol) tool.

### Add MCP Servers

```bash
# Basic syntax
bash command:"qodercli mcp add <name> -- <command>"

# Example: Playwright for browser control
bash command:"qodercli mcp add playwright -- npx -y @playwright/mcp@latest"

# With server type (stdio, sse, streamable-http)
bash command:"qodercli mcp add myserver -t stdio -- npx -y @package/mcp"

# With scope (user or project)
bash command:"qodercli mcp add myserver -s project -- npx -y @package/mcp"
```

### Recommended MCP Tools

```bash
# Context7 - Upstash context management
bash command:"qodercli mcp add context7 -- npx -y @upstash/context7-mcp@latest"

# DeepWiki - Wikipedia/knowledge access
bash command:"qodercli mcp add deepwiki -- npx -y mcp-deepwiki@latest"

# Chrome DevTools - Browser automation
bash command:"qodercli mcp add chrome-devtools -- npx chrome-devtools-mcp@latest"
```

### Manage MCP Servers

```bash
# List servers
bash command:"qodercli mcp list"

# Remove server
bash command:"qodercli mcp remove playwright"

# List shows configuration files:
# - ~/.qoder.json (user-level, not committed)
# - ${project}/.mcp.json (project-level, usually committed)
```

### MCP Server Files

- **User-level**: `~/.qoder.json` - Not committed to git
- **Project-level**: `${project}/.mcp.json` - Usually committed

---

## 🔐 Permissions

Qoder CLI enforces precise tool execution permissions.

### Configuration Files (precedence: high → low)

1. `${project}/.qoder/settings.local.json` - Project-level, highest (gitignore)
2. `${project}/.qoder/settings.json` - Project-level
3. `~/.qoder/settings.json` - User-level

### Permission Strategies

| Strategy | Description |
|----------|-------------|
| `allow` | Automatically allow matching operations |
| `deny` | Automatically deny matching operations |
| `ask` | Prompt for permission (default for outside project) |

### Example Configuration

```json
{
  "permissions": {
    "ask": [
      "Read(!/Users/demo/projects/myproject/**)",
      "Edit(!/Users/demo/projects/myproject/**)"
    ],
    "allow": [
      "Read(/Users/demo/projects/myproject/**)",
      "Edit(/Users/demo/projects/myproject/**)"
    ],
    "deny": [
      "Bash(rm -rf /**)"
    ]
  }
}
```

### Permission Types

#### 1. Read & Edit Rules

Patterns follow gitignore-style matching:

| Pattern Form | Description | Example | Matches |
|--------------|-------------|---------|---------|
| `/path` | Absolute from system root | `Read(/Users/demo/**)` | `/Users/demo/xx` |
| `~/path` | From home directory | `Read(~/Documents/*.png)` | `/Users/demo/Documents/xx.png` |
| `path` or `./path` | Relative to current dir | `Read(/*.java)` | `./xx.java` |
| `!**` | Negation pattern | `Read(!**/node_modules/**)` | Excludes node_modules |

#### 2. WebFetch Rules

Restrict domains for network fetch tools:

```json
{
  "permissions": {
    "allow": [
      "WebFetch(domain:example.com)",
      "WebFetch(domain:*.github.io)"
    ]
  }
}
```

#### 3. Bash Rules

Restrict commands for shell execution:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run build)",
      "Bash(npm run test:*)",
      "Bash(curl http://site.com/:*)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo *)"
    ]
  }
}
```

---

## 📝 Memory (AGENTS.md)

Qoder CLI uses `AGENTS.md` as memory - content is auto-loaded as context.

### File Locations

- **User-level**: `~/.qoder/AGENTS.md` - Applies to all projects
- **Project-level**: `${project}/AGENTS.md` - Applies to current project

### Typical Content

- Development standards and notes
- Overall system architecture
- Project-specific conventions
- API documentation
- Testing requirements

### Automatically Generate

```bash
# In TUI: /init
bash pty:true workdir:~/project command:"qodercli"
# Then type: /init
```

### Manually Manage

```bash
# Create AGENTS.md in project root
cat > ~/project/AGENTS.md << 'EOF'
# Project Guidelines

## Architecture
- MVC pattern
- REST API design

## Code Style
- ESLint strict mode
- Prettier formatting

## Testing
- Jest for unit tests
- 80% coverage minimum
EOF

# In TUI: # to enter memory edit mode (vim-style)
# In TUI: /memory to choose and edit user/project files
```

---

## ⚡ Commands (Custom Slash Commands)

Commands extend slash functionality via `.md` files.

### Create a Command

Store in:
- `~/.qoder/commands/<name>.md` - User-level
- `${project}/commands/<name>.md` - Project-level

**Example: quest.md**
```markdown
---
description: "Intelligent workflow orchestrator for feature development"
---
First use the design subagent for system design, then use the code-review subagent to complete code review, and finally run tests to verify.
```

### Use Commands

```bash
# In TUI: /quest
bash pty:true workdir:~/project command:"qodercli"
# Then type: /quest
```

---

## 🔔 Hooks

Hooks integrate with external systems at key execution stages.

### Configuration Files

- `~/.qoder/settings.json` - User-level
- `${project}/.qoder/settings.json` - Project-level
- `${project}/.qoder/settings.local.json` - Project-level (highest precedence)

### Example: Notification Hook

**settings.json:**
```json
{
  "hooks": {
    "Notification": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "~/notification.sh"
          }
        ]
      }
    ]
  }
}
```

**notification.sh:**
```bash
#!/bin/bash
input=$(cat)

sessionId=$(echo $input | jq -r '.session_id')
messageInfo=$(echo $input | jq -r '.message')
workspacePath=$(echo $input | jq -r '.cwd')

if [[ "$messageInfo" =~ ^Agent ]]; then
  osascript -e 'display notification "✅ Task completed." with title "QoderCLI"'
else
  osascript -e 'display notification "⌛️ Authorization required." with title "QoderCLI"'
fi

exit 0
```

### Available Hook Types

Currently supported:
- **Notification** - Task completion/authorization notifications

Future hook types (planned):
- Tool invocation hooks
- Session intervention hooks
- Pre/post execution hooks

---

## 🛠️ Advanced Startup Options

| Option | Description | Example |
|--------|-------------|---------|
| `-w` | Specify workspace directory | `qodercli -w /path/to/project` |
| `-c` | Continue last session | `qodercli -c` |
| `-r` | Resume specific session | `qodercli -r <session-id>` |
| `--allowed-tools` | Allow only specified tools | `qodercli --allowed-tools=READ,WRITE` |
| `--disallowed-tools` | Disallow specified tools | `qodercli --disallowed-tools=READ,WRITE` |
| `--max-turns` | Maximum dialog turns | `qodercli --max-turns=10` |
| `--yolo` | Skip permission checks | `qodercli --yolo` |
| `--worktree` | Create worktree job | `qodercli --worktree "task"` |
| `--branch` | Set branch for worktree | `qodercli --worktree --branch=main` |
| `--agents` | Define custom agents inline | `qodercli --agents='{...}'` |
| `--attachment` | Attach image files | `qodercli --attachment=img.png` |
| `--with-claude-config` | Load .claude folder configs | `qodercli --with-claude-config` |

---

## ⚠️ Rules

1. **Use pty:true for TUI mode** - interactive CLI needs a terminal!
2. **Print mode for automation** - use `-p` flag for non-interactive tasks
3. **Respect workdir** - Qoder sees only the specified directory's context
4. **Monitor with process:log** - check background session progress
5. **Use worktrees for parallel work** - avoid read/write conflicts
6. **Initialize AGENTS.md** - helps Qoder understand project context
7. **Configure permissions** - set appropriate access rules per project
8. **Leverage subagents** - specialized agents for specific tasks
9. **Add MCP servers** - extend capabilities with external tools
10. **Works in all sessions** - environment variables are inherited automatically
11. **Use ultimate model for complex tasks** - refactoring, architecture, code review
12. **Quest mode for spec-driven development** - let AI coordinate subagents

---

## Progress Updates (Critical)

When you spawn Qoder CLI in the background, keep the user in the loop:

- Send 1 short message when you start (what's running + where)
- Then only update again when something changes:
  - a milestone completes (build finished, tests passed)
  - the CLI asks a question / needs input
  - you hit an error or need user action
  - the CLI finishes (include what changed + where)
- If you kill a session, immediately say you killed it and why

This prevents the user from seeing only "Agent failed before reply" and having no idea what happened.

---

## Auto-Notify on Completion

For long-running background tasks, append a wake trigger:

```bash
bash pty:true workdir:~/project background:true command:"qodercli --model=ultimate 'Build a REST API for todos.

When completely finished, run: openclaw system event --text \"Done: Built todos REST API with CRUD endpoints\" --mode now'"
```

This triggers an immediate wake event — you get pinged in seconds, not minutes.

---

## 🌐 Cross-Session Usage

### In Group Chats (DingTalk, Discord, Slack)

```bash
# Just use normal commands - environment is inherited
bash workdir:~/project command:"qodercli -p 'Help me fix this bug'"

# No special setup needed!
```

### In Direct Messages

Same as group chats - works out of the box.

### In Shared Workspaces

```bash
# Specify the workspace explicitly
bash workdir:/shared/project command:"qodercli --model=ultimate -p 'Refactor this'"
```

### Privacy Note

- Qoder CLI only accesses the specified `workdir`
- Environment variables are inherited from the host shell
- No credentials are exposed in chat messages
- Each session has isolated Qoder CLI state

---

## 📊 Comparison with Other Coding Agents

| Feature | Qoder CLI | Codex | Claude Code |
|---------|-----------|-------|-------------|
| TUI Mode | ✅ | ✅ | ✅ |
| Print Mode | ✅ | ✅ | ❌ |
| Subagents | ✅ | ❌ | ❌ |
| Worktrees | ✅ | ❌ | ❌ |
| MCP Servers | ✅ | ✅ | ✅ |
| Memory (AGENTS.md) | ✅ | ✅ | ✅ |
| Custom Commands | ✅ | ❌ | ❌ |
| Hooks | ✅ | ❌ | ❌ |
| Model Selection | ✅ (auto-routing) | ❌ | ❌ |
| Quest Mode | ✅ | ❌ | ❌ |
| Permission System | ✅ (granular) | ⚠️ | ⚠️ |
| All-Sessions Ready | ✅ | ⚠️ | ⚠️ |

**Qoder CLI strengths:**
- Subagents for specialized tasks
- Worktrees for parallel development
- Quest mode for spec-driven development
- Custom commands and hooks
- Automatic model routing
- Granular permission system
- Cross-session compatibility

---

## 📋 Quick Reference Card

```bash
# Quick task (print mode, auto model)
qodercli -p "Your prompt"

# High-quality task (ultimate model)
qodercli --model=ultimate -p "Your prompt"

# Interactive session (TUI mode, needs PTY)
qodercli

# Quest mode (spec-driven)
qodercli -p "/quest"

# Background task (worktree)
qodercli --worktree -p "Your task"

# Check status
qodercli status

# Skip permissions (use with caution)
qodercli --yolo -p "Your prompt"

# Continue last session
qodercli -c -p "Continue"

# JSON output
qodercli --output-format=json -p "Analyze"

# With custom subagents
qodercli --agents='{"reviewer":{...}}' -p "Review this"
```

---

## 🔧 Troubleshooting

### Not Logged In
```bash
# Check status
qodercli status

# Set environment variable
export QODER_PERSONAL_ACCESS_TOKEN="your_token"

# Or use TUI login
qodercli
# Then: /login
```

### Permission Denied
```bash
# Use yolo mode (caution)
qodercli --yolo -p "task"

# Or configure permissions in ~/.qoder/settings.json
```

### TUI Not Working
```bash
# Ensure PTY mode
bash pty:true command:"qodercli"

# Check terminal compatibility
qodercli --version
```

### Model Selection Issues
```bash
# Explicitly specify model
qodercli --model=ultimate -p "task"

# Or use auto for automatic routing
qodercli --model=auto -p "task"
```
