# Instructions for AI agents (Claude Code, Codex, and others)

You are working on a GlutenScout Go repository. Follow the engineering playbook:
https://github.com/Master-004/playbook

## Secrets (most important)
- Never read, print, copy or commit secrets: `.env*`, `*.pem`, `*.key`, `*.tfvars`, `*.tfstate`, `~/.aws`, `~/.ssh`.
- Never hard-code API keys, passwords or tokens. Secrets come from AWS Secrets Manager via `platform/config`.
- If you find something that looks like a secret, stop and tell the human. Do not repeat it.

## Code
- Language: Go. Format with `goimports`. Code must pass `make check`.
- Only use packages on the approved list (the depguard `allow` list in `.golangci.yml`).
  If you need another package, say so and suggest an ADR. Do not add it yourself.
- Use our shared packages: `platform/log` for logging, `platform/auth` for auth,
  `platform/httpx` for HTTP servers, `platform/config` for settings.
- Unfinished features go behind a feature flag.

## Tests
- Every change you write ships with tests (unit and/or component). Aim for 80% coverage.
- When fixing a bug, first write a test that fails because of the bug.

## Git
- Small changes. Branch names: `feat/<issue#>-short-name` or `fix/<issue#>-short-name`.
- Commit messages follow Conventional Commits (`feat:`, `fix:`, `chore:` ...).
- You may open PRs, but never approve, merge, tag releases, or push to `main`.
