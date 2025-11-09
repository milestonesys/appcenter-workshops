# Module 2: Exploring App Builder & Documentation

**Duration:** 15 minutes

Welcome to Module 2! In this module, you'll explore App Builder - the command-line tool and development workflow for creating custom applications for the App Center.

## 🎯 Learning Objectives

By the end of this module, you will:
- ✅ Understand the App Builder CLI and its capabilities
- ✅ Be familiar with the application development workflow
- ✅ Know how to access and use documentation effectively
- ✅ Understand application structure and components
- ✅ Be prepared to build your own applications

## 📋 Prerequisites

- Module 1 completed successfully
- Visual Studio Code open and ready
- Terminal/Command Prompt access
- Runtime Platform operational

## 🚀 Step-by-Step Instructions

### Step 1: Introduction to App Builder CLI (3 minutes)

1. **App Builder CLI:**

   App Builder is already installed in your development environment. It is a command-line interface (CLI) tool that helps you create, build, deploy, and manage applications for the App Center.
   
   For a more detailed walkthrough, see the [official documentation](https://download.milestonesys.com/app-builder/Documentation/App-Builder.pdf) in [Step 3](#step-3-exploring-documentation--resources-10-minutes).

   ```bash
   # Open terminal in VS Code or a regular terminal
   # View available commands
   app-builder -h
   
   # Key commands to explore:
   app-builder -h      # Shows available commands/help
   app-builder login   # Login to the Runtime Platform to interact with it
   app-builder build   # Build App as Helm Chart (the defaul command)
   app-builder push    # Push app to the sandbox repository
   ```

### Step 2: Understanding the Development Workflow (2 minutes)

1. **Application Lifecycle Overview:**
   ```
   Create → Develop → Build → Deploy → Test → Ready to Publish? → Bundle → Submit
      ↑                                        ↓
      └──────── Update ← Monitor ←─────────────┘
   ```
   - **Create:** Use App Builder to scaffold new applications.
   - **Develop:** Write application logic and UI components.
   - **Build:** Compile and package the application using App Builder.
   - **Deploy:** Push the application to the App Center for distribution.
   - **Test:** Validate application functionality locally or in a sandbox.
   - **Monitor:** Track application performance and usage.
   - **Update:** Implement changes and improvements based on feedback.
   - **Bundle & Submit:** Prepare the application for publishing to Milestone to make the app available in the App Center for customers.

2. **Key Components:**
   - **app.yaml:** Application configuration and metadata
   - **Source Code:** Main application logic
   - **Resources:** Static files, icons, images, app description

### Step 3: Exploring Documentation & Resources (10 minutes)

1. **Official Documentation:**
   - Navigate to [App Builder Documentation](https://download.milestonesys.com/app-builder/Documentation/App-Builder.pdf)
   - Key sections to explore:
     - A simple example
     - The Sandbox Repository
     - Detailed command line usage

2. **Code Samples:**
   - [Module 4](../module-4-samples/README.md) will cover Code samples using the App Builder.

## 🔍 Verification Checklist

Before proceeding to Module 4, ensure you have:

- [ ] App Builder CLI functional
- [ ] Explored key documentation sections
- [ ] Familiar with app.yaml configuration format
- [ ] Bookmarked important documentation resources

## 📚 Additional Resources

- [App Builder Documentation](https://download.milestonesys.com/app-builder/Documentation/App-Builder.pdf)
- [Sample Applications](https://github.com/milestonesys/appcenter-samples)

## ➡️ Next Steps

Once you've completed this module successfully, proceed to:
**[Module 3: Exploring App Center](../module-3-app-center/README.md)**

---

**Need Help?** Ask your workshop instructors.