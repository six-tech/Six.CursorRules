# .NET Project Management Best Practices

This directory contains comprehensive rules for managing modern .NET projects with industry-standard practices, covering everything from solution architecture and dependency management to testing, benchmarking, and library development. Focus on security, performance, maintainability, and developer experience through proven patterns and tools.

## 🏗️ [Solution Management](dotnet-solution.mdc)

**Use this when you're:**
- Setting up a new .NET solution with modern architecture
- Implementing consistent build properties and SDK versioning
- Configuring secure multi-environment package sources
- Establishing proper project structure and build configurations
- Managing complex solutions with multiple project types

**These rules establish:**
- ✅ Modern `.slnx` solution format over legacy `.sln`
- 🔒 SDK version control via `global.json` for consistent builds
- 📋 Shared metadata and build properties via hierarchical `Directory.Build.props`
- 🔐 Secure multi-environment package sources via `nuget.config` with environment variables
- 🚀 Optimized build and compilation strategies for performance
- 🏛️ Proper solution architecture with apps/, libs/, tests/, and benchmarks/ directories

## 📚 [Library Management](dotnet-library.mdc)

**Use this when you're:**
- Creating or maintaining .NET library projects
- Library needs to be packaged as a NuGet package
- Setting up proper package metadata and documentation
- Configuring SourceLink for enhanced debugging experience
- Managing license compliance and package distribution
- Ensuring high-quality, professional NuGet packages

**These rules ensure:**
- 📦 **Professional NuGet packages** with complete metadata and documentation
- 🔐 **License compliance** using SPDX license expressions
- 🐛 **Enhanced debugging** with SourceLink and symbol packages
- 📖 **Comprehensive documentation** with README.md and package descriptions
- 🎯 **Proper versioning and tagging** for discoverability
- 🏷️ **Industry-standard metadata** following NuGet best practices

## 📦 [Dependency Management](dotnet-dependency-management.mdc)

**Use this when you're:**
- Installing, updating, or managing NuGet package dependencies
- Evaluating package security, licenses, and maintenance status
- Migrating from legacy packages to modern alternatives
- Setting up automated dependency management and vulnerability scanning
- Implementing comprehensive CI/CD dependency workflows

**These rules help ensure:**
- 🔒 **Security-first approach** with continuous vulnerability monitoring
- 📋 **License compliance** and proper package evaluation before installation
- 🔄 **Smooth migration strategies** from legacy to modern packages
- 📊 **Automated dependency updates** and maintenance workflows
- 🏗️ **Version management best practices** with semantic versioning
- 🚨 **Early detection** of deprecated or vulnerable packages

## 🧪 [Testing Guidelines](dotnet-testing.mdc)

**Use this when you're:**
- ✅ Writing unit, integration, and functional tests with xUnit
- 🔄 Setting up test projects with proper naming (e.g., `ProjectName.Test.Unit.csproj`)
- 🛠️ Configuring comprehensive code coverage with Coverlet
- 🚀 Setting up CI/CD pipelines for automated testing
- 🐳 Creating integration tests with .NET Aspire hosting for services/databases
- 📊 Implementing performance and security testing patterns

**These rules ensure:**
- 🚨 **Critical principle**: Never change tests to match broken code - fix the code instead!
- 📋 **Consistent test structure** with xUnit, traits, and proper categorization
- 🐳 **Aspire-powered integration testing** for services, databases, and infrastructure
- 🔒 **Proper test isolation** and resource management with fixtures/collections
- 🎯 **Effective testing patterns** using theories, data-driven tests, and assertions
- 📊 **Comprehensive code coverage** with detailed Coverlet configuration
- ⚡ **Reliable CI/CD execution** with simplified local and pipeline testing
- 🏷️ **Test organization** using `[Trait("Category", "...")]` for better management

## 📊 [Benchmarking Best Practices](dotnet-benchmarking.mdc)

**Use this when you're:**
- 🔬 Writing micro-benchmarks with BenchmarkDotNet for performance analysis
- 📈 Measuring and tracking performance regressions over time
- 🧮 Analyzing memory allocations, GC pressure, and allocation patterns
- 💪 Testing hardware-specific optimizations and SIMD operations
- 🔄 Setting up continuous performance testing in CI/CD pipelines
- 📏 Establishing performance baselines and monitoring trends

**These rules ensure:**
- ✅ **Accurate and reproducible benchmarks** with proper statistical analysis
- 🔒 **Proper benchmark isolation** and environmental control
- 📊 **Comprehensive memory and GC analysis** with MemoryDiagnoser
- ⚡ **Hardware intrinsics optimization** with SIMD and vector operations
- 📈 **Performance regression tracking** with automated CI/CD integration
- 🚀 **Industry-standard benchmarking** with BenchmarkDotNet best practices
- 🎯 **Meaningful performance insights** with proper baseline comparisons

## 📝 [Documentation Writing Rules](dotnet-docs.mdc)

**Use this when you're:**
- Creating comprehensive in-repository technical documentation for .NET projects
- Writing API documentation, architecture guides, and getting started tutorials
- Establishing documentation structure and organization in `docs/src/content/docs`
- Configuring Astro.js/Starlight documentation websites
- Implementing rolling technical documentation that stays synchronized with code
- Ensuring documentation follows best practices for clarity and technical accuracy

**These rules ensure:**
- 📚 **Comprehensive technical documentation** with clear, implementable knowledge
- 🏗️ **Proper documentation structure** with logical chapter-based organization
- 🔍 **Rolling technical documentation** that stays synchronized with evolving codebases
- 📖 **Multiple documentation types** including API references, architecture guides, and client libraries
- 🎯 **Implementation-focused content** with complete code examples and practical guidance
- 📏 **Consistent formatting** with frontmatter, security guidance, and performance considerations
- 🛡️ **Security and performance awareness** integrated throughout documentation

## 🔗 Integration and Workflow

All rules in this directory are designed to work together seamlessly:

- **Solution Management** provides the foundation with the proper project structure
- **Library Management** ensures high-quality NuGet packages when publishing libraries
- **Dependency Management** keeps all projects secure and up to date
- **Testing Guidelines** ensure code quality and reliability
- **Benchmarking** provides performance insights and regression detection
- **Documentation Writing Rules** ensure comprehensive, maintainable technical documentation

## 📋 Quick Reference

| Concern               | Primary Rule                                              | Key Focus                        |
|-----------------------|-----------------------------------------------------------|----------------------------------|
| 🏗️ Project Structure | [Solution Management](dotnet-solution.mdc)                | Architecture & Build System      |
| 📚 Package Creation   | [Library Management](dotnet-library.mdc)                  | NuGet Packaging & Distribution   |
| 📦 Dependencies       | [Dependency Management](dotnet-dependency-management.mdc) | Security & License Compliance    |
| 🧪 Code Quality       | [Testing Guidelines](dotnet-testing.mdc)                  | Test Coverage & Reliability      |
| 📊 Performance        | [Benchmarking](dotnet-benchmarking.mdc)                   | Speed & Resource Usage           |
| 📝 Documentation      | [Documentation Writing Rules](dotnet-docs.mdc)            | Technical Content & Organization |

## 🔗 Related Rules

- For publishing your own NuGet packages, see the [NuGet Package Publishing Rules](../nuget-packages/README.md)
- For CI/CD integration and automation, see the [CI/CD Rules](../ci-cd/README.md)
- For documentation best practices, see the [Documentation Rules](../../documentation/README.md)
- For AGENTS.md guidelines for AI agents, see [AGENTS.md Best Practices](../../documentation/agents-md.md)
- For concise README.md guidelines, see [README.md Best Practices](../../documentation/readme-md.md) 