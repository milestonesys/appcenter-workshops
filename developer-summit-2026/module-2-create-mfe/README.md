# Module 2: Scaffold Your MFE

**Duration:** ~15 minutes

**Goal:** a generated MFE running locally.

You're already in your workshop folder (`developer-summit-2026`) from Module 1. Run the commands below in the VS Code terminal inside the dev container.

## Step 1: Generate the MFE

```bash
npx @milestone/create-mfe-app@latest
```

Answer the prompts:

| Prompt | Answer |
| --- | --- |
| Name | **Your** app name from Module 1, e.g. `jd-health-dashboard` |
| Realm | `devsummit` |
| Partner or internal | `Partner` |
| Bundler | `Webpack` |
| Port | `3003` |
| API URL | Press **Enter** to keep the default, `http://localhost:8080` |

> 💡 The API URL is only the target that requests to `/api` are forwarded to while you run the MFE locally with `npm start`. It doesn't affect deploying or running in Smart Client, where the MFE talks to the App Platform directly. You can change it later in `client/webpack.config.js` (the `proxy` section) if you want to test against real data locally. See the generated `client/README.md`.

This creates a project folder named after your app, e.g. `jd-health-dashboard/`, with a `client/` (the MFE) and a `server/` (the Express server that hosts it).

## Step 2: Install dependencies and run it locally

Install the client's dependencies and start it. Replace `jd-health-dashboard` with your app name:

```bash
cd jd-health-dashboard/client
npm install
npm start
```

`npm install` also installs the Milestone agent skills into your project (`.github/skills/`), which your AI agent uses in Module 4. The server isn't needed to run the MFE locally. You'll install and build it when you deploy in Module 3.

> 💡 If `npm install` fails with a `401` error, your npm token has expired. Run `npm-auth` again (see Module 1, Step 5).

Open <http://localhost:3003>. Leave the terminal and browser running: it reloads automatically while you work.

> ⚠️ **Keep this terminal running `npm start`.** For the next modules, open a **new terminal** in VS Code. Use **either** of these:
>
> - **From the menu:** choose **Terminal → New Terminal**.
> - **From the terminal panel:** click the **+** button at the top right of the panel.
>
> There's also a keyboard shortcut: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>&#96;</kbd> (<kbd>⌃</kbd>+<kbd>⇧</kbd>+<kbd>&#96;</kbd> on macOS) on a US keyboard. It differs with other keyboard layouts. The menu shows the right one for yours.
>
> Switch between your terminals in the list on the right of the terminal panel. A new terminal starts in the workshop folder.

The file you'll mostly work on is `client/src/App.tsx`.

## ✅ Done when

- [ ] The MFE shows at <http://localhost:3003>

➡️ **[Module 3: Deploy and See It in Smart Client](../module-3-deploy/README.md)**
