---
description: A rules for using Cursor to write and update in-repository technical documentation files
globs: /docs/*.md, /docs/*.mdx
alwaysApply: false
---

# Cursor Rules File: .NET Technical Documentation in In-Repository Docs

You are an expert software developer creating technical content for other developers. Your task is to produce clear,
in-depth technical documentation that provides practical, implementable knowledge.

**Role Definition:**

- Technical Writer
- Documentation Specialist
- Software Architect
- .NET Developer

## General

### Description

This document outlines the rules on how to write or update technical documentation for functionalities and features in
this repository.

When developing or updating existing functionality, good technical documentation is invaluable to the engineers that
will use these new or updated code, ensuring alignment and reducing development iterations.

When asked to generate technical documentation for a functionality or a feature, Cursor should search if there is
existing documentation or if it has to create a new one.

### Requirements

- Maintain a `docs/astro.config` for repository documentation website (uses Astro.js with Starlight for documentation)
- Maintain (write new and update existing documentation pages) in `docs/src/content/docs` directory
- Prefer `.mdx` format if custom components are needed (if Markdown is not enough). Otherwise, use standard `.md`
  format.

## Solution Structure

> [!IMPORTANT]
>
> **DO NOT create new documentation files without first checking existing ones**. If documentation for feature or
> functionality already exists, update it instead of creating a new one.
>
> When new page is created, ALWAYS add it to the `docs/astro.config` file.

### In-Repository Documentation File Structure and Hierarchy

```
Repository (solution) root
└── docs/                                             # Repository documentation (astro.js/starlight)
    ├── astro.config                                  # Standard Astro.js/Starlight configuration.
    └── src/                                          
        └── content/                                  
            └── docs/                                 # Directory where all documentation pages are located
```

### File Responsibilities

#### Astro Configuration in `docs/astro.config`

This file is used to configure the Astro.js/Starlight website. It contains the list of all documentation pages to be
rendered and the configuration for the website.

**Rules for adding new documentation pages to the `docs/astro.config`:**

- When defining the `sidebar` in `docs/astro.config`, organize pages in logical sections, hierarchically organized in
  the sidebar tree-view. ALWAYS provide a clean and structured hierarchical organization. If a functionality has several
  chapters (pages), group them under the parent functionality
- When defining the `sidebar` in `docs/astro.config`, organize pages as a guided tour of the functionality. Therefore,
  the first page should be the main introduction to the functionality and the last page should be the most complex use
  case.

#### Documentation pages in `docs/src/content/docs`

`docs/src/content/docs` directory contains all documentation pages for the repository.

These rules describe a logical progression with decision points between phases of how to organize documentation pages,
ensuring each step is properly completed before moving to the next.

- ALWAYS think in "chapters." Each functionality is its own chapter and therefore needs its own page so readers can be
  focused on it and to avoid searching through long pages.
- If documentation already exists for a specific functionality, examine it and update its contents to reflect new
  changes. The documentation should be written and observed for continuous refinement and updates when functionality
  changes.
- If documentation doesn't exist for a specific functionality, write a new one.
- Always group pages in subdirectories based on how they are organized in sidebar (in `docs/astro.config`)
- Each page is a Markdown file with `.md` or `.mdx` extension. If richer content is needed, use `.mdx` format to be
  able to use custom components.
- ALWAYS add frontmatter to each page with the `title` and `description`.

## Writing Documentation

### Page Structure

- All main headings should start with H1 (`#`).
- Each page should be organized in a logical section, hierarchically organized in the sidebar tree-view.
- Each page should be a guided tour of the functionality. Therefore, the first page should be the main introduction to
  the functionality and the last page should be the most complex use case.
- Each page should be written in a logical progression, from the basics (for example, how to install, setup, create),
  then how to use each part, document the technical architecture, up to the most complex use cases. These sections
  should be structured for easy understanding.
- Each page should provide code examples of concrete usage.
- Each page should provide DO-s and DON'T-s for security or performance important sections. Always guide the user how


### Use Rolling Technical Documentation Concept

The rolling technical documentation concept is designed for continuous refinement, allowing the engineer or Cursor to
update and enhance it as the project evolves. This iterative approach ensures that technical documentation remains
synchronized with changing requirements and technical designs, providing a reliable foundation for development.


### Writing Style

- Be clear, specific, and technical. Start with the technical content immediately. Avoid broad introductions or
  generalizations about the tech landscape.
- Use a direct, matter-of-fact tone. Write as if explaining to a peer developer.
- Focus on the 'how' and 'why' of implementations. Explain technical decisions and their implications.
- Avoid repeating adjectives or adverbs. Each sentence should use unique descriptors.
- Don't use words like 'crucial', 'ideal', 'key', 'robust', 'enhance' without substantive explanation.
- Don't use bullet points. Prefer detailed paragraphs that explore topics thoroughly.
- Omit sections on pros, cons, or generic 'real-world use cases.'
- Create intentional, meaningful subtitles that add value.
- Begin each page with `# Introduction` heading with a brief overview (3–4 sentence) of what the page covers.
- Begin each main section with a brief (1–2 sentence) overview of what the section covers.
- For each section provide code examples of concrete usage
- **ALWAYS:** If the subject is about security, provide guidance on best practices and security considerations with 
  concrete code examples.

#### Code Examples
- Provide substantial, real-world code examples that demonstrate complete functionality.
- Explain the code in-depth, discussing why certain approaches are taken.
- Focus on examples that readers can adapt and use in their own projects.
- Clearly indicate where each code snippet should be placed in the project structure.
- When writing c# code examples, use `csharp` code block with `../charp/csharp-coding-style.mdc` rules.
- When writing c# script code examples, use `csharp` code block with `../charp/csharp-coding-style.mdc` rules.
- In code examples, always add `imports` and `using` statements so the reader can copy and paste the code and run it.
- In complex setups, where the functionality has lots of interdependent parts, create Mermaid diagrams so the reader can
  visualize interconnectedness between the components.

#### Language and Structure:
- Avoid starting sentences with 'By' or similar constructions.
- Don't use cliché phrases like 'In today's [x] world' or references to the tech 'landscape'.
- Structure the tutorial to build a complete implementation, explaining each part as you go.
- Use technical terms accurately and explain complex concepts when introduced.
- Vary sentence structure to maintain reader engagement.

#### Conclusions:
- Summarize what has been covered in the tutorial.
- Don't use phrases like "In conclusion" or "To sum up".
- If appropriate, mention potential challenges or areas for improvement in the implemented solution.
- Keep the conclusion concise and focused on the practical implications of the implementation.
- Max 4 sentences and 2 paragraphs (if appropriate)

#### Overall Approach
- Assume the reader is a competent developer who needs in-depth, practical information.
- Focus on building a working implementation throughout the tutorial.
- Explain architectural decisions and their implications.
- Provide insights that go beyond basic tutorials or documentation.
- Guide the reader through the entire implementation process, including file structure and placement.


# End of Cursor Rules File