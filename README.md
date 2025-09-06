![Six Cursor Rules](assets/six-cursor-rules-banner-wide-1980-01.png)

# Six Cursor Rules

A comprehensive collection of Cursor AI assistant rules designed to enhance software development quality, consistency,
and productivity. This project provides structured guidelines for various aspects of software development, including
coding standards, specification writing, package publishing, and tool management.

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
