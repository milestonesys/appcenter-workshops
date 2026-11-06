# App Platform Dev Container

A ready-made development environment for building apps for the **Milestone
XProtect App Platform**, for Milestone technology partners, integrators, and
anyone else developing for the platform. Everything the toolchain needs (App
Builder, kubectl, Helm, Node.js, the right `yq`) is already installed in a
pre-built image, so there is nothing to set up beyond copying one directory into
your project.

The image is published publicly at
`runtimeplatform.azurecr.io/videoos.io/app-platform-dev-container`, so no registry
account or login is needed to use it.

## Prerequisites

- **A Docker daemon on your machine.** The container talks to your host's
  daemon rather than running one of its own, because App Builder starts
  containers of its own. Docker Desktop, Rancher Desktop, Colima, Podman and
  plain Docker CE all work. See
  [If you can't use Docker Desktop](#if-you-cant-use-docker-desktop).
- **VS Code with the [Dev Containers extension]**, or the
  [`devcontainer` CLI](#without-vs-code) if you use a different editor.
- On Windows, **WSL2 only**; see [Windows](#windows). Native Windows is
  not supported.

[Dev Containers extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers

## Getting started

1. Copy this `.devcontainer/` directory (the one holding this README) into the
   root of your project. If your project already has a `.devcontainer/`, merge
   the settings rather than replacing them. See
   [Don't change the mounts](#dont-change-the-mounts) for the ones that matter.
2. Open the project in VS Code and choose **Reopen in Container** (or
   *Dev Containers: Rebuild and Reopen in Container* from the command palette).
3. Wait for the image to pull the first time (it is a few GB). Then you get a
   shell with the whole toolchain on `PATH`.

Check that it worked:

```bash
helm version

app-builder -h # Note: This will pull the App Builder image if it hasn't been pulled yet
```

## What's in the image

| Tool | Notes |
| --- | --- |
| **App Builder** | Run as `app-builder` (or `app-builder.sh`) |
| **Docker CLI** | Wired to your host's daemon (docker-outside-of-docker) |
| **kubectl** | v1.36.3, with bash completion |
| **Helm** | v3.21.4 (3.x, for chart compatibility), with bash completion |
| **Node.js** | v24.11.0 (24.x LTS), with `npm` and `npx` |
| **Task** | v3.44.0 ([taskfile.dev](https://taskfile.dev)), with bash completion |
| **k9s** | v0.51.0, a Kubernetes cluster TUI, with bash completion |
| **lazydocker** | v0.25.2, a Docker TUI |
| **lazygit** | v0.64.1, a Git TUI |
| **glow** | v3.0.0, a terminal Markdown renderer |
| **bat** | v0.26.1, a `cat` clone with syntax highlighting |
| **Neovim** | v0.12.4, the `nvim` editor |
| **Python 3** | Ubuntu's `python3` |
| **yq** | kislyuk/yq, the **jq wrapper**, which is what the App Platform sample Makefiles need |
| **jq** | |
| **OpenSSH client** | `ssh`, `scp`, `ssh-keygen` |
| **xclip** | Used by `app-builder dashboard` |
| **npm-auth** | Run as `npm-auth` (or `npm-auth.sh`). Authenticates `npm` against the `@milestone` registry |

Plus the usual base-image conveniences: `git`, `curl`, `wget`, `bash-completion`
and a non-root `vscode` user.

### Adding a language runtime or optional tools

.NET, Go and AI coding agents are **not** in the image, because only some
projects or developers need them. Each is one uncommented line in
`.devcontainer/devcontainer.json`:

```jsonc
"features": {
    "ghcr.io/devcontainers/features/dotnet:2": { "version": "10.0" },
    "ghcr.io/devcontainers/features/go:1": { "version": "latest" },
    "ghcr.io/devcontainers-extra/features/opencode:1": { "version": "latest" },
    "ghcr.io/devcontainers/features/copilot-cli:1": { "version": "latest" },
    "ghcr.io/anthropics/devcontainer-features/claude-code:1": {},
    "ghcr.io/devcontainers-extra/features/npm-package:1": { "package": "@openai/codex", "version": "latest" }
    // or, instead of Codex:
    // "ghcr.io/devcontainers-extra/features/npm-package:1": { "package": "@google/gemini-cli", "version": "latest" }
}
```

| Feature | Adds | Size | Needs |
|---|---|---|---|
| `dotnet` | .NET SDK | ~550 MB | |
| `go` | Go toolchain | ~450 MB | |
| `opencode` | [OpenCode](https://opencode.ai) (`opencode`) | ~180 MB | Nothing, see [below](#opencode) |
| `copilot-cli` | [GitHub Copilot CLI](https://github.com/github/copilot-cli) (`copilot`) | ~180 MB | A GitHub Copilot subscription |
| `claude-code` | [Claude Code](https://docs.claude.com/en/docs/claude-code) (`claude`) plus its VS Code extension | ~430 MB | A Claude subscription or an Anthropic API key |
| `npm-package` | [OpenAI Codex CLI](https://github.com/openai/codex) (`codex`) | ~630 MB | A ChatGPT plan or an OpenAI API key |
| `npm-package` | [Gemini CLI](https://github.com/google-gemini/gemini-cli) (`gemini`) | ~200 MB | A Google account or a Gemini API key |

**Codex and Gemini are either/or.** Both use a generic feature that installs an
npm package globally, and a feature can be listed only once, so uncomment one of
the two entries, not both. Claude Code's feature has no `version` option and
always installs the latest release.

Rebuild the container to apply. Anything from
[containers.dev/features](https://containers.dev/features) can be added the same
way, and removing an entry and rebuilding gives the disk space back.

#### Logging in to an AI agent

Logins on your host are not forwarded into the container, and one you make in
the container lasts until it is rebuilt. Start the agent and follow its prompt,
or:

- **Copilot CLI**: run `copilot` and type `/login`, or export a token with the
  Copilot permission as `COPILOT_GITHUB_TOKEN` (`GH_TOKEN` and `GITHUB_TOKEN`
  work too).
- **Claude Code**: run `claude` and pick a Claude account or an API key, or
  export `ANTHROPIC_API_KEY`.
- **Codex**: run `codex login`, or pipe an API key in with
  `printenv OPENAI_API_KEY | codex login --with-api-key`.
- **Gemini CLI**: run `gemini` and pick *Sign in with Google*, or export
  `GEMINI_API_KEY`.

Agents run inside the container with the same access you have there, including
the host's Docker daemon and your kubeconfig, so review what you let them run
unattended.

#### OpenCode

OpenCode works **without logging in**: out of the box it offers a handful of
free models through OpenCode Zen (`opencode models opencode` lists them;
`big-pickle` is the default). They come with limitations:

- The line-up changes, and most are free only for a limited time.
- Several of them may use your prompts and code to improve the model, so check
  the [Zen docs](https://opencode.ai/docs/zen/) before pointing one at code you
  are not allowed to share.
- They are generally less capable than the paid frontier models.

To use your own provider (Anthropic, OpenAI, GitHub Copilot, …), run
`opencode auth login` inside the container.

## Authenticating npm against the `@milestone` registry

Some App Platform packages are published to Milestone's `@milestone` npm
registry, which needs credentials. Milestone employees and technology partners
both have them. If your project depends on those packages, run:

```bash
npm-auth
```

It prompts for your UPN (User Principal Name) and password. You can find your
UPN in the Milestone Developer Portal, under your profile. The password prompt
is hidden, so nothing is echoed to the terminal. `npm-auth` then exchanges them
with Milestone's npm auth endpoint and configures the `@milestone` registry and
auth token in your npm config (`~/.npmrc` in the container). Neither value is
ever printed, logged, or accepted as a command-line flag or environment
variable.

The token **expires after 12 hours**. When `npm install` starts failing with an
authentication error against the `@milestone` registry, run `npm-auth` again.
You also need to re-run it after rebuilding the container, because `~/.npmrc`
lives inside the container and does not survive a rebuild.

## Your kubeconfig

Your host `~/.kube` is mounted into the container, so the cluster contexts you
already have work without copying anything. `kubectl`, `helm`, `k9s` and App
Builder all see the same config, and changes are shared both ways because it is
a bind mount, not a copy.

If the directory doesn't exist on your host yet, the configuration creates it
before the container starts.

## Don't change the mounts

The template's `workspaceMount`, `workspaceFolder`, `mounts`, `containerEnv` and
`postCreateCommand` entries exist to keep file paths **identical inside and
outside** the container. That is load-bearing:

App Builder runs a container of its own against your **host's** Docker daemon,
passing along paths it resolved inside the dev container. A container-only path
like `/workspaces/my-app` means nothing to the host daemon, and instead of
failing, it creates an empty directory on the host and mounts that. Your files
appear to vanish, and the error you eventually get points at a path you have
never seen, e.g. `Cannot find bundle file named /root/bundle.tar` from
`push-bundle` on a file that plainly exists.

So the template mounts your project at its host path rather than the default
`/workspaces/<name>`, mounts `~/.kube` at its host path, and runs
`app-platform-init` after creation to link the container's `~/.kube` to it. If
you do end up with a default workspace mount, `app-platform-init` prints a
warning explaining the fix.

Mounts are fixed when the container is created, so **editing
`devcontainer.json` only takes effect on a rebuild**.

## Pushing images to a registry

The image sets `DOCKER_CONFIG=/home/vscode/.docker-cli`, which means the Docker
CLI inside the container does not read `~/.docker/config.json` and therefore
does not inherit your host's registry logins.

This is deliberate. When VS Code attaches to a dev container it writes a
`credsStore` into that file pointing at a helper that only works while VS Code
is driving; in any other shell (`devcontainer exec`, `docker exec`, a script)
every push then fails with:

```
error getting credentials - err: exit status 255, out: ``
```

even for an anonymous registry, because the 401 token exchange still resolves
credentials.

Anonymous registries, including the App Platform sandbox registry, work as-is.
For a private registry, run `docker login` inside the container.

## Known limitations

- **`app-builder dashboard` can't copy to your clipboard.** It shells out to
  `xclip`, which needs an X display the container doesn't have. Use
  `app-builder dashboard-create-token` and copy the token yourself. The
  dashboard itself is reachable at `https://localhost:10443` **on the host**:
  the port is published by the host daemon, so no port forwarding is involved.
- **linux/amd64 only.** On Apple silicon the container runs emulated; see
  [Apple silicon Macs](#apple-silicon-macs).
- **Windows: WSL2 only.** The project must live in, and be opened from, WSL,
  with *Execute In WSL* enabled; see [Windows](#windows).
- **Host paths must be shareable with Docker.** On macOS, both your project
  directory and `~/.kube` have to fall inside the file-sharing configuration of
  Docker Desktop (or of your Lima/Colima VM).
- **The listed VS Code extensions come from the Visual Studio Marketplace**,
  whose [terms of use](https://cdn.vsassets.io/v/M253_20250303.9/_content/Microsoft-Visual-Studio-Marketplace-Terms-of-Use.pdf)
  allow installing them only in Microsoft's own products (Visual Studio,
  VS Code, GitHub Codespaces, Azure DevOps). In Cursor, VSCodium or code-server,
  use the [`devcontainer` CLI](#without-vs-code) with that editor's own
  extension registry.

## Without VS Code

Nothing here depends on VS Code. The open-source
[`devcontainer` CLI](https://github.com/devcontainers/cli) reads the same
`devcontainer.json`, which is how to use this from Cursor, VSCodium, Neovim or a
plain terminal. It runs on the **host** and needs Node.js 18+ there:

```bash
npm install -g @devcontainers/cli
```

From your project directory:

```bash
devcontainer up --workspace-folder .                    # create/start the container
devcontainer exec --workspace-folder . bash             # get a shell in it
devcontainer exec --workspace-folder . app-builder list # run a command inside it
```

Because the workspace is mounted at its host path, that shell starts in the same
absolute directory you were in on the host.

Differences worth knowing:

- **There is no `down` or `stop`.** `up` reuses an existing container; pass
  `--remove-existing-container` to force a fresh one, which is also what you
  need after editing `devcontainer.json`. To clean up, go through Docker:

  ```bash
  docker ps -aq --filter "label=devcontainer.local_folder=$PWD" | xargs -r docker rm -f
  ```

- **`customizations.vscode` is ignored**. Extensions and settings are applied
  by the Dev Containers extension, not the CLI.
- **Prefer `devcontainer exec` over `docker exec`**: it honours `remoteUser`,
  the container environment and the working folder. A raw `docker exec` needs
  `-u vscode -w <host-path>` to match.
- **Non-Docker runtimes**: pass `--docker-path podman` (or another compatible
  CLI) to `up` and `exec`.

## Windows

On Windows, **only WSL2 is supported**. Opening the project from a native
Windows path such as `C:\Users\you\my-app` does not work: `${localEnv:HOME}`
in the configuration is undefined in a native Windows environment, and the
Windows paths would break the host path alignment described under
[Don't change the mounts](#dont-change-the-mounts).

1. **Keep the project inside the WSL filesystem**, e.g. `~/src/my-app` in your
   distro, not under `/mnt/c/...`. Your `~/.kube` is then the one in WSL, too.
2. **Make Docker available inside WSL**, either Docker Desktop with WSL
   integration enabled for your distro, or Docker Engine installed directly in
   the distro, which needs no Docker Desktop licence.
3. **Enable *Dev > Containers: Execute In WSL*** in VS Code
   (`"dev.containers.executeInWSL": true` in your user settings), so the Dev
   Containers extension runs its Docker commands and the host-side
   `initializeCommand` in WSL rather than in Windows. If you have several
   distros, set *Execute In WSL Distro* (`dev.containers.executeInWSLDistro`)
   to the one holding your project.
4. **Open the project from WSL**: run `code .` in a WSL shell, or use
   *WSL: Connect to WSL* first, and then choose **Reopen in Container**.

## Apple silicon Macs

The image is **linux/amd64 only**, so on an M-series Mac it runs under x86_64
emulation. That works and is what the App Platform toolchain expects, but it is
worth configuring:

1. **Turn on Rosetta** in *Docker Desktop → Settings → General → "Use Rosetta for
   x86_64/amd64 emulation on Apple Silicon"*. It is **off by default** and only
   selectable once *Virtual Machine Manager* is set to **Apple Virtualization
   framework**. It is dramatically faster than QEMU for CPU-bound work like
   `npm install`, `dotnet build` and `docker build`. Pair it with **VirtioFS**
   file sharing, which matters here because your whole workspace is a bind mount.
2. **Silence the platform warning** by exporting
   `DOCKER_DEFAULT_PLATFORM=linux/amd64` in your shell profile, or adding
   `"runArgs": ["--platform=linux/amd64"]` to `devcontainer.json`.
3. **Give the VM headroom**: 4+ CPUs and 8+ GB under *Settings → Resources*.
   Emulation costs both CPU time and memory.

Two consequences specific to this setup: App Builder's own container runs on the
host daemon, so it is emulated too (the slowdown isn't confined to the dev
container), and images you build come out amd64, which is normally what you
want for the App Platform.

Rosetta translates user-space x86_64 only, and not every instruction set. If
something fails with an illegal instruction, turning the setting back off (so
QEMU handles it) is the first thing to try.

## If you can't use Docker Desktop

Docker Desktop [requires a paid subscription](https://docs.docker.com/subscription/desktop-license/)
for organisations with 250+ employees or over $10M in annual revenue. If yours
is above either threshold and has no Docker subscription, use one of these
instead. Nothing in the image is affected; the in-container Docker CLI is
Apache-2.0 Moby either way.

**Colima** is CLI-only and the closest to a drop-in replacement:

```bash
brew install colima docker
colima start --vm-type=vz --vz-rosetta --mount-type=virtiofs --cpu 4 --memory 8
```

Keep the VM aarch64 (the default) and let Rosetta handle amd64 containers;
`--arch x86_64` emulates the whole VM, which is slower. Note that only `$HOME`
and `/tmp/colima` are shared into the VM by default. Since this template mounts
your project and `~/.kube` at their host paths, both must be under `$HOME`, or
added to `mounts:` in `~/.colima/default/colima.yaml` followed by
`colima restart`.

**Rancher Desktop** is a GUI option that runs dockerd, wires up the standard
socket itself, and offers VZ + Rosetta in its emulation settings. The middle
ground if you want Docker Desktop's ergonomics without the licence.

**Podman** works, but is the least-tested route for this image: `app-builder`
needs a Docker-compatible socket at `/var/run/docker.sock`
(`sudo podman-mac-helper install`), and rootful vs. rootless affects bind-mount
file ownership.

## Licensing

The image is © Milestone Systems A/S and bundles third-party software that keeps
its own terms: Apache-2.0 (kubectl, Helm, k9s, yq), MIT (Node.js, Task,
lazydocker, lazygit, jq, glow), MIT-or-Apache-2.0 (bat), Apache-2.0-plus-Vim-license (Neovim) and
GPL-2.0-or-later (xclip, bash-completion, and GPL software inherited from the
Ubuntu base image). Nothing in the image copylefts your application code: these
are separate programs your build invokes as tools.

The full component list, each licence text, and the written offer for GPL
sources ship inside the image:

```bash
ls /usr/share/licenses/
cat /usr/share/licenses/THIRD-PARTY-NOTICES.md
```

Milestone's own terms for the image are in `/usr/share/licenses/LICENSE`.

## Getting help

- **App Platform developer documentation**: See the [Milestone Developer
  Portal documentation](https://portal.developer.milestonesys.com/).
- **Sample apps**: See the [App Platform samples repository](https://github.com/milestonesys/appcenter-samples).
- **Questions and problems with this dev container**: Please write in the [Milestone Developer Forum](https://forum.milestonesys.com/).

When reporting a problem, include the image digest
(`docker image inspect --format '{{index .RepoDigests 0}}' runtimeplatform.azurecr.io/videoos.io/app-platform-dev-container:latest`
on the host) and your host OS and Docker runtime.
