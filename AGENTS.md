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
