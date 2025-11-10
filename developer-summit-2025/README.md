# Developer Summit 2025 - XProtect Runtime Platform Workshop

Welcome to the hands-on workshop for Milestone's new Runtime Platform, App Center, and App Builder! This repository contains everything you need to get started building applications in the Milestone ecosystem.

## 🎯 Workshop Overview

This workshop is designed for partners who want to get their hands dirty with the Milestone Runtime Platform and learn how to build, deploy, and manage applications using the new development tools. By the end of this workshop, you'll have:

- Set up the Runtime Platform and App Center development environment
- Explored the App Center marketplace and management capabilities
- Explored App Builder interface and documentation
- Built and deployed your first Runtime Platform application using sample code
- Cloned and worked with sample applications from the sample repository
- Understood how these components integrate with the broader Milestone architecture

## 🏗️ Milestone Solution Architecture & Runtime Platform

### High-Level Architecture Overview

The Milestone Runtime Platform represents a fundamental evolution in how applications integrate with and extend Milestone VMS capabilities. Here's how it fits into the overall solution architecture:

```
┌───────────────────────────────────────────────────────────────────────────┐
│                          Milestone Ecosystem                              │
├───────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │              │  │              │  │              │  │              │   │
│  │  App Center  │  │ App Builder  │  │ Management   │  │ Smart Client │   │
│  │ (Discovery & │  │ (Development │  │    Client    │  │              │   │
│  │ Distribution)│  │ Environment) │  │              │  │              │   │
│  │              │  │              │  │              │  │              │   │
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘   │
├───────────────────────────────────────────────────────────────────────────┤
│                       Runtime Platform Layer                              │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │  • Application Hosting & Lifecycle Management                       │  │
│  │  • Security & Sandboxing                                            │  │
│  │  • Resource Management & Scaling                                    │  │
│  │  • Event Processing & Data Streaming                                │  │
│  │  • API Gateway & Service Discovery                                  │  │
│  │  • VMS Core will over time run as containerized apps here           │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
├───────────────────────────────────────────────────────────────────────────┤
│                          XProtect VMS Core                                │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │  • Recording Server                                                 │  │
│  │  • Management Server                                                │  │
│  │  • Event Server                                                     │  │
│  │  • Device Management                                                │  │
│  │  • Storage & Archiving                                              │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
├───────────────────────────────────────────────────────────────────────────┤
│                    Hardware & Infrastructure Layer                        │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │  • Cameras & Sensors                                                │  │
│  │  • Network Infrastructure                                           │  │
│  │  • Storage Systems                                                  │  │
│  │  • Edge Devices                                                     │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────────────────────────────┘
```

### Runtime Platform Key Benefits

**🚀 Modern Application Architecture**
- Containerized applications with automatic scaling
- Microservices-based architecture for better maintainability
- Event-driven programming model for real-time responsiveness

**🔒 Enhanced Security**
- Application sandboxing and isolation
- Fine-grained permission models
- Secure communication channels between components

**⚡ Performance & Scalability**
- Automatic resource allocation based on demand
- Horizontal scaling for high-throughput scenarios
- Optimized data processing pipelines

**🔧 Developer Experience**
- Simplified deployment and lifecycle management
- Rich API ecosystem
- Built-in monitoring and debugging tools

## 📋 Prerequisites

We provide a complete development environment with everything you need pre-installed. You only need:

- [ ] **Remote Desktop Client** installed on your device (Windows Remote Desktop, Windows App (former Microsoft Remote Desktop) for Mac, Remmina for Linux, or similar)
- [ ] **Workshop Credentials Sheet** (provided on-site with connection details)
- [ ] **Familiarity** with containerization concepts (Docker) - helpful but not required

### What's Pre-configured for You:
- ✅ **Complete Development Environment** ready to use with:
   - Runtime Installer
   - Visual Studio Code
   - Git and development tools
- ✅ **XProtect VMS 2025 R3** fully configured and running
- ✅ **Docker** for containerized application development
- ✅ **Network connectivity** to XProtect and App repositories



## 🛠️ Workshop Modules

### Module 1: Setting Up the Development Environment
- [ ] Connect to your pre-configured development environment
- [ ] Install Runtime Platform and App Center
- [ ] Confirm App Center accessibility and configuration
- [ ] Install sample application
- [ ] Tour of the development environment and tools

**Duration:** ~20 minutes  
**Location:** [📁 `./module-1-setup/`](./module-1-setup/README.md)

### Module 2: Exploring App Builder & Documentation
- [ ] App Builder CLI walkthrough
- [ ] Understanding the development workflow
- [ ] Exploring documentation and resources

**Duration:** 15 minutes  
**Location:** [📁 `./module-2-app-builder/`](./module-2-app-builder/README.md)

### Module 3: Exploring App Center
- [ ] Navigate the App Center interface
- [ ] Browse available applications and marketplace
- [ ] Install and manage applications
- [ ] Explore publishing workflows

**Duration:** 10 minutes  
**Location:** [📁 `./module-3-app-center/`](./module-3-app-center/README.md)

### Module 4: Working with Sample Applications
- [ ] Clone the official sample repository: `https://github.com/milestonesys/appcenter-samples`
- [ ] Explore different sample applications and their purposes
- [ ] Build and deploy sample applications
- [ ] Modify samples to understand core concepts
- [ ] Deploy your customized applications to the Runtime Platform

**Duration:** 60 minutes  
**Location:** [📁 `./module-4-samples/`](./module-4-samples/README.md)

## 🚀 Getting Started

### Step 1: Connect to Your Development Environment
1. **Locate your credentials sheet** (provided on-site)
2. **Open your Remote Desktop client** on your device
3. **Connect using the provided details:**
   - Server/Host: `[provided on credentials sheet]`
   - Username: `[provided on credentials sheet]`
   - Password: `[provided on credentials sheet]`

### Step 2: Verify Your Environment
Once connected to the remote desktop:

**Note:** Launching an application for the first time may take a few moments as the system initializes.

1. **Open the terminal** (press the super/Windows key or click the top left corner, then start typing "terminal" and select it)
2. **(Optional) Change password** to something you prefer, and is easier to input during the installation, execute and follow the prompts:
   ```bash
   passwd
   ```
3. **Clone this workshop repository and open it in VS Code:** 
   ```bash
   mkdir ~/workspace && cd ~/workspace
   git clone https://github.com/milestonesys/appcenter-workshops.git
   cd appcenter-workshops/developer-summit-2025
   code .
   ```

### Step 3: Start the Workshop
Navigate to [📁 `./module-1-setup/`](./module-1-setup/README.md) and follow the step-by-step instructions to begin your journey with the Runtime Platform!

---

## 📚 Additional Resources

### Documentation
- [Runtime Installation Guide](https://download.milestonesys.com/app-builder/Documentation/Runtime%20Installation%20Wizard.pdf)
- [App Builder Reference](https://download.milestonesys.com/app-builder/Documentation/App-Builder.pdf)

The two documents above are also available locally in the development environment under `~/Documents`.

### Community
- [Developer Forum](https://developer.milestonesys.com)
- [App Center Sample Applications](https://github.com/milestonesys/appcenter-samples)

## 🤝 Support & Feedback

- **Workshop Issues:** Create an issue in this repository
- **Technical Support:** Contact your Milestone partner representative
<!-- - **Documentation Feedback:** [Submit feedback](https://doc.milestonesys.com/feedback/) -->

<!-- ## 📄 License

This workshop content is provided under the MIT License. See `LICENSE` file for details. -->

## 🙏 Acknowledgments

Special thanks to the Milestone Systems Open Platform team and all workshop contributors.

---

**Ready to get started?** Head over to [Module 1: Setting Up the Development Environment](./module-1-setup/) to begin your journey!