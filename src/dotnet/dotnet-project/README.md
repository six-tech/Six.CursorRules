# .NET SDK Management Rules

This directory contains rules for managing .NET solutions with modern practices, secure dependencies, and consistent build configurations. Focus on security, performance, and maintainability through proper SDK management and dependency selection.

## [Solution Management](dotnet-solution.mdc)

Use this when you're:
- Setting up a new .NET solution (prefer .slnx format)
- Implementing consistent build properties across projects
- Configuring secure multi-environment package sources
- Need to ensure consistent SDK versions across development environments

These rules help establish:
- Modern .slnx solution format over legacy .sln
- SDK version control via `global.json`
- Shared metadata via `Directory.Build.props` in the solution root directory
- Secure multi-environment package sources via `nuget.config` with environment variables


## [Library Management](dotnet-library.mdc)
Use this when you're:
- Setting up a new .NET library project
- TODO


## [Dependency Management](dotnet-dependency-management.mdc)

Use this when you're:
- Installing or updating NuGet packages
- Evaluating package security and licenses
- Migrating from legacy packages to modern alternatives
- Setting up automated dependency management
- Implementing dependency scanning in CI/CD

These rules help ensure:
- Safe and consistent package management via CLI
- Guidance on packages to avoid and their modern alternatives
- License compliance and security scanning
- Version management best practices
- Automated vulnerability detection


## 🧪 [Testing Guidelines](dotnet-testing.mdc)

Use these rules when:
- ✅ Writing unit, integration, and functional tests
- 🔄 Setting up test projects with proper naming describing the type of the tests (for example `ProjectName.Test.Unit.csproj`)
- 🛠️ Configuring comprehensive code coverage
- 🚀 Setting up CI/CD pipelines for testing
- 🐳 Creating integration tests with .NET Aspire hosting for testing

These rules ensure:
- 🚨 **Critical principle**: Never change tests to match broken code - fix the code instead!
- 📋 **Consistent test structure** with xUnit and trait categorization
- 🐳 **Aspire-exclusive integration testing** for services, databases, and infrastructure
- 🔒 **Proper test isolation** and resource management
- 🎯 **Effective use of fixtures, theories, and collections**
- 📊 **Comprehensive code coverage** with detailed coverlet configuration
- ⚡ **Reliable CI/CD execution** with simplified local and pipeline testing
- 🏷️ **Test categorization** using `[Trait("Category", "...")]` for organization 



## 📊 [Benchmarking](dotnet-benchmarking.mdc)

Use these rules when:
- 🔬 Writing micro-benchmarks with BenchmarkDotNet
- 📈 Measuring performance regressions
- 🧮 Analyzing memory allocations
- 💪 Testing hardware-specific optimizations
- 🔄 Setting up performance testing in CI/CD

These rules ensure:
- ✅ Accurate and reproducible benchmarks
- 🔒 Proper benchmark isolation
- 📊 Memory and GC analysis
- ⚡ Hardware intrinsics optimization
- 📈 Performance regression tracking
- 🚀 CI/CD integration for benchmarks 


## Related Rules

- For publishing your own NuGet packages, see the [NuGet Package Publishing Rules](../nuget-packages/README.md)
- For CI/CD integration, see the [CI/CD Rules](../ci-cd/README.md)
- For .NET tool management, see the [.NET Tool Rules](../dotnet-tools/README.md) 