# Module 1: Set Up Your Workspace

**Duration:** ~20 minutes

**Goal:** a project folder running in the dev container, with your AI agent logged in, access to Milestone's npm packages, and a connection to the shared App Platform.

## Step 1: Open your project folder in VS Code

You've already created the folder under your home directory (on Windows, your WSL home) and copied the dev container template into it.

Open the folder in VS Code:

```bash
cd ~/workspace/dev-summit-2026
code .
```

VS Code offers to **Reopen in Container**. If you want to add an AI agent in Step 2, don't click it yet. The configuration has to be edited before the container is built.

## Step 2 (optional): Add your AI agent

Skip this step if you don't want an AI agent in the container.

Open `.devcontainer/devcontainer.json`. Under `"features"`, uncomment the line for your agent (OpenCode, Copilot CLI, Claude Code, Codex or Gemini). Save the file.

> ⚠️ Don't change anything else in the file. The mount settings are what make App Builder work from inside the container.

## Step 3: Reopen in the dev container

Now choose **Reopen in Container**. If the prompt has gone, open the Command Palette (**Ctrl+Shift+P** / **Cmd+Shift+P**) and run **Dev Containers: Reopen in Container**.

Then open a terminal in VS Code (**Terminal → New Terminal**). From now on, run every command in this terminal, which is inside the container.

## Step 4: Log in to your agent

Skip this step if you didn't add an agent in Step 2.

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

- [ ] Your agent starts and is logged in (if you added one)
- [ ] `npm view @milestone/create-mfe-app version` prints a version
- [ ] `app-builder list` works
- [ ] You have a unique app name

➡️ **[Module 2: Scaffold Your MFE](../module-2-create-mfe/README.md)**
