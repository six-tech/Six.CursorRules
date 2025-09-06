# 💻 C# Coding Guidelines

This directory contains rules for writing clean, maintainable C# code with a focus on modern practices, functional patterns, and more.

## ✨ [Coding Style](csharp-coding-style.mdc)

Use these rules when:
- 📝 Writing new C# code
- 🔄 Refactoring existing code
- 👀 Reviewing pull requests
- 🏗️ Setting up new C# projects

These rules promote:
- 🚀 **Modern C# 13+ features** (record types, pattern matching, collection expressions)
- 🧮 **Functional programming patterns** with LINQ and lambda expressions
- 🎯 **Simple and focused abstractions** with minimal coupling
- 🔒 **Immutable data structures** and nullable reference types
- 📖 **Clean visual style** (no 'private' keyword, expressive syntax)
- ⚡ **Performance optimizations** (Span<T>/Memory<T>, AggressiveInlining)
- 📛 **Descriptive naming conventions** (PascalCase, camelCase, interface prefixes)
- ✅ **Testable design** with dependency injection

## 🔧 [Single File Apps (Scripts)](csharp-scripting-style.mdc)

Use these rules when:
- 📜 Creating C# single file apps (.NET 10 feature)
- 🎯 Writing simple, single-purpose scripts
- 🖥️ Building command-line tools and utilities
- ⚡ Replacing bash/PowerShell scripts with C#
- 🧪 Creating testable automation scripts

These rules ensure:
- 🚀 **.NET 10 single file app format** with `#!/usr/bin/env dotnet`
- 📦 **Package importing** using `#:#package` directives
- 🎨 **Rich console output** with `Spectre.Console` library
- 🔧 **External process management** using `CliWrap` library
- 🏗️ **Structured script organization** (setup → main → cleanup phases)
- 🛡️ **Comprehensive error handling** with appropriate exit codes
- 🎮 **Interactive user interfaces** with prompts and progress bars
- 🧪 **Testable script architecture** with dependency injection
- 📦 **Script packaging and distribution** for cross-platform deployment

