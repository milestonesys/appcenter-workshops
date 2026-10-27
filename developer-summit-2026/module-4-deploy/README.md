# Module 4: Deploy and See It in Smart Client

**Duration:** ~25 minutes

Time to ship! In this module you'll package your MFE as an App Platform app, push it to the shared sandbox, install it through App Center, and open it in XProtect Smart Client.

## 🎯 Learning Objectives

By the end of this module, you will:
- ✅ Have built a container image of your MFE and pushed it to the sandbox registry
- ✅ Have built and pushed your app (Helm chart) to the sandbox repository with App Builder
- ✅ Have installed your app through App Center
- ✅ Have seen your MFE running inside Smart Client
- ✅ Know the loop for shipping a new version

## 📋 Prerequisites

- [Module 3](../module-3-build-with-ai/README.md) completed, with your work committed
- The sandbox registry is allowed in your Docker daemon ([Prerequisites, step 1](../PREREQUISITES.md#allow-the-workshops-sandbox-registry))
- Your credentials handout: **sandbox host**, **App Center URL**, **XProtect login**

## 🗺️ The Flow

```
 Dockerfile ──docker build──▶ image ──docker push──▶ sandbox registry (:5000)
                                                          │
 app-definition-template.yaml ──▶ app-definition.yaml     │ pulls image
            │                                             ▼
            └─app-builder build/push──▶ sandbox repo ──▶ App Center ──install──▶ App Platform
                                                                                     │
                                                    Smart Client ◀── loads MFE ──────┘
```

## 🚀 Step-by-Step Instructions

All commands run in the VS Code terminal **inside the dev container**, from your **project root** (e.g. `~/workspace/dev-summit-2026/jd-health-dashboard`).

### Step 1: Set Some Variables (1 minute)

```bash
export APP=jd-health-dashboard          # your MFE name from Module 2
export VERSION=1.0.0                     # bump this for every new deployment
export SANDBOX=<sandbox-host>:5000       # from your handout
```

> 💡 These only live in the current terminal. Set them again if you open a new one.

### Step 2: Let the Docker Build Use Your npm Token (2 minutes)

The Docker build runs `npm ci`, which needs access to the `@milestone` packages. Pass in your npm token as a **build secret**, so it's available during the build but never ends up in the image. Add a secret mount to every `npm ci` line in the `Dockerfile`:

```bash
sed -i 's#^RUN \(.*npm ci\)#RUN --mount=type=secret,id=npmrc,target=/root/.npmrc \1#' Dockerfile
grep -n "mount=type=secret" Dockerfile   # should show 3 lines
```

> **TODO:** Remove this step if a future `create-mfe-app` version supports a build secret out of the box.

### Step 3: Build and Push the Container Image (6 minutes)

```bash
docker build --secret id=npmrc,src=$HOME/.npmrc \
  -t $SANDBOX/sandbox.io/$APP/$APP:$VERSION .

docker push $SANDBOX/sandbox.io/$APP/$APP:$VERSION
```

The first build takes a few minutes (longer on Apple silicon, where it runs under emulation). Grab a coffee ☕.

### Step 4: Describe Your App (2 minutes)

App Center shows `docs/readme.md` as your app's description. Replace the placeholder text with a few lines about what your dashboard does. Or ask your agent:

> *"Fill in `docs/readme.md` for App Center: what this app does for operators, its features, and its limitations. Keep it short. Also update the `description` in `app-definition-template.yaml` to one sentence."*

### Step 5: Generate `app-definition.yaml` (2 minutes)

`app-definition-template.yaml` contains placeholders for the version, image repository, readme and changelog. Fill them in:

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

Check the result. `version` should be your version, and `image` should be `sandbox.io/<your-app>/<your-app>:<version>`:

```bash
grep -nE "^version|image:" app-definition.yaml
```

> 💡 This does the same as `build-scripts/prepare-app-definition.ps1`, which needs PowerShell. The dev container doesn't have PowerShell.

### Step 6: Build and Push the App with App Builder (3 minutes)

```bash
app-builder validate     # check app-definition.yaml against the schema
app-builder build        # creates <app>-<version>.tgz (a Helm chart)
app-builder push         # pushes it to the sandbox repository
app-builder list         # your app should now be listed
```

> 💡 App Builder uses `app-definition.yaml` in the current folder by default.

### Step 7: Install from App Center (3 minutes)

1. Open **App Center** (URL on your handout) in your browser and log in with the XProtect credentials from your handout.
2. Find your app (`<prefix>-health-dashboard`) among the sandbox apps and click **Install**.
3. Wait until the state changes from *Installing* to *Installed*. Click your app and look at its **logs** and **metrics** to check that it's running.

> **TODO:** Confirm the exact App Center navigation for sandbox apps on the workshop App Platform version.

<details>
<summary>Prefer the terminal?</summary>

```bash
app-builder install-from-repo
app-builder status
```

</details>

### Step 8: Open It in Smart Client (5 minutes)

**Got your own Smart Client 2026 R1 (or newer)?** Make sure you've installed the workshop certificate ([Prerequisites, step 8](../PREREQUISITES.md#8-install-the-workshop-certificate)), then log in to the XProtect server from your handout.

**No Smart Client?** Come to the instructor desk and pick up a **laminated login card** for one of our Smart Client VMs. Connect to it with Remote Desktop using the details on the card. Please **return the card** when you're done, so the next person can use the VM.

Then:

1. Log in to XProtect with the credentials from your handout.
2. Find your MFE. It appears with the title from `mainmenu.title` in `client/public/manifest.json`.
3. 🎉 That's your dashboard, built with AI, running inside Smart Client!

> **TODO:** Describe exactly where MFEs appear in Smart Client 2026 R1 (tab / menu / view item), ideally with a screenshot in `../img/`.

Now try it as an operator would: does the overview tell you within 2 seconds whether there's a blind spot?

### Step 9: Ship a New Version (optional, 3 minutes)

Changed something? The loop is:

```bash
export VERSION=1.0.1                      # 1. bump the version
docker build --secret id=npmrc,src=$HOME/.npmrc -t $SANDBOX/sandbox.io/$APP/$APP:$VERSION .
docker push $SANDBOX/sandbox.io/$APP/$APP:$VERSION
# 2. re-run the awk command from Step 5
app-builder build && app-builder push     # 3. push the new app version
```

Then upgrade the app in App Center and reload Smart Client.

> 💡 **Let your agent automate this.** Ask it: *"Create a `Taskfile.yml` with tasks for build, push and deploy, based on the commands in this module."* The dev container already has [Task](https://taskfile.dev).

### Step 10: Clean Up (1 minute)

The App Platform is shared, so please uninstall your app at the end of the session:

- In App Center: open your app and click **Uninstall**. Or run `app-builder uninstall`.

## 🔍 Verification Checklist

- [ ] `docker push` succeeded for your image
- [ ] `app-builder list` shows your app and version
- [ ] Your app shows *Installed* in App Center and its logs look healthy
- [ ] Your MFE loads in Smart Client
- [ ] (At the end) your app is uninstalled and any loaner login card is returned

## 🛠️ Troubleshooting

**`docker build` fails with `401`/`E401` during `npm ci`**
- Your npm token has expired (12 h) or the secret wasn't passed. Run `npm-auth`, then check that `--secret id=npmrc,src=$HOME/.npmrc` is in your build command and that Step 2 was applied.

**`docker push` fails with `http: server gave HTTP response to HTTPS client`**
- The sandbox registry isn't in your Docker daemon's `insecure-registries`. See [Prerequisites](../PREREQUISITES.md#allow-the-workshops-sandbox-registry). This is set on the **host's** Docker, not inside the container.

**`docker push` fails with `error getting credentials`**
- Run `docker logout $SANDBOX` inside the container and try again. The sandbox registry is anonymous.

**`app-builder validate` complains**
- Read the error. It's usually a placeholder that wasn't replaced (`__...__`) or an invalid name. Paste it into your agent.

**The app is stuck in *Installing*, or the pod keeps restarting**
- Check the logs in App Center, or run `app-builder status` and `app-builder events`.
- `ImagePullBackOff`: the image name in `app-definition.yaml` doesn't match what you pushed. Compare it with the tag from Step 3.

**The app is installed but doesn't show up in Smart Client**
- Check that the certificate is installed on the Smart Client machine (or use a loaner VM).
- Ask your agent to compare names between `app-definition.yaml`, `client/public/manifest.json` and `client/webpack.config.js` (see Module 2, Step 5).
- Restart Smart Client.

**The MFE shows an error or no data in Smart Client, but worked standalone**
- The real API and authentication only exist inside the shell. Check the browser dev tools (if available) or the app logs, and ask your agent for help with the error message.

## 🎓 Wrap-up

In about two hours you have:
- Set up a reproducible App Platform dev environment with an AI agent
- Scaffolded an MFE with `create-mfe-app`
- Turned real user research into a prompt and built a feature with your agent, helped by Milestone's MCP server and skills
- Packaged and deployed it to the App Platform, and run it in Smart Client

**Keep going at home:** your project is yours. Try the [stretch goals](../module-3-build-with-ai/README.md#-stretch-goals), or start something new with `npx @milestone/create-mfe-app`.

## 📚 Additional Resources

- [App Center Sample Applications](https://github.com/milestonesys/appcenter-samples)
- [Developer Forum](https://developer.milestonesys.com)
- [App Builder documentation](https://download.milestonesys.com/app-builder/Documentation/App-Builder.pdf)

---

**Thank you for joining!** 🙏 We'd love your feedback. Please tell an instructor what worked and what didn't.
