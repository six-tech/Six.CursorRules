# Documentation Best Practices

This directory contains comprehensive rules for creating effective documentation that enhances developer experience,
supports AI coding agents, and maintains high-quality project communication. Focus on clear, scannable content that
serves both human developers and AI assistants.

## 📝 [README.mdc Best Practices](readme-md.mdc)

**Use this when you're:**

- Creating concise README.md files for .NET projects and packages
- Optimizing documentation for both NuGet and GitHub platforms
- Establishing consistent formatting across all project READMEs
- Balancing brevity with essential information
- Directing developers to comprehensive documentation

**These rules ensure:**

- 🚀 **Quick introductions** that help developers get started in under 2 minutes
- 📚 **Strategic documentation links** directing to detailed docs in `docs/` directory
- 🎯 **Platform optimization** for NuGet package galleries and GitHub repositories
- 📏 **Concise content** (under 400 lines) focused on essential information
- 🔍 **Scannable structure** with clear headings and visual hierarchy
- 📊 **Consistent formatting** across all projects and packages

## 🤖 [AGENTS.mdc Best Practices](agents-md.mdc)

**Use this when you're:**

- Creating comprehensive AGENTS.md files for AI coding agents
- Extracting crucial information from all documentation sources in `docs/src/content/docs`
- Providing detailed technical context with runnable commands and working examples
- Documenting build processes, testing strategies, and coding conventions
- Supporting multiple AI agents (Cursor, GitHub Copilot, etc.) with one file
- Establishing agent-specific guidance that includes extracted documentation content

**These rules ensure:**

- 🔧 **Complete technical context** with runnable commands and specific instructions
- 📖 **CRITICAL EXTRACTION REQUIREMENT** - All crucial information from `docs/src/content/docs` must be extracted and
  included
- 🏗️ **Architecture guidance** covering patterns, conventions, and best practices
- 🧪 **Development workflows** with build, test, and deployment procedures
- 🔒 **Security considerations** and performance optimization techniques
- 🎯 **Agent-specific tips** tailored for different AI coding assistants
- 📋 **Comprehensive checklists** ensuring complete extraction from documentation sources

## 🔗 Integration and Workflow

All documentation rules in this directory work together to create a complete documentation ecosystem:

- **README.md Best Practices** provide quick introductions and entry points
- **AGENTS.md Best Practices** offer detailed technical guidance by EXTRACTING crucial information from
  `docs/src/content/docs`
- **Both formats** work together: README.md links to docs, AGENTS.md extracts and includes the content directly

## 📋 Quick Reference

| Concern                 | Primary Rule                              | Key Focus                                       |
|-------------------------|-------------------------------------------|-------------------------------------------------|
| 📖 Human Documentation  | [README.md Best Practices](readme-md.mdc) | Quick introductions & platform optimization     |
| 🤖 AI Agent Guidance    | [AGENTS.md Best Practices](agents-md.mdc) | EXTRACTED technical content & runnable commands |
| 📚 Source Documentation | `docs/src/content/docs`                   | Original detailed guides for extraction         |

## 🔗 Related Rules

- For .NET project documentation standards, see [Documentation Writing Rules](../dotnet/general/dotnet-docs.md)
- For NuGet package documentation, see [Library Management Rules](../dotnet/general/dotnet-library.md)
- For CI/CD documentation, see [CI/CD Rules](../dotnet/ci-cd/README.md)
- For general .NET project management, see [Project Management Rules](../dotnet/general/README.md)
