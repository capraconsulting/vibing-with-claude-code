# Section 4: CLAUDE.md & Memory

**Duration:** 10 minutes (5 min presentation + 5 min hands-on)

---

## Key Concepts

### What is CLAUDE.md?

`CLAUDE.md` is a special file that Claude Code reads at the start of every conversation. It's your project's instruction manual for Claude — coding standards, project context, and preferences that apply to every session.

Think of it like a `.editorconfig` or `.prettierrc`, but for Claude.

### Bootstrap with /init

The fastest way to create a `CLAUDE.md` is to let Claude analyze your project:

```
/init
```

Claude will scan your project structure, detect frameworks, and generate a `CLAUDE.md` with sensible defaults.

### What to Put in CLAUDE.md

**Good things to include:**

```markdown
# Project Overview
Recipe scaler web app built with React and TypeScript.

# Tech Stack
- React 18 with TypeScript
- Vite for bundling
- Tailwind CSS for styling

# Commands
- `npm run dev` — start dev server
- `npm test` — run tests
- `npm run lint` — lint code

# Code Style
- Use functional components with hooks
- Prefer named exports
- Use Tailwind classes, not CSS files

# Important Context
- All recipe data is stored in localStorage (no backend)
- Serving sizes must be positive integers
```

**Avoid putting in CLAUDE.md:**
- Temporary instructions (use conversation instead)
- Entire file contents (Claude can read files itself)
- Generic programming advice (Claude already knows this)

### CLAUDE.md Hierarchy

Claude Code reads `CLAUDE.md` files at multiple levels:

| Location | Scope | Use for |
|----------|-------|---------|
| `~/.claude/CLAUDE.md` | All projects | Personal preferences (e.g., "always use TypeScript") |
| `./CLAUDE.md` | This project | Project-specific standards and context |
| `./src/CLAUDE.md` | Subdirectory | Module-specific instructions |

Files closer to the code take precedence.

### Auto Memory

Claude Code can also remember things across sessions automatically. When you tell Claude something important during a conversation (like "always use single quotes in this project"), it may save that to its memory for future sessions.

You can also explicitly ask:

```
Remember that this project uses pnpm, not npm.
```

---

## Hands-On: Set Up Your CLAUDE.md (5 min)

### Step 1: Generate CLAUDE.md

In your Claude Code session, run:

```
/init
```

Review the generated file. Does it accurately describe your project?

### Step 2: Customize It

Edit the generated `CLAUDE.md` to add anything project-specific:

```
Add to the CLAUDE.md that we're using [your specific choices, e.g., "Tailwind for styling" or "no external dependencies"].
```

Or open `CLAUDE.md` in your editor and make manual edits.

### Step 3: Test It

Start a new conversation (`/clear`) and ask Claude something about your project:

```
What tech stack is this project using?
```

Claude should know the answer from your `CLAUDE.md` without you having to explain it again.

---

## Check-In

Before we move on, you should have:

- [ ] A `CLAUDE.md` file in your project root
- [ ] Customized it with at least one project-specific instruction
- [ ] Verified Claude reads it by starting a fresh conversation

**Next up:** [MCP Servers](05-mcp-servers.md)
