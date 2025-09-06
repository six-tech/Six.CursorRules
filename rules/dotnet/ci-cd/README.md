# 🌊 CI/CD Rules and Best Practices

This directory contains comprehensive rules and best practices for CI/CD processes in .NET projects. These rules ensure
consistency, quality, and security across build and deployment processes, supporting both GitHub Actions and Azure
DevOps pipelines.

## 🏗️ [.NET Build System](dotnet-gh-workflow-build.mdc)

Use these rules when:

- 🏭 Setting up automated build pipelines for .NET projects
- 📦 Managing project structure and build configurations
- 🔄 Implementing version management and release processes
- 🧪 Organizing testing strategies and quality assurance
- 🌐 Configuring cross-platform builds and deployments
- 📊 Setting up monitoring and performance tracking

These rules ensure:

- 🔧 **Native .NET CLI usage** as the primary build mechanism
- 📁 **Cross-platform compatibility** across Windows, Linux, and macOS
- 🏷️ **Automated version management** with semantic versioning
- 🧪 **Comprehensive testing** with proper test result publishing
- 📈 **Performance optimization** through efficient build caching
- 🔐 **Security integration** with proper credential management
- 📊 **Quality assurance** with automated validation and reporting

## 🔐 [Code Signing](dotnet-gh-workflow-code-signing.mdc)

Use these rules when:

- 🛡️ Implementing secure code signing for .NET applications
- 🔑 Managing signing certificates and cryptographic keys
- 🔒 Configuring secure CI/CD pipelines for signed releases
- 📋 Setting up audit trails and compliance requirements
- 🏢 Implementing enterprise-grade security practices
- 📊 Monitoring certificate lifecycle and renewal processes

These rules ensure:

- 🔐 **Automated code signing** in CI/CD pipelines using SignClient
- 🛡️ **Security best practices** for credential management and access control
- 📋 **Comprehensive audit logging** for all signing operations
- 🔄 **Certificate lifecycle management** with expiration monitoring
- 🏢 **Compliance requirements** for enterprise deployments
- ✅ **Signature verification** before package distribution

## 📦 [NuGet Package Publishing](dotnet-gh-workflow-nuget-publishing.mdc)

Use these rules when:

- 📤 Publishing high-quality NuGet packages to public or private feeds
- 🏷️ Managing package versioning and metadata
- 📚 Creating comprehensive package documentation
- 🔍 Ensuring package discoverability and quality
- 🔒 Implementing secure publishing workflows
- 📊 Setting up automated package validation and testing

These rules ensure:

- 📋 **Semantic versioning** following SemVer principles
- 📚 **Comprehensive metadata** with proper descriptions and documentation
- 🔗 **Source debugging support** with symbol packages and SourceLink
- 🛡️ **Security validation** with automated package scanning
- 📊 **Quality assurance** through pre-publish checklists
- 🔄 **Automated publishing** workflows for reliable releases
- 📈 **Package discoverability** with proper tagging and categorization

## 🚀 **Using These Rules**

### **Automatic Enforcement**

These rules are automatically enforced through Cursor when working with relevant files, providing inline guidance and
validation to ensure your CI/CD configurations follow established best practices.

### **Pipeline Compatibility**

All rules support both GitHub Actions and Azure DevOps pipelines, ensuring consistent practices regardless of your CI/CD
platform choice.

### **Integration Points**

- **Build Systems**: Connect with project structure and testing rules
- **Security**: Integrate with authentication and authorization requirements
- **Quality**: Work alongside testing and documentation standards
- **Compliance**: Support audit and regulatory requirements

## 📞 **Support and Contact**

For questions about our CI/CD practices or to propose changes to these rules, please contact the DevOps team. 