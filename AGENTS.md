# Repository Guidelines

## Project Structure & Module Organization

ARTEX-KO combines a Go backend with a Next.js/React TypeScript frontend. `cmd/artex/` is the executable entry point; root packages such as `agent/`, `server/`, `db/`, and `report/` implement backend features. Go tests live beside code as `*_test.go`. Frontend routes, components, and styles live in `web/src/`; assets are in `web/public/` and translations in `web/messages/`. `detections/` contains defensive rules and test harnesses; `docs/` contains design documents.

## Build, Test, and Development Commands

Use Go matching `go.mod` (1.26.3), Node.js 22+, and Docker for integration checks.

- `go build ./...` — compile backend packages.
- `go vet ./...` and `go test ./... -count=1` — run static analysis and Go tests.
- From `web/`, run `npm ci`, then `npm run dev` for frontend development.
- From `web/`, run `npm run build:static` for the static export and TypeScript merge gate.
- From `web/`, run `npm run check` or `npm run check:fix` for Biome checks or fixes.
- `docker compose up -d` — start the application and PostgreSQL after copying `.env.example` to `.env` and setting `POSTGRES_PASSWORD`.
- `python3 -I scripts/check-doc-links.py` — validate documentation links.

## Coding Style & Naming Conventions

Format Go with `gofmt` (tabs); use conventional exported Go names and `TestXxx` test functions. Follow surrounding frontend filenames and component conventions. Biome specifies two-space indentation, double quotes, semicolons, and a 120-column width; staged frontend files use lint-staged.

## Testing Guidelines

Add regression tests for changed behavior using Go’s testing framework. Database tests in `agent`, `config`, `db`, `evidence`, `llmrec`, and `server` skip without database configuration. Set `ARTEX_PG_DSN` and test each affected package against a fresh PostgreSQL database; see [CONTRIBUTING.en.md](CONTRIBUTING.en.md). Run `detections/tests/run-all.sh` for detection changes. Existing frontend lint debt is informational; introduce no new errors.

## Commit & Pull Request Guidelines

History uses `type(scope): description` with Korean descriptions; scope is optional. Use topic branches such as `fix/...` or `i18n/...`. Follow the PR template: explain what changed and why, link related issues, record verification, and attach screenshots for UI changes. Add user-facing changes to the changelog’s `[Unreleased]` section.

## Localization & Security

Read the contribution policy before localization changes. Preserve internal reasoning prompts; add UI translations through message keys. Verify runtime behavior only against owned, authorized, or isolated targets. Keep credentials and scan artifacts out of commits; report vulnerabilities through [SECURITY.en.md](SECURITY.en.md).
