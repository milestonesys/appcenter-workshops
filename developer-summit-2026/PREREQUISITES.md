# Prerequisites

Please do this **before you arrive**. It takes 30-45 minutes, mostly downloading. All tools (Node.js, App Builder, kubectl, Helm, ...) come pre-installed in a **dev container**, so you only need Docker and VS Code on your machine.

You need macOS, Linux, or Windows with [WSL 2](https://learn.microsoft.com/en-us/windows/wsl/install) (native Windows is not supported), and 8 GB+ RAM available to Docker.

## 1. Install Docker

Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) or an alternative such as [Rancher Desktop](https://rancherdesktop.io/), [Colima](https://github.com/abiosoft/colima) or Docker Engine.

> Docker Desktop requires a [paid subscription](https://docs.docker.com/subscription/desktop-license/) for larger organisations.

Then allow the workshop's sandbox registry. In Docker Desktop, go to **Settings → Docker Engine**, add this and click *Apply & restart*:

```json
"insecure-registries": ["192.168.0.101:5000"]
```
### Docker Engine
Using Docker Engine instead of Docker Desktop, you can add the insecure registry by editing the Docker daemon configuration file, typically located at `/etc/docker/daemon.json`, and adding the above insecure registry configuration.

Then restart the Docker service:

```bash
sudo systemctl restart docker
```

## 2. Install VS Code

Install [VS Code](https://code.visualstudio.com/) and the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers).

- **Windows:** use WSL2 with Docker available inside WSL, keep your projects in the WSL filesystem, and enable the VS Code setting *Dev > Containers: Execute In WSL*.
- **Apple silicon Mac:** in Docker Desktop, enable *Use Rosetta for x86_64/amd64 emulation* and give Docker 4+ CPUs and 8+ GB memory.

## 3. Download the dev container and app builder

Pull the image (a few gigabytes, so please do this before you arrive at the breakout session):

```bash
docker pull runtimeplatform.azurecr.io/videoos.io/app-platform-dev-container:latest
docker pull runtimeplatform.azurecr.io/videoos.io/app-builder:2.5.0
```

## 4. Accounts

- **Milestone Developer Portal account.** Needed to install Milestone's NPM packages and read documentation. If you are not a tech partner yet, we can supply a temporary account to access the NPM packages for the workshop.
- **An AI coding agent.** Claude Code, GitHub Copilot CLI, OpenAI Codex, Gemini CLI, or [OpenCode](https://opencode.ai). OpenCode works without an account, using free models, but these are less capable and may use your prompts for training.

## 5. Smart Client (optional)

If running on Windows it is beneficial to have **XProtect Smart Client 2026 R1 Update 8** (or newer) installed on your machine. Get it here: [XProtect Smart Client download](https://www.milestonesys.com/my-milestone/download-software/?prod=2196&type=11&ver=2238&lang=27).

Install the **workshop certificate**, as described in [smart-client-certificate/README.md](smart-client-certificate/README.md). Otherwise, you can use one of our Smart Client VMs at the workshop or borrow a laptop for testing.

## 6. Smoke test

1. Open the `developer-summit-2026` folder as the root folder in VS Code.
2. Choose **Reopen in Container** when prompted by VS Code.
3. It might take a moment up to a few minutes for the container to build and start.
4. Open a new VS Code terminal, run:
   ```bash
   node -v && helm version && app-builder -h
   ```

If all three print something sensible, you're ready. 🎉
