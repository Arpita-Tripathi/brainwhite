# repo-starter

Starter files for the GitHub repository. For now they live in the `startup/` folder. When the real `brainwhite` repo is created, copy them into its root. The exact commands are in `docs/SETUP.md`, section 3.

## Replace these first

| Placeholder | Where | Replace with |
|---|---|---|
| `@LEAD`, `@P1`, `@P2`, `@P3`, `@P4` | `.github/CODEOWNERS` | Real GitHub usernames |
| `<BACKEND_TEST_COMMAND>` and similar | `CLAUDE.md` | The real commands, once the project exists |

## Files

| File | Purpose |
|---|---|
| `.github/CODEOWNERS` | Who must review which folder |
| `.github/pull_request_template.md` | Checklist for every pull request |
| `.github/ISSUE_TEMPLATE/` | Task and bug templates |
| `.github/workflows/ci.yml` | Checks on every pull request (skeleton) |
| `.github/workflows/cd.yml` | `develop` to staging, `main` to production (skeleton, deploy steps are placeholders) |
| `.github/dependabot.yml` | Weekly dependency updates |
| `infra/docker-compose.yml` | Local Postgres (pgvector) and MinIO |
| `CLAUDE.md` | Shared rules for every AI assistant on the team |
| `.claude/settings.json` | Shared Claude Code permissions (allow, ask, deny) |
| `.gitignore`, `.env.example` | Keep secrets and local data out of Git |
| `docs-adr/000-template.md` | Template for decision records (copy to `docs/adr/`) |

## Status

Nothing here has been run on a real repo. `ci.yml` and `cd.yml` are skeletons; jobs skip until the project files exist, and the deploy steps fail on purpose until a real deploy step is written. CODEOWNERS only blocks merges if "Require review from Code Owners" is on in the branch rules.
