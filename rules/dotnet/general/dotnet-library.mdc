---
description: This document provides comprehensive guidance for publishing high-quality NuGet packages that follow industry best practices.
globs: libs/Directory.Build.props, libs/Directory.Build.targets, libs/*.props, libs/*.targets, libs/*.csproj, libs/*.fsproj, libs/*/README.md, libs/*/project_icon.png
alwaysApply: false
---

# Cursor Rules File: Creating and Maintaining .NET library projects

**Role Definition:**
- .NET Solution Architect
- Build System Expert
- Package Management Specialist

## General

### Description
This document provides comprehensive guidance for creating and maintaining high-quality c# library projects
(.csproj with `library` type) that follow industry best practices.

The c# projects of `library` type are all automatically prepared for NuGet packaging by providing all
necessary metadata that packaging needs, that follow industry best practices.

Uses [Six.SolutionTemplate](https://github.com/six-tech/Six.SolutionTemplate) a GitHub template for .NET solutions.

**Six.SolutionTemplate** provides a solid foundation for maintainable .NET solutions with consistent SDK versions,
shared metadata, AI IDEs integration (Cursor and Kiro), embedded documentation using `Astro.js` and `Starlight`, and
more.

Full documentation: [Six.SolutionTemplate](https://six-tech.github.io/Six.SolutionTemplate/)

### Requirements

**- NEVER: Place sensitive information in the generated code (e.g. passwords, API keys, personal information, etc.)**
- Use SPDX license expressions for proper license declaration
- Configure comprehensive package metadata for discoverability
- Enable SourceLink for enhanced debugging experience
- Maintain detailed README.md documentation
- Follow NuGet packaging best practices and conventions
- Ensure proper versioning and package tagging
- Configure symbol packages for debugging support

## License Configuration

### Use License Expressions

Always use SPDX license expressions instead of deprecated license URLs or embedding license files in your package.

#### ✅ DO: Use license expression

```xml

<PropertyGroup>
    <PackageLicenseExpression>MIT</PackageLicenseExpression>
</PropertyGroup>
```

#### ❌ DON'T: Use deprecated licenseUrl

```xml

<PropertyGroup>
    <PackageLicenseUrl>https://licenses.nuget.org/MIT</PackageLicenseUrl>
</PropertyGroup>
```

### Common License Expression Examples

#### MIT License

```xml

<PropertyGroup>
    <PackageLicenseExpression>MIT</PackageLicenseExpression>
</PropertyGroup>
```

#### Apache 2.0 License

```xml

<PropertyGroup>
    <PackageLicenseExpression>Apache-2.0</PackageLicenseExpression>
</PropertyGroup>
```

#### BSD 3-Clause License

```xml

<PropertyGroup>
    <PackageLicenseExpression>BSD-3-Clause</PackageLicenseExpression>
</PropertyGroup>
```

#### GPL v3 License

```xml

<PropertyGroup>
    <PackageLicenseExpression>GPL-3.0-only</PackageLicenseExpression>
</PropertyGroup>
```

#### Multiple Licenses (OR)

```xml

<PropertyGroup>
    <PackageLicenseExpression>MIT OR Apache-2.0</PackageLicenseExpression>
</PropertyGroup>
```

## Package Documentation

### Include README.md

- Always include a README.md file in your package to provide clear documentation for users.
- Use `../../documentation/readme-md.mdc` for guidance rules on writing effective README files.

#### ✅ DO: Include README with proper configuration

```xml

<PropertyGroup>
    <PackageReadmeFile>README.md</PackageReadmeFile>
</PropertyGroup>
<ItemGroup>
<None Include="README.md" Pack="true" PackagePath="/"/>
</ItemGroup>
```

#### ✅ DO: Include README from a different location

```xml

<PropertyGroup>
    <PackageReadmeFile>README.md</PackageReadmeFile>
</PropertyGroup>
<ItemGroup>
<None Include="docs/README.md" Pack="true" PackagePath="/"/>
</ItemGroup>
```

#### ❌ DON'T: Forget to include the README in the package

```xml

<PropertyGroup>
    <PackageReadmeFile>README.md</PackageReadmeFile>
    <!-- Missing the ItemGroup that includes the file -->
</PropertyGroup>
```

## Metadata Organization

### Directory.Build.props for Common Metadata

Use library `Directory.Build.props` file in `libs/Directory.Build.props` for common metadata shared across multiple 
libraries in a solution.

#### ✅ DO: Place common metadata in library Directory.Build.props (`libs/Directory.Build.props`)

```xml

<Project>
    <PropertyGroup>
        <TargetFramework>net9.0</TargetFramework>

        <!-- Version -->
        <Version>1.0.0-alpha.1</Version>

        <!-- Packaging -->
        <IsPackable>true</IsPackable>
        <SignAssembly>true</SignAssembly>
        <GeneratePackageOnBuild>false</GeneratePackageOnBuild>

        <!-- Company/Organization/Legal Information -->
        <Authors>Six Technology</Authors>
        <Company>Six Technology</Company>
        <Description>Awesome library by Six</Description>
        <PackageTags>six;awesome;library</PackageTags>
        <PackageLicenseExpression>MIT</PackageLicenseExpression>
        <PackageRequireLicenseAcceptance>false</PackageRequireLicenseAcceptance>
        <Copyright>© $([System.DateTime]::Now.Year) Six Technology, Inc. All rights reserved.</Copyright>

        <!-- Repository Information -->
        <PackageProjectUrl>https://your-project-url.com</PackageProjectUrl>
        <RepositoryUrl>https://github.com/library-repository-url.com</RepositoryUrl>
        <PublishRepositoryUrl>true</PublishRepositoryUrl>
        <RepositoryType>git</RepositoryType>

        <!-- Embedded Assets -->
        <PackageReadmeFile>README.md</PackageReadmeFile>
        <PackageIcon>$(MSBuildThisFileDirectory)\project_icon.png</PackageIcon>
    </PropertyGroup>
</Project>
```

### Project-Specific Metadata

Keep package-specific metadata in the project file (`.csproj`, `.fsproj`).

#### ✅ DO: Place package-specific metadata in the project file (new or override common metadata)

```xml

<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>net9.0</TargetFramework>

        <!-- Package-specific metadata -->
        <PackageId>Contoso.AwesomeLibrary.Core</PackageId>
        <Version>1.2.3</Version>
        <Description>A core library for doing awesome things efficiently and reliably.</Description>
        <PackageTags>awesome;library;performance;utilities</PackageTags>

        <!-- Package-specific configuration -->
        <GenerateDocumentationFile>true</GenerateDocumentationFile>
    </PropertyGroup>
</Project>
```

#### ❌ DON'T: Duplicate common metadata in project files

```xml

<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>net9.0</TargetFramework>

        <!-- DON'T duplicate these in every project file -->
        <Authors>Contoso, Inc.</Authors>
        <Company>Contoso, Inc.</Company>
        <Copyright>© 2023 Contoso, Inc. All rights reserved.</Copyright>
        <PackageLicenseExpression>MIT</PackageLicenseExpression>
        <PackageProjectUrl>https://github.com/contoso/awesome-library</PackageProjectUrl>

        <!-- Package-specific metadata -->
        <PackageId>Contoso.AwesomeLibrary.Core</PackageId>
        <Version>1.2.3</Version>
        <Description>A core library for doing awesome things efficiently and reliably.</Description>
        <PackageTags>awesome;library;performance;utilities</PackageTags>
    </PropertyGroup>
</Project>
```

## Source Debugging Support

### Enable SourceLink

SourceLink enables step-through debugging of your package's source code directly from NuGet packages.

#### ✅ DO: Configure SourceLink for GitHub

```xml

<PropertyGroup>
    <PublishRepositoryUrl>true</PublishRepositoryUrl>
    <EmbedUntrackedSources>true</EmbedUntrackedSources>
    <IncludeSymbols>true</IncludeSymbols>
    <SymbolPackageFormat>snupkg</SymbolPackageFormat>
    <!-- Recommended for deterministic builds in CI -->
    <ContinuousIntegrationBuild Condition="'$(CI)' == 'true'">true</ContinuousIntegrationBuild>
</PropertyGroup>

<ItemGroup>
<PackageReference Include="Microsoft.SourceLink.GitHub" Version="1.1.1" PrivateAssets="All"/>
</ItemGroup>
```

#### ✅ DO: Configure SourceLink for Azure DevOps

```xml

<PropertyGroup>
    <PublishRepositoryUrl>true</PublishRepositoryUrl>
    <EmbedUntrackedSources>true</EmbedUntrackedSources>
    <IncludeSymbols>true</IncludeSymbols>
    <SymbolPackageFormat>snupkg</SymbolPackageFormat>
</PropertyGroup>

<ItemGroup>
<PackageReference Include="Microsoft.SourceLink.AzureRepos.Git" Version="1.1.1" PrivateAssets="All"/>
</ItemGroup>
```

#### ✅ DO: Configure SourceLink for GitLab

```xml

<PropertyGroup>
    <PublishRepositoryUrl>true</PublishRepositoryUrl>
    <EmbedUntrackedSources>true</EmbedUntrackedSources>
    <IncludeSymbols>true</IncludeSymbols>
    <SymbolPackageFormat>snupkg</SymbolPackageFormat>
</PropertyGroup>

<ItemGroup>
<PackageReference Include="Microsoft.SourceLink.GitLab" Version="1.1.1" PrivateAssets="All"/>
</ItemGroup>
```

#### ❌ DON'T: Publish packages without symbol support

```xml

<PropertyGroup>
    <!-- Missing SourceLink and symbol configuration -->
    <Version>1.0.0</Version>
</PropertyGroup>
```

### Symbol Packages

Always include symbol packages (`.snupkg`)

#### ✅ DO: Configure symbol package generation

```xml

<PropertyGroup>
    <IncludeSymbols>true</IncludeSymbols>
    <SymbolPackageFormat>snupkg</SymbolPackageFormat>
</PropertyGroup>
```