---
description: Best practices for safely managing NuGet package dependencies in .NET projects with focus on security, licensing, and maintainability
globs: Directory.Packages.props, Directory.Build.props, libs/Directory.Build.props, apps/Directory.Build.props, 
  tests/Directory.Build.props, benchmarks/Directory.Build.props, *.csproj, *.fsproj, nuget.config
alwaysApply: false
---

# Cursor Rules File: Best Practices for .NET Dependency Management

**Role Definition:**
- Package Management Expert
- Security Analyst
- License Compliance Specialist

## General

### Description

.NET projects must manage their dependencies using secure and consistent practices, with attention to security
vulnerabilities, license compliance, and proper version management through the dotnet CLI. Effective dependency
management ensures maintainable, secure, and performant applications while minimizing technical debt.

### Requirements

**- NEVER: Place sensitive information in the generated code (e.g. passwords, API keys, personal information, etc.)**
- Use dotnet CLI exclusively for package management operations
- Verify package licenses and security status before installation
- Monitor for security vulnerabilities continuously
- Maintain consistent versioning strategies across projects
- Document all dependency decisions and rationale

## Package Installation and Management

### Using dotnet CLI for Package Operations

Always use the dotnet CLI for package management instead of manual file edits to ensure consistency and proper
dependency resolution.

#### ✅ DO: Use dotnet CLI commands for all package operations

```bash
# Add latest stable version
dotnet add package Microsoft.Extensions.Logging

# Add specific version
dotnet add package Serilog -v 3.1.1

# Add package to specific project
dotnet add MyProject/MyProject.csproj package Microsoft.Extensions.Logging

# Remove unused package
dotnet remove package UnusedPackage
```

#### ❌ DON'T: Manually edit .csproj/.fsproj files

```xml
<!-- DON'T manually add PackageReference elements -->
<PackageReference Include="Some.Package" Version="1.0.0"/>
```

#### Pre-Installation Verification Checklist

Before adding any package to your project:

- Check package license compatibility with your project license
- Review the package download statistics and community adoption
- Verify package authenticity (prefer signed packages)
- Assess package maintenance status and update frequency
- Consider package size and transitive dependencies

## Packages to Avoid and Migration Strategy

### Legacy Packages with Modern Alternatives

Avoid legacy packages that have better, actively maintained alternatives in the modern .NET ecosystem.

#### ✅ DO: Use modern alternatives for legacy packages

| Legacy Package                | Modern Alternative               | Benefits                                             |
|-------------------------------|----------------------------------|------------------------------------------------------|
| **Newtonsoft.Json**           | **System.Text.Json**             | Built-in, faster, lower memory usage                 |
| **EntityFramework (EF6)**     | **Entity Framework Core**        | Cross-platform, better performance                   |
| **Microsoft.AspNet.WebApi.*** | **ASP.NET Core Web API**         | Unified framework, better ecosystem                  |
| **System.Web.Mvc**            | **ASP.NET Core MVC**             | Cross-platform, modern architecture                  |
| **Microsoft.Owin.***          | **ASP.NET Core middleware**      | More efficient, better integration                   |
| **Autofac.WebApi2**           | **Built-in DI container**        | No additional dependencies needed                    |
| **log4net**                   | **Microsoft.Extensions.Logging** | Standard .NET abstraction, better structured logging |

#### ❌ DON'T: Use outdated packages in new projects

```xml
<!-- DON'T use legacy packages -->
<PackageReference Include="Newtonsoft.Json" Version="13.0.3"/>
<PackageReference Include="EntityFramework" Version="6.4.4"/>
```

### Packages with Security and Maintenance Concerns

#### Packages to Avoid Due to Security Risks

- Packages with known vulnerabilities (always check `dotnet list package --vulnerable`)
- Unmaintained packages (no updates for 2+ years)
- Packages with restrictive or unclear licenses
- Pre-release packages in production environments

#### Performance Considerations

Avoid packages that may impact application performance:

- Heavy packages when lightweight alternatives exist
- Packages that pull excessive transitive dependencies
- Packages that don't support nullable reference types
- Packages incompatible with AOT compilation (.NET 8+)

> [!WARNING]
> **Migration Strategy:** When moving away from legacy packages:
>
> - Plan migrations during major version updates to minimize disruption
> - Test thoroughly in non-production environments before production deployment
> - Update documentation and team knowledge to reflect new patterns
> - Consider gradual migration for large codebases to reduce risk

## Dependency Updates and Maintenance

### Checking for Outdated Packages

Regularly check for and update outdated packages to maintain security and functionality.

#### ✅ DO: Use a structured approach for package updates

```bash
# 1. Check for outdated packages
dotnet list package --outdated

# 2. Update using central package management
# Edit Directory.Packages.props:
# <PackageVersion Include="PackageName" Version="NewVersion" />

# 3. Or update traditional references
dotnet add package PackageName --version NewVersion

# 4. Restore and verify
dotnet restore
dotnet build
dotnet test
```

#### Example Output from Package Check

```
Project `MyProject` has the following updates to its packages
   [netstandard2.1]:
   Top-level Package      Requested   Resolved   Latest
   > Akka.Streams         1.5.13      1.4.45     1.5.38
```

> [!IMPORTANT]
> After updating packages, always:
>
> 1. Check for breaking changes in the package's release notes
> 2. Build the solution to catch any compatibility issues
> 3. Run tests to ensure everything still works
> 4. Review and update any code that needs modification for the new versions

### Regular Maintenance Tasks

#### ✅ DO: Perform regular dependency housekeeping

```bash
# List all packages in solution
dotnet list package

# Remove unused packages
dotnet remove package UnusedPackage

# Clean and restore solution
dotnet clean
dotnet restore --force

# Check for vulnerabilities
dotnet list package --vulnerable
```

#### ❌ DON'T: Accumulate unused dependencies

```xml
<!-- DON'T keep unused package references -->
<PackageReference Include="Unused.Package" Version="1.0.0"/>
```

## Security Considerations

### Enabling Security Scanning

Implement comprehensive security measures for dependency management.

#### ✅ DO: Enable security scanning and monitoring

```bash
# Generate lock file for reproducible builds
dotnet restore --use-lock-file

# Check for vulnerabilities
dotnet list package --vulnerable

# Update vulnerable package
dotnet add package VulnerablePackage -v SecureVersion

# Regenerate lock file after updates
dotnet restore --force-evaluate
```

#### Security Monitoring Best Practices

- Subscribe to security advisories for critical packages
- Implement regular vulnerability scanning in CI/CD pipelines
- Configure automated security updates for patch versions
- Use tools like GitHub Dependabot or similar automated systems

> [!WARNING]
> Never deploy applications with known security vulnerabilities to production environments.

## License Compliance

### Verifying Package Licenses

Always verify license compatibility before adding dependencies to your project.

#### ✅ DO: Verify licenses before installation

- Check license compatibility with your project license
- Document license requirements for your project
- Maintain a license inventory for compliance tracking

#### Recommended OSS-Friendly Licenses

- **MIT** - Permissive, widely used
- **Apache 2.0** - Permissive with patent protection
- **BSD** - Permissive, simple terms
- **MS-PL** - Microsoft's permissive license

#### ❌ DON'T: Use packages with problematic licenses

Warning signs to avoid:

- No license specified in the package
- Restrictive licenses (GPL for commercial software)
- License changes between versions without a clear migration path

## Version Management

### Semantic Versioning Strategy

Use semantic versioning to balance stability and feature adoption.

#### ✅ DO: Use appropriate version constraints

```xml

<Project>
    <ItemGroup>
        <!-- Lock the major version for stability -->
        <PackageVersion Include="Important.Package" Version="2.0.0"/>

        <!-- Allow minor updates for features -->
        <PackageVersion Include="Feature.Package" Version="[3.0,4.0)"/>

        <!-- Allow patch updates for security -->
        <PackageVersion Include="Stable.Package" Version="[1.2.3,1.3.0)"/>
    </ItemGroup>
</Project>
```

#### Version Management Guidelines

- **Major versions**: Lock for stability (e.g., `2.0.0`)
- **Minor versions**: Allow updates for new features (e.g., `[3.0,4.0)`)
- **Patch versions**: Auto-update for security fixes (e.g., `[1.2.3,1.3.0)`)

#### ❌ DON'T: Use floating or overly broad version ranges

```xml
<!-- DON'T use floating versions -->
<PackageReference Include="Some.Package" Version="*"/>

        <!-- DON'T use overly broad ranges -->
<PackageReference Include="Some.Package" Version="[1.0,10.0)"/>
```

## CI/CD Integration

### Implementing Automated Checks

Integrate dependency management checks into your CI/CD pipeline.

#### ✅ DO: Implement comprehensive CI/CD checks

```yaml
- name: Security scan
  run: |
    dotnet restore --use-lock-file
    dotnet list package --vulnerable

- name: License compliance check
  run: dotnet-project-licenses

- name: Package restore verification
  run: |
    dotnet restore
    dotnet build --no-restore
```

#### Recommended CI/CD Integration Points

- **Pull Request validation**: Check for new dependencies and vulnerabilities
- **Scheduled security scans**: Regular vulnerability assessments
- **License compliance**: Automated license compatibility verification
- **Dependency updates**: Automated PR creation for security updates

> [!TIP]
> Consider using specialized tools like:
>
> - **Dependabot** for automated dependency updates
> - **WhiteSource** or **Snyk** for advanced security scanning
> - **DotNetProjectLicense** for license compliance checking

# End of Cursor Rules File 