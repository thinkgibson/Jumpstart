---
name: planning-doc
description: "Creates comprehensive planning documents for feature implementations. Use when given requirements, acceptance criteria, or GitHub issues to plan before coding."
---

# Skill: Planning Document Creation

## Goal

Create a structured planning document that enables any implementing agent to understand requirements, architecture, risks, and testing strategy without additional context.

---

## Protocol

### 1. Determine Source Type

| Source | Naming Convention |
|--------|-------------------|
| **GitHub Issue** | File: `planning/{reponame}_gitissue_{ID}.md`, Title: `GitHub Issue #{ID}: {Title}` |
| **Generic Requirements** | File: `planning/{kebab-case-summary}.md`, Title: `{Summary Title}` |

### 2. Gather Context

Before writing the plan, collect:
- **Requirements**: Full text of issue/ticket/acceptance criteria using the retrieve-git-issue skill(../retrieve-git-issue/SKILL.md)
- **Project Structure**: Run `list_dir` on key directories (components, tests, lib)
- **Existing Code**: Use `view_file_outline` on related files
- **Related Skills**: Check for existing skills (git workflow, testing, etc.)

### 3. Identify Issue Type

| Type | Focus |
|------|-------|
| **Enhancement** | New features, user flows, architecture integration |
| **Bug Fix** | Root cause, affected areas, regression prevention |
| **Refactor** | Breaking changes, migration path, backwards compatibility |

---

## Document Structure & Guidelines

Create the planning document with these sections.

### File Naming
- **GitHub Issue**: `planning/gitissue-{ID}.md` (e.g., `gitissue-42.md`)
- **Generic**: `planning/{kebab-case-summary}.md` (e.g., `user-authentication.md`)

### Template

See [`TEMPLATE.md`](./TEMPLATE.md) for the full planning document template.

---

## Quality Checklist

Before finalizing the planning document, verify:

- [ ] All requirements from the issue are addressed
- [ ] File paths match project conventions
- [ ] Related skills are referenced via relative paths (not duplicated)
- [ ] Risks specific to this codebase are identified
- [ ] Test cases cover happy path and edge cases
- [ ] Checklist enables incremental progress