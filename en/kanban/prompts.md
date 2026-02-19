# KardOps -- Prompts

Catalog of reusable prompts for operating the KardOps board via AI agents.
Replace the placeholders `<...>` with actual values before using.

---

## 1. Card Implementation

### 1.1 Implement card from todo

```text
Consider kanban/instructions.md
Implement the card kanban/1-todo/<CARD-XXX-slug>.md

Strictly follow the implementation plan described in the card.
Upon completing each step, update the card's "Progress" section with date and result.
At the end, run the validation described in the card and check the acceptance criteria that were met.
If all criteria pass, update the status to `done` and move the card to kanban/4-done/.
```

### 1.2 Continue card in progress

```text
Consider kanban/instructions.md
Continue the implementation of card kanban/2-doing/<CARD-XXX-slug>.md

Resume from the last step recorded in the "Progress" section.
Follow the implementation plan and update progress for each completed step.
If you encounter an impediment, move the card to kanban/3-blocked/ and register the reason.
If you finish, run the validation, check the criteria, and move to kanban/4-done/.
```

### 1.3 Unblock and resume blocked card

```text
Consider kanban/instructions.md
The card kanban/3-blocked/<CARD-XXX-slug>.md has been unblocked.

Move it to kanban/2-doing/, register the blockage resolution in "Progress" with date,
and resume implementation from the last completed step.
```

---

## 2. Card Creation

### 2.1 Create card from backlog item

```text
Consider kanban/instructions.md and kanban/9-templates/CARD-001-template.md

Create a new card in kanban/1-todo/ from the following backlog item:
- Origin: 0-backlog/backlog.md#<item-reference>
- Title: <short-title>
- Priority: <P0|P1|P2|P3>

Fill in all mandatory sections: description, scope (includes/does not include),
implementation plan with executable steps, verifiable acceptance criteria,
and validation commands. Use the next available sequential ID.
```

### 2.2 Break backlog item into multiple cards

```text
Consider kanban/instructions.md and kanban/9-templates/CARD-001-template.md

Analyze the backlog item "<item-description>" and break it into smaller,
independent cards. Each card must:
- Have a closed scope and be implementable in isolation
- Follow the complete official template
- Be created in kanban/1-todo/ with sequential ID
- Reference the origin in the backlog

List the created cards at the end.
```

---

## 3. Board Cleanup and Reorganization

### 3.1 Full board audit

```text
Consider kanban/instructions.md

Perform a full audit of the Kanban board in kanban/:
1. List all cards in each column (1-todo, 2-doing, 3-blocked, 4-done).
2. Verify that each card has all mandatory fields (id, title, priority, origin, description, scope, plan, criteria, validation, status, progress).
3. Verify that each card's status is consistent with the column it is in.
4. Verify that there are no duplicate IDs across columns.
5. Verify that WIP in 2-doing respects the limit of 3 cards.
6. Verify that cards in 3-blocked have the reason and unblock action registered.
7. Verify that cards in 4-done have criteria checked and validation executed.

Present a report with issues found and suggested corrections.
```

### 3.2 Fix board inconsistencies

```text
Consider kanban/instructions.md

Based on the Kanban board audit in kanban/:
1. Fix cards whose `status` field does not match the column they are in.
2. Move cards that are in the wrong column to the correct column according to their status.
3. Fill in missing mandatory fields with placeholder values marked as [PENDING].
4. Remove ID duplicates, keeping the most recent version (by progress).
5. Register all corrections made in the "Progress" section of each affected card.
```

### 3.3 Clean up old completed cards

```text
Consider kanban/instructions.md

Analyze the cards in kanban/4-done/:
1. List all completed cards with their completion dates.
2. Identify cards completed more than <N> days ago.
3. Verify that the corresponding backlog items are marked as completed.
4. Suggest which cards can be archived (moved to a kanban/5-archive/ folder).

Do not execute the archiving automatically -- only list and wait for confirmation.
```

### 3.4 Reorganize backlog and sync with cards

```text
Consider kanban/instructions.md

Synchronize the backlog (kanban/0-backlog/backlog.md) with the current board state:
1. Identify backlog items that already have cards created (in any column).
2. Mark backlog items whose cards are already in 4-done with the completion date.
3. Identify backlog items without associated cards and list them.
4. Identify orphan cards (without a valid origin reference in the backlog) and list them.
5. Suggest backlog reordering by priority.
```

---

## 4. Board Management

### 4.1 General board status (daily/standup)

```text
Consider kanban/instructions.md

Generate a board status summary for standup:
- How many cards in each column (todo, doing, blocked, done).
- Cards in progress (2-doing): title, last recorded progress.
- Blocked cards (3-blocked): title, reason for blockage.
- Cards ready to start (1-todo): title and priority (ordered by P0->P3).
- Alerts: WIP exceeded, cards stalled without recent progress.
```

### 4.2 Prioritize next card for execution

```text
Consider kanban/instructions.md

Analyze the cards in kanban/1-todo/ and recommend the next card to be started,
considering:
- Priority (P0 > P1 > P2 > P3)
- WIP limit in 2-doing (maximum 3)
- Dependencies or blockages from other cards
- Estimated complexity from the implementation plan

Justify the recommendation.
```

### 4.3 Move card between columns

```text
Consider kanban/instructions.md

Move the card <CARD-XXX-slug>.md from kanban/<source-column>/ to kanban/<target-column>/.
Update the card's `status` field to reflect the new column.
Register the movement in the "Progress" section with date.
Validate the consistency rules (e.g., a card going to done must have criteria checked).
```

---

## 5. Maintenance and Quality

### 5.1 Validate quality of a specific card

```text
Consider kanban/instructions.md

Evaluate the quality of card kanban/<column>/<CARD-XXX-slug>.md:
- Are all mandatory fields filled in?
- Does the implementation plan have executable and ordered steps?
- Are the acceptance criteria objectively verifiable?
- Does the validation include actual project commands?
- Does the scope have clear boundaries (includes/does not include)?

Suggest concrete improvements if necessary.
```

### 5.2 Standardize existing cards to match template

```text
Consider kanban/instructions.md and kanban/9-templates/CARD-001-template.md

Review all existing cards on the board (all columns) and:
1. Identify cards that do not follow the official template structure.
2. For each non-standard card, adjust the structure to match the template.
3. Preserve the original content -- only reorganize and add missing sections.
4. Register the adjustment in the "Progress" section of each modified card.
```

### 5.3 Update official template

```text
Consider kanban/instructions.md

Review the template kanban/9-templates/CARD-001-template.md to ensure that:
- It reflects all rules and mandatory fields from kanban/instructions.md.
- It contains useful examples in each section to facilitate filling in.
- It is aligned with the project's current conventions.

Make the necessary adjustments to the template.
```

---

## 6. Reports and Metrics

### 6.1 Generate throughput report

```text
Consider kanban/instructions.md

Analyze the cards in kanban/4-done/ and generate a throughput report:
- Total completed cards.
- Cards completed per period (weekly, if inferable from progress dates).
- Average cycle time (from creation/todo to done, based on progress dates).
- Distribution by priority (how many P0, P1, P2, P3 completed).
```

### 6.2 Generate blockage report

```text
Consider kanban/instructions.md

Analyze the history of cards in kanban/3-blocked/ and kanban/4-done/ (that went through blockage):
- How many cards were blocked.
- Most frequent blockage reasons.
- Average time spent in blocked (if inferable from progress).
- Suggestions to reduce future blockages.
```
