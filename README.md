![Six Cursor Rules](assets/six-cursor-rules-banner-wide-1980-01.png)

# Six Cursor Rules

A comprehensive collection of Cursor AI assistant rules designed to enhance software development quality, consistency, and productivity. This project provides structured guidelines for various aspects of software development including coding standards, specification writing, package publishing, and tool management.

## What are Cursor Rules?

Cursor Rules are configuration files (`.mdc` format) that guide AI assistants in Cursor IDE to follow specific development practices, coding standards, and workflow patterns. These rules ensure consistent code quality, proper documentation, and adherence to industry best practices across different domains and technologies.

## Available Rules

### 📋 [Coding by Specification](.cursor/rules/coding-by-spec.mdc)

**Purpose**: Establishes a structured approach to writing software specifications and implementing code that adheres to those specifications.

**Key Features**:
- **Requirements Phase**: Captures user stories and acceptance criteria using EARS (Easy Approach to Requirements' Syntax) notation
- **Design Phase**: Documents technical architecture, sequence diagrams, and implementation considerations
- **Implementation Planning**: Creates detailed task breakdowns with clear descriptions and outcomes
- **Execution Tracking**: Provides real-time status updates for task completion

**Example EARS Notation**:
```
WHEN a user submits a form with invalid data
THE SYSTEM SHALL display validation errors next to the relevant fields
```

### 🎨 [Coding Style](.cursor/rules/coding-style.mdc)

**Purpose**: Defines comprehensive guidelines for writing clean, maintainable, and idiomatic C# code with a focus on functional patterns and modern C# features.

**Key Areas**:
- **Type Definitions**: Prefer records for data types, seal classes by default, use value objects to avoid primitive obsession
- **Functional Patterns**: Effective use of pattern matching, pure methods, and immutable collections
- **Code Organization**: Separate state from behavior, appropriate use of extension methods
- **Error Handling**: Result types for expected failures, exceptions for exceptional cases
- **Async Programming**: Proper use of async/await, cancellation tokens, and avoiding common pitfalls
- **Nullability**: Enable nullable reference types, proper null checking, and nullability attributes

**Example Patterns**:
- Modern pattern matching with switch expressions
- Pure functions for predictable behavior
- Immutable collections with `System.Collections.Immutable`
- Proper async/await with cancellation support

### 🔧 [Consuming dotnet Tool](.cursor/rules/consuming-dotnettool.mdc)

**Purpose**: Best practices for managing and organizing dotnet tool instances through tool manifests for consistent development environments.

**Key Features**:
- **Tool Manifest Management**: Maintain `.config/dotnet-tools.json` for reproducible builds
- **Version Control**: Explicit versioning to prevent unexpected updates
- **CI/CD Integration**: Automated tool restoration in build pipelines
- **Documentation**: Clear instructions for tool usage and troubleshooting

**Workflow**:
1. Initialize tool manifest with `dotnet new tool-manifest`
2. Install tools with explicit versions
3. Restore tools in CI/CD pipelines
4. Regularly audit and update tool versions

### 📝 [Meta Rules](.cursor/rules/meta.mdc)

**Purpose**: Meta-rules for maintaining and updating `.mdc` rule files themselves, ensuring consistency across the rule ecosystem.

**Key Guidelines**:
- **Line Ending Preservation**: Maintain existing CRLF/LF formatting
- **Whitespace Handling**: Preserve trailing whitespace and indentation styles
- **File Organization**: Logical grouping of content within rule files
- **Documentation Standards**: Clear examples with "do" and "don't" patterns

**File Creation Criteria**:
- Only create new rule files when content doesn't fit existing files
- Document rationale for new file creation
- Maintain consistent formatting within MDC files

### 📦 [NuGet Package Publishing](.cursor/rules/nuget-package-publishing.mdc)

**Purpose**: Comprehensive guide for publishing high-quality NuGet packages that follow industry best practices and ensure excellent developer experience.

**Key Areas**:
- **License Configuration**: Use SPDX license expressions instead of deprecated license URLs
- **Package Documentation**: Include comprehensive README.md with usage examples
- **Metadata Organization**: Centralized metadata in `Directory.Build.props`
- **Source Debugging**: Enable SourceLink for step-through debugging
- **Versioning**: Follow semantic versioning (SemVer) principles
- **Quality Assurance**: Pre-publish checklists and automated validation

**Example Configurations**:
- Proper license expressions: `MIT`, `Apache-2.0`, `BSD-3-Clause`
- SourceLink setup for GitHub, Azure DevOps, and GitLab
- Symbol package generation (`.snupkg`) for debugging support
- Automated CI/CD validation pipelines

### 🛠️ [Publishing dotnet Tool](.cursor/rules/publishing-dotnettool.mdc)

**Purpose**: Guidelines for creating, packaging, and publishing dotnet tools as NuGet packages, ensuring they work across different .NET versions and environments.

**Key Requirements**:
- **Project Configuration**: Use `<PackAsTool>true</PackAsTool>` in project files
- **Target Framework**: Target latest LTS version (.NET 8) with `<RollForward>LatestMajor</RollForward>`
- **Documentation**: Comprehensive README with installation and usage instructions
- **Versioning**: Follow semantic versioning for tool updates
- **Testing**: Validate tool functionality in local install scenarios

**Publishing Workflow**:
1. Configure project with proper tool metadata
2. Create detailed README and inline help
3. Pack using `dotnet pack` command
4. Test local installation and functionality
5. Publish to NuGet.org or private feeds

## Installation & Usage

### For Individual Projects

1. Copy the desired `.mdc` rule files to your project's `.cursor/rules/` directory
2. Customize the rules as needed for your specific requirements
3. The Cursor AI assistant will automatically apply these rules when working in your project

### For Teams

1. Create a centralized repository of Cursor rules
2. Share the rules across team projects
3. Establish team-specific customizations
4. Regularly update rules based on team feedback and evolving best practices

## Contributing

We welcome contributions to improve and expand the Cursor Rules collection:

1. **New Rules**: Propose new rule files for specific technologies or domains
2. **Rule Improvements**: Enhance existing rules with better examples or additional guidelines
3. **Documentation**: Improve rule documentation and examples
4. **Best Practices**: Share successful implementations and patterns

### Contribution Guidelines

- Follow the existing rule structure and formatting
- Include comprehensive examples with clear "do" and "don't" scenarios
- Ensure rules are technology-agnostic where possible
- Test rules in real-world scenarios before proposing
- Document the rationale and benefits of new rules

## Related Resources

- [Cursor IDE Documentation](https://cursor.sh/docs)
- [C# Coding Standards](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [NuGet Package Publishing](https://docs.microsoft.com/en-us/nuget/create-packages/overview-and-workflow)
- [Semantic Versioning](https://semver.org/)
- [SPDX License List](https://spdx.org/licenses/)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
