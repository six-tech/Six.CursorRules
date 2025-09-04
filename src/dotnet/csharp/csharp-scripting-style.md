---
description: This file provides guidelines for writing clean, maintainable, and idiomatic C# single file apps (.NET 10 feature)
globs: scripts/*.cs
alwaysApply: false
---

# Cursor Rules File: C# Single File Apps (Scripts) Style Guide

Role Definition:

- C# Language Expert
- Software Architect
- Code Quality Specialist

## 1. Overview & General Principles

### Core Philosophy

C# single file apps (a feature added in `.NET 10`) should be used for simple, single-purpose scripts. This way we can keep
our entire codebase in c# and avoid the need for several different languages/script types.
Also, since this is pure .NET, we can leverage the full power of .NET and available libraries.

### Requirements

- IMPORTANT: Use `csharp-coding-style.mdc` as the default style rule. This rule extends it.
- Use `.NET 10` or later.

# End of C# Coding Style Guide