# Section 1: Your Project — Daily Dev Log

**Duration:** 5 minutes
**Format:** Presentation

---

## What You'll Build Today

Everyone builds the same project — a **Daily Dev Log**: a personal app to track your daily work updates, standup-style. Log what you built, what you're planning, and any blockers. By the end of the workshop, you'll have a running app with a backend REST API, a React frontend, and an MCP server that lets Claude query and add entries through natural language.

This isn't just a demo app — it's something you can actually use.

---

## The Data Model

The app stores **log entries**. Each entry represents one day:

```
Entry {
  id
  date
  did      — array of strings ("what I worked on")
  doing    — array of strings ("what I'm planning")
  blockers — array of strings (optional)
  notes    — free-form text (optional)
}
```

Simple. One resource, easy to extend.

---

## The API

Here are the endpoints you'll build:

| Endpoint | What it does |
|----------|-------------|
| `GET /api/entries` | List all entries (newest first) |
| `POST /api/entries` | Add a new entry |
| `GET /api/entries/today` | Get today's entry (if it exists) |
| `GET /api/entries/:id` | Get a specific entry |
| `DELETE /api/entries/:id` | Delete an entry |
| `GET /api/entries/stats` | Stats: streak, total entries, most common blockers |

---

## What the MCP Section Will Do

Later in the workshop, you'll build an MCP server that wraps this API. Once it's registered with Claude Code, you'll be able to:

```
What did I work on last week?
Log today's standup: built the login form, planning to add tests, blocker: waiting for API keys
How many days in a row have I logged?
Do I have any recurring blockers?
```

Claude will call your API, reason over the data, and answer in natural language. This is the MCP section — you build the plumbing, Claude does the querying.

---

## Check-In

Before we move on:

- [ ] You understand what the app does
- [ ] Your project directory is ready: `mkdir ~/daily-dev-log && cd ~/daily-dev-log`

**Next up:** [First Steps with Claude Code](02-first-steps.md)
