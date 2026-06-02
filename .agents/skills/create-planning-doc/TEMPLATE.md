# {Title}
<!-- For GitHub issues: "GitHub Issue #42: Add User Authentication" -->
<!-- For generic: "Add User Authentication" -->

## Overview
<!-- 1-2 sentences explaining the goal. List concrete features and user flows. -->
Brief description of what the change accomplishes.

### Features
Bulleted list of specific features/requirements.

---

## Expected Code Changes

### New Files
<!-- List all files to create with purpose. Use high-level descriptions. -->
| File | Purpose |
|------|---------|
| `path/to/file.tsx` | Description |

### Modified Files
<!-- List existing files and what changes. -->
| File | Changes |
|------|---------|
| `path/to/existing.tsx` | What will change |

---

## Architecture Notes
<!-- Include state management, component hierarchy, integration points, data flow, and ext dependencies. -->
- State management approach
- Component hierarchy
- Integration points with existing code
- Data flow diagrams (if complex)

---

## Git Branch & Commit Strategy

### Branch Name
- `gitissue-{ID}/{short-description}` or `feature/{short-description}`

### Commit Message
- **Subject**: `gitissue-{ID}: {description}`
- **Body**: {Optional detailed context}

---

## Possible Risks & Conflicts
<!-- Consider breaking changes, key collisions, styling conflicts, mobile/responsive issues, browser compat. -->

| Risk | Mitigation |
|------|------------|
| {Risk description} | {How to prevent/handle} |

---

## Test Coverage

> [!IMPORTANT]
> - Follow the [design-tests skill](../design-tests/SKILL.md) for testing best practices.
> - **MANDATORY**: You MUST perform a baseline comparison using the [run-e2e-tests skill](../run-e2e-tests/SKILL.md) to ensure no regressions were introduced.
> - **New Functionality**: Must have E2E test coverage.
> - **Bug Fixes**: Must include new E2E test to prevent regression.

### Unit Tests (`__tests__/path/to/file.test.tsx`)
- [ ] Test case 1
- [ ] Test case 2

### E2E Tests (`e2e/tests/feature.spec.ts`)
- [ ] User flow test 1
- [ ] User flow test 2

### Test Commands
```bash
npm run test -- ComponentName
npm run test:e2e -- feature.spec.ts
npm run ci-flow
```

---

## Execution Phases
<!-- Split complex tasks into logical phases (e.g., Phase 1: API/Backend, Phase 2: UI Components, Phase 3: Integration). -->
1. **Phase 1**: {Description}
2. **Phase 2**: {Description}

---

## Implementation Checklist

> [!IMPORTANT]
> If you are using a `task.md`, all items from this checklist must be included in it to ensure synchronization.


### Preparation
- [ ] Move issue to "in progress" using the update-git-issue skill
- [ ] Create git branch using the [create-branch-git skill](../create-branch-git/SKILL.md)

### Implementation
- [ ] **Phase 1**: Implement {Phase 1 Description}
- [ ] **Phase 2**: Implement {Phase 2 Description}
- [ ] ... (Continue for all defined phases)
- [ ] Verify implementation against "Expected Code Changes"

### Verification
- [ ] Run unit tests: `npm run test -- filter`
- [ ] Run E2E tests: `npm run test:e2e -- filter`
- [ ] **MANDATORY**: Run E2E baseline comparison using the [run-e2e-tests skill](../run-e2e-tests/SKILL.md)
- [ ] Run full CI flow: `npm run ci-flow`

### Submission
- [ ] Commit changes (Format: `feat: description` or `gitissue-{ID}: description`) using the [commit-git skill](../commit-git/SKILL.md)
- [ ] Move the issue to "in review" using the update-git-issue skill
- [ ] **MANDATORY**: Request user approval DO NOT PROCEED UNTIL APPROVAL IS RECEIVED
- [ ] Create PR using the [create-pr-git skill](../create-pr-git/SKILL.md)
- [ ] Merge the PR using the [merge-git skill](../merge-git/SKILL.md)
- [ ] Attach planning doc and walkthrough to the GitHub issue (e.g., via `gh issue comment`) using the update-git-issue skill
- [ ] Move the issue to "done" using the update-git-issue skill
- [ ] Update architecture and requirements documents in `architecture/` with any changes made during implementation
