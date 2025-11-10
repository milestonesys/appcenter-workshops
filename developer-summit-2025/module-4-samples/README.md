# Module 4: Working with Sample Applications

**Duration:** 60 minutes

Welcome to the final module! This is where everything comes together. You'll clone the official sample repository, explore real applications, build and deploy them, and customize them to understand core concepts.

## 🎯 Learning Objectives

By the end of this module, you will:
- ✅ Have cloned and explored the official sample applications repository
- ✅ Built and deployed multiple sample applications
- ✅ Understood different application patterns and use cases
- ✅ Modified sample applications to see how changes affect functionality
- ✅ Be confident in the complete development workflow

## 📋 Prerequisites

- Modules 1, 2, and 3 completed successfully
- App Builder CLI configured and working
- Visual Studio Code with terminal access
- Runtime Platform and App Center operational

## 📝 Important Setup Note

### AI Bridge Sample Requirement

Before working with the **AI Bridge sample**, you need to manually create an application registration secret.

**Why is this needed?**
- The AI Bridge sample requires the processing server
- Normally, this would be installed via the Management Client plugin for App Center
- The Management Client would automatically create the required secret
- Since the Management Client is not available in this workshop environment, manual setup is required

**Setup Steps:**
1. If not logged in already, log in with app-builder:
   ```bash
   app-builder login
   ```

2. Create the application registration secret:
   ```bash
   create-app-registration-secret
   ```

3. Proceed with the AI Bridge sample deployment as documented

> ⚠️ **Note:** This manual step is only required for the AI Bridge sample and only in workshop environments without Management Client access.

## 🚀 Step-by-Step Instructions

### Step 1: Clone the Official Sample Repository (5 minutes)

1. **Clone the Repository:**
   ```bash
   # Navigate to your workspace
   cd ~/workspace
   
   # Clone the official samples repository
   git clone https://github.com/milestonesys/appcenter-samples.git
   
   # Navigate to the samples directory
   cd appcenter-samples
   
   # Open in VS Code
   code .
   ```

2. **Explore Repository Structure:**
   Look at the explorer in VS Code or the terminal:
   ```bash
   # List available samples
   tree -d -L 2
    .
    ├── build
    ├── img
    └── src
        ├── aibridge-prometheus-sample
        ├── apigateway-webserver-sample
        ├── dotnet-webserver-sample
        ├── hello-world
        ├── kafka-topics
        ├── postgresql
        ├── restful-config-api
        └── utils
   ```

3. **Review Documentation:**
   - Open and read the main `README.md` in the root of the repository.
   - Review each sample's individual documentation
   - Understand prerequisites and requirements

### Step 2: Explore Sample Applications (55 minutes)

1. **Follow the instructions in each sample's README:**
   Happy coding!

### Step 3: Customize and Modify Samples - optional (remaining time)

1. **Simple Modifications:**
   
   **Modify Any Sample Application:**
   Modify the code of any sample application to see how changes affect functionality.

### Application Architecture Patterns:

```
┌─────────────────────────────────────────┐
│           Sample Application            │
├─────────────────────────────────────────┤
│  Frontend (UI)                          │
│  ├── Web interface                      │
│  ├── API client                         │
│  └── User interactions                  │
├─────────────────────────────────────────┤
│  Backend (Logic)                        │
│  ├── Business logic                     │
│  ├── Data processing                    │
│  └── External integrations              │
├─────────────────────────────────────────┤
│  XProtect Integration                   │
│  ├── Camera access                      │
│  ├── Event handling                     │
│  └── User management                    │
└─────────────────────────────────────────┘
```

## 🔍 Verification Checklist

Before completing the workshop, ensure you have:

- [ ] Successfully cloned the appcenter-samples repository
- [ ] Explored at least 3 different sample applications
- [ ] Built and deployed at least 3 sample applications
- [ ] (Optional) Made meaningful modifications to at least 1 sample
- [ ] All applications are running correctly in App Center
- [ ] Tested the functionality of your modified applications

## 🛠️ Troubleshooting

### Common Issues:

**Build Failures:**
- Build failures can vary depending on the tech stack, but some general tips:
  - Verify all dependencies are installed
  - Review build logs for specific errors
  - Ensure app.yaml is valid

**Deployment Issues:**
- Verify Runtime Platform capacity
- Check application naming conflicts
- Review deployment logs
- Use app-builder `events` and `status` commands to diagnose issues

**Runtime Errors:**
- Check application logs in App Center
- Verify XProtect connectivity
- Ensure required services are running
- Review configuration settings

**Modified Code Not Working:**
- Verify syntax is correct
- Check for missing dependencies
- Ensure proper rebuild and redeploy
- Review application logs for errors

## 💡 Next Steps After the Workshop

### Continue Learning:
1. **Explore More Samples:**
   - Work through all sample applications
   - Study advanced integration patterns
   - Create micro frontends (MFEs) which can be integrated into XProtect using the Runtime Platform.

2. **Build Your Own Applications:**
   - Start with simple use cases
   - Gradually add complexity
   - Focus on solving real problems

3. **Join the Community:**
   - Participate in developer forums
   - Share your applications
   - Contribute to sample repository

### Development Best Practices:
- Version control all your code
- Document your applications thoroughly
- Test applications in development environment
- Monitor performance and resource usage
- Keep applications updated with latest APIs

## 📚 Additional Resources

- [Complete Sample Applications](https://github.com/milestonesys/appcenter-samples)

## 🎉 Workshop Completion

**Congratulations!** You've completed the Milestone App Center workshop. You now have:

- ✅ A working development environment
- ✅ Understanding of App Center and application management
- ✅ Knowledge of App Builder and development workflow
- ✅ Hands-on experience with real sample applications
- ✅ The foundation to build your own App Center applications

### Workshop Survey
Please take a few minutes to provide feedback on your workshop experience:
- What worked well?
- What could be improved?
- What topics would you like to see covered in future workshops?

---

**Thank you for participating!** We look forward to seeing the innovative applications you'll build for the App Center.