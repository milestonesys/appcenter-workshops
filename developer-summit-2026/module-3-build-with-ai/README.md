# Module 3: Build with Your AI Agent

**Duration:** ~55 minutes

**Goal:** implement a real user story with your AI agent.

## The user stories

These stories come from Milestone's Operator value stream and are based on real user research.

![User stories and operator vs. administrator insights](../img/user-stories.png)

As an **operator**, I want to...

1. ...get a **system health overview** at a glance: is the system running properly? ⭐ *Start here*
2. ...see whether any **cameras, devices or servers are offline**, at a glance.
3. ...see the **name and other relevant details** of anything offline, so I can pass it on to the administrator.
4. *Stretch 👑:* ...be **notified** when something suddenly goes offline.

**Remember who you're building for.** Operators want to know *"Can we still monitor effectively?"*. They need fast, high-level, actionable information, not deep diagnostics. That's the administrator's job.

## Step 1: Write your prompt

Pick a story and fill in this template:

```markdown
I'm building an MFE for the Milestone XProtect App Platform, generated with create-mfe-app.
The entry component is client/src/App.tsx. It will run inside XProtect Smart Client.

User: <who they are and what they care about>
User story: As an operator, I want to <story>, so that <why>.
Acceptance criteria:
- <what the user sees / can do>
- <what happens when everything is fine, when something is wrong, and when data can't load>

Constraints:
- Use the Milestone MCP server to find the right APIs. Don't guess.
- Follow the Milestone look & feel: MUI components and Milestone design tokens.
- Read-only. Keep data access in a separate hook, with mock data for local development.

Ask me clarifying questions and show me a short plan before you write code.
```

<details>
<summary>Example: Story 1</summary>

```markdown
I'm building an MFE for the Milestone XProtect App Platform, generated with create-mfe-app.
The entry component is client/src/App.tsx. It will run inside XProtect Smart Client.

User: an XProtect operator in a control room. Not technical. Needs fast, high-level,
actionable information, and acts by notifying the administrator.
User story: As an operator, I want a system health overview at a glance,
so that I immediately know whether I have any blind spots.
Acceptance criteria:
- One clear overall status at the top: "All systems operational" or "Attention needed".
- One card each for Cameras, Devices and Servers, showing "<online> / <total> online".
- Refreshes every 30 seconds and shows when it was last updated.
- Shows a loading state, and a friendly error with a Retry button.

Constraints:
- Use the Milestone MCP server to find the right APIs. Don't guess.
- Follow the Milestone look & feel: MUI components and Milestone design tokens.
- Read-only. Keep data access in a useSystemHealth() hook, with mock data for local development.

Ask me clarifying questions and show me a short plan before you write code.
```

</details>

> **TODO:** Add a hint about which API to use for camera, device and server status.

## Step 2: Plan, then build in small steps

1. Paste your prompt. Answer the agent's questions and review its plan before it writes code.
2. Let it build **one step at a time**. After each step, check the result in the browser.
3. Give specific feedback. *"Make the overall status bigger and use the success colour"* works better than *"make it nicer"*.
4. When something works, commit it (`git init` once, then `git commit -am "..."`), so you can always roll back.

> ⏱️ After about 40 minutes, make sure you have something working to deploy. Polish can wait.

## Stretch goals

- **Story 4:** notify the operator when something goes offline.
- **Live updates:** let the Express server in `server/` push status changes to the MFE over a WebSocket.

## ✅ Done when

- [ ] At least Story 1 works in the browser
- [ ] It looks like Milestone (MUI and design tokens, no hard-coded colours)
- [ ] Your work is committed

➡️ **[Module 4: Deploy and See It in Smart Client](../module-4-deploy/README.md)**
