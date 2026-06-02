# Jumpstart

Jumpstart is an agentic vibecoding framework for quickly starting and building a new project. There are 2 main phases:

1. **Jumpstart:** Turn your idea into a requirements document, architecture plan, and GitHub repo.
2. **Iterate:** Build your project piece-by-piece using the issue, plan, execute loop.

## Prerequisites

Before using Jumpstart, ensure you have the following:

- **Git** — Installed and configured on your machine
- **GitHub CLI (`gh`)** — Installed and authenticated (`gh auth status`)
- **A compatible AI coding agent** — Such as Claude Code, Cursor, GitHub Copilot, Roo Code, etc.

## Quick Start

Commands prefixed with `/` invoke the corresponding skill in your agent. A skill is a packaged set of instructions that guides the agent through a specific task, such as creating a GitHub issue or generating a planning document.

1. **Setup Jumpstart** — Follow the [Setup](#setup) section to add the required skills to your local project
2. `/jumpstart-project` to create your initial requirements, architecture plan, and GitHub repository
3. `/create-git-issue` to turn an implementation phase, feature, bug, or idea into a GitHub issue
4. `/create-planning-doc` to turn a GitHub issue into a thoroughly documented plan
5. `/execute-plan` to turn a planning doc into a working, tested, and committed code

Repeat steps 3-5 until the project is completed.

## Setup

1. Clone or copy the Jumpstart repository into your local project folder. Ensure the `/.agents` directory is placed at the root of your project.
2. Verify the skills have been picked up by your agent. Different agents expect different directory names (e.g., `.claude`, `.cursor`, `.github/agents`). If the `/.agents` directory isn't being detected, rename it to match your agent's expected name.
3. Use `/jumpstart-project` to create your initial requirements and architecture documents. The agent will likely ask you additional questions to clarify the requirements. It will also pause for your feedback after creating each document. Finally, it will offer to create a new private GitHub repository if you don't have one already.

## Workflow

Repeat this workflow for each implementation phase/feature/bug/task. Each step should be a new agent chat to prevent hallucinations and task creep.

1. `/create-git-issue` — Describe the bug, feature, or task you want completed. The agent writes a draft, asks for your approval, then creates a GitHub issue.
2. `/create-planning-doc` — Reference a GitHub issue number. The agent creates a planning document based on the template and asks for your feedback.
3. `/execute-plan` — Reference a planning document. The agent implements changes in a feature branch and pauses for your approval after testing passes. If approved, it merges the changes and cleans up the feature branch.

## Architecture

Jumpstart is organized into the following directory structure:

```
├── README.md
├── /.agents/
│   ├── rules/          # Agent behavioral rules
│   └── skills/         # Agent skill definitions
├── architecture/       # Requirements & architecture docs
└── planning/           # Implementation planning docs
```

### /.agents/rules

There are only two rules included with Jumpstart, one for basic coding guidelines and the other for enforcing adherence to planning docs. Feel free to add/create any additional rules you'd like.

### /.agents/skills

There are many skills in Jumpstart but most are for guiding the agent through its tasks.

The only skills you need to directly call are the main four: `/jumpstart-project`, `/create-git-issue`, `/create-planning-doc`, and `/execute-plan`.

If you wish to change the format of the requirements, architecture, or planning doc you can modify the template files:
- `/.agents/skills/jumpstart-project/REQUIREMENTS_TEMPLATE.md`
- `/.agents/skills/jumpstart-project/ARCHITECTURE_TEMPLATE.md`
- `/.agents/skills/create-planning-doc/TEMPLATE.md`

### /architecture/

Contains both the architecture and requirements documents created by the `/jumpstart-project` skill. Both documents will be updated by the agent if necessary during the final phase of the `/execute-plan` skill.

### /planning/

Contains the planning documents created by the `/create-planning-doc` skill.

## Tips

- You can bypass using GitHub issues by calling the `/create-planning-doc` with a description of the feature/bug/task request.
- Make git issues story-sized, testable chunks. A good issue scope is a single user-facing feature or bug fix that can be implemented in 1-2 hours of agent work.
- Jumpstart works best when auto-approve is enabled (with proper safety guardrails). Complex plans can take up to an hour to fully implement before asking for user feedback.
- Multi-threading agents is possible but you will need to use git worktrees and plan for dependencies.