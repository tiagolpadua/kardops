# KardOps

**KardOps** is a methodology for coordinating AI coding agents using Kanban cards as the unit of work.

It provides end-to-end traceability, conflict-free parallel execution, minimum verifiable quality, and clear ownership and handoff between agents.

## Choose your language

| Language       | Docs             |
| -------------- | ---------------- |
| English        | [en/](en/)       |
| Portugues (BR) | [pt-br/](pt-br/) |
| Espanol        | [es/](es/)       |

## Quick Start

1. Choose your language above.
2. Read the `README.md` in that folder.
3. Copy the `kanban/` folder into your project.
4. Create your validation script (e.g., `run-validate.sh`).
5. Start creating cards!

## Structure

Each language folder contains:

```text
<lang>/
├── README.md              # Full methodology overview
├── instructions.md        # Detailed operational rules
├── prompts.md             # Reusable prompt catalog for agents
└── kanban/                # Ready-to-copy board structure
    ├── 0-backlog/
    │   └── backlog.md
    ├── 1-todo/
    ├── 2-doing/
    ├── 3-blocked/
    ├── 4-done/
    ├── 5-archive/
    └── 9-templates/
        └── CARD-001-template.md
```

## Contributing

Contributions and translations to new languages are welcome. To add a new language:

1. Create a new folder with the language code (e.g., `fr/`, `de/`).
2. Translate `README.md`, `instructions.md`, and `prompts.md`.
3. Copy the `kanban/` structure with translated content in `backlog.md` and the card template.
4. Keep all folder names unchanged — they are part of the KardOps methodology.

## License

MIT
