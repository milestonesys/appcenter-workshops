# Module 4: Deploy and See It in Smart Client

**Duration:** ~25 minutes

**Goal:** your MFE installed on the shared App Platform and running in Smart Client.

Run everything from your **project root** (e.g. `~/workspace/dev-summit-2026/jd-health-dashboard`).

## Step 1: Set variables

```bash
export APP=jd-health-dashboard           # your app name
export VERSION=1.0.0
```

## Step 2: Create the app definition

Fill in the placeholders in `app-definition-template.yaml`:

```bash
awk -v v="$VERSION" -v repo="sandbox.io/$APP" '
  /__README_BLOCK__|__CHANGELOG_BLOCK__/ {
    f = /__README_BLOCK__/ ? "docs/readme.md" : "docs/changelog.md"
    ind = substr($0, 1, index($0, "__") - 1)
    while ((getline line < f) > 0) print ind line
    close(f); next
  }
  { gsub(/__APP_DEFINITION_VERSION__/, v); gsub(/__DOCKER_VERSION__/, v); gsub(/__REPOSITORY__/, repo); print }
' app-definition-template.yaml > app-definition.yaml
```

> 💡 Optional: ask your agent to describe your app in `docs/readme.md` first. App Center shows that text.

## Step 3: Push the app with App Builder

```bash
app-builder push
app-builder list      # your app should be listed
```

## Step 4: Install the app

```bash
app-builder install-from-repo
```

## Step 5: Check it in App Center

Open **App Center** (URL on your handout) and log in. Find your app among the sandbox apps and wait until it shows *Installed*.

## Step 6: Open it in Smart Client

- **Own Smart Client:** log in to the XProtect server from your handout.
- **No Smart Client:** pick up a **login card** at the instructor desk and connect to one of our Smart Client VMs. Please return the card when you're done.

Find your MFE and try it as an operator would. 🎉

> **TODO:** Where the MFE appears in Smart Client.

## Step 7: Clean up

At the end of the session, uninstall your app in App Center. The platform is shared.

## 🔁 Shipping a new version

Bump `VERSION` (e.g. `export VERSION=1.0.1`), then repeat Steps 2-4. `install-from-repo` needs a new version each time, so always bump before you install again.

> 💡 Tip: ask your agent to turn these steps into a `Taskfile.yml`.

> **TODO:** Provide a ready-made `Taskfile.yml` or `Makefile` that bumps the version, creates the app definition, pushes and installs in one command.

## ✅ Done when

- [ ] Your app is installed in App Center
- [ ] Your MFE runs in Smart Client

## 🎓 Well done!

You've gone from an empty folder to an AI-built MFE running in Smart Client. Your project is yours to keep: try the stretch goals, or start something new with `npx @milestone/create-mfe-app`.

**Thank you for joining!** We'd love your feedback. Please tell an instructor what worked and what didn't.
