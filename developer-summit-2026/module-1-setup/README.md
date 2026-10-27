# Module 1: Set Up Your Workspace

**Duration:** ~20 minutes

**Goal:** a project folder running in the dev container, with your AI agent logged in, access to Milestone's npm packages, and a connection to the shared App Platform.

## Step 1: Create your project folder

Create the folder under your home directory (on Windows, your WSL home), and copy the dev container template into it:

```bash
mkdir -p ~/workspace/dev-summit-2026 && cd ~/workspace/dev-summit-2026
cp -r /path/to/.devcontainer .     # TODO: final template location
```

## Step 2: Add your AI agent

Open `.devcontainer/devcontainer.json`. Under `"features"`, uncomment the line for your agent (OpenCode, Copilot CLI, Claude Code, Codex or Gemini).

> ⚠️ Don't change anything else in the file. The mount settings are what make App Builder work from inside the container.

## Step 3: Open the dev container

```bash
code .
```

Choose **Reopen in Container**, then open a terminal in VS Code (**Terminal → New Terminal**). From now on, run every command in this terminal, which is inside the container.

## Step 4: Log in to your agent

Start your agent (e.g. `claude`, `copilot`, `opencode`, `codex` or `gemini`) and follow its login prompt. Logins from your host don't carry over into the container. Then exit the agent again.

## Step 5: Authenticate npm

```bash
npm-auth
npm view @milestone/create-mfe-app version
```

Log in with your **Developer Portal** account. The second command should print a version number. The token lasts 12 hours. If you get `401` errors later, run `npm-auth` again.

## Step 6: Connect to the App Platform

Log in with the App Platform details from your handout:

```bash
app-builder login
app-builder list
```

`app-builder list` shows the apps in the shared sandbox, possibly including other participants' apps.

## Step 7: Pick your app name

Your app name is your **initials** plus `-health-dashboard`, using lowercase letters, digits and hyphens only. For example: `jd-health-dashboard`. Check `app-builder list` to make sure nobody else has taken it.

## ✅ Done when

- [ ] Your agent starts and is logged in
- [ ] `npm view @milestone/create-mfe-app version` prints a version
- [ ] `app-builder list` works
- [ ] You have a unique app name

➡️ **[Module 2: Scaffold Your MFE](../module-2-create-mfe/README.md)**
