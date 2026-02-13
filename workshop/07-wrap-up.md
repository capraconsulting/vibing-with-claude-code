# Section 7: Wrap-Up & Commit Your Work

**Duration:** 5 minutes

---

## Capstone: Commit and Push

You've built a project — let's save it properly. Ask Claude to commit your work:

```
Review all the changes we've made, then create a git commit with a descriptive message.
```

If you want to push to GitHub and create a PR:

```
Push this to a new branch called "workshop-project" and create a pull request.
```

Claude will:
1. Stage the relevant files
2. Write a commit message based on what was actually changed
3. Push and create the PR if you asked for it

This is a natural workflow you'll use daily — Claude handles the git ceremony so you can focus on the code.

---

## What You Learned Today

In 90 minutes, you've gone from zero to productive with Claude Code:

1. **Brainstormed** a personal project idea with API and data model thinking
2. **Built a working prototype** with a backend API and frontend
3. **Configured your terminal** for the best Claude Code experience
4. **Mastered permission modes** — Plan, Accept Edits, and Default
5. **Learned checkpointing** — safely undo changes at any point
6. **Learned effective prompting** — specific, step-by-step, with images and context
7. **Set up CLAUDE.md** to give Claude persistent project knowledge
8. **Built a custom MCP server** and added Playwright for browser automation
9. **Created custom skills and hooks** for reusable workflows and automation
10. **Learned about plugins and agent teams** — package extensions and coordinate parallel work
11. **Committed your work** using Claude's git integration

## Top Tips

1. **Start with Plan Mode** for anything non-trivial. Think first, code second.
2. **Use Accept Edits mode** once you trust the direction — it's much faster.
3. **Be specific in your prompts.** Tell Claude what, where, and how.
4. **Break big tasks into small steps.** 3-step prompts work better than 30-step ones.
5. **Read what Claude does before approving.** You'll learn from watching it work.
6. **Use `/compact` regularly** to keep your context clean in long sessions.
7. **Keep CLAUDE.md updated** as your project evolves.
8. **Press Esc** if Claude is going in the wrong direction. Don't wait for it to finish.
9. **Paste screenshots** instead of describing visual issues — Claude can see images.
10. **Use `!` for quick commands** like `!npm test` or `!git status`.

## Common Pitfalls

| Pitfall | Instead |
|---------|---------|
| Approving changes without reading them | Take 5 seconds to scan each change |
| Writing vague prompts and hoping for the best | Be specific about what you want |
| Trying to build everything in one prompt | Break it into steps |
| Never using Plan Mode | Use it at the start of every feature |
| Staying in Default mode for everything | Switch to Accept Edits when you trust the direction |
| Ignoring errors and asking Claude to "just fix it" | Paste the actual error message |
| Letting the context get too long | Use `/compact` or `/clear` regularly |
| Describing a UI bug in words | Paste a screenshot instead — Claude can see it |
| Never checking costs | Run `/cost` periodically to build intuition |

## What to Explore Next

| Feature | What it does | How to start |
|---------|-------------|-------------|
| **Subagents** | Delegate tasks to specialized AI workers | `.claude/agents/` or ask Claude to "use the explore agent" |
| **Headless mode** | Run Claude non-interactively for CI/scripts | `claude -p "your prompt"` |
| **GitHub Actions** | Claude responds to PR comments | `@claude` in a PR comment |
| **Claude Code on the web** | Run sessions in the browser, no local setup | [claude.ai/code](https://claude.ai/code) |
| **Slack integration** | Route bug reports from Slack to PRs | `@Claude` in a Slack message |
| **Plugin marketplaces** | Discover and install community plugins | `/plugin install` |
| **Custom permissions** | Per-project tool rules | `.claude/settings.json` |
| **Extended thinking** | Deeper reasoning for hard problems | `Option+T` to toggle, `/model` for effort level |
| **Verbose mode** | See Claude's internal reasoning | `Ctrl+O` to toggle |
| **Debugging** | Troubleshoot Claude Code issues | `/doctor` for health check, `/debug` for session issues |

## Resources

### Official Documentation

- **Claude Code docs:** https://code.claude.com/docs
- **MCP protocol:** https://modelcontextprotocol.io
- **MCP servers directory:** https://github.com/modelcontextprotocol/servers

### Community

- **GitHub:** https://github.com/anthropics/claude-code — Report issues and feature requests
- **Discord:** Anthropic's Discord server has a Claude Code channel

---

## What to Do Next

1. **Keep building your workshop project** — you have a solid foundation and a git history now
2. **Set up Claude Code in your real projects** — add `CLAUDE.md` files to your work repos
3. **Build MCP servers for your work tools** — the same pattern you used today works for any API
4. **Share what you learned** — show a colleague how Claude Code works

---

Thank you for attending. Happy building!
