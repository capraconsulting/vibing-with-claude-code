# Section 3: Effective Prompting

**Duration:** 10 minutes (5 min presentation + 5 min hands-on)

---

## Key Concepts

### 1. Be Specific

The biggest difference between a good and bad prompt is specificity.

**Vague (less useful):**
```
Make the UI better
```

**Specific (much better):**
```
Add input validation to the recipe form:
- Serving count must be a positive number
- Show an inline error message in red below the input if invalid
- Disable the submit button until all fields are valid
```

**Why it matters:** Claude Code can do a lot, but it can't read your mind. Specific prompts get you exactly what you want on the first try instead of going back and forth.

### 2. Break Big Tasks Into Steps

Don't ask for everything at once. Build incrementally.

**Too much at once:**
```
Build a complete workout tracker with user auth, exercise logging, progress charts, and social sharing.
```

**Step by step:**
```
Step 1: Add a form to log an exercise (name, sets, reps, weight)
Step 2: Save logged exercises to localStorage
Step 3: Display a list of today's exercises below the form
```

You can give Claude a numbered list of steps in a single prompt — it will work through them in order.

### 3. Reference Files and Code

When you want Claude to work with specific parts of your codebase, be explicit:

```
In src/components/Header.tsx, change the navigation links to use React Router instead of anchor tags.
```

You can also reference files by dragging them into the terminal or using tab completion for file paths.

### 4. Use Images and Screenshots

Claude Code is multimodal — it can see and analyze images. This is incredibly useful for:

- **Pasting error screenshots** instead of transcribing them
- **Sharing design mockups** and asking Claude to implement them
- **Showing UI bugs** visually

How to share images:

| Method | How |
|--------|-----|
| **Paste** | Copy an image, then Ctrl+V in Claude Code |
| **Drag and drop** | Drag an image file into the terminal |
| **File path** | Reference it directly: `Look at ./mockup.png and implement this layout` |

Try it:

```
Here's a screenshot of the current UI. Move the navigation to the left side and make it vertical.
```

### 5. Run Quick Shell Commands With !

Need to quickly run a command without Claude involved? Prefix it with `!`:

```
!npm run dev
!git status
!ls src/
```

The output gets added to your conversation context, so Claude can see the result too. This is faster than asking Claude to run it for you when you know exactly what command you need.

### 6. Manage Your Context

Claude Code remembers your entire conversation. But long conversations can get noisy. Use these tools:

| Command | What it does |
|---------|-------------|
| `/clear` | Clears the conversation history and starts fresh |
| `/compact` | Summarizes the conversation to free up context space |
| `/context` | Shows a visual grid of how your context window is being used |
| `/cost` | Shows token usage and cost for the current session |

**When to use `/clear`:** When you're switching to a completely different task.

**When to use `/compact`:** When the conversation is getting long but you want to keep the overall context.

**Check `/cost` periodically** to understand how much context you're consuming. This helps you develop intuition for when to compact or clear.

### 7. Parallel Work with Git Worktrees

When you're working with Claude Code, each session runs in one directory. If you want to work on multiple features at the same time — say, one Claude session builds a search feature while another fixes bugs — they'll conflict if they're editing the same files in the same directory.

**Git worktrees** solve this. A worktree is a separate checkout of the same repo in a different directory, on its own branch. Each worktree shares the same `.git` history but has its own working files.

#### How It Works

```bash
# From your main project directory, create a worktree for a new feature
git worktree add ../my-project-search feature/search

# Now you have two directories:
# ~/my-project/              ← main branch (your current work)
# ~/my-project-search/       ← feature/search branch (new worktree)
```

Open a separate terminal in each directory, run `claude` in both, and you have two independent Claude Code sessions working in parallel on different branches — no conflicts.

#### Useful Commands

| Command | What it does |
|---------|-------------|
| `git worktree add ../dir branch-name` | Create a new worktree on a branch |
| `git worktree list` | Show all active worktrees |
| `git worktree remove ../dir` | Remove a worktree when done |

#### When to Use Worktrees

- **Parallel features** — Two Claude sessions building different features simultaneously
- **Bug fix while building** — Keep your feature branch untouched, fix the bug in a separate worktree
- **Code review** — Check out a PR branch in a worktree without switching your main branch
- **Experimentation** — Try a risky approach in a worktree, delete it if it doesn't work out

#### Workflow Example

```bash
# Terminal 1: Continue your main feature
cd ~/my-project
claude
> "Keep working on the dashboard layout"

# Terminal 2: Fix a bug in parallel
cd ~/my-project-bugfix
claude
> "Fix the login validation bug on this branch"
```

When both are done, merge both branches through normal git workflows (PRs, merge, rebase).

### 8. Let Claude Verify Its Own Work

Instead of checking everything yourself, ask Claude to verify:

```
Run the tests to make sure nothing is broken.
```

```
Check that the form validation works — try submitting with empty fields.
```

```
Take a screenshot and describe what the page looks like now.
```

Claude can run commands, read output, and course-correct on its own.

---

## Hands-On: Improve Your Project (5 min)

### Exercise 1: Write a Specific Prompt

Add a feature to your Daily Dev Log. Write a prompt that includes **what** to change, **where**, and **how** it should behave. For example:

```
In the entry form, add input validation:
- The "did" and "doing" fields must have at least one item before the form can be submitted
- Show an inline error message in red if the user tries to submit with empty fields
- Disable the submit button until the form is valid
```

### Exercise 2: Multi-Step Prompt

Write a single prompt with numbered steps:

```
Please make these changes:
1. Add a "Delete" button next to each entry in the list
2. Clicking delete should remove the entry and update the display immediately
3. Add a brief confirmation before deleting ("Are you sure?")
```

Then run `/compact` to free up context and notice how Claude still retains the key session context.

---

## Prompting Patterns Quick Reference

| Pattern | Example |
|---------|---------|
| **Describe the outcome** | "After this change, the user should see a success message after submitting" |
| **Give examples** | "Format dates like 'Jan 15, 2025' not '2025-01-15'" |
| **Specify constraints** | "Don't add any new dependencies" |
| **Ask for tests** | "Add tests for the new validation logic" |
| **Provide error context** | "I'm getting 'TypeError: Cannot read property x of undefined' when I click submit" |
| **Quick shell command** | `!npm test` to run a command and share output with Claude |

---

## Check-In

Before we move on, you should have:

- [ ] Written at least one specific, detailed prompt
- [ ] Tried a multi-step prompt
- [ ] Used `/compact` at least once

**Next up:** [CLAUDE.md & Memory](04-claude-md.md)
