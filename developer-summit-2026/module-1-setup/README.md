# Module 1: Set Up Your Workspace

**Duration:** ~20 minutes

In this module you'll create your project folder, start the dev container with your AI agent, get access to Milestone's npm packages, and connect to the shared App Platform.

> 💡 You won't need to install the Milestone **MCP server** or **agent skills** by hand. `create-mfe-app` adds both to your project in [Module 2](../module-2-create-mfe/README.md).

## 🎯 Learning Objectives

By the end of this module, you will:
- ✅ Have a project folder running in the App Platform dev container
- ✅ Have your AI agent installed and logged in inside the container
- ✅ Be authenticated against the `@milestone` npm registry
- ✅ Be connected to the shared App Platform sandbox
- ✅ Have picked a unique app prefix

## 📋 Prerequisites

- Everything in [PREREQUISITES.md](../PREREQUISITES.md) is done, including the smoke test
- Your **credentials handout** from the instructors

## 🚀 Step-by-Step Instructions

### Step 1: Create Your Project Folder (2 minutes)

Create a folder **under your home directory**. On Windows, that means your **WSL** home directory.

```bash
mkdir -p ~/workspace/dev-summit-2026
cd ~/workspace/dev-summit-2026
```

Copy the `.devcontainer/` folder from the template (see [Prerequisites step 5](../PREREQUISITES.md#5-get-the-dev-container-template)) into it:

```bash
cp -r /path/to/template/.devcontainer .
```

> **TODO:** Replace with the final template location, e.g. `cp -r ~/workspace/appcenter-workshops/developer-summit-2026/.devcontainer .`

> 💡 **Why under `$HOME`?** The dev container mounts your project at the **same path** inside and outside the container, because App Builder hands paths to your host's Docker daemon. The path must exist on the host and be shared with Docker.

### Step 2: Choose Your AI Agent (3 minutes)

Open `.devcontainer/devcontainer.json` and find the `"features"` section. **Uncomment the line for your agent**:

```jsonc
"features": {
    // OpenCode (free models available, no login needed)
    // "ghcr.io/devcontainers-extra/features/opencode:1": { "version": "latest" },

    // GitHub Copilot CLI
    // "ghcr.io/devcontainers/features/copilot-cli:1": { "version": "latest" },

    // Claude Code (CLI + VS Code extension)
    // "ghcr.io/anthropics/devcontainer-features/claude-code:1": {},

    // OpenAI Codex CLI -- OR -- Gemini CLI (only ONE of these two)
    // "ghcr.io/devcontainers-extra/features/npm-package:1": { "package": "@openai/codex", "version": "latest" }
    // "ghcr.io/devcontainers-extra/features/npm-package:1": { "package": "@google/gemini-cli", "version": "latest" }
}
```

> ⚠️ Mind the JSON commas: every entry except the last one needs a trailing comma.  
> ⚠️ **Don't change** `workspaceMount`, `workspaceFolder`, `mounts`, `containerEnv` or `postCreateCommand`. They're what make App Builder work from inside the container.

### Step 3: Open the Folder in the Dev Container (3 minutes)

1. Open the folder in VS Code:
   ```bash
   code .
   ```
2. When VS Code offers to **Reopen in Container**, click it. Or open the Command Palette (`Ctrl/Cmd+Shift+P`) and run **Dev Containers: Rebuild and Reopen in Container**.
3. Wait for the container to build. It's quick if you pulled the image beforehand, and the agent feature is added on top.
4. Open a terminal in VS Code (**Terminal → New Terminal**). The prompt should start with `vscode ➜`. You're now **inside** the container.

Verify:

```bash
node -v
app-builder -h
```

> 💡 From here on, **all commands run in the VS Code terminal inside the container**, unless stated otherwise.

### Step 4: Log In to Your AI Agent (3 minutes)

Logins from your host don't carry over into the container. Start your agent once and follow its login prompt:

| Agent | How to log in |
| --- | --- |
| Claude Code | Run `claude` and pick a Claude account or API key, or `export ANTHROPIC_API_KEY=...` |
| Copilot CLI | Run `copilot`, then type `/login`, or `export COPILOT_GITHUB_TOKEN=...` |
| Codex | `codex login`, or `printenv OPENAI_API_KEY \| codex login --with-api-key` |
| Gemini CLI | Run `gemini` and choose *Sign in with Google*, or `export GEMINI_API_KEY=...` |
| OpenCode | Nothing to do. It uses a free model by default (`opencode models opencode` lists them). `opencode auth login` to use your own provider. |

Exit the agent again once you're logged in. We'll come back to it in Module 3.

> ⚠️ Your agent runs with the **same access as you** inside the container, including your host's Docker daemon and your kubeconfig. Review what you let it run unattended.

> 💡 A login made inside the container lasts until the container is **rebuilt**.

### Step 5: Authenticate npm Against the `@milestone` Registry (2 minutes)

```bash
npm-auth
```

Enter your **Milestone Developer Portal** e-mail and password when prompted. The password isn't echoed. `npm-auth` stores a token in your user npm config (`~/.npmrc`). It never prints, logs or accepts your credentials as arguments.

> 💡 The token is valid for **12 hours**. If npm starts returning `401` later on, just run `npm-auth` again.

Verify that you can reach the Milestone packages:

```bash
npm view @milestone/create-mfe-app version
```

You should see a version number (e.g. `4.x.x`). If you get a `401` or `404`, see [Troubleshooting](#-troubleshooting).

### Step 6: Connect to the Shared App Platform (3 minutes)

You'll build your app locally and push it to **our shared App Platform sandbox**. Nothing runs on your laptop's cluster.

1. Log in to the App Platform with App Builder, using the **App Platform address, user name and password** from your credentials handout:
   ```bash
   app-builder login
   ```
   App Builder stores the connection in `~/.kube`, which is the same folder as `~/.kube` on your host. You only need to do this once.

   > **TODO:** Confirm the exact `app-builder login` prompts against the workshop App Platform, and what the handout needs to contain.

2. Verify that you can see the sandbox repository:
   ```bash
   app-builder list
   ```
   You should see a list of apps, possibly including other participants' apps. 👋

### Step 7: Pick Your App Prefix (1 minute)

Everyone shares the same App Platform, so **app names must be unique**. Use your initials (plus a digit if needed) as a prefix, all **lowercase letters, digits and hyphens only**:

```
my prefix:  ______        e.g.  jd
my app:     ______-health-dashboard      e.g.  jd-health-dashboard
```

> 💡 Run `app-builder list` and check that nobody else has taken your name.

Write it down. You'll need it in Module 2 and Module 4.

## 🔍 Verification Checklist

Before moving on, make sure that:

- [ ] VS Code shows **Dev Container: App Platform Dev Container** in the bottom-left corner
- [ ] Your agent starts and is logged in
- [ ] `npm view @milestone/create-mfe-app version` prints a version
- [ ] `app-builder list` lists apps from the shared sandbox
- [ ] You have a unique app prefix

## 🛠️ Troubleshooting

**The "Reopen in Container" prompt doesn't appear**
- Make sure the Dev Containers extension is installed and that you opened the folder that *contains* `.devcontainer/`.
- Run **Dev Containers: Reopen in Container** from the Command Palette.

**Container build fails with `bind source path does not exist`**
- Your project or `~/.kube` isn't shared with Docker. Move the project under `$HOME` (or into WSL on Windows).

**Container build fails after editing `devcontainer.json`**
- Usually a JSON syntax error, most often a missing or extra comma in `"features"`.

**Very slow on an Apple silicon Mac**
- Turn on Rosetta + VirtioFS in Docker Desktop (see [Prerequisites](../PREREQUISITES.md#apple-silicon-macs-m1-m4)).

**`npm view` gives `401 Unauthorized` / `404 Not Found`**
- Run `npm-auth` again and double-check your Developer Portal credentials. Tokens expire after 12 hours.
- Check `npm config get @milestone:registry`. It should point at Milestone's registry.

**`app-builder list` fails or hangs**
- Check that you're on the workshop Wi-Fi and that you used the App Platform address from your handout.
- Run `app-builder logout`, then `app-builder login` again.

**My files "disappear" when App Builder runs**
- You've probably changed the mounts. Restore the original `devcontainer.json` and rebuild.

## ➡️ Next Steps

Continue to **[Module 2: Scaffold Your MFE](../module-2-create-mfe/README.md)**.

---

**Need help?** Raise your hand. An instructor will come by.
