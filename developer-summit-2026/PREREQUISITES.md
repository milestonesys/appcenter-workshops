# Prerequisites

> 📝 **Draft. Subject to change before the summit.**

Please do this **before you arrive**. It takes 30-45 minutes, mostly downloading. All tools (Node.js, App Builder, kubectl, Helm, ...) come pre-installed in a **dev container**, so you only need Docker and VS Code on your machine.

You need macOS, Linux, or Windows with **WSL2** (native Windows is not supported), and 8 GB+ RAM available to Docker.

## 1. Install Docker

Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) or an alternative such as [Rancher Desktop](https://rancherdesktop.io/), [Colima](https://github.com/abiosoft/colima) or Docker Engine.

> Docker Desktop requires a [paid subscription](https://docs.docker.com/subscription/desktop-license/) for larger organisations.

Then allow the workshop's sandbox registry. In Docker Desktop, go to **Settings → Docker Engine**, add this and click *Apply & restart*:

```json
"insecure-registries": ["<workshop-sandbox-host>:5000"]
```

> **TODO:** Fill in the sandbox host.

## 2. Install VS Code

Install [VS Code](https://code.visualstudio.com/) and the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers).

- **Windows:** use WSL2 with Docker available inside WSL, keep your projects in the WSL filesystem, and enable the VS Code setting *Dev > Containers: Execute In WSL*.
- **Apple silicon Mac:** in Docker Desktop, enable *Use Rosetta for x86_64/amd64 emulation* and give Docker 4+ CPUs and 8+ GB memory.

## 3. Download the dev container

Pull the image (a few GB, so please don't do this at the venue):

```bash
docker pull ghcr.io/jakobnielsen/app-platform-dev-container:latest
```

Download the `.devcontainer/` template folder from: **TODO: link**.

## 4. Accounts

- **Milestone Developer Portal account.** Needed to install Milestone's npm packages. **TODO:** link to sign-up.
- **An AI coding agent.** Claude Code, GitHub Copilot CLI, OpenAI Codex, Gemini CLI, or [OpenCode](https://opencode.ai). OpenCode works without an account, using free models, but these are less capable and may use your prompts for training.

## 5. Smart Client (optional)

If you have **XProtect Smart Client 2026 R1** or newer, install the **workshop certificate** we've sent you on that machine. Otherwise, borrow one of our Smart Client VMs at the workshop.

> **TODO:** Attach the certificate and describe how to install it.

## 6. Smoke test

1. Create an empty folder and copy `.devcontainer/` into it.
2. Open the folder in VS Code and choose **Reopen in Container**.
3. In the VS Code terminal, run:
   ```bash
   node -v && helm version && app-builder -h
   ```

If all three print something sensible, you're ready. 🎉
