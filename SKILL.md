---
name: qoder-cli
description: "Delegate coding tasks to Qoder CLI using Print mode (non-interactive). Use when: (1) building/creating new features or apps, (2) code reviews, (3) refactoring, (4) iterative coding that needs file exploration. Supports subagents, worktrees, MCP servers, quest mode, commands, and hooks. Includes timeout prevention strategies (background execution, auto-notify, worktrees). Works in all session types. NOT for: simple one-liner fixes (just edit), reading code (use read tool). Requires qodercli installed."
metadata: { "openclaw": { "emoji": "🤖", "requires": { "anyBins": ["qodercli"] } } }
---

# Qoder Agent (Print Mode - Non-Interactive)

**Use Print mode (`-p` or `-q`)** for all Qoder CLI work in OpenClaw. TUI mode is not supported in automated environments.

**✅ All-Sessions Ready:** This skill works in:

- Direct 1:1 chats
- Group chats (DingTalk, Discord, Slack, etc.)
- Shared workspace sessions
- Private sessions

---

## ⚠️ Important: Print Mode Only

**TUI mode is NOT supported** in OpenClaw or other automated environments due to TTY requirements.

**Always use Print mode** with the `-p` flag (or `-q` for quiet mode):

```bash
# ✅ Correct - Print mode (non-interactive)
bash workdir:~/project command:"qodercli -p 'Add error handling'"

# ✅ Also correct - Quiet mode (hide spinner)
bash workdir:~/project command:"qodercli -q -p 'Add error handling'"

# ❌ Wrong - TUI mode requires interactive terminal
bash pty:true command:"qodercli"  # Will fail
```

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

## 📋 Slash Commands (TUI Mode Reference)

While TUI mode is not supported in OpenClaw, knowing these commands helps understand Qoder's capabilities:

| Command            | Description                                      |
| ------------------ | ------------------------------------------------ |
| `/login`           | Login to Qoder account                           |
| `/help`            | Show TUI help info                               |
| `/init`            | Initialize/update `AGENTS.md` memory file        |
| `/memory`          | Edit `AGENTS.md` memory file                     |
| `/quest`           | Spec-driven task delegation                      |
| `/review`          | Review local code changes                        |
| `/resume`          | View/restore sessions                            |
| `/clear`           | Clear current session history                    |
| `/compact`         | Summarize current session history                |
| `/usage`           | Show account status and credits usage            |
| `/status`          | View CLI status (version, model, account, etc.)  |
| `/config`          | Show system configuration                        |
| `/agents`          | Manage subagents (view, create, manage)          |
| `/bashes`          | View running background Bash tasks               |
| `/release-notes`   | Show CLI changelog                               |
| `/vim`             | Open external editor for input                   |
| `/feedback`        | Report issues                                    |
| `/quit`            | Exit TUI                                         |
| `/logout`          | Logout from Qoder account                        |

---

## 🚀 Quick Start

### Basic Usage

```bash
# Quick one-shot task
bash workdir:~/project command:"qodercli -p 'Add error handling to the API calls'"

# With ultimate model for best quality
bash workdir:~/project command:"qodercli --model=ultimate -p 'Refactor this module'"

# With JSON output
bash workdir:~/project command:"qodercli --output-format=json -p 'Analyze this code'"

# Continue last session
bash workdir:~/project command:"qodercli -c -p 'Continue the refactoring'"

# Max turns limit
bash workdir:~/project command:"qodercli --max-turns=10 -p 'Fix the bug'"

# Yolo mode (skip permissions)
bash workdir:~/project command:"qodercli --yolo -p 'Make the changes'"
```

---

## 🎯 Print Mode Flags

| Flag                  | Description                            | Example                                  |
| --------------------- | -------------------------------------- | ---------------------------------------- |
| `-p`                  | **Required** - Run non-interactively   | `qodercli -p "task"`                     |
| `-q`                  | Quiet mode (hide spinner)              | `qodercli -q -p "task"`                  |
| `--output-format`     | Output format: `text`, `json`, `stream-json` | `qodercli --output-format=json`          |
| `-w`                  | Specify workspace directory            | `qodercli -w /path/to/project`           |
| `-c`                  | Continue last session                  | `qodercli -c -p "continue"`              |
| `-r`                  | Resume specific session by ID          | `qodercli -r <session-id> -p "continue"` |
| `--model`             | Model tier selection                   | `qodercli --model=ultimate`              |
| `--max-turns`         | Maximum dialog turns (0 = unlimited)   | `qodercli --max-turns=10`                |
| `--max-output-tokens` | Max tokens: `16k`, `32k`               | `qodercli --max-output-tokens=32k`       |
| `--yolo`              | Skip permission checks                 | `qodercli --yolo -p "task"`              |
| `--allowed-tools`     | Allow only specified tools             | `qodercli --allowed-tools=READ,WRITE`    |
| `--disallowed-tools`  | Disallow specified tools               | `qodercli --disallowed-tools=Bash`       |
| `--agents`            | JSON object defining custom agents     | `qodercli --agents='{"reviewer":{...}}'` |
| `--attachment`        | Attach image files (repeatable)        | `qodercli --attachment=img.png`          |
| `--worktree`          | Create worktree job for parallel work  | `qodercli --worktree -p "task"`          |
| `--branch`            | Set branch for worktree job            | `qodercli --worktree --branch=main`      |

---

## 🧠 Model Selection

Qoder CLI uses **automatic model routing** - it selects the globally optimal model based on task characteristics. You can override this:

| Model Value   | Use Case                                                  | Speed      | Quality    | Cost       |
| ------------- | --------------------------------------------------------- | ---------- | ---------- | ---------- |
| `auto`        | **Default** - automatic routing                           | ⚡⚡⚡     | ⭐⭐⭐     | 💰💰💰     |
| `efficient`   | Quick tasks, simple queries                               | ⚡⚡⚡⚡   | ⭐⭐       | 💰💰       |
| `lite`        | Very simple tasks                                         | ⚡⚡⚡⚡⚡ | ⭐         | 💰         |
| `performance` | Complex tasks needing depth                               | ⚡⚡       | ⭐⭐⭐⭐   | 💰💰💰💰   |
| `ultimate`    | **Best quality** - refactoring, architecture, code review | ⚡         | ⭐⭐⭐⭐⭐ | 💰💰💰💰💰 |
| `qmodel`      | Qwen model family                                         | ⚡⚡⚡     | ⭐⭐⭐⭐   | 💰💰💰     |
| `q35model`    | Qwen 3.5 specific                                         | ⚡⚡⚡     | ⭐⭐⭐⭐   | 💰💰💰     |
| `mmodel`      | MiniMax model                                             | ⚡⚡⚡     | ⭐⭐⭐⭐   | 💰💰💰     |
| `gmodel`      | GPT model family                                          | ⚡⚡       | ⭐⭐⭐⭐⭐ | 💰💰💰💰   |

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
# Quest mode via prompt
bash workdir:~/project command:"qodercli --model=ultimate -p 'Build a REST API with authentication, rate limiting, and logging'"
```

Quest Mode automatically:

1. Analyzes requirements
2. Routes to appropriate subagents
3. Coordinates multi-step development
4. Ensures consistency across files

---

## 🤖 Subagents

Subagents are specialized AI agents for specific tasks with their own context windows and tool permissions. You can configure custom system prompts to guide their behavior.

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

### Create a Subagent (Auto)

In TUI mode, use `/agents`, tab to select **User** or **Project**, then choose **Create new agent** and enter your description.

### Use Subagents

```bash
# Explicit invocation
bash workdir:~/project command:"qodercli -p 'Use code-review subagent to check code issues'"

# Implicit invocation (AI auto-routes based on task)
bash workdir:~/project command:"qodercli -p 'Analyze this code for potential performance issues'"

# Chained subagents
bash workdir:~/project command:"qodercli -p 'First use design subagent for system design, then use code-review subagent for code review'"

# Custom agents inline via --agents flag
bash workdir:~/project command:"qodercli --agents='{\"reviewer\":{\"description\":\"Reviews code\",\"prompt\":\"You are a code reviewer\"}}' -p 'Review this'"
```

### Subagent Best Practices

- **Specialize**: Each subagent should have a clear, focused responsibility
- **Tool permissions**: Only grant the tools each subagent needs
- **Chain wisely**: Use multiple subagents in sequence for complex workflows
- **System prompts**: Write clear, actionable instructions in the prompt

---

## 🌳 Worktree (Parallel Jobs)

Worktree jobs are concurrent jobs that use Git worktrees to run tasks in parallel, avoiding read/write conflicts.

**Requirements:** Git installed and usable locally.

### Commands

| Command                                 | Description                       |
| --------------------------------------- | --------------------------------- |
| `qodercli --worktree "job description"` | Create and start new worktree job |
| `qodercli jobs --worktree`              | List existing worktree jobs       |
| `qodercli rm <jobId>`                   | Remove a job (delete worktree)    |

### Create a Job

```bash
# Basic worktree job (non-interactive)
bash workdir:~/project command:"qodercli --worktree -p 'Fix issue #78'"

# With branch specification
bash workdir:~/project command:"qodercli --worktree --branch=main -p 'Implement feature'"

# With max turns
bash workdir:~/project command:"qodercli --worktree --max-turns=20 -p 'Complex refactoring'"

# Run in background and stop container when done
bash workdir:~/project background:true command:"qodercli --worktree -p 'Fix bug' && exit"
```

### View Jobs

```bash
bash workdir:~/project command:"qodercli jobs --worktree"
```

Output example:
```
Qoder jobs for workspace: /Users/demo/project

Worktree Jobs:
ID              INIT PROMPT    PATH                                STATUS      CREATED             
11758283139787  [I] 你好        ~/.qoder/worktrees/11758283139787   running     5 minutes ago       
11758283382928  [N] 你好        ~/.qoder/worktrees/11758283382928   exited      1 minute ago        

Total: 3 worktree job(s)
```

**Fields:**
- **ID**: Job unique identifier (not container ID)
- **INIT PROMPT**: Initial task description
- **PATH**: Git worktree directory path
- **STATUS**: Container status (running/exited)
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

# With type (-t: stdio, sse, streamable-http) and scope (-s: user/project)
bash command:"qodercli mcp add playwright -t stdio -s project -- npx -y @playwright/mcp@latest"
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
```

### MCP Server Files

- **User-level**: `~/.qoder.json` - Won't be committed
- **Project-level**: `${project}/.mcp.json` - Usually committed

---

## 🔐 Permissions

Qoder CLI enforces fine-grained tool execution permissions.

### Configuration Files (precedence: high → low)

1. `${project}/.qoder/settings.local.json` - Project-level, highest (add to .gitignore)
2. `${project}/.qoder/settings.json` - Project-level
3. `~/.qoder/settings.json` - User-level

### Permission Strategies

| Strategy | Description                                         |
| -------- | --------------------------------------------------- |
| `allow`  | Automatically allow matching operations             |
| `deny`   | Automatically deny matching operations              |
| `ask`    | Prompt for permission (default for outside project) |

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
    "deny": []
  }
}
```

By default, CLI uses safer "Ask" strategy for file access outside the project directory, and automatically creates standard read/write rules within the project directory on startup.

### Permission Types

#### 1. Read & Edit Rules

Patterns follow gitignore-style matching:

| Pattern Form       | Description               | Example                     | Matches                        |
| ------------------ | ------------------------- | --------------------------- | ------------------------------ |
| `/path`            | Absolute from system root | `Read(/Users/demo/**)`      | `/Users/demo/xx`               |
| `~/path`           | From home directory       | `Read(~/Documents/*.png)`   | `/Users/demo/Documents/xx.png` |
| `path` or `./path` | Relative to current dir   | `Read(/*.java)`             | `./xx.java`                    |
| `!**`              | Negation pattern          | `Read(!**/node_modules/**)` | Excludes node_modules          |

Read rules apply to all file-reading tools: Grep, Glob, LS.

#### 2. WebFetch Rules

Restrict domains for network fetch tools:

```json
{
  "permissions": {
    "allow": ["WebFetch(domain:example.com)", "WebFetch(domain:*.github.io)"]
  }
}
```

- `WebFetch(domain:example.com)` restricts fetching to **example.com** only

#### 3. Bash Rules

Restrict commands for shell execution tools:

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

- `Bash(npm run build)` matches exactly `npm run build`
- `Bash(npm run test:*)` matches commands starting with `npm run test:`
- `Bash(curl http://site.com/:*)` matches curl commands starting with `curl http://site.com/`

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

### Generate/Manage

```bash
# Auto-generate AGENTS.md in project (via TUI /init command)
# Or manually create:
cat > ~/project/AGENTS.md << 'EOF'
# Project Guidelines

## Architecture
- MVC pattern
- REST API design

## Code Style
- ESLint strict mode
- Prettier formatting
EOF
```

---

## 📜 Commands (Custom Slash Commands)

Commands extend slash command functionality via `.md` files. Define reusable prompts as commands.

### File Locations

- **User-level**: `~/.qoder/commands/<name>.md` - Applies to all projects
- **Project-level**: `${project}/commands/<name>.md` - Current project only

### Create a Command

Create a markdown file with frontmatter:

```markdown
---
description: "Intelligent workflow orchestrator for feature development"
---

First use design subagent to complete system design, then use code-review subagent to review the code
```

Save as `~/.qoder/commands/quest.md`, then use in TUI with `/quest`.

### Use in Print Mode

While Commands are designed for TUI, you can replicate the behavior in Print mode:

```bash
# Replicate a command's behavior
bash workdir:~/project command:"qodercli --model=ultimate -p 'First use design subagent for system design, then use code-review subagent for code review'"
```

---

## 🔔 Hooks

Qoder CLI provides Hook capabilities at key execution stages for external integration (notifications, external tools, etc.).

### Configuration Files

Hooks are defined in configuration files (precedence: high → low):

1. `${project}/.qoder/settings.local.json` - Project-level, highest (add to .gitignore)
2. `${project}/.qoder/settings.json` - Project-level
3. `~/.qoder/settings.json` - User-level

### Example Configuration

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

### Create Notification Script

Create `~/notification.sh`:

```bash
#!/bin/bash

input=$(cat)

sessionId=$(echo $input | jq -r '.session_id')
messageInfo=$(echo $input | jq -r '.message')
workspacePath=$(echo $input | jq -r '.cwd')

if [[ "$messageInfo" =~ ^Agent ]]; then
  osascript -e 'display notification "✅ Task completed!" with title "QoderCLI"'
else
  osascript -e 'display notification "⌛️ Task requires authorization..." with title "QoderCLI"'
fi

exit 0
```

Make it executable:
```bash
chmod +x ~/notification.sh
```

> **Note:** Currently, Qoder CLI only supports Notification-type Hooks. Different hook stages can介入 Agent's main execution flow while remaining decoupled from CLI. More Hook types (tool invocation, session intervention) may be added in the future.

---

## ⚡ Advanced Options

| Option               | Description                 | Example                               |
| -------------------- | --------------------------- | ------------------------------------- |
| `-w`                 | Specify workspace directory | `qodercli -w /path/to/project`        |
| `-c`                 | Continue last session       | `qodercli -c -p "continue"`           |
| `-r`                 | Resume specific session     | `qodercli -r <session-id>`            |
| `--allowed-tools`    | Allow only specified tools  | `qodercli --allowed-tools=READ,WRITE` |
| `--disallowed-tools` | Disallow specified tools    | `qodercli --disallowed-tools=Bash`    |
| `--max-turns`        | Maximum dialog turns        | `qodercli --max-turns=10`             |
| `--yolo`             | Skip permission checks      | `qodercli --yolo`                     |
| `--worktree`         | Create worktree job         | `qodercli --worktree "task"`          |
| `--branch`           | Set branch for worktree     | `qodercli --worktree --branch=main`   |
| `--agents`           | Define custom agents inline | `qodercli --agents='{...}'`           |
| `--attachment`       | Attach image files          | `qodercli --attachment=img.png`       |

---

## ⚠️ Rules

1. **Print mode only** - TUI mode not supported in OpenClaw
2. **Always use `-p` or `-q` flag** - Non-interactive mode required
3. **Respect workdir** - Qoder sees only the specified directory's context
4. **Monitor with process:log** - Check background session progress
5. **Use worktrees for parallel work** - Avoid read/write conflicts
6. **Initialize AGENTS.md** - Helps Qoder understand project context
7. **Configure permissions** - Set appropriate access rules per project
8. **Leverage subagents** - Specialized agents for specific tasks
9. **Add MCP servers** - Extend capabilities with external tools
10. **Works in all sessions** - Environment variables are inherited automatically
11. **Use ultimate model for complex tasks** - Refactoring, architecture, code review
12. **Use Commands for reusable workflows** - Define custom slash commands via `.md` files
13. **Configure Hooks for notifications** - Get notified when tasks complete
14. **Manage worktree jobs** - Use `jobs --worktree` to view, `rm` to delete
15. **Prevent timeouts** - Use `background:true` for tasks >2 minutes
16. **Always notify user** - Send start message + completion notification
17. **Break large tasks** - Split multi-hour work into phases
18. **Use --max-turns** - Control duration for shorter tasks

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

## ⏱️ Handling Long-Running Tasks (Timeout Prevention)

Qoder CLI tasks can take minutes to hours. Use these strategies to avoid agent timeouts:

### Strategy 1: Background Execution + Auto-Notify (Recommended)

For tasks expected to take >2 minutes:

```bash
# Run in background with completion notification
bash workdir:~/project background:true command:"qodercli --model=ultimate -p 'Build a REST API for todos'

# Wake trigger when done
openclaw system event --text 'Qoder completed: Built todos REST API' --mode now"
```

**Benefits:**
- Agent won't timeout (runs in background)
- User gets notified immediately when done
- You can monitor progress asynchronously

### Strategy 2: Worktree for Parallel Long Tasks

For complex tasks that need isolation:

```bash
# Create worktree job (runs in isolated container)
bash workdir:~/project background:true command:"qodercli --worktree --branch=main -p 'Refactor authentication module'

# Notify when complete
openclaw system event --text 'Worktree job completed: Auth refactoring done' --mode now"
```

**Benefits:**
- Isolated from main workspace
- Can run multiple jobs in parallel
- No file conflicts

### Strategy 3: Break Into Smaller Tasks

For very large tasks, split into phases:

```bash
# Phase 1: Analysis
bash workdir:~/project command:"qodercli -p 'Analyze the codebase and create a refactoring plan'"

# Phase 2: Implementation (background)
bash workdir:~/project background:true command:"qodercli --model=ultimate -p 'Implement the refactoring plan from phase 1'

openclaw system event --text 'Phase 2 complete: Refactoring implemented' --mode now"

# Phase 3: Testing (after phase 2 completes)
bash workdir:~/project command:"qodercli -p 'Write tests for the refactored code'"
```

### Strategy 4: Use --max-turns to Control Duration

Limit the conversation turns to prevent runaway tasks:

```bash
# Limit to 10 turns (~5-10 minutes)
bash workdir:~/project command:"qodercli --max-turns=10 -p 'Fix the login bug'"

# For longer tasks, use background + notify
bash workdir:~/project background:true command:"qodercli --max-turns=30 -p 'Implement user registration'

openclaw system event --text 'Registration feature complete' --mode now"
```

### Strategy 5: Periodic Progress Updates

For very long tasks, add intermediate checkpoints:

```bash
bash workdir:~/project background:true command:"
qodercli --model=ultimate -p 'Build complete CRUD app'

# Intermediate notification after build
echo 'Build phase complete, starting tests...' > /tmp/qoder_progress

# Final notification
openclaw system event --text 'CRUD app complete: Build + Tests done' --mode now
"
```

---

## 📊 Timeout Prevention Quick Reference

| Task Duration | Strategy | Example |
| ------------- | -------- | ------- |
| <1 min | Direct (no background) | `qodercli -p "Fix typo"` |
| 1-5 min | Direct with `--max-turns` | `qodercli --max-turns=10 -p "Add validation"` |
| 5-30 min | Background + notify | `background:true ... openclaw system event` |
| 30+ min | Worktree + background | `--worktree background:true ...` |
| Hours | Break into phases | Multiple sequential tasks |

---

## ⚠️ Critical: Always Notify User

When using background execution:

1. **Tell user immediately** what's running and where
2. **Give ETA** if possible (e.g., "This will take ~10 minutes")
3. **Explain notification** (e.g., "I'll ping you when it's done")
4. **Follow through** - send the completion notification

**Example user message:**
> "🚀 Starting: Building REST API with authentication (this will take ~15 minutes). I'll notify you when it's complete!"

**Completion message:**
> "✅ Done: REST API built successfully! Created 5 files with CRUD endpoints + JWT auth. Check `/Users/clawbot/.openclaw/workspace/projects/todo-api`"

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

| Feature            | Qoder CLI         | Codex | Claude Code |
| ------------------ | ----------------- | ----- | ----------- |
| Print Mode         | ✅                | ✅    | ❌          |
| Subagents          | ✅                | ❌    | ❌          |
| Worktrees          | ✅                | ❌    | ❌          |
| MCP Servers        | ✅                | ✅    | ✅          |
| Memory (AGENTS.md) | ✅                | ✅    | ✅          |
| Model Selection    | ✅ (auto-routing) | ❌    | ❌          |
| Quest Mode         | ✅                | ❌    | ❌          |
| Commands (.md)     | ✅                | ❌    | ❌          |
| Hooks              | ✅                | ❌    | ❌          |
| Permission System  | ✅ (granular)     | ⚠️    | ⚠️          |
| All-Sessions Ready | ✅                | ⚠️    | ⚠️          |

**Qoder CLI strengths:**

- **Subagents** for specialized tasks with custom prompts
- **Worktrees** for parallel development without conflicts
- **Quest mode** for spec-driven development
- **Automatic model routing** - picks optimal model per task
- **Commands** - reusable workflows via `.md` files
- **Hooks** - external integrations and notifications
- **Granular permission system** - fine-grained access control
- **Cross-session compatibility** - works everywhere

---

## 📋 Quick Reference Card

```bash
# Quick task (print mode, auto model)
qodercli -p "Your prompt"

# Quiet mode (hide spinner)
qodercli -q -p "Your prompt"

# High-quality task (ultimate model)
qodercli --model=ultimate -p "Your prompt"

# Quest mode (spec-driven development)
qodercli -p "Build a REST API with authentication, rate limiting, and logging"

# Background task (worktree for parallel work)
qodercli --worktree -p "Your task"

# Worktree with specific branch
qodercli --worktree --branch=main -p "Implement feature"

# Check status (version, model, account, connectivity)
qodercli status

# Skip permissions (use with caution)
qodercli --yolo -p "Your prompt"

# Continue last session
qodercli -c -p "Continue"

# Resume specific session by ID
qodercli -r <session-id> -p "Continue"

# JSON output
qodercli --output-format=json -p "Analyze"

# With custom subagents inline
qodercli --agents='{"reviewer":{...}}' -p "Review this"

# Limit dialog turns
qodercli --max-turns=10 -p "Fix the bug"

# Attach image files
qodercli --attachment=img.png -p "Analyze this screenshot"

# MCP server management
qodercli mcp list
qodercli mcp add playwright -- npx -y @playwright/mcp@latest
qodercli mcp remove playwright

# Worktree job management
qodercli jobs --worktree
qodercli rm <jobId>

# === Timeout Prevention ===

# Background task with auto-notify (>2 min tasks)
bash workdir:~/project background:true command:"qodercli --model=ultimate -p 'Large task'

openclaw system event --text 'Task complete' --mode now"

# Quick task with turn limit (<2 min)
qodercli --max-turns=10 -p "Quick fix"

# Long task in worktree (30+ min)
bash workdir:~/project background:true command:"qodercli --worktree --max-turns=50 -p 'Major refactoring'"
```

---

## 🔧 Troubleshooting

### Not Logged In

```bash
# Check status
qodercli status

# Set environment variable
export QODER_PERSONAL_ACCESS_TOKEN="your_token"

# Or login via TUI (if available)
qodercli /login
```

### Permission Denied

```bash
# Use yolo mode (caution - skips all permission checks)
qodercli --yolo -p "task"

# Or configure permissions in ~/.qoder/settings.json
# Or add project-specific rules in ${project}/.qoder/settings.json
```

### Model Selection Issues

```bash
# Explicitly specify model
qodercli --model=ultimate -p "task"

# Or use auto for automatic routing (default)
qodercli --model=auto -p "task"
```

### TUI Mode Error

**TUI mode is NOT supported in OpenClaw.** Always use Print mode:

```bash
# ✅ Correct
qodercli -p "Your task"
qodercli -q -p "Your task"  # Quiet mode

# ❌ Wrong (will fail)
qodercli  # TUI requires interactive terminal
```

### Worktree Job Stuck

```bash
# View all jobs
qodercli jobs --worktree

# Remove stuck job
qodercli rm <jobId>
```

### MCP Server Not Working

```bash
# List installed servers
qodercli mcp list

# Remove and re-add
qodercli mcp remove <name>
qodercli mcp add <name> -- <command>

# Check MCP config files
# User-level: ~/.qoder.json
# Project-level: ${project}/.mcp.json
```

### Output Truncated

```bash
# Increase max output tokens
qodercli --max-output-tokens=32k -p "task"

# Or use JSON output for structured parsing
qodercli --output-format=json -p "task"
```

### Task Timing Out

**Problem:** Qoder CLI task takes too long, agent times out before completion.

**Solutions:**

```bash
# 1. Use background execution for long tasks
bash workdir:~/project background:true command:"qodercli --model=ultimate -p 'Large task'

openclaw system event --text 'Task complete' --mode now"

# 2. Limit turns for shorter tasks
qodercli --max-turns=10 -p "Quick fix"

# 3. Use worktree for isolation
bash workdir:~/project background:true command:"qodercli --worktree -p 'Complex refactoring'"

# 4. Break into phases
# Phase 1
qodercli -p "Analyze and create plan"
# Phase 2 (background)
bash workdir:~/project background:true command:"qodercli -p 'Implement plan'"
```

**Best Practice:** If a task might take >2 minutes, use `background:true` from the start.
