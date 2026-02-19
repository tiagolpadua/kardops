# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Markdown quality tooling with `markdownlint-cli2`, Husky pre-commit hook, and auto-fix script (`npm run fix:md`).
- CI workflow to lint Markdown on pull requests and pushes to `main`.
- Release workflow to package Kanban assets (`en`, `es`, `pt-br`) and publish GitHub Release artifacts.
- Node project metadata and lockfile (`package.json`, `package-lock.json`) for reproducible tooling setup.

### Changed

- Release/version source of truth is now `package.json` `version` (including workflow validation and README guidance).
- Root README expanded with tool-independence and release/versioning documentation.
- Portuguese (BR) and Spanish documentation normalized with proper accents in methodology guides, prompts, and card templates.
- `.gitignore` updated to ignore `node_modules` (including nested paths) and common Node debug logs.

## [0.1.0] - 2026-02-19

### Added

- Initial public structure for KardOps in English, Portuguese (BR), and Spanish.
