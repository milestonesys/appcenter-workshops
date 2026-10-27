# Prerequisites - Developer Summit 2026 Workshop

> 📝 **Draft. Subject to change before the summit.**

Please complete these steps **before you arrive**. They take about 30-45 minutes, most of it spent downloading. The workshop builds on a pre-built **dev container**: a Docker image with the whole App Platform toolchain (Node.js, App Builder, kubectl, Helm, ...) already installed. That way you don't have to install those tools yourself.

## ✅ Checklist

- [ ] [1. A Docker daemon is installed and running, and the sandbox registry is allowed](#1-docker)
- [ ] [2. VS Code with the Dev Containers extension](#2-vs-code-and-dev-containers)
- [ ] [3. OS-specific setup is done (Windows / Apple silicon)](#3-os-specific-setup)
- [ ] [4. The dev container image is pulled](#4-pull-the-dev-container-image)
- [ ] [5. You have the dev container template](#5-get-the-dev-container-template)
- [ ] [6. Your Milestone Developer Portal account works](#6-milestone-developer-portal-account)
- [ ] [7. You have access to an AI coding agent](#7-an-ai-coding-agent)
- [ ] [8. The workshop certificate is installed](#8-install-the-workshop-certificate)
- [ ] [9. (Optional) Smart Client 2026 R1 is installed](#9-optional-xprotect-smart-client)
- [ ] [10. The smoke test passes](#10-smoke-test)

## Hardware & OS

- **macOS** (Intel or Apple silicon), **Linux**, or **Windows with WSL2**. Native Windows (outside WSL) is **not** supported.
- **8 GB+ RAM** available to Docker and **~10 GB** free disk space.
- A laptop you're allowed to install Docker on and run containers on.

## 1. Docker

The dev container uses your machine's Docker daemon. It doesn't run one of its own, because App Builder starts containers of its own. Any of these work:

| Option | Notes |
| --- | --- |
| [Docker Desktop](https://www.docker.com/products/docker-desktop/) | Easiest. Requires a [paid subscription](https://docs.docker.com/subscription/desktop-license/) for organisations with 250+ employees or more than $10M in annual revenue. |
| [Rancher Desktop](https://rancherdesktop.io/) | Free GUI alternative. Choose the **dockerd (moby)** container engine. |
| [Colima](https://github.com/abiosoft/colima) (macOS) | Free, CLI only: `brew install colima docker && colima start --vm-type=vz --vz-rosetta --mount-type=virtiofs --cpu 4 --memory 8` |
| Docker Engine (Linux / WSL2) | Free, no Desktop licence needed. |
| Podman | Works, but is the least tested. Needs a Docker-compatible socket at `/var/run/docker.sock`. |

Verify:

```bash
docker run --rm hello-world
```

### Allow the workshop's sandbox registry

In Module 4 you'll push your app's container image to the App Platform **sandbox registry**, which is served over plain HTTP. Docker refuses that unless you allow it explicitly. Add this to your Docker daemon configuration:

```json
{
  "insecure-registries": ["<workshop-sandbox-host>:5000"]
}
```

- **Docker Desktop / Rancher Desktop:** *Settings → Docker Engine*, add the entry to the JSON and click *Apply & restart*.
- **Docker Engine (Linux / WSL2):** edit `/etc/docker/daemon.json`, then run `sudo systemctl restart docker`.
- **Colima:** `colima start --edit`, then add the host under `docker: insecure-registries:`.

> **TODO:** Fill in the workshop sandbox host name / IP.

## 2. VS Code and Dev Containers

- Install [Visual Studio Code](https://code.visualstudio.com/).
- Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) (`ms-vscode-remote.remote-containers`).

<details>
<summary>Prefer another editor (Cursor, VSCodium, Neovim, a plain terminal)?</summary>

Use the open-source [`devcontainer` CLI](https://github.com/devcontainers/cli). It needs Node.js 18+ on your **host**:

```bash
npm install -g @devcontainers/cli
# from your project folder:
devcontainer up --workspace-folder .
devcontainer exec --workspace-folder . bash
```

</details>

## 3. OS-specific setup

### Windows (WSL2 only)

1. Install [WSL2](https://learn.microsoft.com/windows/wsl/install) with an Ubuntu distro.
2. Make Docker available **inside WSL**: Docker Desktop with *WSL integration* enabled for your distro, or Docker Engine installed directly in the distro.
3. In VS Code, enable **Dev > Containers: Execute In WSL** (`"dev.containers.executeInWSL": true`).
4. Keep your workshop folder **inside the WSL filesystem** (e.g. `~/workspace/...`), **not** under `/mnt/c/...`. Open it from a WSL shell with `code .`.

### Apple silicon Macs (M1-M4)

The image is **linux/amd64 only**, so it runs under emulation. To make that fast:

1. **Docker Desktop → Settings → General**: set *Virtual Machine Manager* to **Apple Virtualization framework**, enable **"Use Rosetta for x86_64/amd64 emulation on Apple Silicon"** and choose **VirtioFS** file sharing.
2. **Settings → Resources**: give Docker **4+ CPUs and 8+ GB memory**.
3. Add this to your shell profile (`~/.zshrc`):
   ```bash
   export DOCKER_DEFAULT_PLATFORM=linux/amd64
   ```

### macOS (all)

Your project folder and `~/.kube` must be inside the folders Docker is allowed to share. Anything under your home folder is fine by default.

## 4. Pull the dev container image

This is the big download (a few GB). **Please do it at home or at the office, not at the venue.**

```bash
docker pull ghcr.io/jakobnielsen/app-platform-dev-container:latest
```

> **TODO:** Update the image reference once the dev container is published under a Milestone registry.

## 5. Get the dev container template

The template is a single `.devcontainer/` folder (`devcontainer.json` + `README.md`). You'll copy it into your project folder in Module 1.

> **TODO:** Add the final download location, e.g. a `.devcontainer/` folder in this repository or a release asset.

## 6. Milestone Developer Portal account

Milestone's npm packages (`@milestone/create-mfe-app`, the MCP server and the agent skills) come from a private registry. Inside the dev container you'll run `npm-auth`, which asks for your **Milestone Developer Portal** e-mail and password and gives you a token that's valid for 12 hours.

- Make sure you have a Developer Portal account and can log in with it.

> **TODO:** Link to where partners sign up for / request a Developer Portal account.

## 7. An AI coding agent

The workshop is agent-agnostic. Bring the one you like. The dev container can add any of these with a single line (you'll do that in Module 1):

| Agent | What you need |
| --- | --- |
| [Claude Code](https://docs.claude.com/en/docs/claude-code) | A Claude subscription or an Anthropic API key |
| [GitHub Copilot CLI](https://github.com/github/copilot-cli) | A GitHub Copilot subscription |
| [OpenAI Codex CLI](https://github.com/openai/codex) | A ChatGPT plan or an OpenAI API key |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | A Google account or a Gemini API key |
| [OpenCode](https://opencode.ai) | **Nothing.** Free models are included, or bring your own provider |

> ⚠️ **About OpenCode's free models:** the line-up changes and most are only free for a limited time. Several may **use your prompts and code for training**, so check the [Zen docs](https://opencode.ai/docs/zen/). They are generally less capable than paid frontier models. Fine for the workshop, but think twice before pointing them at your own code.

> 💡 Logins on your host are **not** forwarded into the container. You'll log in again inside it, so have your credentials or API key handy.

## 8. Install the workshop certificate

Smart Client must trust the TLS certificate of our App Platform before it can load your MFE. Before the summit we'll send you a **public key / certificate file**.

- Install it on the machine where you'll run **Smart Client**.
- Skip this step if you'll borrow one of our Smart Client VMs. They're already set up.

> **TODO:** Attach or link the certificate file and describe the install steps (e.g. Windows: import into *Local Computer → Trusted Root Certification Authorities*).

## 9. (Optional) XProtect Smart Client

If you have **XProtect Smart Client 2026 R1** (or newer) on a Windows machine, you can use it to see your MFE at the end of the workshop.

No Smart Client? No problem. We have Windows VMs with Smart Client installed. Pick up a **login card** at the instructor desk when you get to Module 4.

## 10. Smoke test

Make sure everything works end to end:

1. Create an empty test folder (inside WSL on Windows) and copy the `.devcontainer/` folder into it.
2. Open the folder in VS Code and choose **Reopen in Container** (or run *Dev Containers: Reopen in Container* from the Command Palette).
3. In the VS Code terminal (which is now inside the container), run:
   ```bash
   node -v          # v24.x
   helm version
   app-builder -h   # pulls the App Builder image the first time
   ```

If all three commands print something sensible, you're ready. 🎉

## 📦 What's in the dev container

| Tool | Notes |
| --- | --- |
| **App Builder** | `app-builder`: builds, validates, bundles and pushes App Platform apps |
| **Node.js** | v24 LTS, with `npm` and `npx` |
| **Docker CLI** | Wired to your host's Docker daemon |
| **kubectl** / **Helm** | Kubernetes tooling, with bash completion |
| **Task** | [taskfile.dev](https://taskfile.dev) task runner |
| **k9s**, **lazydocker**, **lazygit** | Handy terminal UIs |
| **npm-auth** | Authenticates `npm` against the `@milestone` registry |
| **git**, **jq**, **yq**, **python3**, **ssh** | The usual suspects |

<details>
<summary>Can't use the dev container? (Native install, unsupported)</summary>

You can install the toolchain directly on Linux/macOS/WSL2: Node.js 24 LTS, Docker, kubectl, Helm 3, and App Builder. Instructors will prioritise helping people on the dev container, so only take this route if you know what you're doing.

</details>

## 🆘 Trouble?

- See the troubleshooting notes in the dev container's own `.devcontainer/README.md`.
- Ask in the workshop channel or contact your Milestone partner representative before the summit.

> **TODO:** Add a contact or channel for pre-summit help.
