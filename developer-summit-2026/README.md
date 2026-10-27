# Developer Summit 2026 - Build a Milestone MFE with AI Tooling

Welcome to the hands-on workshop! You've seen the talk on using AI tooling to build micro frontends (MFEs) for the Milestone XProtect App Platform. Now it's your turn. In about two hours you'll go from an empty folder to your own MFE running inside XProtect Smart Client.

## 🎯 What You'll Build

A **System Health dashboard MFE** for XProtect **operators**. It's based on real user research from Milestone's Operator value stream: an operator wants to know, at a glance, whether the system is running properly and whether any cameras, devices or servers are offline.

By the end of this workshop, you'll have:

- Set up a ready-made development environment (dev container) with your AI agent of choice
- Scaffolded a new MFE with `@milestone/create-mfe-app`
- Used an AI agent, with Milestone's MCP server and agent skills, to implement a user story
- Packaged your MFE with App Builder and pushed it to a shared App Platform sandbox
- Installed it through App Center and seen it running in Smart Client

## 🏗️ How the Pieces Fit Together

You build on your own laptop. We provide everything else.

```
┌──────────────────── Your laptop ────────────────────┐
│  VS Code + Dev Container                            │
│  ┌───────────────────────────────────────────────┐  │
│  │  Node.js · App Builder · kubectl · Helm       │  │
│  │  Your AI agent  ⇄  Milestone MCP + skills     │  │
│  │  Your MFE project (create-mfe-app)            │  │
│  └───────────────────────────────────────────────┘  │
└────────────────────────┬────────────────────────────┘
                         │  app-builder push / bundle
                         ▼
┌──────────── Shared, provided by Milestone ──────────┐
│  App Platform (sandbox registry + App Center)       │
│            │ install                                │
│            ▼                                        │
│  Your MFE  ──────▶  XProtect VMS  ◀──── Smart Client│
│                                  (yours or a loaner)│
└─────────────────────────────────────────────────────┘
```

> ⚠️ **The App Platform is shared by everyone in the room.** That's why you'll give your app a unique name prefix (your initials) in [Module 1](./module-1-setup/README.md).

## 📋 Prerequisites

Please complete **[PREREQUISITES.md](./PREREQUISITES.md) before the workshop**. The dev container image is several GB, and conference Wi-Fi is not your friend.

At the workshop you'll also get a **credentials handout** with:
- App Platform / sandbox connection details
- App Center URL
- XProtect server address and a login

## 🛠️ Workshop Modules

### Module 1: Set Up Your Workspace
- [ ] Create your project folder and open it in the dev container
- [ ] Enable and log in to your AI agent
- [ ] Authenticate npm against the `@milestone` registry
- [ ] Connect to the shared App Platform
- [ ] Pick a unique app prefix

**Duration:** ~20 minutes  
**Location:** [📁 `./module-1-setup/`](./module-1-setup/README.md)

### Module 2: Scaffold Your MFE
- [ ] Generate a new MFE with `npx @milestone/create-mfe-app`
- [ ] Take a tour of the generated project
- [ ] Run it locally
- [ ] Connect your agent to the Milestone MCP server and agent skills

**Duration:** ~15 minutes  
**Location:** [📁 `./module-2-create-mfe/`](./module-2-create-mfe/README.md)

### Module 3: Build the Feature with Your AI Agent
- [ ] Pick a user story
- [ ] Write a good prompt from the template
- [ ] Iterate with your agent until the story is done

**Duration:** ~55 minutes  
**Location:** [📁 `./module-3-build-with-ai/`](./module-3-build-with-ai/README.md)

### Module 4: Deploy and See It in Smart Client
- [ ] Package and push your MFE to the sandbox with App Builder
- [ ] Install it from App Center
- [ ] Open it in Smart Client

**Duration:** ~25 minutes  
**Location:** [📁 `./module-4-deploy/`](./module-4-deploy/README.md)

## 🚀 Getting Started

1. Make sure you've completed the [prerequisites](./PREREQUISITES.md).
2. Grab a credentials handout from an instructor.
3. Head to **[Module 1: Set Up Your Workspace](./module-1-setup/README.md)**.

---

## 📚 Additional Resources

- [App Center Sample Applications](https://github.com/milestonesys/appcenter-samples)
- [Developer Forum](https://developer.milestonesys.com)
- [Developer Summit 2025 workshop](../developer-summit-2025/README.md): Runtime Platform, App Center and App Builder basics

## 🤝 Support & Feedback

- **During the workshop:** raise your hand. Instructors are walking around.
- **Workshop issues:** create an issue in this repository.
- **Technical support:** contact your Milestone partner representative.
