---
name: jumpstart-project
description: "Transforms a project idea into a comprehensive architectural overview including C4 diagrams, tech stack recommendations, and phased implementation plans. Use when starting a new project from scratch."
---

# Skill: Jumpstart Project Architecture

**Goal**: Transform a raw project idea into a structured architectural blueprint — including C4 model diagrams, technology stack rationale, and phased implementation plan — that can guide all subsequent development.

---

## When to Use

Use this skill when:

- Starting a brand-new project (no existing codebase)
- The user has a high-level idea but no architecture, diagrams, or plan
- You need to produce an output others (or other agents) can execute against
- The project involves multiple services, components, or deployment concerns

Do **not** use this skill when:

- The project already has an architecture document (update it instead)
- The task is a small feature or bug fix in an existing project
- You're in the middle of execution (use `execute-plan` instead)

---

## Protocol

### Step 1: Discovery Interview

Use the [`clarify-requirements`](../clarify-requirements/SKILL.md) skill to interview the user and clarify the project vision.

When invoking `clarify-requirements`, ensure the following areas are covered:

| Area | Key Questions |
|------|---------------|
| **Core Purpose** | What problem does this solve? Who is the target user? What is the primary outcome? |
| **Users & Roles** | Who uses the system? Are there different user types (admin, guest, premium)? |
| **Key Features** | What are the 3-5 most important features? What is the MVP scope vs. future? |
| **Constraints** | Budget? Timeline? Team size? Deployment environment (cloud, on-prem, edge)? Compliance needs? |
| **Preferences** | Preferred languages, frameworks, or platforms? Any strong opinions (e.g., "must be Rust" or "React only")? |

> **Exit criteria**: The `clarify-requirements` skill has completed and the user has confirmed a shared understanding of the vision and scope.

### Step 2: Synthesize Requirements

Write a concise requirements summary. Save it as `architecture/{project-name}-requirements.md` using the [`REQUIREMENTS_TEMPLATE.md`](./REQUIREMENTS_TEMPLATE.md).

Present the requirements document to the user. Ask for feedback, changes, or approval. Iterate based on their input until confirmed.

> **Exit criteria**: The user has reviewed and approved the requirements document.

### Step 3: Create Architecture Document

Save the complete architecture to `architecture/{project-name}-architecture.md` using the [`ARCHITECTURE_TEMPLATE.md`](./ARCHITECTURE_TEMPLATE.md). Below is guidance for completing each section of the template.

#### C4 Diagrams

Identify the architecture at each C4 level and produce Mermaid diagrams.

| Level | What to Show | Guidance |
|-------|-------------|----------|
| **1 — Context** | System as a black box, its users, external dependencies | Include every actor and system the project interacts with |
| **2 — Container** | High-level deployable units (web app, API, DB, queue) | One box per process/service; show communication protocols |
| **3 — Component** | Internal modules of the most complex container | Only for the container that warrants decomposition |
| **4 — Code** | Key classes, state machines, or algorithms | **Optional** — only if there's non-obvious complexity |

#### Tech Stack

For each layer, recommend a technology and state the rationale concisely. Cover: frontend framework, backend runtime/framework, database, infrastructure (hosting/CI/CD/monitoring), and auth strategy.

**Guiding principles**:
- Prefer familiar, well-supported technologies for the target problem
- Prefer "boring" (proven, stable) tech unless bleeding edge is justified
- Minimize total distinct technologies — each adds cognitive and operational cost
- If the user expressed preferences, honor them unless there's a strong reason not to

#### Implementation Phases

Break the project into delivery phases. Each phase must produce a **working, deployable increment**.

| Phase | Scope | Dependencies | Typical Complexity |
|-------|-------|--------------|-------------------|
| **Phase 1: Foundation** | Skeleton, CI/CD, DB schema, auth, unit + E2E test setup | None | Medium |
| **Phase 2: Core Feature X** | First major feature | Phase 1 | High |
| **Phase 3: Core Feature Y** | Second major feature | Phase 1 | High |
| **Phase 4: Polish & Deploy** | Testing, docs, production config | Phase 2, 3 | Low |

**Rules**:
- Phase 1 MUST include: project scaffolding, basic CI/CD, database schema, authentication, and unit + E2E test infrastructure
- No phase should depend on an uncompleted phase
- Each phase should be independently testable

### Step 4: Review and Iterate

Present the completed architecture document to the user. Ask for feedback, changes, or approval. Iterate on the document based on their input until the user confirms satisfaction.

> **Exit criteria**: The user has reviewed and approved the architecture document.

### Step 5: Initialize Git Repository

After the user has approved the architecture document, ask the user if they would like to initialize a Git repository and create an initial commit. If they agree, invoke the [`initialize-git-repo`](../initialize-git-repo/SKILL.md) skill to create a private GitHub repository and set up the local repository with an initial commit.

> **Exit criteria**: The user has confirmed they want to proceed (or declined), and if agreed, the Git repository has been initialized.

---

## Quality Checklist

Before finishing, verify:

- [ ] User confirmed project vision and scope (Step 1)
- [ ] Requirements document saved to `architecture/` (Step 2)
- [ ] Requirements document reviewed and approved by user (Step 2)
- [ ] Architecture document saved to `architecture/` using the template (Step 3)
- [ ] Architecture document reviewed and approved by user (Step 4)
- [ ] Both documents cross-reference each other

---

## Related Skills

- [`clarify-requirements`](../clarify-requirements/SKILL.md) — Use for deeper requirements interview if the project is complex
- [`initialize-git-repo`](../initialize-git-repo/SKILL.md) — Use in Step 5 to create the GitHub repository and initial commit