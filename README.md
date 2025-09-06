![Six Cursor Rules](assets/six-cursor-rules-banner-wide-1980-01.png)

# Six Cursor Rules

A comprehensive collection of Cursor AI assistant rules designed to enhance software development quality, consistency,
and productivity across multiple technology stacks. This project provides structured guidelines covering:

- **Technical documentation** - Standards for writing and maintaining project documentation
- **.NET ecosystem** - ASP.NET, Avalonia, Blazor, CI/CD, and related technologies
- **C# development** - Language-specific coding standards and best practices
- **Web technologies** - Astro, TypeScript, and modern web development frameworks
- **Specification-driven development** - Structured approaches to requirements and implementation
- **General software engineering practices** - Coding standards, testing, benchmarking, dependency management, and package publishing

## What are Cursor Rules?

Cursor Rules are configuration files (`.mdc` format) that guide AI assistants in Cursor IDE to follow specific
development practices, coding standards, and workflow patterns. These rules ensure consistent code quality, proper
documentation, and adherence to industry best practices across different domains and technologies.

## Installation

To use these Cursor Rules in your project:

1. **Copy the rules** - Copy the desired `.mdc` rule files from this repository to your project's `.cursor/rules/`
   directory
2. **Create the directory** - If the `.cursor/rules/` directory doesn't exist, create it in your solution root
3. **Restart Cursor** - Restart Cursor IDE to ensure the new rules are loaded
4. **Verify activation** - The AI assistant will automatically apply these rules when working in your project

**Directory Structure:**

```
YourSolution/
├── .cursor/
│   └── rules/
│       ├── coding-by-spec.mdc
│       ├── coding-style.mdc
│       ├── consuming-dotnettool.mdc
│       ├── meta.mdc
│       ├── nuget-package-publishing.mdc
│       └── publishing-dotnettool.mdc
├── src/
├── tests/
└── README.md
```

## Available Rules

### .NET Development

#### 🔧 **General .NET**
##### [Solution Management](src/dotnet/general/dotnet-solution.mdc)
Comprehensive guidelines for maintaining .NET solutions with consistent SDK versions, shared metadata, and secure package sources. Ensures cross-project consistency, maintainability, and security using Six.SolutionTemplate as the foundation.

##### [Library Development](src/dotnet/general/dotnet-library.mdc)
Best practices for creating high-quality .NET libraries with proper metadata, SourceLink integration, and NuGet publishing. Focuses on package discoverability, documentation, and following NuGet conventions for maximum ecosystem compatibility.

##### [Dependency Management](src/dotnet/general/dotnet-dependency-management.mdc)
Guidelines for managing NuGet dependencies with security focus, version control, and migration strategies. Addresses package vulnerabilities, license compliance, and maintaining healthy dependency graphs across .NET projects.

##### [Testing Best Practices](src/dotnet/general/dotnet-testing.mdc)
Comprehensive testing strategies including unit, integration, and performance testing with xUnit, BenchmarkDotNet, and Coverlet. Emphasizes test isolation, Arrange-Act-Assert patterns, and automated testing workflows.

##### [Benchmarking Guidelines](src/dotnet/general/dotnet-benchmarking.mdc)
Performance benchmarking best practices using BenchmarkDotNet for measuring and optimizing .NET code performance. Covers memory analysis, async performance testing, and CI/CD integration for continuous performance monitoring.

##### [Documentation Standards](src/dotnet/general/dotnet-docs.mdc)
Standards for writing technical documentation in `docs/src/content/docs/` with proper structure and formatting. Ensures consistent documentation quality and discoverability across .NET projects.


#### 💻 **C#**
##### [C# Coding Style](src/dotnet/csharp/csharp-coding-style.mdc)
Guidelines for writing clean, maintainable, and idiomatic C# code with functional patterns and modern language features. Emphasizes readability, proper abstractions, and leveraging C# 13+ features for optimal code quality.

##### [C# Scripting](src/dotnet/csharp/csharp-scripting-style.mdc)
Best practices for C# scripting with .NET, including single-file applications, package directives, and rich console output. Covers Spectre.Console for CLI applications and CliWrap for external process management.



#### 🏗️ **ASP.NET Core Development**
##### [ASP.NET Core API](src/dotnet/aspnet/aspnet-api.mdc)
Best practices for building secure, scalable ASP.NET Core APIs using Minimal APIs and modern patterns. Covers dependency injection, error handling, security, performance optimization, and comprehensive API documentation.

##### [Fast Endpoints API](src/dotnet/aspnet/aspnet-api-fast-endpoints.mdc)
Guidelines for building high-performance ASP.NET Core APIs using Fast Endpoints framework. Focuses on endpoint architecture, validation, security, performance optimization, and comprehensive testing strategies.

##### [Orleans Development](src/dotnet/aspnet/aspnet-orleans.mdc)
Best practices for developing distributed applications using Microsoft Orleans framework. Covers grain development, clustering, persistence, and integration with .NET Aspire for local orchestration.

#### 🎨 **UI & Desktop Development**
##### [Avalonia MVVM](src/dotnet/avalonia/avalonia-mvvm.mdc)
Comprehensive guidelines for building cross-platform desktop applications using Avalonia UI with MVVM architecture. Covers reactive programming with ReactiveUI, XAML best practices, and cross-platform compatibility for Windows, macOS, and Linux.

##### [Blazor Development](src/dotnet/blazor/blazor-coding-style.mdc)
Best practices for building modern web applications with Blazor, covering component architecture, data binding, performance optimization, and integration with ASP.NET Core APIs for full-stack development.




#### 🔄 **CI/CD & DevOps**
##### [.NET Build System](src/dotnet/ci-cd/dotnet-gh-workflow-build.mdc)
Comprehensive best practices for .NET build systems, CI/CD pipelines, and automated processes. Covers cross-platform builds, version management, testing integration, and both GitHub Actions and Azure DevOps pipelines.

##### [Code Signing](src/dotnet/ci-cd/dotnet-gh-workflow-code-signing.mdc)
Security practices for .NET code signing with SignClient integration in CI/CD pipelines. Ensures authenticity, integrity, and compliance through automated signing workflows and certificate management.

##### [NuGet Publishing](src/dotnet/ci-cd/dotnet-gh-workflow-nuget-publishing.mdc)
Best practices for publishing high-quality NuGet packages with proper versioning, metadata, and distribution. Covers semantic versioning, package validation, security, and automated publishing workflows.



### 🌐 **Web Technologies**
#### [TypeScript Development](src/web/typescript/typescript-coding-style.mdc)
Guidelines for writing type-safe, maintainable TypeScript code with modern JavaScript features and functional patterns. Focuses on proper typing, performance optimization, and scalable architecture for web applications.

#### [Astro Framework](src/web/astro/astro-coding-style.mdc)
Best practices for building fast, content-focused websites using Astro framework. Covers component development, performance optimization, and integration with modern web technologies for optimal user experience.



### 📋 **Specification Driven Development**
#### [Specification-Driven Development](src/specs-generation/coding-by-spec.mdc)
Cursor-based approach that simulates specification-driven development similar to Kiro, enabling structured software development through AI-assisted specification files. Uses EARS notation for clear requirements, technical design documentation, and iterative refinement processes to bridge the gap between requirements and implementation.


### 📋 **Technical Documentation**
#### [README Documentation](src/documentation/readme-md.mdc)
Standards for writing comprehensive README files with proper structure, formatting, and content organization. Ensures consistent documentation quality across all projects and repositories.

#### [AI Agents Documentation](src/documentation/agents-md.mdc)
Guidelines for documenting AI agents and their capabilities within development projects. Ensures clear communication of AI-assisted workflows and automated processes.



## Writing New Rules Using .meta.mdc

### What is `.meta.mdc`?

`.meta.mdc` represents the **meta-framework** and **standardized structure** used to create consistent, high-quality Cursor rules across the Six Cursor Rules collection. 

It's not a single file, but rather a **methodology and template** that ensures all rules follow the same proven structure for optimal AI assistant guidance.

### Why Use the `.meta.mdc` Approach?

The `.meta.mdc` methodology provides several key benefits:

- **🔄 Consistency**: All rules follow the same structure, making them predictable and easy to understand
- **🎯 Clarity**: Clear role definitions and requirements help AI assistants understand their scope
- **📝 Documentation**: Standardized format ensures comprehensive coverage of topics
- **🔧 Maintainability**: Common structure makes rules easier to update and maintain
- **🎨 Best Practices**: Incorporates proven patterns for effective AI-assisted development

### How to Write New Rules Using `.meta.mdc`

#### 1. **Front Matter Structure**
Every rule starts with standardized front matter:

```yaml
---
description: Brief description of what this rule covers
globs: file-patterns-where-rule-applies.md,*.cs,*.ts
alwaysApply: false  # Set to true for universal rules
---
```

#### 2. **Title Format**
Use the standardized title format:
```markdown
# Cursor Rules File: [Descriptive Rule Name]
```

#### 3. **Role Definition Section**
Define the AI assistant's roles and expertise areas:

```markdown
**Role Definition:**
- Primary Expert Role
- Secondary Expert Role
- Specialized Skill Area
```

#### 4. **General Section Structure**
Include these essential subsections:

```markdown
## General

### Description
Clear explanation of the rule's purpose and scope.

### Requirements
- **NEVER**: Critical restrictions (e.g., security, sensitive data)
- Specific technical requirements
- Best practices to follow
- Tools or frameworks to use
```

#### 5. **Domain-Specific Sections**
Organize content into logical sections based on the domain:

```markdown
## [Domain Area 1]
Content specific to the first major area...

## [Domain Area 2]
Content for the second major area...

### [Subsection]
Detailed guidance within a domain area...
```

#### 6. **DO/DON'T Patterns**
Use clear success/failure patterns:

```markdown
### ✅ DO: Recommended Approach
```code
// Good example
```

### ❌ DON'T: Avoid This Pattern
```code
// Bad example to avoid
```
```

#### 7. **File Organization**
Place new rules in appropriate directories:

```
src/
├── dotnet/           # .NET ecosystem rules
├── web/             # Web technologies
├── specs-generation/# Specification rules
└── documentation/   # Documentation standards
```

### Best Practices for Rule Creation

#### **Content Guidelines**
- **Be Specific**: Use concrete examples and actionable guidance
- **Include Context**: Explain why practices matter, not just what to do
- **Progressive Disclosure**: Start with basics, then advanced concepts
- **Cross-References**: Link to related rules when relevant

#### **Technical Requirements**
- **File Extensions**: Use `.mdc` for Cursor rule files
- **Glob Patterns**: Define specific file patterns for rule activation
- **Security First**: Include security considerations in requirements
- **Testing Focus**: Emphasize testing and validation practices

#### **Documentation Standards**
- **Clear Descriptions**: Front matter descriptions should be concise but informative
- **Comprehensive Coverage**: Address common scenarios and edge cases
- **Regular Updates**: Keep rules current with evolving best practices

### Example Rule Template

```markdown
---
description: Guidelines for [specific technology/practice]
globs: *.extension, path/patterns/**
alwaysApply: false
---

# Cursor Rules File: [Technology/Practice] Best Practices

**Role Definition:**
- Primary Technology Expert
- Best Practices Specialist
- Implementation Guide

## General

### Description
Brief overview of what this rule covers and why it matters.

### Requirements
- **NEVER**: Critical restrictions
- Key technical requirements
- Best practices to follow

## [Main Topic Area]
Detailed guidance for the primary focus area...

## [Secondary Topic Area]
Additional guidance for related concepts...

## Special Thanks
```

### Rule Validation Checklist

Before finalizing a new rule, ensure it includes:

- [ ] Clear, descriptive front matter
- [ ] Well-defined role definitions
- [ ] Comprehensive requirements section
- [ ] Practical examples and code snippets
- [ ] DO/DON'T patterns for clarity
- [ ] Cross-references to related rules
- [ ] Security considerations
- [ ] Testing guidance
- [ ] Proper file organization

This `.meta.mdc` approach ensures that all Six Cursor Rules maintain high quality, consistency, and effectiveness in guiding AI-assisted development across the entire technology stack.

## Special Thanks

**Some of these rules are derived from:**
[.NET Cursor Rules by Aaronontheweb](https://github.com/Aaronontheweb/dotnet-cursor-rules)
[Cursor Official Directory](https://cursor.directory/rules)

## Related Resources

- [Cursor IDE Documentation](https://cursor.sh/docs)
- [C# Coding Standards](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [NuGet Package Publishing](https://docs.microsoft.com/en-us/nuget/create-packages/overview-and-workflow)
- [Semantic Versioning](https://semver.org/)
- [SPDX License List](https://spdx.org/licenses/)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
