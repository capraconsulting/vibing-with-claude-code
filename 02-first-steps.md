# Section 2: First Steps with Claude Code

**Duration:** 20 minutes (5 min presentation + 15 min hands-on)

---

## Key Concepts

### Starting Claude Code

Open your terminal, navigate to your project directory, and launch Claude Code:

```bash
cd ~/my-workshop-project
claude
```

Claude Code runs in your terminal. It can read and write files, run commands, and search your codebase — all through conversation.

### Terminal Setup

Claude Code works best with a properly configured terminal. A few quick tweaks:

**Line breaks:** Type `\` then Enter to create a newline in your prompt. In iTerm2, WezTerm, Ghostty, and Kitty, **Shift+Enter** works natively. For VS Code, Alacritty, or Warp, run `/terminal-setup` inside Claude Code to configure Shift+Enter automatically.

**Vim mode:** If you're a Vim user, type `/vim` to enable Vim keybindings in the input (normal/insert modes, motions, text objects).

**Theme:** Run `/config` to match Claude Code's theme to your terminal.

**Notifications:** In iTerm2, enable alerts for completed tasks under Preferences → Profiles → Terminal → "Silence bell" and Filter Alerts. For custom notifications, you can set up a [notification hook](https://code.claude.com/docs/en/hooks#notification).

### Permission Modes

Claude Code has three permission modes you cycle through with **Shift+Tab**:

| Mode | Indicator | What Claude can do |
|------|-----------|-------------------|
| **Default** | (none) | Asks permission for each edit and command |
| **Accept Edits** | `⏵⏵ accept edits on` | Auto-accepts file edits, still asks for commands |
| **Plan Mode** | `⏸ plan mode on` | Read-only — Claude can only read and think, no changes |

Press **Shift+Tab** to cycle: Default → Accept Edits → Plan Mode → Default.

**Start in Plan Mode** when beginning a new feature — let Claude think through the approach before making changes. Switch to **Accept Edits** when you trust the direction and want to move faster.

### Choosing a Model

Claude Code can use different models. Switch with `/model`:

| Model | Best for |
|-------|----------|
| **Opus** | Complex reasoning, architecture, difficult bugs |
| **Sonnet** | Daily coding, good balance of speed and quality |
| **Haiku** | Quick answers, simple tasks |

On Opus, you can also adjust the **effort level** (low/medium/high) using the arrow keys in the `/model` menu. Higher effort = deeper thinking = better results on hard problems.

### Safety Controls

You're always in control:

- **Esc** — Stop Claude mid-generation if it's going in the wrong direction
- **Reject changes** — When Claude proposes file edits or commands, you can deny them
- `/rewind` — Undo the last turn and try a different approach

### Checkpointing

Claude Code automatically creates a checkpoint before each edit, so you can safely undo changes at any time.

**How it works:**
- Every prompt creates a new checkpoint
- Checkpoints persist across sessions (available when you `/resume`)
- Cleaned up automatically after 30 days

**Using checkpoints:** Press **Esc twice** (`Esc` + `Esc`) or type `/rewind` to open the rewind menu. You'll see a scrollable list of every prompt from your session. Select one, then choose:

| Action | What it does |
|--------|-------------|
| **Restore code and conversation** | Revert both files and conversation to that point |
| **Restore conversation** | Rewind chat but keep current code |
| **Restore code** | Revert files but keep the conversation |
| **Summarize from here** | Compress everything after this point into a summary (frees context) |

**Important:** Checkpointing only tracks file edits made by Claude's tools. Changes from bash commands (`rm`, `mv`, `cp`) are **not** tracked. Checkpoints complement but don't replace git — think of them as "local undo" and git as "permanent history."

### Sessions

Claude Code auto-saves your conversations. This means you can close your terminal, come back later, and pick up where you left off:

```
/rename my-project-v1     # Name this session for easy finding
/resume                   # Browse and resume a previous session
```

From the CLI:
```bash
claude --continue          # Continue the most recent session
claude --resume            # Open the session picker
```

---

## Hands-On: Build Your Project Foundation (15 min)

### Step 1: Start with Plan Mode

Launch Claude Code and toggle to Plan Mode (press Shift+Tab twice). Then describe your project:

```
I want to build [your project name].

It should [describe what it does, the data it manages, and the API you thought about in the brainstorm].

What tech stack would you recommend, and how should we structure the project? I want both a frontend and a backend API.
```

Read Claude's plan. Does it make sense? Is it too ambitious? Too simple?

If you want to adjust the plan, stay in Plan Mode and tell Claude:

```
Let's keep it simpler — just [the core feature]. Skip [anything unnecessary].
```

### Step 2: Generate the Project

Once you're happy with the plan, switch to Accept Edits mode (Shift+Tab) and tell Claude to build it:

```
Let's go ahead and build this. Start with the backend API and the basic frontend.
```

Claude will:
1. Create files and write code
2. Ask permission before running commands (like `npm install`)
3. Show you what it's doing at each step

In Accept Edits mode, Claude will write files without asking but still prompt you for shell commands. This is a good balance of speed and control.

**Read what Claude is doing.** Don't just approve everything blindly — understanding what's happening is part of learning.

### Step 3: Run Your Project

Ask Claude to help you run it:

```
How do I run this? Start it up for me.
```

Claude will suggest or run the appropriate command (`npm start`, `python app.py`, `open index.html`, etc.).

### Step 4: Make Your First Change

Now that it's running, ask for a small change:

```
Change the title to "[your project name]" and use a blue color scheme.
```

This is a small, safe change — perfect for getting a feel for the edit-review cycle.

### Step 5: Name Your Session

Before moving on, name your session so you can easily find it later:

```
/rename workshop-project
```

---

## Things to Try

If you finish early, experiment:

- Ask Claude to explain a file: `Explain what server.ts does`
- Ask Claude to add a small feature: `Add a button that clears the form`
- Try `/rewind` to undo a change and try a different prompt
- Switch to Plan Mode and plan a bigger feature before building it
- Try `/model sonnet` to switch models and notice the speed difference

---

## Common Issues

**Claude is writing too much code at once**
Use Plan Mode first, and break your requests into smaller steps.

**Claude installed the wrong dependencies**
Tell it: `Remove [package] and use [alternative] instead`

**The app doesn't run**
Paste the error message directly to Claude: `I'm getting this error: [paste error]`

**Claude is doing something unexpected**
Press Esc to stop, then clarify what you actually want.

---

## Check-In

Before we move on, you should have:

- [ ] A running project with a backend API and frontend
- [ ] Made at least one change through Claude Code
- [ ] Tried at least two permission modes (Plan Mode and Accept Edits)
- [ ] Named your session with `/rename`

**Next up:** [Effective Prompting](03-effective-prompting.md)
