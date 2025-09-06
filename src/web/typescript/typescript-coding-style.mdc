---
description: This file provides guidelines for writing clean, maintainable TypeScript code with a focus on functional patterns, proper typing, and modern JavaScript features.
globs: *.ts, *.css, *.scss
alwaysApply: false
---

# Cursor Rules File: Typescript Coding Style Guide

**Role Definition:**
- You are an expert in TypeScript and CSS styling.
- Front-end Developer
- Full-stack JavaScript/TypeScript Developer
- Type Safety Specialist

## General

### Description

TypeScript code must be written to maximize type safety, readability, and maintainability while leveraging modern
JavaScript features and functional programming patterns. This guide provides comprehensive best practices for writing
robust TypeScript applications with proper error handling, performance optimization, and scalable architecture patterns.

### Requirements

- Write concise, technical TypeScript code with accurate examples
- Use functional and declarative programming patterns by default but use classes when appropriate
- Prefer iteration and modularization over code duplication
- Use descriptive variable names with auxiliary verbs (e.g., isLoading, hasError)
- Structure files: exported component, subcomponents, helpers, static content, types
- Use TypeScript for all code; prefer interfaces to types
- Avoid enums; use maps instead
- Use functional components with TypeScript interfaces
- Use strict mode in TypeScript for better type safety

## Code Style and Structure

- Write concise, technical TypeScript code with accurate examples.
- Use functional and declarative programming patterns by default but use classes when appropriate.
- Prefer iteration and modularization over code duplication.
- Use descriptive variable names with auxiliary verbs (e.g., isLoading, hasError).
- Structure files: exported component, subcomponents, helpers, static content, types.

## Naming Conventions

- Use lowercase with dashes for directories (e.g., components/auth-wizard).
- Favor named exports for components.

## TypeScript Usage

- Use TypeScript for all code; prefer interfaces to types.
- Avoid enums; use maps instead.
- Use functional components with TypeScript interfaces.
- Use strict mode in TypeScript for better type safety.

### Type Definitions

```typescript
// Good: Use interfaces for object shapes
interface User {
    readonly id: string;
    readonly name: string;
    readonly email: string;
    readonly createdAt: Date;
}

// Good: Use type aliases for unions and primitives
type UserStatus = 'active' | 'inactive' | 'suspended';
type UserId = string & { readonly __brand: 'UserId' };

// Avoid: Using classes for data transfer
class UserDTO {
    public id: string;
    public name: string;
    // Classes create unnecessary complexity for data
}
```

```typescript
// Good: Use maps instead of enums
const UserRole = {
    Admin: 'admin',
    Editor: 'editor',
    Viewer: 'viewer'
} as const;

type UserRoleType = typeof UserRole[keyof typeof UserRole];

// Avoid: Enums create issues with tree-shaking
enum UserRole {
    Admin = 'admin',
    Editor = 'editor',
    Viewer = 'viewer'
}
```

## Syntax and Formatting

- Use the "function" keyword for pure functions.
- Avoid unnecessary curly braces in conditionals; use concise syntax for simple statements.
- Use declarative JSX.
- Use Prettier for consistent code formatting.

## UI and Styling

- Implement a responsive design with Flexbox and CSS breakpoints.
- Use CSS variables for theming and styling.

## Performance Optimization

- (If using React) Minimize the use of useState and useEffect; prefer context and reducers for state management.
- Optimize images: use WebP format where supported, include size data, implement lazy loading.
- (If using React) Implement code splitting and lazy loading for non-critical components with React's Suspense and
  dynamic imports.
- (If using React) Avoid unnecessary re-renders by memoizing components and using useMemo and useCallback hooks
  appropriately.

## Error Handling and Validation

- Use Zod for runtime validation and error handling.
- Prioritize error handling and edge cases:
  - Handle errors at the beginning of functions.
  - Use early returns for error conditions to avoid deeply nested if statements.
  - Avoid unnecessary else statements; use an if-return pattern instead.
  - Implement global error boundaries to catch and handle unexpected errors.

### Error Handling Patterns

```typescript
// Good: Early return pattern
function processUser(user: User | null): Result<User, Error> {
    if (!user) {
        return {success: false, error: new Error('User not found')};
    }

    if (!user.email) {
        return {success: false, error: new Error('User email is required')};
    }

    // Process user
    return {success: true, data: user};
}

// Avoid: Deep nesting
function processUser(user: User | null): Result<User, Error> {
    if (user) {
        if (user.email) {
            // Process user
            return {success: true, data: user};
        } else {
            return {success: false, error: new Error('User email is required')};
        }
    } else {
        return {success: false, error: new Error('User not found')};
    }
}
```

```typescript
// Good: Use Result types for expected failures
import {z} from 'zod';

const UserSchema = z.object({
    id: z.string().uuid(),
    name: z.string().min(1),
    email: z.string().email()
});

function validateUser(data: unknown): Result<User, z.ZodError> {
    const result = UserSchema.safeParse(data);
    if (!result.success) {
        return {success: false, error: result.error};
    }
    return {success: true, data: result.data};
}

// Avoid: Throwing exceptions for expected validation errors
function validateUser(data: unknown): User {
    return UserSchema.parse(data); // Throws on validation error
}
```

## Security

- Sanitize user inputs to prevent XSS attacks.

# End of Cursor Rules File