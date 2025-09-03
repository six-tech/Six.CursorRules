---
description: A rules for using Cursor to write and update technical documentation files in the .NET solution
globs: /docs/*.md, /docs/*.mdx
alwaysApply: false
---

# Cursor Rules

This document outlines the rules on how to write or update technical documentation for functionalities and features in this repository.

When developing or updating existing functionality, good technical documentation is invaluable to the engineers that will use these new or updated code, ensuring alignment and reducing development iterations.
 
When asked to generate technical documentation for a functionality or a feature, Cursor should search if there is existing documentation or if it has to create a new one. 

## Workflow
The workflow follows a logical progression with decision points between phases, ensuring each step is properly completed before moving to the next.

These are the rules how to approach writing in both cases:

- If documentation already exist for a specific functionality, examine it and update it's contents to reflect new changes. The documentation should be written and observed for continuous refinement and updates when functionality changes.
- If documentation doesn't exist for a specific functionality, write a new one.

In both cases always follow these rules:


### Locations

- Documentation should be written in `.md` or `.mdx` format. Use `.mdx` format when custom Astro/Starlight components are used.
- All documentation markdown files should be placed in solution root's `docs` directory in the subdirectory named as .csproj where functionality is located
- ALWAYS think in "chapters". Each functionality is its own chapter and therefore needs its own page so readers can be focused on it and to avoid searching through long pages.
- ALWAYS provide clean and structured directory organization. If functionality has several chapters, group them in a separate directory under the parent functionality


### Writing Style

- Be clear, specific and technical.
- Each chapter should have a main title named as functionality it describes as `# Functionality Name`.
- For each chapter write `## Introduction` heading to provide brief introduction what certain functionality is for, what benefits it provides and what users will gain by using it.
- After `Introduction`, write sections of how to use the functionality/feature. This part should follow logica progression, from the basics (for example, how to install, setup, create), then how to use each part, document the technical architecture, up to the most complex use cases. These sections should be structured for easy understanding.
- For each section provide code examples of concrete usage
- Provide DO-s and DON'T-s for security or performance important sections. Always guide the user how to write most secure and performant code for a certain functionality and reference to `../csharp/csharp-coding-style.mdc` rules.
- In complex setups, where the functionality has lots of interdependent parts, create Mermaid diagrams so the reader can visualize interconnectedness between the components.


## Best practices
How do I iterate on my specs?

The rolling technical documentation concept is designed for continuous refinement, allowing the engineer or Cursor to update and enhance it as the project evolves. This iterative approach ensures that technical documentation remain synchronized with changing requirements and technical designs, providing a reliable foundation for development.

**Update Documentation**: Upon user's request, either modify the needed documentation file(s) directly or ask user to provide guidance. Cursor will then generate a new documentation markdown file or update existing that reflects the updated requirements.


# End of Cursor Rules File