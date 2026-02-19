# KardOps -- Operational Instructions

This document defines the operational rules of the KardOps methodology for task management via Markdown files within `kanban/`.

## 1. Structure

- `0-backlog/backlog.md`: source of truth for what still needs to be done in the project.
- `1-todo/`: cards ready to start.
- `2-doing/`: cards in progress.
- `3-blocked/`: cards temporarily blocked by external dependency, unrelated failure, or technical impediment.
- `4-done/`: completed and validated cards.
- `5-archive/`: archived completed cards (more than 30 days in `4-done/`).
- `9-templates/`: official card templates.

## 2. Workflow

1. Register demands in `0-backlog/backlog.md`.
2. Break backlog into smaller, independent cards.
3. Create one `.md` file per card in `1-todo/` using the official template.
4. **[IMPORTANT]** When starting execution, **MOVE IMMEDIATELY** the card to `2-doing/`.
   - Update the status in the card to `status: doing`.
   - Never work on a card that is in `1-todo/`.
5. Implement the task and update the card with objective progress (date + result).
6. If there is a real impediment, move to `3-blocked/` and register the cause/blockage.
7. Upon unblocking, move from `3-blocked/` to `2-doing/`.
8. Before finalizing, run the project's validation script. Each repository must maintain its own validation script at the root, covering at minimum: static analysis and automated tests. The script format is flexible (shell script, Makefile, npm script, etc.).
9. Only if validation passes and criteria are checked, update status to `status: done` and move to `4-done/`.

**MANDATORY FLOW: `1-todo/` -> `2-doing/` -> `4-done/` (or `3-blocked/` during)**

**ANTI-PATTERN: DO NOT move directly from `1-todo/` to `4-done/` without going through `2-doing/`**

## 3. Mandatory Rules for Cards

Every card must contain:

- `id`: unique identifier (e.g., `CARD-001`).
- `title`: short and clear objective.
- `priority`: `P0`, `P1`, `P2` or `P3`.
- `origin`: traceable reference (backlog, technical analysis, issue, or other project document).
- `description`: functional/technical context.
- `scope`: what is included and what is not.
- `implementation plan`: technical steps in order.
- `acceptance criteria`: verifiable checklist.
- `validation`: commands/tests that prove completion.
- `status`: `todo`, `doing`, `blocked` or `done`.
- `progress`: short and objective temporal log.

Optional fields:

- `size`: `S`, `M` or `L` -- effort estimate to support WIP decisions and planning.

## 4. Consistency Rules

- **The correct flow is always: `1-todo/` -> `2-doing/` -> `4-done/` (or `3-blocked/` during execution).**
- **NEVER move directly from `1-todo/` to `4-done/`.**
- The same `id` cannot exist in more than one column at the same time.
- Movement is via `mv` (never copy cards between columns).
- Card in `2-doing` must have:
  - `status: doing`
  - progress updated regularly with timestamps
- Card in `4-done` must have:
  - `status: done`
  - acceptance criteria checked
  - mandatory validation checked
  - completion date in progress
- Card in `3-blocked` must specify:
  - `status: blocked`
  - reason for blockage
  - action needed to unblock
  - timestamp of when it was blocked

## 5. File Naming Convention

Pattern: `kanban/<column>/CARD-XXX-short-slug.md`

Example: `kanban/1-todo/CARD-012-fix-offline-sync.md`

## 6. ID Sequence Control

The `last_id` field at the end of `0-backlog/backlog.md` records the last ID used. Before creating a new card, check this field and use the next sequential number. After creating, update the field.

## 7. Official Template

Official template: `kanban/9-templates/CARD-001-template.md`

Recommended process:

1. Duplicate the template to `1-todo/` with a new name.
2. Update `id`, title, priority, and origin.
3. Fill in description, scope, plan, criteria, and validation.

## 8. Minimum Quality per Card

- Implementation plan with executable steps (not just vague decisions).
- Acceptance criteria that can be objectively verified.
- Validation with actual project commands.
- Scope with clear boundaries to prevent indefinite growth.

## 9. WIP and Blockage Policy

- Recommended WIP in `2-doing`: maximum 3 simultaneous cards.
- Blocked cards should not remain in `2-doing`.
- Blockage due to unrelated failure should be registered in `progress` and in the validation item.

## 10. Allowed State Transitions

**Normal Flow (Happy Path):**

```text
1-todo/ (status: todo)
  | [upon starting execution]
2-doing/ (status: doing)
  | [after completing, testing, and validating]
4-done/ (status: done)
```

**Flow with Blockage:**

```text
1-todo/ (status: todo)
  | [upon starting execution]
2-doing/ (status: doing)
  | [upon encountering a real impediment]
3-blocked/ (status: blocked)
  | [upon unblocking]
2-doing/ (status: doing)
  | [after completing, testing, and validating]
4-done/ (status: done)
```

**Transitions NOT Allowed:**

- `1-todo/` -> `4-done/` (skipping `2-doing/`)
- `1-todo/` -> `3-blocked/` (blocking without starting)
- `4-done/` -> `2-doing/` (reopening without explicit reason)
- Leaving a card in `1-todo/` while it is being worked on

## 11. Transition Checklists

### todo -> doing

Before moving:

- [ ] I have fully read the card, including the implementation plan
- [ ] I understand all steps in the plan
- [ ] I have identified the prerequisites and dependencies
- [ ] I am clear on what the acceptance criteria are
- [ ] I have identified the validation commands

Upon moving:

- [ ] I moved the file from `1-todo/` to `2-doing/`
- [ ] I updated `status: doing` in the card
- [ ] I added a start timestamp in the progress section

### doing -> blocked

Upon moving:

- [ ] I moved the file from `2-doing/` to `3-blocked/`
- [ ] I updated `status: blocked` in the card
- [ ] I filled in the `## Blockage` section with reason, impact, and action to unblock
- [ ] I added a blockage timestamp in the progress section

### blocked -> doing

Upon moving:

- [ ] I moved the file from `3-blocked/` to `2-doing/`
- [ ] I updated `status: doing` in the card
- [ ] I recorded the blockage resolution in the progress section with date

### doing -> done

Before moving:

- [ ] I implemented all steps in the plan
- [ ] I ran local validations successfully
- [ ] I checked all acceptance criteria as [x]
- [ ] I ran all tests (lint, build, unit tests, etc.)
- [ ] I added completion timestamps in the progress section
- [ ] I updated `status: done` in the card

Upon moving:

- [ ] I moved the file from `2-doing/` to `4-done/`
- [ ] I verified that `status: done`
- [ ] I verified that all criteria are checked [x]
- [ ] I verified that all validations were executed [x]

## 12. Backlog Updates

- When creating cards from the backlog, maintain traceability (`origin`).
- When all cards for an item are in `4-done`, mark the item as completed in `0-backlog/backlog.md` with date.
- Do not delete backlog history; prefer completion marking.

## 13. Validation Script

Each repository adopting this methodology must maintain a validation script at the root. The name and format of the script are flexible (shell script, Makefile, npm script, etc.). The script must cover, at minimum:

1. **Static analysis / linting** -- detect code errors and style violations.
2. **Automated tests** -- run the project's test suite.

The script may include additional steps as needed by the project (coverage, build, type checking, etc.), as long as the two minimum steps are present.

If the project does not yet have the script, creating it is a prerequisite before moving any card to `4-done/`.

## 14. Archiving Policy

Cards in `4-done/` for more than 30 days may be moved to `5-archive/`. Archiving:

- preserves the card intact (no changes)
- should be recorded in the backlog when all cards for the item have been archived
- can be executed via an audit prompt (see `prompts.md` -- section 3.3)

## 15. Definition of Done (DoD)

A card is only done when:

1. Implementation completed.
2. Acceptance criteria checked.
3. Project validation script executed successfully.
4. Card updated to `status: done`.
5. Card moved to `4-done/`.
