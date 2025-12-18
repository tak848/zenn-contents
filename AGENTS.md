# Repository Guidelines

## Overview

This repository contains Zenn content (articles/books) managed with `zenn-cli`, plus linting rules for Japanese writing via `textlint`.

## Project Structure

- `articles/`: Zenn articles (`.md`) with YAML front matter (title/emoji/topics/published, etc.).
- `books/`: Zenn books (currently empty except `.keep`).
- `images/`: Image assets referenced from Markdown.
- `z/`: Misc notes/utilities (e.g. `z/opensearch.md`).
- Config:
  - `package.json`, `pnpm-lock.yaml`: Node dependencies (`pnpm`).
  - `.textlintrc`: `textlint` rules (Japanese presets + AI-writing preset).
  - `aqua.yaml`: Toolchain versions (Node/pnpm, etc.).

## Build, Preview, and Lint Commands

- Install deps: `pnpm install`
- Preview locally (hot reload): `pnpm exec zenn preview`
- Create content:
  - New article: `pnpm exec zenn new:article`
  - New book: `pnpm exec zenn new:book`
- Lint writing (recommended before publishing/PR):
  - Check: `pnpm exec textlint "articles/**/*.md" "books/**/*.md"`
  - Auto-fix: `pnpm exec textlint --fix "articles/**/*.md" "books/**/*.md"`

## Writing Style & Naming

- Prefer date-prefixed filenames for articles, e.g. `articles/20251218-pdf-xml.md`.
- Keep YAML front matter valid and consistent (quotes, arrays, booleans).
- Follow `textlint` guidance for punctuation, sentence length, and tone consistency.

## Testing Guidelines

There is no automated test suite (`pnpm test` is a placeholder). Validate changes by running `zenn preview` and `textlint`.

## Commit & Pull Request Guidelines

- Commit messages commonly use Conventional Commit-style prefixes: `docs:`, `feat:`, `fix:`, `chore(deps):`.
- PRs should include: what changed, which article(s) impacted, and a screenshot or note that `zenn preview` and `textlint` were run.

## Agent-Specific Instructions

- Output language: write all responses and documentation updates in Japanese.
- When you edit `articles/` or `books/`, run a final check with textlint (use the textlint MCP if available) before responding:
  - `pnpm exec textlint "articles/**/*.md" "books/**/*.md"`
  - `pnpm exec textlint --fix "articles/**/*.md" "books/**/*.md"`

## Security & Local Configuration

- Keep secrets out of git. Use local-only env files (e.g. `.envrc.local`) for API keys and ensure they are not committed.
- If you use `aqua`, install pinned tools from `aqua.yaml` (e.g. `aqua i`) to match the repo’s Node/pnpm versions.


# AI-DLC and Spec-Driven Development

Kiro-style Spec Driven Development implementation on AI-DLC (AI Development Life Cycle)

## Project Memory
Project memory keeps persistent guidance (steering, specs notes, component docs) so Codex honors your standards each run. Treat it as the long-lived source of truth for patterns, conventions, and decisions.

- Use `.kiro/steering/` for project-wide policies: architecture principles, naming schemes, security constraints, tech stack decisions, api standards, etc.
- Use local `AGENTS.md` files for feature or library context (e.g. `src/lib/payments/AGENTS.md`): describe domain assumptions, API contracts, or testing conventions specific to that folder. Codex auto-loads these when working in the matching path.
- Specs notes stay with each spec (under `.kiro/specs/`) to guide specification-level workflows.

## Project Context

### Paths
- Steering: `.kiro/steering/`
- Specs: `.kiro/specs/`

### Steering vs Specification

**Steering** (`.kiro/steering/`) - Guide AI with project-wide rules and context
**Specs** (`.kiro/specs/`) - Formalize development process for individual features

### Active Specifications
- Check `.kiro/specs/` for active specifications
- Use `/prompts:kiro-spec-status [feature-name]` to check progress

## Development Guidelines
- Think in English, generate responses in Japanese. All Markdown content written to project files (e.g., requirements.md, design.md, tasks.md, research.md, validation reports) MUST be written in the target language configured for this specification (see spec.json.language).

## Minimal Workflow
- Phase 0 (optional): `/prompts:kiro-steering`, `/prompts:kiro-steering-custom`
- Phase 1 (Specification):
  - `/prompts:kiro-spec-init "description"`
  - `/prompts:kiro-spec-requirements {feature}`
  - `/prompts:kiro-validate-gap {feature}` (optional: for existing codebase)
  - `/prompts:kiro-spec-design {feature} [-y]`
  - `/prompts:kiro-validate-design {feature}` (optional: design review)
  - `/prompts:kiro-spec-tasks {feature} [-y]`
- Phase 2 (Implementation): `/prompts:kiro-spec-impl {feature} [tasks]`
  - `/prompts:kiro-validate-impl {feature}` (optional: after implementation)
- Progress check: `/prompts:kiro-spec-status {feature}` (use anytime)

## Development Rules
- 3-phase approval workflow: Requirements → Design → Tasks → Implementation
- Human review required each phase; use `-y` only for intentional fast-track
- Keep steering current and verify alignment with `/prompts:kiro-spec-status`
- Follow the user's instructions precisely, and within that scope act autonomously: gather the necessary context and complete the requested work end-to-end in this run, asking questions only when essential information is missing or the instructions are critically ambiguous.

## Steering Configuration
- Load entire `.kiro/steering/` as project memory
- Default files: `product.md`, `tech.md`, `structure.md`
- Custom files are supported (managed via `/prompts:kiro-steering-custom`)
