---
description: Comprehensive guidelines for generating and maintaining specification files to bridge the gap between requirements and implementation
globs: .cursor/specs/**/requirements.md, .cursor/specs/**/design.md, .cursor/specs/**/tasks.md
alwaysApply: false
---

# Cursor Rules File: Code by Specifications Best Practices

## Role Definition

- Product Manager
- Technical Lead
- Software Architect
- Quality Assurance Engineer
- Development Team Lead

## General

### Description

Establish a structured approach to software development using specification-driven practices that bridge the gap between conceptual product requirements and technical implementation. Create comprehensive specs that ensure alignment between stakeholders, reduce development iterations, and provide clear guidelines for implementation and testing.

### Requirements

**- NEVER: Place sensitive information in the generated code (e.g. passwords, API keys, personal information, etc.)**

- Generate three core specification files for each feature: requirements.md, design.md, and tasks.md
- Use structured EARS notation for writing clear, testable requirements
- Document technical architecture with sequence diagrams and implementation considerations
- Create detailed implementation plans with discrete, trackable tasks
- Maintain iterative refinement capabilities for evolving specifications
- Ensure all specs are stored in the .cursor/specs directory structure
- Follow consistent formatting and documentation standards

## Specification File Structure

### Core Specification Files

When generating specs for a feature, create three key files in a new directory named after the feature within the `.cursor/specs` directory.

#### ✅ DO: Create complete specification structure

```bash
.cursor/specs/
└── user-authentication/
    ├── requirements.md    # User stories and acceptance criteria in EARS notation
    ├── design.md         # Technical architecture and sequence diagrams
    └── tasks.md          # Detailed implementation plan with trackable tasks
```

#### ❌ DON'T: Create incomplete specifications

```bash
# DON'T create specs with missing files
.cursor/specs/
└── user-authentication/
    ├── requirements.md    # Only requirements, missing design and tasks
    └── design.md
```

## Development Workflow

### Specification-Driven Development Process

Follow a structured workflow that ensures each phase is properly completed before moving to the next.

#### ✅ DO: Follow the complete specification workflow

1. **Requirements Phase**: Define user stories and acceptance criteria in structured EARS notation
2. **Design Phase**: Document technical architecture with sequence diagrams and implementation considerations
3. **Implementation Planning**: Break down work into discrete, trackable tasks with clear outcomes
4. **Execution Phase**: Track progress with the ability to update and refine specs as needed

#### ❌ DON'T: Skip specification phases

```bash
# DON'T jump straight to coding without proper specs
# DON'T create designs without clear requirements
# DON'T start implementation without detailed task breakdown
```

> [!TIP]
> **Benefits of Specification-Driven Development:**
>
> - Clear alignment between product requirements and technical implementation
> - Reduced development iterations through upfront planning
> - Better collaboration between product and engineering teams
> - Improved testability through structured requirements
> - Easier maintenance and future enhancements

## Requirements Specification

### EARS Notation for Requirements

Write clear, testable requirements using EARS (Easy Approach to Requirements' Syntax) notation in the `requirements.md` file.

#### ✅ DO: Use structured EARS notation

```markdown
WHEN a user submits a form with invalid data,
THE SYSTEM SHALL display validation errors next to the relevant fields

WHEN a user attempts to access a protected resource without authentication,
THE SYSTEM SHALL redirect to the login page

WHEN an authenticated user logs out,
THE SYSTEM SHALL clear the session and redirect to the home page
```

#### ❌ DON'T: Use unstructured requirements

```markdown
# DON'T write vague requirements
Users should be able to log in somehow and see errors when they do something wrong.
```

#### ✅ DO: Include comprehensive acceptance criteria

```markdown
## User Authentication Requirements

WHEN a user provides valid credentials,
THE SYSTEM SHALL authenticate the user and create a session

WHEN a user provides invalid credentials,
THE SYSTEM SHALL display an error message and remain on the login page

WHEN a user exceeds maximum login attempts,
THE SYSTEM SHALL temporarily lock the account and send a notification email
```

> [!TIP]
> **Benefits of EARS Notation:**
>
> - **Clarity**: Requirements are unambiguous and easy to understand
> - **Testability**: Each requirement can be directly translated into test cases
> - **Traceability**: Individual requirements can be tracked through implementation
> - **Completeness**: The format encourages thinking through all conditions and behaviors

## Technical Design Documentation

### Architecture and Design Specifications

Document technical architecture, sequence diagrams, and implementation considerations in the `design.md` file.

#### ✅ DO: Include comprehensive design documentation

```markdown
# User Authentication Design

## Architecture Overview
- Frontend: Blazor WebAssembly
- Backend: ASP.NET Core Web API
- Database: SQL Server with Entity Framework Core
- Authentication: JWT tokens with refresh token rotation

## Sequence Diagram
```
User -> Login Form: Enter credentials
Login Form -> API: POST /api/auth/login
API -> Database: Validate credentials
Database -> API: User data
API -> User: JWT token + refresh token
```

## Component Interactions
- `LoginComponent`: Handles user input and API communication
- `AuthService`: Manages token storage and refresh
- `AuthMiddleware`: Validates tokens on protected endpoints
```

#### ❌ DON'T: Create design docs without implementation details

```markdown
# DON'T create vague design documentation
The system will use authentication. It will be secure and fast.
```

#### ✅ DO: Document implementation considerations

```markdown
## Security Considerations
- Password hashing using ASP.NET Core Identity
- JWT tokens with 15-minute expiration
- Refresh tokens stored securely in HttpOnly cookies
- Rate limiting on authentication endpoints

## Performance Considerations
- Password validation with client-side and server-side checks
- Token caching for reduced database queries
- CDN integration for static assets
```

## Implementation Planning

### Task Breakdown and Tracking

Create detailed implementation plans with discrete, trackable tasks in the `tasks.md` file.

#### ✅ DO: Break down work into manageable tasks

```markdown
# User Authentication Implementation Tasks

## Phase 1: Backend Authentication API
- [ ] Create User entity and database context
- [ ] Implement password hashing service
- [ ] Create authentication controller with login endpoint
- [ ] Add JWT token generation and validation
- [ ] Implement refresh token functionality

## Phase 2: Frontend Login Component
- [ ] Create login form component
- [ ] Implement form validation
- [ ] Add API integration for authentication
- [ ] Implement token storage and management
- [ ] Add loading states and error handling

## Phase 3: Protected Routes and Middleware
- [ ] Create authentication middleware
- [ ] Implement route protection
- [ ] Add automatic token refresh
- [ ] Create logout functionality
```

#### ❌ DON'T: Create vague or overly large tasks

```markdown
# DON'T create unmanageable tasks
- [ ] Implement authentication (this is too broad)
- [ ] Make the app work (this is not specific)
```

#### ✅ DO: Include task dependencies and estimates

```markdown
## Task Details

### Task 1.1: Create User Entity
**Description**: Define the User entity with necessary properties for authentication
**Dependencies**: None
**Estimated Time**: 30 minutes
**Acceptance Criteria**:
- User entity created with Id, Email, PasswordHash, CreatedAt
- Entity Framework migration created
- Basic validation attributes added

### Task 1.2: Implement Password Hashing
**Description**: Create secure password hashing service
**Dependencies**: Task 1.1
**Estimated Time**: 45 minutes
**Acceptance Criteria**:
- Password hashing service implemented
- Unit tests for password validation
- Integration with ASP.NET Core Identity
```

## Specification Maintenance and Iteration

### Iterative Refinement Process

The specification-driven development approach is designed for continuous refinement, allowing specs to evolve as the project progresses.

#### ✅ DO: Follow iterative refinement workflow

1. **Update Requirements**: Modify `requirements.md` when new requirements emerge or existing ones change
2. **Update Design**: Refine `design.md` to reflect architectural changes or new technical considerations
3. **Update Tasks**: Adjust `tasks.md` to accommodate requirement or design changes

#### ❌ DON'T: Let specifications become outdated

```markdown
# DON'T ignore changing requirements
# DON'T continue with outdated designs
# DON'T work from stale task lists
```

### Specification Version Control

#### ✅ DO: Maintain specification history

```bash
# Track specification changes with git
git add .cursor/specs/user-authentication/
git commit -m "feat: update user authentication specs

- Added password complexity requirements
- Updated JWT token expiration design
- Added additional test scenarios"
```

#### ✅ DO: Use clear commit messages for spec changes

```bash
# Good commit messages for spec changes
git commit -m "spec: add rate limiting requirements to authentication"
git commit -m "design: update architecture for multi-tenant support"
git commit -m "tasks: break down OAuth integration into smaller tasks"
```

### Specification Quality Assurance

#### ✅ DO: Review specifications regularly

- **Requirements Review**: Ensure all requirements are testable and complete
- **Design Review**: Validate technical decisions and architectural choices
- **Task Review**: Confirm tasks are properly sized and dependencies are clear

#### ✅ DO: Include acceptance criteria for all requirements

```markdown
WHEN a user successfully logs in
THE SYSTEM SHALL redirect to the dashboard
AND display a welcome message
AND log the authentication event
```

> [!TIP]
> **Specification Quality Checklist:**
>
> - **Requirements**: Clear, testable, and complete using EARS notation
> - **Design**: Comprehensive architecture with implementation considerations
> - **Tasks**: Properly sized, with dependencies and acceptance criteria
> - **Traceability**: Clear links between requirements, design, and implementation
> - **Maintainability**: Easy to update and evolve with project changes

### Integration with Development Workflow

#### ✅ DO: Use specifications as living documentation

- Reference specifications in code comments
- Link pull requests to specific requirements
- Update specs as part of code reviews
- Use specs for automated testing generation

#### ✅ DO: Maintain specification consistency

```markdown
# Ensure consistent formatting across all spec files
# Use standard templates for common patterns
# Maintain consistent terminology throughout
# Regular reviews to ensure alignment
```

> [!WARNING]
> **Common Specification Pitfalls:**
>
> - **Vague Requirements**: Always use EARS notation for clarity
> - **Incomplete Design**: Include all architectural decisions and trade-offs
> - **Overly Large Tasks**: Break down complex work into manageable units
> - **Outdated Specs**: Regularly review and update specifications
> - **Missing Traceability**: Ensure clear links between all specification elements

### Specification Templates and Tools

#### ✅ DO: Use consistent templates

```markdown
# Standard Requirements Template
## [Feature Name] Requirements

### Functional Requirements
WHEN [condition]
THE SYSTEM SHALL [behavior]

### Non-Functional Requirements
WHEN [condition]
THE SYSTEM SHALL [performance/security/usability criteria]
```

#### ✅ DO: Leverage AI assistance

```markdown
# Use Cursor's AI capabilities to:
# - Generate initial specification drafts
# - Refine requirements based on feedback
# - Create comprehensive test scenarios
# - Identify missing requirements or edge cases
```

# End of Cursor Rules File