# KardOps

**KardOps** is a methodology for coordinating AI coding agents using Kanban cards as the unit of work.

It provides end-to-end traceability, conflict-free parallel execution, minimum verifiable quality, and clear ownership and handoff between agents.

## Choose your language

| Language       | Docs             |
| -------------- | ---------------- |
| English        | [en/kanban/](en/kanban/)       |
| Portugues (BR) | [pt-br/kanban/](pt-br/kanban/) |
| Espanol        | [es/kanban/](es/kanban/)       |

## Quick Start

1. Choose your language above.
2. Read `<lang>/kanban/README.md`.
3. Copy the `<lang>/kanban/` folder into your project.
4. Create your validation script (e.g., `npm run lint && npm test`).
5. Start creating cards!

You can also download the pre-packaged Kanban asset for your language directly from the project's [GitHub Releases page](https://github.com/tiagolpadua/kardops/releases).

## Tool Independence

KardOps does not require any additional tool, platform, or paid product.

You can run the methodology with what your team already uses day to day:

- Your IDE
- Your command line
- Your current AI coding agent

## Structure

Each language pack is organized as:

```text
<lang>/
└── kanban/
    ├── README.md              # Full methodology overview
    ├── instructions.md        # Detailed operational rules
    ├── prompts.md             # Reusable prompt catalog for agents
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
2. Create `kanban/` inside it.
3. Translate `kanban/README.md`, `kanban/instructions.md`, and `kanban/prompts.md`.
4. Copy the board structure under `kanban/` with translated content in `0-backlog/backlog.md` and `9-templates/CARD-001-template.md`.
5. Keep all folder names unchanged — they are part of the KardOps methodology.

## Versioning and Releases

This project uses [Semantic Versioning](https://semver.org/) (`MAJOR.MINOR.PATCH`).

- `package.json` `version` is the single source of truth.
- Git tags must follow `vX.Y.Z` and match the value in `package.json`.
- `CHANGELOG.md` follows [Keep a Changelog](https://keepachangelog.com/).

Release checklist:

1. Move completed changes from `Unreleased` to a new version section in `CHANGELOG.md`.
2. Update `package.json` `version` (example: `0.2.0`).
3. Commit: `chore(release): v0.2.0`.
4. Create annotated tag: `git tag -a v0.2.0 -m "Release v0.2.0"`.
5. Push commit and tag: `git push && git push --tags`.
6. Publish a GitHub Release for tag `v0.2.0`.

## License

MIT
