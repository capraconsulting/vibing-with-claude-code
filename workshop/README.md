# Claude Code Hands-On Workshop

**Duration:** 1.5 hours (90 minutes)
**Format:** Presentation + hands-on exercises
**Audience:** Developers who want to learn Claude Code

## What You'll Build

Each participant brainstorms and begins building their own personal hobby project using Claude Code. By the end of this workshop, you'll have a working prototype and know how to use Claude Code effectively for your future projects.

## Prerequisites

Complete the [pre-work guide](00-prerequisites.md). This covers installation, authentication, and verifying that your setup works.

## Schedule

| Time | Section | Format | Duration |
|------|---------|--------|----------|
| 0:00 | [Welcome & Brainstorm](01-brainstorm.md) | Presentation + individual brainstorm | 15 min |
| 0:15 | [First Steps with Claude Code](02-first-steps.md) | Presentation (5 min) + Hands-on (15 min) | 20 min |
| 0:35 | [Effective Prompting](03-effective-prompting.md) | Presentation (5 min) + Hands-on (10 min) | 15 min |
| 0:50 | [CLAUDE.md & Memory](04-claude-md.md) | Presentation (5 min) + Hands-on (5 min) | 10 min |
| 1:00 | [MCP Servers](05-mcp-servers.md) | Presentation (5 min) + Hands-on (10 min) | 15 min |
| 1:15 | [Skills, Plugins & Agent Teams](06-skills-and-commands.md) | Presentation (5 min) + Hands-on (5 min) | 10 min |
| 1:25 | [Wrap-up & Commit](07-wrap-up.md) | Hands-on + presentation | 5 min |

## Quick Reference

Keep the [cheatsheet](cheatsheet.md) open during the workshop for quick lookups.

## Presentation Slides

Slides are in the `slides/` directory, one file per section. They use [Marp](https://marp.app/) format (markdown with `---` slide separators). Speaker notes are in `<!-- -->` comments.

| Section | Slides |
|---------|--------|
| Welcome & Brainstorm | [slides/01-brainstorm.md](slides/01-brainstorm.md) |
| First Steps | [slides/02-first-steps.md](slides/02-first-steps.md) |
| Effective Prompting | [slides/03-effective-prompting.md](slides/03-effective-prompting.md) |
| CLAUDE.md & Memory | [slides/04-claude-md.md](slides/04-claude-md.md) |
| MCP Servers | [slides/05-mcp-servers.md](slides/05-mcp-servers.md) |
| Skills, Plugins & Agent Teams | [slides/06-skills-and-commands.md](slides/06-skills-and-commands.md) |
| Wrap-up & Commit | [slides/07-wrap-up.md](slides/07-wrap-up.md) |

To present, install Marp and run:

```bash
# Install
npm install -g @marp-team/marp-cli

# Present with speaker notes (press P for presenter view)
marp slides/01-brainstorm.md -s

# Export all slides to PDF
marp slides/ -o slides/output/ --pdf

# Or use the Marp for VS Code extension for live preview
```

---

## Facilitator Notes

### Before the Workshop

- Send [00-prerequisites.md](00-prerequisites.md) to participants in advance
- Test the slides with `marp -s` to verify presenter view and notes work
- Have 2-3 example hobby project ideas ready to demonstrate

### During the Workshop

- **Keep presentations short** — the schedule allocates 60%+ of time to hands-on work
- Walk the room during hands-on sections; common issues:
  - Participants writing prompts that are too vague
  - Forgetting to use Plan Mode for bigger changes
  - Not reading what Claude Code is doing before approving
- Encourage participants to share their screens if they get stuck

### Pacing Tips

- The brainstorm section is critical — participants who don't have a clear project idea will struggle later
- Section 02 (First Steps) is the longest hands-on block; most participants need the full 15 minutes
- Sections 04-06 can be compressed if you're running behind; the core learning happens in 01-03

### After the Workshop

- Share the [cheatsheet](cheatsheet.md) as a takeaway
- Point participants to the resources in [07-wrap-up.md](07-wrap-up.md)
