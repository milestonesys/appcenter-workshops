# Module 2: Scaffold Your MFE

**Duration:** ~15 minutes

**Goal:** a generated MFE running locally, with your AI agent connected to Milestone's MCP server and agent skills.

## Step 1: Generate the MFE

```bash
cd ~/workspace/dev-summit-2026
npx @milestone/create-mfe-app@latest
```

Answer the prompts:

| Prompt | Answer |
| --- | --- |
| Name | Your app name, e.g. `jd-health-dashboard` |
| Realm | `devsummit` |
| Partner or internal | `Partner` |
| Bundler | `Webpack` |
| Port | `3003` |
| API URL | The App Platform URL from your handout |

> **TODO:** Confirm the realm and API URL for the workshop.

## Step 2: Run it locally

```bash
cd jd-health-dashboard/client
npm start
```

Open <http://localhost:3003>. Leave it running: it reloads automatically while you work.

The file you'll mostly work on is `client/src/App.tsx`.

## Step 3: Start your agent in the project

Open a **new terminal**, go to the project root (`jd-health-dashboard/`) and start your agent there.

The project already includes:
- **MCP server config** (`.mcp.json`, `.vscode/mcp.json`) for the Milestone App Platform knowledge MCP server
- **Milestone agent skills** (`.github/skills/`), for example the Milestone look & feel

Claude Code and Copilot in VS Code pick these up automatically. Approve the MCP servers when asked. For other agents, add this MCP server manually: `npx -y @milestone/app-platform-knowledge-mcp-server`.

> **TODO:** Short per-agent notes for MCP servers and skills.

Check that it works by asking your agent:

> *"Which Milestone MCP tools and skills do you have? Then explain briefly how this MFE gets loaded by the shell."*

## ✅ Done when

- [ ] The MFE shows at <http://localhost:3003>
- [ ] Your agent can see the Milestone MCP server

➡️ **[Module 3: Build with Your AI Agent](../module-3-build-with-ai/README.md)**
