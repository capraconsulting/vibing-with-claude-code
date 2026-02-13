# Claude Code Cheatsheet

---

## Starting & Stopping

| Command | Description |
|---------|------------|
| `claude` | Start Claude Code in current directory |
| `claude "prompt"` | Start with an initial prompt |
| `claude -p "prompt"` | Non-interactive (headless) mode |
| `claude --continue` | Continue most recent session |
| `claude --resume` | Open session picker |
| `claude --model sonnet` | Start with a specific model |
| `claude --plugin-dir ./path` | Load a plugin for testing |
| `/exit` or Ctrl+C | Exit Claude Code |
| Esc | Stop current generation |

---

## Permission Modes

Press **Shift+Tab** to cycle: Default → Accept Edits → Plan Mode → Default

| Mode | Indicator | What Claude can do |
|------|-----------|-------------------|
| **Default** | (none) | Asks permission for each edit and command |
| **Accept Edits** | `⏵⏵ accept edits on` | Auto-accepts file edits, still asks for commands |
| **Plan Mode** | `⏸ plan mode on` | Read-only — can only read files and think, no changes |

**When to use each:**
- **Plan Mode** → Starting a new feature, exploring unfamiliar code, reviewing an approach
- **Accept Edits** → You trust the direction and want to move faster
- **Default** → Working on sensitive code, learning, or reviewing each change

---

## Models & Thinking

| Command | Description |
|---------|------------|
| `/model` | Switch model + set effort level (arrow keys) |
| `/fast` | Toggle fast mode (2.5x speed, higher cost) |
| Option+T / Alt+T | Toggle extended thinking |

| Model | Best for | Speed |
|-------|----------|-------|
| **Opus** | Complex reasoning, architecture, hard bugs | Slower |
| **Sonnet** | Daily coding, good balance | Medium |
| **Haiku** | Quick answers, simple tasks | Fastest |

**Effort levels** (Opus only, set in `/model` with arrow keys):
- **Low** → Fast responses, minimal thinking
- **Medium** → Balanced (default)
- **High** → Deep reasoning for hard problems

---

## Terminal Setup

| Setting | How |
|---------|-----|
| **Line breaks** | `\` + Enter (all terminals), Shift+Enter (iTerm2, WezTerm, Ghostty, Kitty) |
| **Shift+Enter setup** | Run `/terminal-setup` in Claude Code (VS Code, Alacritty, Warp) |
| **Vim mode** | `/vim` to toggle, or enable via `/config` |
| **Theme** | `/config` to match your terminal theme |
| **Option as Meta** | Terminal.app: Settings → Profiles → Keyboard → "Use Option as Meta Key" |

**Notifications (iTerm2):** Preferences → Profiles → Terminal → "Silence bell" + Filter Alerts

**Handling large inputs:** Avoid pasting very long content directly. Write to a file and ask Claude to read it instead.

---

## Sessions

| Command | Description |
|---------|------------|
| `/rename <name>` | Name the current session |
| `/rename` | Auto-generate a session name |
| `/resume` | Browse and resume a previous session |
| `claude --continue` | Continue most recent session from CLI |
| `claude --resume <name>` | Resume a named session from CLI |
| `claude --from-pr 123` | Resume sessions linked to a PR |

Sessions auto-save. Close your terminal and come back later — your work is preserved.

---

## Context Management

| Command | Description |
|---------|------------|
| `/compact` | Summarize conversation to free up context space |
| `/compact Focus on API` | Compact with specific focus instructions |
| `/clear` | Clear conversation history completely |
| `/context` | Visual grid of context window usage |
| `/cost` | Show token usage and cost for this session |
| `/rewind` | Undo the last turn (rolls back file changes too) |
| **Esc + Esc** | Open rewind menu (same as `/rewind`) |

**When to use:**
- `/compact` → Conversation getting long, Claude losing track (~every 30-40 min)
- `/clear` → Switching to a completely different task
- `/cost` → Check periodically to build intuition for token usage

---

## Quick Commands

| Prefix | Description | Example |
|--------|------------|---------|
| `!` | Run shell command directly (output added to context) | `!npm test`, `!git status` |
| `/` | Slash command or skill | `/init`, `/review`, `/compact` |
| `@` | Reference a file (tab completion) | `@src/app.tsx` |

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Enter** | Send message |
| **Shift+Tab** | Cycle permission modes (Default → Accept Edits → Plan) |
| **Esc** | Stop generation / cancel current action |
| **Up / Down** | Cycle through previous messages |
| **Ctrl+V** | Paste image from clipboard |
| **Ctrl+O** | Toggle verbose mode (see internal tool calls) |
| **Ctrl+C** | Cancel / exit |
| **Ctrl+L** | Clear terminal screen (keeps conversation) |
| **Option+T / Alt+T** | Toggle extended thinking |
| **Ctrl+G** | Open current prompt in external editor |
| **\\+Enter** | New line in prompt (all terminals) |
| **Option+Enter** | New line in prompt (macOS) |

---

## Images & Screenshots

| Method | How |
|--------|-----|
| **Paste** | Copy image, then **Ctrl+V** |
| **Drag and drop** | Drag image file into terminal window |
| **File path** | Reference directly: `Look at ./mockup.png and implement this layout` |

Supported formats: PNG, JPG, GIF, WebP. Also reads PDFs.

**Use cases:**
- Paste a screenshot of a bug instead of describing it
- Drag in a design mockup and ask Claude to implement it
- Reference an error screenshot for debugging

---

## Prompting Patterns

### Be Specific (what + where + how)

```
Add input validation to the recipe form in src/components/RecipeForm.tsx:
- Serving count must be a positive number
- Show inline error message in red below the input
- Disable submit button until all fields are valid
```

### Break Into Steps

```
1. Add a "Delete" button next to each item in the list
2. Clicking delete should remove the item via the API
3. Add a confirmation dialog before deleting
```

### Reference Files Explicitly

```
In src/api/routes.ts, add a GET /stats endpoint that returns
total count, average, and most recent entry.
```

### Provide Error Context

```
I'm getting this error when I click submit:
TypeError: Cannot read property 'map' of undefined
```

### Ask Claude to Verify

```
Run the tests to make sure nothing broke.
```
```
Open the app in the browser and check if the form validation works.
```

### Specify Constraints

```
Add search functionality. Don't add any new dependencies —
use the built-in Array.filter method.
```

### Give Examples

```
Format dates like "Jan 15, 2025" not "2025-01-15".
```

---

## CLAUDE.md

Claude reads `CLAUDE.md` at the start of **every session**. Use `/init` to generate one.

### Template

```markdown
# Project Overview
Workout tracker with React frontend and Express API.

# Tech Stack
- React 18 + TypeScript, Vite, Tailwind CSS
- Express.js backend with SQLite
- Jest for testing

# Commands
- `npm run dev` — start dev server
- `npm run server` — start API server
- `npm test` — run tests
- `npm run lint` — lint code

# Code Style
- Functional components with hooks
- Named exports (no default exports)
- Tailwind classes, not CSS files
- API routes in src/api/routes/

# Important Context
- All data persisted in SQLite (./data/db.sqlite)
- API runs on port 3001, frontend on port 3000
```

### Hierarchy

| Location | Scope |
|----------|-------|
| `~/.claude/CLAUDE.md` | All your projects (personal preferences) |
| `./CLAUDE.md` | This project |
| `./src/CLAUDE.md` | Subdirectory-specific |

Files closer to the code take precedence.

### Auto Memory

```
Remember that this project uses pnpm, not npm.
```

Claude also picks up important context from conversations automatically.

---

## Checkpointing

Claude automatically checkpoints before every edit. Use **Esc + Esc** or `/rewind` to open the rewind menu.

| Action | What it does |
|--------|-------------|
| **Restore code and conversation** | Revert both files and chat to that point |
| **Restore conversation** | Rewind chat, keep current code |
| **Restore code** | Revert files, keep conversation |
| **Summarize from here** | Compress everything after this point into a summary |

- Checkpoints persist across sessions (available after `/resume`)
- Only tracks Claude's file editing tools — bash commands (`rm`, `mv`) are **not** tracked
- Cleaned up after 30 days

---

## Git Workflow

Ask Claude to handle git for you:

```
Review all changes and create a commit with a descriptive message.
```

```
Push to a new branch called "feature-dark-mode" and create a pull request.
```

```
What changed since the last commit?
```

Claude reads git status, writes commit messages, creates branches, and opens PRs via `gh`.

---

## MCP Servers

### Managing Servers

```bash
claude mcp add <name> -- <command> [args...]    # Add a server
claude mcp list                                  # List servers
claude mcp remove <name>                         # Remove a server
```

### Pre-Built Servers

| Server | Install | Use Case |
|--------|---------|----------|
| **Playwright** | `claude mcp add playwright -- npx -y @anthropic-ai/mcp-playwright@latest` | Browser automation, screenshots, UI testing |
| **Fetch** | `claude mcp add fetch -- npx -y @anthropic-ai/mcp-fetch@latest` | Fetch web pages and APIs |
| **GitHub** | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` | Issues, PRs, repo management |
| **Postgres** | `claude mcp add pg -- npx -y @modelcontextprotocol/server-postgres <url>` | Query PostgreSQL databases |

Browse more: github.com/modelcontextprotocol/servers

### Building Your Own MCP Server

Ask Claude:
```
Build an MCP server for my project's API. Expose tools for
querying, creating, and getting stats. Use @modelcontextprotocol/sdk.
Make it a TypeScript file at mcp-server/index.ts.
```

Register it:
```bash
claude mcp add my-project -- npx tsx mcp-server/index.ts
```

### Good Tool Descriptions

```
name: "search_workouts"
description: "Search workouts by date range, exercise name,
or muscle group. Returns exercises with sets, reps, weight.
Use when the user asks about training history."
```

Config stored in `.mcp.json` — commit it to share with your team.

---

## Skills (Custom Slash Commands)

Create `.claude/skills/<name>.md`:

```markdown
---
description: Review code for bugs and issues
user_invocable: true
---

Review recent changes. For each issue found:
1. State the file and line number
2. Describe the issue
3. Suggest a fix

Focus on bugs, security, performance, and style.
```

Invoke with: `/<name>`

### With Arguments

```markdown
---
description: Plan and implement a new feature
user_invocable: true
---

I want to add a new feature: $ARGUMENTS

1. Plan the approach
2. List files that need to change
3. Implement step by step
4. Run tests
```

Usage: `/feature add a dark mode toggle`

### Skill Types

| Type | Frontmatter | Trigger |
|------|------------|---------|
| **User-invocable** | `user_invocable: true` | You type `/name` |
| **Model-invocable** | `model_invocable: true` | Claude uses it automatically when relevant |

### Skill Locations

| Location | Scope |
|----------|-------|
| `.claude/skills/` | This project only |
| `~/.claude/skills/` | All your projects |

---

## Hooks (Automated Scripts)

Shell commands that run automatically — no AI involved.

### Configuration

Add to `.claude/settings.json` or `~/.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "npx prettier --write $CLAUDE_FILE_PATH"
      }
    ]
  }
}
```

### Events

| Event | When it fires | Use case |
|-------|--------------|----------|
| `PreToolUse` | Before a tool runs | Block edits to protected files (exit 2) |
| `PostToolUse` | After a tool completes | Auto-format, notifications |
| `SessionStart` | When a session begins | Setup scripts, environment checks |
| `UserPromptSubmit` | When you send a prompt | Input validation, logging |
| `Notification` | When Claude needs attention | Play a sound, desktop notifications |
| `PreCompact` | Before context compaction | Inject critical context |

### Exit Codes

- **Exit 0** → Allow the action
- **Exit 2** → Block the action (Claude sees the error message)

### Example: Block .env Edits

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "echo $CLAUDE_FILE_PATH | grep -q '.env' && echo 'Blocked: .env files' && exit 2 || exit 0"
      }
    ]
  }
}
```

### Example: Play a Sound When Claude Is Done (macOS)

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "command": "afplay /System/Library/Sounds/Glass.aiff"
      }
    ]
  }
}
```

Other macOS sounds: `Ping.aiff`, `Pop.aiff`, `Purr.aiff`, `Hero.aiff`, `Submarine.aiff` (in `/System/Library/Sounds/`)

---

## Plugins

Bundle skills, hooks, agents, and MCP servers into a shareable, installable unit.

### Plugin Structure

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json          # Name, description, version
├── skills/                   # Skill folders with SKILL.md
├── hooks/
│   └── hooks.json            # Hook configuration
└── .mcp.json                 # MCP server configs
```

### Plugin vs Standalone

| Approach | Skill names | Best for |
|----------|------------|----------|
| **Standalone** (`.claude/`) | `/review` | Personal, single-project |
| **Plugin** | `/my-plugin:review` | Team sharing, multi-project |

### Quick Start

```bash
mkdir -p my-plugin/.claude-plugin
# Create my-plugin/.claude-plugin/plugin.json with name, description, version
# Add skills/, hooks/, .mcp.json as needed
claude --plugin-dir ./my-plugin     # Test locally
```

Skills are namespaced: `/plugin-name:skill-name`. Distribute via plugin marketplaces.

---

## Agent Teams (Experimental)

Coordinate multiple Claude Code sessions working in parallel with shared tasks and messaging.

### Enable

```json
// .claude/settings.json
{ "env": { "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1" } }
```

### Quick Start

```
Create an agent team with 3 reviewers: security, performance, and test coverage.
```

### Controls

| Key / Command | Action |
|---------------|--------|
| **Shift+Up/Down** | Select a teammate to message |
| **Shift+Tab** | Enter delegate mode (lead coordinates only, no coding) |
| **Ctrl+T** | Toggle shared task list |
| **Enter** on teammate | View their session |
| **Esc** on teammate | Interrupt their current turn |

### Display Modes

| Mode | How | Requirement |
|------|-----|-------------|
| **In-process** | All in one terminal | Any terminal |
| **Split panes** | Each teammate in own pane | tmux or iTerm2 |

Set via `"teammateMode": "in-process"` or `"tmux"` in settings, or `--teammate-mode` flag.

### Agent Teams vs Subagents

| | Subagents | Agent Teams |
|---|-----------|-------------|
| Communication | Report back to main only | Message each other directly |
| Best for | Focused tasks, quick workers | Complex parallel work needing collaboration |
| Token cost | Lower | Higher |

---

## Subagents

Claude delegates to specialized subagents automatically. Create custom ones in `.claude/agents/`.

| Built-in | What it does |
|----------|-------------|
| **Explore** | Fast, read-only codebase search (uses Haiku for speed) |
| **Plan** | Research agent for planning mode |
| **Bash** | Isolated command execution |
| **General-purpose** | Complex multi-step tasks with full tool access |

---

## Debugging & Troubleshooting

| Command | What it does |
|---------|------------|
| `/doctor` | Check Claude Code installation health |
| `/debug` | Troubleshoot current session issues |
| `Ctrl+O` | Toggle verbose mode (see all tool calls and reasoning) |
| `claude --debug` | Start with debug logging enabled |

### Common Issues

| Problem | Solution |
|---------|---------|
| Claude not reading CLAUDE.md | Check file is in project root, run `/clear` and retry |
| MCP server not connecting | Run `claude mcp list`, check server command works manually |
| Context too long | Run `/compact` or `/clear` |
| Wrong model | Run `/model` to check and switch |
| Claude going wrong direction | Press **Esc** immediately, then clarify |
| App doesn't start | Paste the error message directly to Claude |

---

## All Slash Commands

| Command | Description |
|---------|------------|
| `/init` | Generate CLAUDE.md for your project |
| `/clear` | Clear conversation history |
| `/compact` | Summarize conversation to save context |
| `/context` | Visualize context window usage |
| `/cost` | Show token usage and cost |
| `/model` | Switch model and effort level |
| `/fast` | Toggle fast mode |
| `/rename` | Name or auto-name current session |
| `/resume` | Resume a previous session |
| `/rewind` | Undo the last turn |
| `/doctor` | Health check |
| `/debug` | Troubleshoot session |
| `/memory` | Edit CLAUDE.md memory files |
| `/permissions` | View or update tool permissions |
| `/hooks` | View configured hooks |
| `/agents` | View and manage subagents |
| `/stats` | Daily usage stats and history |
| `/status` | Version, model, and account info |
| `/export` | Export conversation to file |
| `/theme` | Change color theme |
| `/vim` | Toggle vim editing mode |
| `/terminal-setup` | Configure Shift+Enter for your terminal |
| `/config` | Adjust Claude Code settings (theme, vim, etc.) |
| `/plugin` | Manage installed plugins |
| `/help` | Get usage help |
| `/exit` | Exit Claude Code |

---

## Configuration Files

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Project instructions for Claude |
| `.claude/settings.json` | Project settings (permissions, hooks) — commit to git |
| `.claude/settings.local.json` | Local settings — gitignored |
| `~/.claude/settings.json` | User-wide settings |
| `.claude/skills/` | Project skills |
| `~/.claude/skills/` | Global skills |
| `.claude/agents/` | Project subagents |
| `.mcp.json` | MCP server configuration — commit to git |

---

## Resources

- **Docs:** code.claude.com/docs
- **MCP:** modelcontextprotocol.io
- **MCP Servers:** github.com/modelcontextprotocol/servers
- **Issues:** github.com/anthropics/claude-code
