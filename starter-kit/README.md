# Starter kit

Config files every GlutenScout repo uses. The `webapp-template` repo already includes all of these;
for an existing repo, copy the whole folder's contents into the repo root.

| File | Purpose | Playbook section |
| --- | --- | --- |
| `.golangci.yml` | Linters, formatters, and the **approved packages list** | 7, 9.4 |
| `lefthook.yml` | Git hooks: format, lint, secret scan, commit message check | 4, 7 |
| `Makefile` | `make check` runs everything CI runs | 7, 8 |
| `.github/workflows/ci.yml` | Calls the shared CI workflow | 7 |
| `.github/dependabot.yml` | Weekly dependency updates | 7 |
| `CODEOWNERS` | Only FTEs approve (move to `.github/CODEOWNERS`) | 4 |
| `.gitignore` | Keeps secrets and build files out of Git | 9.2 |
| `.claude/settings.json` | Blocks Claude Code from reading secret files | 6 |
| `AGENTS.md` / `CLAUDE.md` | Rules for AI agents | 6 |
| `.vscode/` | Shared editor settings and recommended extensions | 2 |
