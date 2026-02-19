# Copilot Instructions — KardOps

## Project Overview

- KardOps is a **documentation-first methodology** for coordinating AI coding agents using Kanban cards.
- The repository is **multi-language**:
  - `en/`
  - `pt-br/`
  - `es/`
- Each language folder contains:
  - `README.md`
  - `instructions.md`
  - `prompts.md`
  - `kanban/` structure with board folders and templates.

## Primary Goal for Changes

When editing this repository, prioritize:

1. Methodology consistency (workflow and state transitions).
2. Clarity and actionability of instructions.
3. Structural consistency across language packs.

## Non-Negotiable Methodology Rules

Always preserve these rules unless the user explicitly asks to change the methodology itself:

- Mandatory flow: `1-todo/` -> `2-doing/` -> `4-done/` (with `3-blocked/` only as an intermediate state when needed).
- Never allow direct transition `1-todo/` -> `4-done/`.
- Cards in execution must be in `2-doing/` with `status: doing`.
- Completed cards must be in `4-done/` with `status: done`, acceptance criteria checked, and validation executed.
- Keep WIP guidance (max 3 cards in `2-doing`) unless explicitly changed.

## Folder and Naming Constraints

- Do not rename core board folders:
  - `0-backlog/`, `1-todo/`, `2-doing/`, `3-blocked/`, `4-done/`, `5-archive/`, `9-templates/`
- Do not change card naming convention without explicit request:
  - `CARD-XXX-short-slug.md`
- Keep `backlog.md` ID sequence semantics (`last_id`/equivalent in each language) intact.

## Editing Guidelines

- Keep edits **minimal and focused** on the user request.
- Preserve existing Markdown structure, headings, tables, checklists, and links.
- Avoid adding process complexity or extra sections not requested.
- Prefer plain, imperative language for operational instructions.
- Avoid unnecessary comments or verbose prose.

## Multi-language Consistency

- If the user asks for a global methodology change, apply equivalent updates to all language folders (`en/`, `pt-br/`, `es/`) unless instructed otherwise.
- If the user asks for a language-specific change (translation, wording), update only the target language.
- Keep semantic parity across translations; adapt wording naturally but do not alter meaning.

## Validation Before Finishing

For documentation changes, always:

- verify internal links still point to valid files/anchors when edited;
- ensure terminology is consistent within the edited language;
- ensure no contradiction with `instructions.md` in the same language.

## What to Avoid

- Do not introduce tooling/framework assumptions not present in the docs.
- Do not create or remove board columns without explicit request.
- Do not modify unrelated files.
- Do not weaken mandatory validation requirements for moving cards to done.

## Preferred Output Style for New Content

- Short sections with clear headings.
- Bullet lists for rules.
- Checklists for transition criteria.
- Concrete examples for naming and flow when useful.

## Quick Reference

- Methodology source of truth (per language): `instructions.md`
- Reusable agent prompts: `prompts.md`
- Ready-to-copy board template: `kanban/`
