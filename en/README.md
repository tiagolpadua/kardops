# KardOps

**KardOps** is a methodology for coordinating AI agents on engineering tasks using Kanban cards as the unit of work.

For detailed operational rules, see [instructions.md](instructions.md).

## 1. Objective

Standardize work execution by AI agents with:

- end-to-end traceability
- fewer conflicts between parallel executions
- minimum verifiable quality before completing cards
- clear ownership and handoff between agents

## 2. Kanban Structure

The `kanban/` folder uses numeric prefixes to ensure ordering:

| Folder                 | Purpose                                                      |
| ---------------------- | ------------------------------------------------------------ |
| `0-backlog/backlog.md` | Ideas and demands not yet committed                          |
| `1-todo/`              | Cards ready for execution                                    |
| `2-doing/`             | Cards under active execution                                 |
| `3-blocked/`           | Cards blocked by external dependency or technical impediment |
| `4-done/`              | Completed and validated cards                                |
| `5-archive/`           | Archived completed cards (more than 30 days in `4-done/`)    |
| `9-templates/`         | Official templates                                           |

Auxiliary files:

- `instructions.md`: detailed operational rules (source of truth)
- `prompts.md`: catalog of reusable prompts for agents

## 3. Unit of Work: Card

Each card represents a small, verifiable technical delivery. Required fields and detailed rules are in [instructions.md -- section 3](instructions.md#3-mandatory-rules-for-cards).

Optional fields:

- `size`: `S`, `M` or `L` -- effort estimate to support WIP decisions and planning

## 4. Operational Flow with Agents

1. Product/Tech Lead registers in `0-backlog/backlog.md`.
2. Breaks down into technical cards and creates files in `1-todo/` using the official template.
3. Orchestrator sends one prompt per card to the agent (referencing the card file).
4. **[IMPORTANT]** Agent moves card to `2-doing/` **immediately** upon starting. Never work on a card that is in `1-todo/`.
5. Agent implements, updates progress with timestamps, and runs the card's validations.
6. If there is a real impediment, moves to `3-blocked/` with the reason and unblock action.
7. Upon unblocking, moves from `3-blocked/` to `2-doing/`.
8. Before finalizing, run the project's validation script. Each repository must maintain its own script covering at minimum: static analysis and automated tests (see [instructions.md -- section 13](instructions.md#13-validation-script)).
9. Only if validation passes and criteria are checked, moves to `4-done/`.

State transitions, checklists, and anti-patterns: see [instructions.md -- sections 10 to 11](instructions.md#10-allowed-state-transitions).

## 5. Using `prompts.md` for Coordination

`prompts.md` functions as a command inbox for agents.

Best practices:

- each prompt should point to a specific card
- avoid broad prompts without scope
- register follow-up when the agent raises a blocker
- keep language imperative and objective

## 6. Parallelism Rules

To avoid collisions between agents:

- one card per agent at a time
- do not allow the same `id` in two columns
- movement is via `mv` (never copy cards between columns)
- keep WIP in `2-doing` to a maximum of 3 simultaneous cards
- prioritize independent cards in parallel
- cards with dependencies must make the precondition explicit in the `Implementation Plan`

## 7. Handoff between Agents

When one agent finishes partially and another continues:

- update `Progress` with the current state
- list exactly what remains
- keep validation commands reproducible
- only move columns when the actual state changes (todo/doing/blocked/done)

## 8. ID Sequence Control

The `last_id` field at the end of `0-backlog/backlog.md` records the last ID used. Before creating a new card, check this field and use the next sequential number. After creating, update the field.

This prevents ID collisions when multiple agents create cards simultaneously.

## 9. Archiving Policy

Cards in `4-done/` for more than 30 days may be moved to `5-archive/`. Archiving:

- preserves the card intact (no changes)
- should be recorded in the backlog when all cards for the item have been archived
- can be executed via an audit prompt (see `prompts.md` -- section 3.3)

## 10. Governance for Other Teams

Recommended ritual:

1. Short planning meeting: prioritize backlog -> todo.
2. Asynchronous execution by agents via cards.
3. Daily review of `2-doing` and `3-blocked`.
4. Closure: validate `4-done` with evidence.

Basic indicators:

- lead time per card
- block rate
- rework (reopened cards)
- weekly throughput (`4-done`)

## 11. Internal References

- Operational rules: [instructions.md](instructions.md)
- Official template: [kanban/9-templates/CARD-001-template.md](kanban/9-templates/CARD-001-template.md)
- Prompt catalog: [prompts.md](prompts.md)
