![Six Cursor Rules](assets/six-cursor-rules-banner-wide-1980-01.png)

# Six Cursor Rules

A comprehensive collection of Cursor AI assistant rules designed to enhance software development quality, consistency, and productivity. This project provides structured guidelines for various aspects of software development including coding standards, specification writing, package publishing, and tool management.

## What are Cursor Rules?

Cursor Rules are configuration files (`.mdc` format) that guide AI assistants in Cursor IDE to follow specific development practices, coding standards, and workflow patterns. These rules ensure consistent code quality, proper documentation, and adherence to industry best practices across different domains and technologies.

## Available Rules

### 📋 [Coding by Specification](.cursor/rules/coding-by-spec.mdc)

**Purpose**: Establishes a structured specification-driven development workflow that generates detailed spec files and ensures code implementation adheres to those specifications.
This is **similar approach as Amazon's Kiro** does but manually with Cursor.

**Generated Files Structure**:
```
.cursor/specs/
└── {feature-name}/
    ├── requirements.md
    ├── design.md
    └── tasks.md
```

#### 📝 requirements.md - User Stories & Acceptance Criteria

**Purpose**: Captures functional requirements using EARS (Easy Approach to Requirements' Syntax) notation for clear, testable specifications.

**Content Structure**:
- User stories written in structured EARS format
- Each requirement in code blocks with specific WHEN/THEN syntax
- Acceptance criteria that can be directly translated to test cases
- Business rules and validation requirements

**Example Content**:
```markdown
# User Authentication Requirements

## Primary User Stories

```
WHEN a user provides valid credentials
THE SYSTEM SHALL authenticate the user and provide access to the application
```

```
WHEN a user provides invalid credentials
THE SYSTEM SHALL display an error message and deny access
```

```
WHEN a user attempts to access a protected resource without authentication
THE SYSTEM SHALL redirect to the login page
```

## Acceptance Criteria

- Login form validates email format
- Password must be at least 8 characters
- System supports "Remember Me" functionality
- Failed login attempts are logged for security
```

#### 🏗️ design.md - Technical Architecture & Design Decisions

**Purpose**: Documents the technical architecture, component interactions, and implementation considerations for the feature.

**Content Structure**:
- High-level system architecture diagram
- Component relationships and data flow
- Database schema designs (if applicable)
- API endpoint specifications
- Security considerations
- Performance requirements
- Technology stack decisions

**Example of Generated Content:**

``` markdown

## Authentication System Design

### Architecture Overview

| Component | Responsibilities | Technology |
|-----------|-----------------|------------|
| **Web Client** | • Login Form<br/>• Registration<br/>• Password Reset | React/Vue/Angular |
| **API Gateway** | • JWT Token Management<br/>• Session Management<br/>• Rate Limiting | API Gateway Service |
| **Auth Service** | • User Store<br/>• Password Hashing<br/>• 2FA Support | ASP.NET Core API |

**Data Flow:** Web Client → API Gateway → Auth Service

### Component Specifications

#### Authentication Service
- **Technology**: ASP.NET Core Web API
- **Database**: PostgreSQL with Entity Framework
- **Security**: JWT tokens with refresh token rotation
- **Caching**: Redis for session management

#### Data Models

public record User
{
    public Guid Id { get; init; }
    public string Email { get; init; }
    public string PasswordHash { get; init; }
    public bool IsEmailVerified { get; init; }
    public DateTime CreatedAt { get; init; }
}


### Security Considerations
- Password hashing using Argon2id
- Rate limiting: 5 attempts per minute per IP
- JWT expiration: 15 minutes access, 7 days refresh
- Audit logging for all authentication events

```


#### ✅ tasks.md - Implementation Task Breakdown

**Purpose**: Provides a detailed, trackable implementation plan with discrete tasks that can be executed systematically.

**Content Structure**:
- Task breakdown with clear descriptions
- Estimated effort and dependencies
- Acceptance criteria for each task
- Progress tracking with status updates
- Links to relevant requirements and design elements

**Example Content**:
```markdown
# Authentication Feature - Implementation Tasks

## Phase 1: Foundation Setup

### Task 1.1: Database Schema Setup
**Status**: ✅ Completed
**Description**: Create user table with necessary fields for authentication
**Acceptance Criteria**:
- User table created with Id, Email, PasswordHash, IsEmailVerified, CreatedAt
- Indexes on Email field for performance
- Migration script created and tested

**Effort**: 2 hours
**Dependencies**: None

### Task 1.2: Authentication Service Project Structure
**Status**: 🔄 In Progress
**Description**: Set up ASP.NET Core Web API project with dependency injection
**Acceptance Criteria**:
- Project created with proper folder structure
- Dependency injection configured
- Basic middleware setup (CORS, authentication, logging)

**Effort**: 4 hours
**Dependencies**: Task 1.1

## Phase 2: Core Authentication Logic

### Task 2.1: Password Hashing Implementation
**Status**: ⏳ Pending
**Description**: Implement secure password hashing using Argon2id
**Acceptance Criteria**:
- Password hashing service implemented
- Password verification functionality
- Unit tests for hashing/verification logic

**Effort**: 3 hours
**Dependencies**: Task 1.2

### Task 2.2: JWT Token Service
**Status**: ⏳ Pending
**Description**: Implement JWT token generation and validation
**Acceptance Criteria**:
- Access token generation (15 min expiry)
- Refresh token generation (7 days expiry)
- Token validation middleware
- Secure key management

**Effort**: 4 hours
**Dependencies**: Task 1.2

## Phase 3: API Endpoints

### Task 3.1: Login Endpoint
**Status**: ⏳ Pending
**Description**: Implement POST /api/auth/login endpoint
**Acceptance Criteria**:
- Accepts email/password in request body
- Returns JWT tokens on successful authentication
- Returns appropriate error messages for failures
- Rate limiting implemented

**Effort**: 3 hours
**Dependencies**: Task 2.1, Task 2.2

### Task 3.2: Registration Endpoint
**Status**: ⏳ Pending
**Description**: Implement POST /api/auth/register endpoint
**Acceptance Criteria**:
- Email validation and uniqueness checking
- Password strength requirements
- Email verification workflow initiated
- Success/error responses properly formatted

**Effort**: 4 hours
**Dependencies**: Task 2.1, Task 1.2

## Phase 4: Integration & Testing

### Task 4.1: Integration Tests
**Status**: ⏳ Pending
**Description**: Create comprehensive integration tests for auth flow
**Acceptance Criteria**:
- Full authentication flow tested
- Error scenarios covered
- Database state verification
- API contract validation

**Effort**: 6 hours
**Dependencies**: All Phase 3 tasks

### Task 4.2: Security Testing
**Status**: ⏳ Pending
**Description**: Security audit and penetration testing
**Acceptance Criteria**:
- SQL injection prevention verified
- XSS protection implemented
- Rate limiting effectiveness tested
- Security headers configured

**Effort**: 4 hours
**Dependencies**: All Phase 3 tasks
```

## 🔄 How to Use Generated Specs for Code Implementation

### Step 1: Spec Generation
```
"Create a spec for user authentication feature"
```
Cursor will generate the three files in `.cursor/specs/user-authentication/`

### Step 2: Requirements Review
- Review `requirements.md` to understand business needs
- Validate acceptance criteria are complete and testable
- Identify any missing requirements

### Step 3: Design Review
- Review `design.md` for technical architecture
- Understand component interactions and data flow
- Verify technology choices align with project standards

### Step 4: Task-Driven Implementation
- Start with Phase 1 tasks from `tasks.md`
- Mark tasks as "In Progress" when starting work
- Update status to "Completed" with checkmark when done
- Use task descriptions as implementation guides

### Step 5: Cursor-Assisted Coding
When implementing, Cursor will:
- Reference the spec files for context
- Generate code that matches the documented architecture
- Ensure implementation aligns with requirements
- Create tests based on acceptance criteria

### Step 6: Iterative Refinement
- Update specs as implementation reveals new requirements
- Add new tasks for discovered work
- Refine design based on implementation learnings

**Example Cursor Commands**:
```
"Implement the login endpoint according to the authentication spec"
"Create unit tests for the password hashing service as defined in tasks.md"
"Generate the User model based on the design.md specifications"
```

This workflow ensures systematic, specification-driven development with clear traceability from requirements to implementation.

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


## Related Resources

- [Cursor IDE Documentation](https://cursor.sh/docs)
- [C# Coding Standards](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [NuGet Package Publishing](https://docs.microsoft.com/en-us/nuget/create-packages/overview-and-workflow)
- [Semantic Versioning](https://semver.org/)
- [SPDX License List](https://spdx.org/licenses/)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
