# Section 6: Skills, Hooks, Plugins & Agent Teams

**Duration:** 15 minutes (5 min presentation + 10 min hands-on)

---

## Key Concepts

### Skills: Reusable Slash Commands

Skills are reusable prompt templates that you invoke with slash commands. They let you package common workflows into repeatable actions.

For example, instead of typing out a detailed prompt every time you want to review code, you create a skill once and invoke it with `/review`.

#### How Skills Work

A skill is a Markdown file (e.g., `review.md`) placed in a `.claude/skills/` directory. When you type `/review` in Claude Code, it loads the contents of that file as a prompt.

#### Creating a Skill

1. Create the skills directory:

```bash
mkdir -p .claude/skills
```

2. Create a skill file, e.g., `.claude/skills/review.md`:

```markdown
---
description: Review code for bugs, style issues, and improvements
user_invocable: true
---

Review the recent changes in this project. For each issue found:

1. State the file and line number
2. Describe the issue
3. Suggest a fix

Focus on:
- Bugs and logic errors
- Security issues
- Performance concerns
- Code style inconsistencies

Be concise. Only flag things that actually matter.
```

3. Use it:

```
/review
```

#### Skill Types

| Type | Frontmatter | How it's triggered |
|------|------------|-------------------|
| **User-invocable** | `user_invocable: true` | You type `/skillname` |
| **Model-invocable** | `model_invocable: true` | Claude automatically uses it when relevant |

Most skills are user-invocable. Model-invocable skills are useful for things like "always run linting after editing files."

#### Skill Locations

| Location | Scope |
|----------|-------|
| `.claude/skills/` | This project only |
| `~/.claude/skills/` | All your projects |

#### Arguments

Use `$ARGUMENTS` to capture everything typed after the command:

`.claude/skills/feature.md`:
```markdown
---
description: Plan and implement a new feature
user_invocable: true
---

I want to add a new feature: $ARGUMENTS

Please:
1. First, use Plan Mode to outline the approach
2. List which files need to change
3. Implement the changes step by step
4. Run tests after implementation
5. Summarize what was done
```

Usage:
```
/feature add a dark mode toggle
```

---

### Hooks: Automation Without AI

Hooks are shell commands that run automatically at specific points in Claude Code's lifecycle. Unlike skills (which are prompts for Claude), hooks are deterministic scripts — no AI involved.

#### Why Hooks?

- **Auto-format code** after every file edit
- **Block edits** to protected files (`.env`, `package-lock.json`)
- **Send notifications** when Claude needs your input
- **Inject context** after conversation compaction

#### Hook Events

| Event | When it fires |
|-------|--------------|
| `PreToolUse` | Before a tool executes (can block it) |
| `PostToolUse` | After a tool completes |
| `SessionStart` | When a session begins |
| `UserPromptSubmit` | When you send a prompt |
| `Notification` | When Claude needs your attention |
| `PreCompact` | Before context compaction |

#### How Hooks Work

Configure hooks in your settings (`.claude/settings.json` or `~/.claude/settings.json`):

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

This example runs Prettier automatically every time Claude edits a file.

#### Exit Codes

- **Exit 0** — Allow the action to proceed
- **Exit 2** — Block the action (Claude sees the error message)

#### Quick Example: Block Edits to .env

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "echo $CLAUDE_FILE_PATH | grep -q '.env' && echo 'Blocked: do not edit .env files' && exit 2 || exit 0"
      }
    ]
  }
}
```

#### Quick Example: Play a Sound When Claude Needs You

The `Notification` event fires when Claude finishes a task or needs your input. On macOS, you can use `afplay` to play a system sound so you don't have to keep watching the terminal:

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

This is especially useful when you kick off a long task and switch to another window — you'll hear the bell when Claude is done. Some other built-in macOS sounds you can use: `Ping.aiff`, `Pop.aiff`, `Purr.aiff`, `Hero.aiff`, or `Submarine.aiff` (all in `/System/Library/Sounds/`).

---

### Plugins: Package and Share Extensions

Once you've built useful skills and hooks, you can bundle them into a **plugin** for reuse across projects or sharing with your team.

A plugin is a directory with a manifest (`.claude-plugin/plugin.json`) and your skills, hooks, agents, and MCP configs:

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json        # Name, description, version
├── skills/                 # Skill folders with SKILL.md files
│   └── review/
│       └── SKILL.md
├── hooks/
│   └── hooks.json          # Hook configuration
└── .mcp.json               # MCP server configs (optional)
```

#### When to Use Plugins vs Standalone

| Approach | Skill names | Best for |
|----------|------------|----------|
| **Standalone** (`.claude/`) | `/review` | Personal workflows, single-project customization |
| **Plugin** (`.claude-plugin/`) | `/my-plugin:review` | Sharing with team, reuse across projects, distributing to others |

Plugin skills are **namespaced** (e.g., `/my-plugin:review`) to prevent conflicts when multiple plugins are installed.

#### Creating a Plugin

1. Create the directory structure:
```bash
mkdir -p my-plugin/.claude-plugin
```

2. Create `my-plugin/.claude-plugin/plugin.json`:
```json
{
  "name": "my-plugin",
  "description": "Our team's shared workflows",
  "version": "1.0.0"
}
```

3. Add skills, hooks, or MCP configs to the plugin directory (same format as standalone, just in the plugin folder instead of `.claude/`).

4. Test locally:
```bash
claude --plugin-dir ./my-plugin
```

#### Converting Existing Config to a Plugin

If you already have skills and hooks in `.claude/`, you can migrate them:

```bash
mkdir -p my-plugin/.claude-plugin
cp -r .claude/skills my-plugin/
# Move hooks from .claude/settings.json to my-plugin/hooks/hooks.json
```

Plugins can also be distributed through **plugin marketplaces** — see the [plugins docs](https://code.claude.com/docs/en/plugins) for details.

---

### Agent Teams: Coordinate Multiple Claude Instances

> **Experimental feature** — disabled by default.

Agent teams let you spawn multiple Claude Code sessions that work together in parallel. One session acts as the **team lead**, coordinating work and assigning tasks. **Teammates** work independently, each with their own context window, and can communicate directly with each other.

#### When to Use Agent Teams

| Use case | Why it helps |
|----------|-------------|
| **Research and review** | Multiple reviewers investigate different aspects simultaneously |
| **New modules or features** | Each teammate owns a separate piece without conflicts |
| **Debugging** | Teammates test competing hypotheses in parallel |
| **Cross-layer work** | Frontend, backend, and tests each owned by a different teammate |

#### Agent Teams vs Subagents

| | Subagents | Agent Teams |
|---|-----------|-------------|
| **Communication** | Report back to main agent only | Teammates message each other directly |
| **Coordination** | Main agent manages all work | Shared task list with self-coordination |
| **Best for** | Focused tasks where only the result matters | Complex work needing discussion and collaboration |
| **Token cost** | Lower | Higher (each teammate is a separate Claude instance) |

Use **subagents** for quick, focused workers. Use **agent teams** when teammates need to share findings and coordinate.

#### Getting Started

1. Enable in settings (`.claude/settings.json`):
```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

2. Ask Claude to create a team:
```
Create an agent team to review PR #142. Spawn three reviewers:
- One focused on security implications
- One checking performance impact
- One validating test coverage
Have them each review and report findings.
```

Claude creates the team, spawns teammates, assigns tasks, and synthesizes results.

#### Controlling Teams

- **Shift+Up/Down** — Select a teammate to message directly
- **Shift+Tab** to delegate mode — Restricts the lead to coordination only (no coding)
- **Ctrl+T** — Toggle the shared task list
- Teams support two display modes: **in-process** (all in one terminal) or **split panes** (each teammate in its own tmux/iTerm2 pane)

For full details, see the [agent teams docs](https://code.claude.com/docs/en/agent-teams).

---

## Hands-On Exercises (5 min)

Pick **at least two** exercises — one skill and one hook. If you finish early, try more.

### Exercise 1: Create a Test Runner Skill

Create a skill that runs your project's tests and diagnoses failures:

```
Create a skill called "test" in .claude/skills/test.md that runs the project's tests and if any fail, reads the failing test, diagnoses the issue, and suggests a fix.
```

Try it:

```
/test
```

### Exercise 2: Create a Skill with Arguments

Create a skill that accepts input so you can reuse it for different tasks:

```
Create a skill called "explain" in .claude/skills/explain.md that takes $ARGUMENTS as a file path or function name, finds it in the codebase, and explains what it does in plain English. Include the complexity, inputs/outputs, and any edge cases.
```

Try it:

```
/explain server.js
/explain handleSubmit
```

### Exercise 3: Set Up Auto-Formatting with a Hook

Add a hook that automatically formats files after Claude edits them:

```
Add a PostToolUse hook to .claude/settings.json that runs Prettier on any file I edit. Only trigger it for Write and Edit tools.
```

After it's set up, ask Claude to edit a file and watch Prettier run automatically.

### Exercise 4: Block Edits to Sensitive Files

Add a safety guard that prevents Claude from touching files it shouldn't:

```
Add a PreToolUse hook to .claude/settings.json that blocks Write and Edit operations on any file matching .env, package-lock.json, or anything in the node_modules directory. It should print a clear error message explaining why the edit was blocked.
```

Test it by asking Claude to edit `.env`:

```
Add a new variable MY_VAR=hello to the .env file.
```

You should see the hook block the edit.

### Exercise 5: Notification Sound on Completion

Set up an audio notification so you know when Claude finishes a long task (macOS only):

```
Add a Notification hook to .claude/settings.json that plays a sound when Claude needs my attention. Use afplay with a macOS system sound.
```

Test it by giving Claude a task and switching to another window — you should hear the sound when it's done.

### Exercise 6: Create a Model-Invocable Skill

Create a skill that Claude uses automatically without you having to invoke it:

```
Create a model-invocable skill in .claude/skills/lint-reminder.md that reminds Claude to run the linter after making changes to any TypeScript or JavaScript file. Set model_invocable to true in the frontmatter.
```

Then ask Claude to make a code change and see if it automatically lints afterward.

### Bonus: Combine Skills and Hooks

If you finished the exercises above, try combining what you've learned:

```
Create a skill called "ship" in .claude/skills/ship.md that:
1. Runs the test suite
2. Runs the linter
3. If everything passes, creates a git commit with a descriptive message
4. Summarizes what was shipped

Also add a PreToolUse hook that blocks any git push command — we only want to commit locally during the workshop.
```

---

## Check-In

Before we move on, you should have:

- [ ] Created at least one custom skill and tested it
- [ ] Set up at least one hook and verified it works
- [ ] Understood the difference between skills (AI prompts) and hooks (deterministic scripts)

**Next up:** [Wrap-up & Resources](07-wrap-up.md)
