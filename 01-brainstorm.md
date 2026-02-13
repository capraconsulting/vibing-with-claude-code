# Section 1: Brainstorm Your Hobby Project

**Duration:** 15 minutes
**Format:** Brief intro (3 min) + individual brainstorm (12 min)

---

## Why a Hobby Project?

The best way to learn a tool is to build something you actually care about. For the rest of this workshop, you'll be using Claude Code to build your own project — so let's figure out what that is.

Your project should be:

- **Something you personally want** — motivation matters
- **Small enough for 60 minutes** — a simple app
- **Concrete** — "a thing that does X" not "an app for productivity"

---

## Brainstorm Questions

Feel free to use the following questions as inspiration. You don't need to answer all of them — just find the one that sparks an idea.

### What do you enjoy?

1. What hobbies do you spend time on outside of work?
1. What do you do to relax on weekends?
1. Is there a sport, game, or activity you track stats for?
1. Do you collect anything?

### What annoys you?

1. What repetitive task do you wish was automated?
1. Is there a spreadsheet you maintain that could be a simple app?
1. Is there a calculation you do regularly by hand?

### What would help your daily life?

1. How do you plan meals or groceries?
1. Is there something you always forget and wish you had a reminder for?
1. Do you organize events, trips, or meetups?

### What would be fun to build?

1.  What would you show off to friends at dinner?
1.  Is there a fun API you've always wanted to experiment with?

### What have you been meaning to do?

1. Is there a side project idea you've had in your notes for months?
1. What's a tutorial you started but never finished?
1. Is there a technology you've been wanting to try?

---

## Example Project Ideas

Need more inspiration? Here are some concrete examples. You'd be surprised how much you can build in 60 minutes with Claude Code — don't be afraid to be ambitious.

| Idea | What it does | Tech |
|------|-------------|------|
| **Recipe scaler** | Paste a recipe URL or text, adjust serving count, convert units (metric/imperial), save favorites to a collection | React |
| **Workout tracker** | Log exercises with sets/reps/weight, visualize progress with charts over time, track personal records | React + Chart.js |
| **Board game night app** | Track scores across multiple game sessions, player rankings, win/loss stats, game history timeline | React |
| **Reading list** | Search books via Open Library API, track reading status, add ratings and notes, filter by genre/rating | React + API |
| **Expense splitter** | Add group expenses, split by percentage or amount, track balances, show "who pays whom" to settle up | React |
| **D&D character sheet** | Interactive character sheet with stat modifiers, spell slots, inventory management, dice roller | React |
| **Weather dashboard** | Current weather + 5-day forecast for saved locations, clothing suggestions, rain/UV alerts | React + Weather API |
| **Pomodoro timer** | Customizable work/break intervals, task list, session history with daily/weekly productivity stats | React |
| **Flashcard study app** | Create decks, spaced repetition algorithm, track mastery per card, import/export decks as JSON | React |
| **Personal finance dashboard** | Categorize transactions (paste CSV), monthly spending charts, budget vs actual, savings goals | React + Chart.js |
| **Meal planner** | Plan weekly meals, auto-generate grocery list, scale recipes by household size, save favorite weeks | React |
| **Habit tracker** | Daily habit check-ins, streak tracking, heatmap calendar view, weekly summary stats | React |

---

## Think Through Your Idea

Once you've picked a project, spend a few minutes thinking about how it could grow. The goal isn't just a quick demo — you want something with enough structure that you can keep building on it after the workshop.

### Think about the data

What are the core "things" in your app? For a workout tracker, that might be **exercises**, **workouts**, and **personal records**. For a meal planner: **recipes**, **meal plans**, and **grocery items**.

Ask yourself:

- What data does the app store?
- How do those things relate to each other? (e.g., a workout contains multiple exercises)
- What would a user want to create, read, update, and delete?

### Think about the API

Your project should have a backend API, not just a frontend. This makes it a real app you can extend — add a mobile client, integrate with other services, or share with friends.

Think about what endpoints you'd need:

- `GET /workouts` — list all workouts
- `POST /workouts` — log a new workout
- `GET /workouts/:id` — view a specific workout
- `PUT /workouts/:id` — update a workout
- `DELETE /workouts/:id` — delete a workout

You don't need to design the full API now — just think about what resources exist and what actions users take on them. Claude Code will help you build it.

### Think about what an LLM could do with your data

Later in the workshop, you'll build an MCP server that lets Claude talk directly to your app's API. This means Claude will be able to query your data and take actions on your behalf — so think about what questions and actions would be useful.

For a workout tracker, Claude could:
- "What was my heaviest deadlift this month?"
- "How many times did I train legs this week?"
- "Log a workout: 3x10 bench press at 80kg"
- "Compare my squat progress over the last 3 months"

For a meal planner, Claude could:
- "What's for dinner on Thursday?"
- "Add a grocery list for this week's meals"
- "Suggest a meal using the leftover chicken from yesterday"

For a reading list, Claude could:
- "What books have I rated 5 stars?"
- "Add 'Dune' to my to-read list"
- "How many books did I finish this year?"

The key question: **what would be genuinely useful to ask an AI that has access to your app's data?** If the answer is interesting, you've got a good project idea.

### Think about what comes next

A good project idea has room to grow. After the workshop, you might want to:

- Add user authentication
- Deploy it somewhere
- Build a mobile app that talks to the same API
- Add real-time updates
- Integrate with external services

If your idea feels like it would be "done" after one session, make it bigger. A "pet feeding log" becomes more interesting as a "pet care app" with feeding schedules, vet appointment tracking, and medication reminders.

---

## Check-In

Before we move on, make sure you have:

- [ ] A project idea you're excited about
- [ ] A rough sense of the data and resources your app would manage
- [ ] Your project directory ready (`cd ~/my-workshop-project`)

**Next up:** [First Steps with Claude Code](02-first-steps.md)
