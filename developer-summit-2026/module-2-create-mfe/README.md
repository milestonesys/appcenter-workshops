# Module 2: Scaffold Your MFE

**Duration:** ~15 minutes

In this module you'll generate a new MFE project with `@milestone/create-mfe-app`, connect your AI agent to the Milestone MCP server and agent skills, and run the MFE locally.

## 🎯 Learning Objectives

By the end of this module, you will:
- ✅ Have generated a full-stack MFE project (React 19 client + Express server)
- ✅ Know your way around the generated files
- ✅ Have your AI agent connected to the Milestone MCP server and agent skills
- ✅ Have the MFE running locally in your browser

## 📋 Prerequisites

- [Module 1](../module-1-setup/README.md) completed
- Your **app prefix** from Module 1, Step 7 (e.g. `jd`)

## 🚀 Step-by-Step Instructions

### Step 1: Generate the MFE (5 minutes)

In the VS Code terminal (inside the dev container), from your project folder:

```bash
cd ~/workspace/dev-summit-2026
npx @milestone/create-mfe-app@latest
```

Answer the prompts like this:

| Prompt | Answer | Why |
| --- | --- | --- |
| *What is the name of your MFE?* | `<prefix>-health-dashboard`, e.g. `jd-health-dashboard` | Must be **unique** on the shared App Platform |
| *What is the realm of your MFE?* | `devsummit` | Groups all workshop apps together |
| *Are you a partner or internal developer?* | `Partner` | Configures the partner npm registry and MCP server |
| *Which bundler would you like to use?* | `Webpack` | Default. Vite works too |
| *Which port should the dev server run on?* | `3003` | Default |
| *What is the API URL of runtime platform?* | `https://<app-platform-host>` from your handout | The local dev server proxies `/api` calls here |

The generator creates the project and runs `npm install` for both the client and the server. This takes a minute or two.

> **TODO:** Confirm the realm to use on the workshop App Platform, and whether the API URL should be the App Platform or the XProtect API Gateway.

> 💡 Prefer one line? `npx @milestone/create-mfe-app@latest jd-health-dashboard -u partner -b webpack -r devsummit` prompts only for what you leave out.

### Step 2: Take a Tour of the Project (3 minutes)

Open the new folder in the VS Code explorer. The most important files:

```
jd-health-dashboard/
├── client/                         # The MFE itself (React 19 + TypeScript + MUI 7)
│   ├── src/App.tsx                 # ⭐ Main component. This is where you'll build
│   ├── src/index.tsx               # Standalone bootstrap with mocked shell props
│   ├── public/manifest.json        # MFE metadata and fragments (menu entry, route)
│   └── webpack.config.js           # Module Federation config (exposes ./App)
├── server/                         # Express server: serves the built MFE + /api proxy
│   └── server.json                 # Port and proxy routes
├── app-definition-template.yaml    # App Platform app definition (used in Module 4)
├── Dockerfile                      # Builds the container image (used in Module 4)
├── docs/readme.md                  # Shown in App Center. Describe your app here
├── build-scripts/                  # Helper scripts for packaging
├── .github/skills/                 # Milestone agent skills (installed by npm install)
├── .mcp.json                       # MCP servers for Claude Code (and others)
├── .vscode/mcp.json                # MCP servers for Copilot in VS Code
└── .cursor/mcp.json                # MCP servers for Cursor
```

Concepts you know from the talk, and where they live:
- **Module Federation:** `client/webpack.config.js` exposes `./App`. The shell (e.g. Smart Client) loads `remote-entry.js` at runtime.
- **Shell props:** `App` receives `TMfeProps` from `@milestone/one-shell-mfe-contract` (`navigate`, `domElement`, ...). In standalone mode, `index.tsx` mocks them.
- **Shared dependencies:** React, MUI and the `@milestone/*` theme/token packages come from the shell. Don't bundle your own versions.

### Step 3: Run It Locally (3 minutes)

```bash
cd jd-health-dashboard/client
npm start
```

VS Code forwards port `3003` automatically. Open <http://localhost:3003>. You should see the starter MFE with the Milestone theme.

Leave it running in its own terminal. It hot-reloads while you and your agent work in Module 3.

### Step 4: Connect Your Agent to the Milestone MCP Server (3 minutes)

The generator set up two things that turn a generic AI agent into one that knows how to build a *Milestone* MFE:

- **Agent skills** in `.github/skills/`, installed from `@milestone/agent_skills` when `npm install` ran in `client/`. They teach the agent Milestone conventions, such as the look & feel (MUI components, `@milestone/design-tokens`, `@milestone/ui-assets`).
- **MCP server config** in `.mcp.json`, `.vscode/mcp.json` and `.cursor/mcp.json`. The MCP servers give the agent the docs.

> **TODO:** `.github/skills/` is where Copilot looks. Document how agents that look elsewhere pick up the skills (e.g. Claude Code's `.claude/skills/`, via `npx milestone-skills install --location ...`).

The MCP config contains:
- `experienceDevelopmentDocs`: the **Milestone App Platform knowledge MCP server** (`@milestone/app-platform-knowledge-mcp-server`)
- `muiMcp`: the official MUI docs MCP server

**Start your agent from the project root** (`jd-health-dashboard/`), so it picks up the project config:

| Agent | What to do |
| --- | --- |
| Claude Code | Run `claude` in the project root. It reads `.mcp.json` and asks you to approve the servers. Say yes. |
| Copilot (VS Code) | `.vscode/mcp.json` is picked up. Start the servers from the MCP view or `MCP: List Servers`. |
| Copilot CLI / OpenCode / Codex / Gemini | Add the same server to your agent's MCP config. Command: `npx -y @milestone/app-platform-knowledge-mcp-server` |

> **TODO:** Add exact per-agent snippets for Copilot CLI, OpenCode, Codex and Gemini, and confirm whether Copilot allows the MCP server by summit time.

> 💡 The MCP server is an `@milestone` npm package, so it relies on the `npm-auth` token from Module 1.

Verify by asking your agent:

> *"Which Milestone MCP tools and agent skills do you have available? List them briefly, then use them to explain in 3 bullets how this MFE gets loaded by the shell."*

It should mention the Milestone MCP server, the Milestone skills (e.g. `milestone-mfe-look-and-feel`), and give an answer that mentions Module Federation and `remote-entry.js`.

### Step 5: Quick Sanity Check of Names (1 minute)

The MFE's name shows up in several files and they have to agree. Ask your agent, or check by hand:

> *"Check that the MFE name, namespace and module name are consistent between `app-definition-template.yaml`, `client/public/manifest.json` and `client/webpack.config.js`. Report any mismatch, but don't change anything yet."*

> 💡 Module Federation turns kebab-case names into snake_case (`jd-health-dashboard` → `jd_health_dashboard`). That's expected. A mismatch in a *module name* is not.

## 🔍 Verification Checklist

- [ ] The project `<prefix>-health-dashboard/` was generated without errors
- [ ] `npm start` in `client/` serves the MFE at <http://localhost:3003>
- [ ] Your agent was started from the project root and lists the Milestone MCP server
- [ ] You know where `App.tsx`, `manifest.json` and `app-definition-template.yaml` are

## 🛠️ Troubleshooting

**`npx` fails with `404 Not Found - @milestone/create-mfe-app`**
- Your npm token is missing or expired. Run `npm-auth` again (see Module 1, Step 5).

**`npm install` fails during generation**
- Same as above. Then `cd` into `client/` and `server/` and run `npm install` manually.

**Port 3003 is already in use**
- Change the port in `client/webpack.config.js` (or `vite.config.ts`) **and** in `server/server.json`.

**The page at localhost:3003 doesn't load**
- Check the **Ports** tab in VS Code and make sure 3003 is forwarded.

**The agent doesn't see the MCP server**
- Make sure you started the agent **in the project root**, where `.mcp.json` is.
- Run `npx -y @milestone/app-platform-knowledge-mcp-server` by hand to see if it starts.

## ➡️ Next Steps

Continue to **[Module 3: Build the Feature with Your AI Agent](../module-3-build-with-ai/README.md)**.

---

**Need help?** Raise your hand. An instructor will come by.
