---
description: A rules for using Cursor to write and update specs files (requirements.md, design.md, and tasks.md files) in this repository and to write code that adheres to the spec files.
globs: requirements.md, design.md, tasks.md
---

# Cursor Rules

This document outlines the rules on how to write specs for features to be coded in this repository.

Specs bridge the gap between conceptual product requirements and technical implementation details, ensuring alignment and reducing development iterations. 
When asked to generate specs for a feature, Cursor should generate three key files in a new directory named by the new feature within the `.cursor/specs` directory that form the foundation of each specification:

- `requirements.md` - Captures user stories and acceptance criteria in structured EARS notation
- `design.md` - Documents technical architecture, sequence diagrams, and implementation considerations
- `tasks.md` - Provides a detailed implementation plan with discrete, trackable tasks

## Workflow
The workflow follows a logical progression with decision points between phases, ensuring each step is properly completed before moving to the next.

1. **Requirements Phase**: Define user stories and acceptance criteria in structured EARS notation
2. **Design Phase**: Document the technical architecture, sequence diagrams, and implementation considerations
3. **Implementation Planning**: Break down the work into discrete, trackable tasks with clear descriptions and outcomes
4. **Execution Phase**: Track progress as tasks are completed, with the ability to update and refine the spec as needed


## Requirements.md Rules

The `requirements.md` file is written in the form of user stories with acceptance criteria in EARS notation. The way you wish your PM would give you requirements!

EARS (Easy Approach to Requirements' Syntax) notation provides a structured format for writing clear, testable requirements. 

In a spec's `requirements.md` file, each requirement is within code block marks (```...```) and follows this pattern:  

```
WHEN [condition/event]
THE SYSTEM SHALL [expected behavior]
```

For example:
```
WHEN a user submits a form with invalid data
THE SYSTEM SHALL display validation errors next to the relevant fields
```

This structured approach offers several benefits:

- Clarity: Requirements are unambiguous and easy to understand
- Testability: Each requirement can be directly translated into test cases
- Traceability: Individual requirements can be tracked through implementation
- Completeness: The format encourages thinking through all conditions and behaviors

Cursor should help the user to transform vague feature requests into these well-structured requirements, making the development process more efficient and reducing misunderstandings between product and engineering teams.


## Design.md Rules

The `design.md` file is where you document technical architecture, sequence diagrams, and implementation considerations. It's a great place to capture the big picture of how the system will work, including the components and their interactions.

Cursor's specs offer a structured approach to design documentation, making it easier to understand and collaborate on complex systems. The `design.md` file is a great place to capture the big picture of how the system will work, including the components and their interactions.


## Tasks.md Rules

The `tasks.md` file is where you provide a detailed implementation plan with discrete, trackable tasks and sub-tasks. Each task is clearly defined, with a clear description, expected outcome, and any necessary resources or dependencies. By setting the specs offer a structured approach to implementation plans, making it easier to understand and collaborate on complex systems.

Cursor should provide a task execution interface for `tasks.md` files that displays real-time status updates. Tasks are updated as in-progress or completed, allowing the user to efficiently track implementation progress and maintain an up-to-date view of development status.

## Best practices
How do I iterate on my specs?

The code-by-specifications concept is designed for continuous refinement, allowing the engineer or Cursor to update and enhance them as the project evolves. This iterative approach ensures that specifications remain synchronized with changing requirements and technical designs, providing a reliable foundation for development.

1. **Update Requirements**: Upon user's request, either modify the `requirements.md` file directly or ask user to provide new requirements. Cursor will then generate a new `requirements.md` file or update existing that reflects the updated requirements.

2. **Update Design**: Navigate to the design.md file for the spec that needs the refinement and update both the design documentation and the associated task list to reflect your modified requirements.

3. **Update tasks**: Navigate to the tasks.md file for the spec that needs the refinement and update the tasks. This will create new tasks that map to the new requirements or update existing task and related requirements.


# End of Cursor Rules File