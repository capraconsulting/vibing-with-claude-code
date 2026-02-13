# Section 5: MCP Servers

**Duration:** 15 minutes (5 min presentation + 10 min hands-on)

---

## Key Concepts

### What is MCP?

**Model Context Protocol (MCP)** is an open standard that lets Claude Code connect to external tools and data sources. Think of MCP servers as plugins — they give Claude new abilities beyond reading files and running commands.

Without MCP, Claude Code can:
- Read/write files in your project
- Run shell commands
- Search your codebase

With MCP servers, Claude Code can also:
- Query your app's API and work with live data
- Interact with external services (GitHub, Slack, databases, etc.)
- Take actions on your behalf through your app

### How MCP Works

```
You <-> Claude Code <-> MCP Server <-> Your App's API
```

An MCP server is a small program that sits between Claude and your API. It exposes **tools** — functions with names, descriptions, and input schemas — that Claude can call. Claude reads the tool descriptions, decides when to use them, and calls them with the right arguments.

You always see what tools Claude is calling and can approve or deny each action.

### Concrete Example: Express API → MCP Server

Say your project has a simple Express API for tracking workouts:

```js
// server.js — your existing Express API
const express = require("express");
const app = express();
app.use(express.json());

let workouts = [];

app.get("/api/workouts", (req, res) => {
  res.json(workouts);
});

app.post("/api/workouts", (req, res) => {
  const workout = { id: Date.now(), ...req.body, date: new Date() };
  workouts.push(workout);
  res.status(201).json(workout);
});

app.get("/api/workouts/stats", (req, res) => {
  res.json({
    total: workouts.length,
    thisWeek: workouts.filter(
      (w) => new Date(w.date) > new Date(Date.now() - 7 * 86400000)
    ).length,
  });
});

app.listen(3001);
```

An MCP server wraps each endpoint as a **tool** that Claude can call. Each tool has a name, a description (so Claude knows *when* to use it), an input schema (the parameters), and a handler that calls your API:

```ts
// mcp-server/index.ts — the MCP server that wraps your API
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

const API = "http://localhost:3001/api";
const server = new McpServer({ name: "workout-tracker", version: "1.0.0" });

// Tool 1: List all workouts
server.tool(
  "list_workouts",
  "List all logged workouts with exercise names, sets, reps, and weight. " +
    "Use when the user asks about their training history.",
  async () => {
    const res = await fetch(`${API}/workouts`);
    return { content: [{ type: "text", text: JSON.stringify(await res.json(), null, 2) }] };
  }
);

// Tool 2: Log a new workout
server.tool(
  "log_workout",
  "Log a new workout entry. Use when the user wants to record an exercise.",
  { exercise: z.string(), sets: z.number(), reps: z.number(), weight: z.number() },
  async ({ exercise, sets, reps, weight }) => {
    const res = await fetch(`${API}/workouts`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ exercise, sets, reps, weight }),
    });
    return { content: [{ type: "text", text: JSON.stringify(await res.json(), null, 2) }] };
  }
);

// Tool 3: Get workout stats
server.tool(
  "get_workout_stats",
  "Get summary statistics: total workouts and count for this week. " +
    "Use when the user asks for an overview or progress check.",
  async () => {
    const res = await fetch(`${API}/workouts/stats`);
    return { content: [{ type: "text", text: JSON.stringify(await res.json(), null, 2) }] };
  }
);

const transport = new StdioServerTransport();
await server.connect(transport);
```

**The key pattern:** Each Express route becomes an MCP tool. The tool description tells Claude *when* to use it. The input schema tells Claude *what parameters* to pass. The handler calls your API and returns the result.

After registering this server (`claude mcp add workout -- npx tsx mcp-server/index.ts`), you can ask Claude things like:

- "How many workouts did I log this week?" → Claude calls `get_workout_stats`
- "Log 3 sets of 10 reps bench press at 80kg" → Claude calls `log_workout`
- "Show me all my workouts" → Claude calls `list_workouts`

Claude chooses the right tool based on your question — you just talk naturally.

### Why Build Your Own?

There are pre-built MCP servers for common services (GitHub, databases, web fetching), but the real power is building one for **your own app**. This turns Claude into an intelligent assistant that understands your data:

- Ask Claude questions about your data in natural language
- Let Claude take actions through your API
- Combine your app's data with Claude's reasoning

---

## Hands-On: Build an MCP Server for Your Project (10 min)

Your app already has an API from the previous sections. Now you'll create an MCP server that lets Claude talk to it.

### Step 1: Ask Claude to Build It

This is the prompt that does the heavy lifting. Adapt it to your project:

```
Build an MCP server for my project's API. The server should expose tools that let you:

- Query and search my data (e.g., list workouts, find by date)
- Create new entries (e.g., log a workout)
- Get summaries and stats (e.g., total workouts this week, personal records)

Use the MCP SDK (@modelcontextprotocol/sdk) and make it a TypeScript file at mcp-server/index.ts.
Each tool should have a clear name and description so you know when to use it.
```

Claude will:
1. Create the MCP server with tools that wrap your API endpoints
2. Add descriptions that help Claude understand when each tool is useful
3. Set up the project structure and dependencies

### Step 2: Register Your MCP Server

Once Claude has built the server, register it with Claude Code:

```bash
claude mcp add my-project -- npx tsx mcp-server/index.ts
```

### Step 3: Try It Out

Start a new Claude Code session and ask questions about your data:

```
What data do I have in my app? Summarize what you can see.
```

```
[Ask a question specific to your project, e.g.:]
What was my most productive week this month?
How many recipes do I have saved?
Show me my reading stats for this year.
```

```
[Ask Claude to take an action, e.g.:]
Log a new workout: 3x10 bench press at 80kg.
Add "Dune" to my reading list with a note that it was recommended by Alex.
```

### What Makes a Good MCP Tool?

The tool descriptions are what Claude reads to decide when to use each tool. Good descriptions make Claude smarter about your data:

**Vague (Claude won't know when to use it):**
```
name: "getData"
description: "Gets data"
```

**Specific (Claude understands exactly when this is useful):**
```
name: "search_workouts"
description: "Search workouts by date range, exercise name, or muscle group. Returns matching workouts with exercises, sets, reps, and weight. Use this when the user asks about their training history or wants to find specific sessions."
```

If Claude isn't using your tools well, improve the descriptions.

---

## Managing MCP Servers

```bash
# List configured servers
claude mcp list

# Remove a server
claude mcp remove <name>

# Check what tools Claude can see
# (inside a Claude Code session:)
What MCP tools do you have available?
```

## How MCP Servers Are Configured

When you run `claude mcp add`, the configuration is saved to `.mcp.json` in your project:

```json
{
  "mcpServers": {
    "my-project": {
      "command": "npx",
      "args": ["tsx", "mcp-server/index.ts"]
    }
  }
}
```

You can commit this file so anyone cloning your project gets the MCP server configured automatically.

## Add a Pre-Built MCP Server: Playwright

Building your own MCP server is powerful, but you can also add pre-built servers that give Claude entirely new capabilities. Let's add **Playwright** — this lets Claude open a browser, navigate to your app, interact with the UI, and take screenshots.

This is incredibly useful for development: Claude can visually verify its own changes, test user flows, and catch UI bugs you'd normally have to check manually.

### Install Playwright MCP

```bash
claude mcp add playwright -- npx -y @anthropic-ai/mcp-playwright@latest
```

### Try It Out

With your app running, ask Claude to interact with it through the browser:

```
Open my app at http://localhost:3000 in the browser and take a screenshot. Does the layout look correct?
```

```
Use the browser to test the full user flow: create a new entry, verify it appears in the list, then delete it.
```

```
Navigate to the app and check if the form validation works — try submitting with empty fields and screenshot the result.
```

This is a great way to combine your custom MCP server (data access) with Playwright (visual verification) — Claude can add data through your API and then verify it shows up correctly in the browser.

### Other Pre-Built Servers

| Server | Install Command | Use Case |
|--------|----------------|----------|
| Playwright | `claude mcp add playwright -- npx -y @anthropic-ai/mcp-playwright@latest` | Browser automation, screenshots, UI testing |
| Fetch | `claude mcp add fetch -- npx -y @anthropic-ai/mcp-fetch@latest` | Fetch web pages and APIs |
| GitHub | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` | GitHub issues, PRs, repo management |
| Postgres | `claude mcp add pg -- npx -y @modelcontextprotocol/server-postgres <url>` | Query PostgreSQL databases |

Browse more at https://github.com/modelcontextprotocol/servers

---

## Check-In

Before we move on, you should have:

- [ ] Built a custom MCP server that wraps your project's API
- [ ] Added Playwright (or another pre-built MCP server)
- [ ] Asked Claude a question that it answered using your MCP tools

**Next up:** [Skills, Hooks & Automation](06-skills-and-commands.md)
