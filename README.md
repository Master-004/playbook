# GlutenScout Engineering Playbook

> **Welcome to the team!** This is how we build software at GlutenScout. Read it in your first week, follow the [Day 1 checklist](#day-1-checklist), and ask questions anywhere. If a rule here no longer fits, open a PR to change it. Don't quietly work around it.

Think of this like a sports team's playbook: everyone can be talented, but without shared plays you get collisions — inconsistent code, merge conflicts, bugs and silos.

**Quick links**

| What | Link |
| --- | --- |
| Live project board | [GitHub Projects — GlutenScout](https://github.com/users/Master-004/projects/2/views/1) |
| This playbook (source) | [github.com/Master-004/playbook](https://github.com/Master-004/playbook) |
| Starter kit (config files every repo uses) | [`starter-kit/`](starter-kit/) |
| Decisions (ADRs) | [`docs/adr/`](docs/adr/) |
| IDE download | [VS Code](https://code.visualstudio.com/download) |
| Go download | [go.dev/dl](https://go.dev/dl/) |

## Contents

- [Day 1 checklist](#day-1-checklist)
- [1. Language Selection](#1-language-selection)
- [2. IDE Selection and Setup](#2-ide-selection-and-setup)
- [3. Development Strategy](#3-development-strategy)
- [4. Git Strategy and Controls](#4-git-strategy-and-controls)
- [5. Issue Tracking](#5-issue-tracking)
- [6. AI Agents and Controls](#6-ai-agents-and-controls)
- [7. Code Quality](#7-code-quality)
- [8. Testing Frameworks](#8-testing-frameworks)
- [9. Other Controls, Tools, and Procedures](#9-other-controls-tools-and-procedures)
  - [9.1 Infrastructure and deployment](#91-infrastructure-and-deployment)
  - [9.2 Secrets management](#92-secrets-management)
  - [9.3 Account security and MFA](#93-account-security-and-mfa)
  - [9.4 Templates, shared packages and approved libraries](#94-templates-shared-packages-and-approved-libraries)
  - [9.5 Architecture Decision Records (ADRs)](#95-architecture-decision-records-adrs)
- [Learning resources](#learning-resources)

---

## Day 1 checklist

- [ ] Get your work email and turn on MFA everywhere ([9.3](#93-account-security-and-mfa))
- [ ] Accept the GitHub invite and turn on GitHub two-factor authentication
- [ ] Install Go, Git, VS Code and Docker ([section 2](#2-ide-selection-and-setup))
- [ ] Get access to the [project board](https://github.com/users/Master-004/projects/2/views/1)
- [ ] Get approved AI tool accounts (company plan only — [section 6](#6-ai-agents-and-controls))
- [ ] Read sections 1–9 of this playbook
- [ ] Do the **Must do** items in [Learning resources](#learning-resources) (first 2 weeks)
- [ ] Pick a `good first issue` from the board and pair with a teammate on your first PR

---

## 1. Language Selection

**We write backend services in Go (Golang).** Python or TypeScript are allowed only where Go is a poor fit (for example a browser UI or a one-off data script) and need a short note in the PR saying why. Decision record: [ADR-0001](docs/adr/0001-use-go.md).

**Why Go**

| Reason | What it means for us |
| --- | --- |
| Popular and well-supported | Lots of packages and answers online, so we rarely build custom libraries |
| Easy to learn | Small, simple language — new engineers are productive fast |
| AI tools are good at it | Lots of public Go code means AI assistants write better Go |
| Fits our needs | Fast, handles many requests at once, compiles to one file that's easy to deploy |
| Founder knows it best | The founder is hands-on early, so reviews and help are faster |

**How we choose a language for something new:** pick the popular option the team already knows, unless a specific strength (raw speed, data science, browser UI) clearly wins. A new language needs an [ADR](#95-architecture-decision-records-adrs).

---

## 2. IDE Selection and Setup

**Our default editor is [VS Code](https://code.visualstudio.com/download) (free).** You may use another editor (like GoLand) if you prefer, but the shared lint, format and test checks still decide whether code is ready.

Why VS Code: free, excellent Go support through the official extension, works with our approved AI tools, and everyone gets the same setup from files in the repo.

### Setup steps

1. **Install Go:** [go.dev/dl](https://go.dev/dl/) — use the version in the repo's `go.mod`. Check with `go version`.
2. **Install Git:** [git-scm.com/downloads](https://git-scm.com/downloads), then:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "you@glutenscout.com"
   ```
3. **Install VS Code:** [code.visualstudio.com/download](https://code.visualstudio.com/download)
4. **Install the Go extension:** [golang.go](https://marketplace.visualstudio.com/items?itemName=golang.go), then open the Command Palette (`Ctrl/Cmd+Shift+P`) → **Go: Install/Update Tools** → select all.
5. **Install Docker Desktop:** [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/) (for running the app and integration tests locally).
6. **Install the dev tools:**
   ```bash
   go install golang.org/x/tools/cmd/goimports@latest
   go install golang.org/x/vuln/cmd/govulncheck@latest
   go install github.com/evilmartians/lefthook@latest
   # golangci-lint: https://golangci-lint.run/welcome/install/
   # gitleaks:      https://github.com/gitleaks/gitleaks#installing
   ```
7. **Clone your repo and open it in VS Code.** Accept the **"Install recommended extensions"** prompt (from [`.vscode/extensions.json`](starter-kit/.vscode/extensions.json)).
8. **Turn on git hooks:** `lefthook install`
9. **Run the checks:** `make check`. If it passes, you're ready.

Shared editor settings live in [`.vscode/settings.json`](starter-kit/.vscode/settings.json): format on save with `goimports`, and `golangci-lint` as the linter. Don't change these per person — propose changes in a PR.

---

## 3. Development Strategy

**We run a Scrum-ban mix:** a Kanban board where work flows continuously, plus a light 2-week rhythm for planning and showing off what we built. We get Scrum's regular check-ins without forcing everything into a fixed sprint box.

> "If your team can't function with the added organization of Scrum, it will often struggle with Kanban." We keep just enough structure to stay organized.

### Roles

- **Product owner** (founder for now) — decides what matters most and keeps the **Ready** column ordered.
- **Engineers** — pull the top issue, build it, review each other's PRs.
- **Facilitator** — rotates every cycle; runs meetings and keeps them short.

### Meetings

| Meeting | When | Length | Purpose |
| --- | --- | --- | --- |
| Daily standup | Every workday, 9:30 am MT | 15 min | Yesterday, today, anything blocking you |
| Grooming + prioritization | Weekly, Tuesday | 30–45 min | Clarify and size issues with **pointing poker**; order the Ready column |
| Planning | Day 1 of each 2-week cycle | 30–60 min | Pick the goal for the next 2 weeks |
| Showcase | Last day of each cycle | 30–45 min | Demo what shipped and get feedback — make it fun |
| Retrospective | Last day of each cycle | 30 min | What went well, what didn't, one thing to change |

### A day in the life

1. Standup → pull the top issue from **Ready**, assign yourself, move it to **In Progress**.
2. Branch off `main`, make small commits, open a PR the same day or next.
3. Review a teammate's PR before starting something new (unblocking others comes first).
4. Merge, watch the deploy to `dev`, move the issue to **Done**.

### Culture

Showcases are a reward for hard work, not a status report. We keep traditions like pointing poker and playful demos (short funny videos are welcome). Celebrate what shipped, even when it's small.

---

## 4. Git Strategy and Controls

**We use trunk-based development.** `main` is the only long-lived branch, and everyone merges small changes into it at least once a day. Like a highway with short on-ramps: nobody drives a side road for weeks and then tries to merge all at once. Decision record: [ADR-0002](docs/adr/0002-trunk-based-development.md).

> **New to trunk-based?** It feels backwards if you're used to long feature branches. Read [trunkbaseddevelopment.com](https://trunkbaseddevelopment.com) and [Feature Toggles](https://martinfowler.com/articles/feature-toggles.html) before your first PR.

### Branches

| Branch | Lives for | Rule |
| --- | --- | --- |
| `main` | Forever | Always deployable. Protected — no direct pushes |
| `feat/<issue#>-short-name` | Hours to 2 days | Branch off `main`, open a PR, squash-merge, delete |
| `fix/<issue#>-short-name` | Hours | Same as feature, for bugs |
| Hotfix | Minutes to hours | A `fix/` branch labeled `urgent`; reviewed first |

- **Releases are tags** on `main` (`v1.4.0`, using [Semantic Versioning](https://semver.org)), not branches.
- **Unfinished work hides behind a feature flag** instead of living on a long branch.

### Commit conventions

We use [Conventional Commits](https://www.conventionalcommits.org), checked by a git hook ([`lefthook.yml`](starter-kit/lefthook.yml)):

```text
feat: add restaurant search by zip code
fix: handle empty menu response
chore: bump golangci-lint to latest
docs: explain how to run e2e tests
```

Allowed types: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `ci`, `perf`, `build`, `revert`.

### Pull request rules

1. Keep PRs small — under about **400 changed lines**.
2. Use the [PR template](.github/pull_request_template.md) and write `Closes #123` to link the issue.
3. All CI checks pass (format, lint, tests, vulnerability scan, secret scan, build).
4. An **AI reviewer** comments on every PR, but only a **full-time employee (FTE)** can approve it.
5. **1 approval from an FTE other than the author.**
6. **Squash-merge** into `main`, then delete the branch.

### Repo controls (set on every repo)

Three simple rules: **FTEs need another FTE to approve. CODEOWNERS makes sure only FTEs count. Only FTEs can tag and release to production.** Contractors and AI agents can open PRs, but can't approve or release them.

**Control 1 — Protect `main` (ruleset)**

1. Repo → **Settings → Rules → Rulesets → New ruleset → New branch ruleset**.
2. Name: `protect-main`. Enforcement: **Active**. Target branches: **Include default branch**.
3. Turn on:
   - **Restrict deletions** and **Block force pushes**
   - **Require a pull request before merging** → Required approvals: **1** → check **Dismiss stale approvals**, **Require review from Code Owners**, and **Require approval of the most recent reviewable push**
   - **Require status checks to pass** → add `ci / checks`
4. **Create**.

**Control 2 — CODEOWNERS**

The file [`.github/CODEOWNERS`](.github/CODEOWNERS) lists who must approve changes. Combined with "Require review from Code Owners" above, only people listed there count as approvers. Today it's the founder; when we move to a GitHub organization it becomes the `@glutenscout/fte` team.

**Control 3 — Only FTEs release to production**

1. **Tag ruleset:** Settings → Rules → Rulesets → **New tag ruleset** → name `protect-release-tags` → target pattern `v*` → turn on **Restrict creations**, **Restrict updates**, **Restrict deletions** → add FTEs (or the `fte` team) to the **Bypass list**.
2. **Production environment:** Settings → **Environments → New environment** → name `production` → check **Required reviewers** and add FTEs → under **Deployment branches and tags**, allow only tags matching `v*`.

The deploy job uses `environment: production`, so it pauses until an FTE clicks **Approve**.

> **Cost note:** rulesets, required reviewers and code owners on **private** repos need a paid plan — [GitHub Pro](https://github.com/pricing) for a personal account or **GitHub Team** for an organization (Team was $4/user/month for the first 12 months at time of writing).

---

## 5. Issue Tracking

**All work lives on our board: [GitHub Projects — GlutenScout](https://github.com/users/Master-004/projects/2/views/1).** Each task is a GitHub Issue, so issues and PRs link automatically. If it isn't on the board, it isn't being worked on. Decision record: [ADR-0003](docs/adr/0003-github-projects.md).

**Why we track work:** to prioritize the next iteration, not forget important things, keep everyone on the right tasks, and always know where things stand.

### Board columns (Status field)

| Column | Meaning | Limit |
| --- | --- | --- |
| **Backlog** | Every idea and request | — |
| **Ready** | Groomed, sized, ordered by priority. Pull from the top | — |
| **In Progress** | Someone is actively building it | 2 per engineer |
| **In Review** | A PR is open | — |
| **Done** | Merged to `main` and deployed | — |

### Issue rules

- Create issues with the templates: [Feature](.github/ISSUE_TEMPLATE/feature.yml) or [Bug](.github/ISSUE_TEMPLATE/bug.yml). Each has acceptance criteria ("done means…").
- Every issue in **Ready** has an assignee-to-be, a **Points** size (1, 2, 3, 5, 8) and a label.
- Labels: `feature`, `bug`, `chore`, `urgent`, `good first issue`.
- Branch names use the issue number: `feat/123-restaurant-search`.
- Write `Closes #123` in the PR. When it merges, the issue closes and moves to **Done**.

**Board automation** (Project → **⋯ → Workflows**): turn on *Item added to project → Backlog*, *Pull request linked → In Review*, and *Item closed → Done*.

---

## 6. AI Agents and Controls

**Approved tools: [Claude / Claude Code](https://claude.com/product/claude-code) and [ChatGPT / Codex](https://openai.com/codex/).** Treat AI like a fast junior teammate: helpful, but a human always signs off.

### Rules

1. **Company business/team plans only.** Use company accounts on plans that don't train on our code. No personal or free accounts for company work.
2. **No secrets in prompts.** Never paste API keys, passwords, customer data or `.env` files into an AI tool.
3. **Tests required for AI-written code.** Any feature or fix written mostly by AI ships with tests that cover it.
4. **AI reviews, FTEs approve.** An AI reviewer comments on every PR; only an FTE can approve and merge.
5. **You own what you merge.** If AI wrote it and you opened the PR, it's your code. Understand it before asking for review.
6. **New AI tool?** Propose it with an [ADR](#95-architecture-decision-records-adrs) before using it on company code.

### Guardrails that are already set up

| Guardrail | File | What it does |
| --- | --- | --- |
| Agent instructions | [`AGENTS.md`](starter-kit/AGENTS.md) | Rules every agent reads (Codex reads `AGENTS.md`; Claude reads it through `CLAUDE.md`) |
| Claude instructions | [`CLAUDE.md`](starter-kit/CLAUDE.md) | Imports `AGENTS.md` so both tools follow the same rules |
| Claude Code permissions | [`.claude/settings.json`](starter-kit/.claude/settings.json) | Blocks Claude Code from reading `.env`, key and secret files ([docs](https://code.claude.com/docs/en/settings)) |
| Secret scan before commit | [`lefthook.yml`](starter-kit/lefthook.yml) | `gitleaks` stops a commit that contains a secret — whether a human or an AI wrote it |
| CI checks | [`go-ci.yml`](.github/workflows/go-ci.yml) | AI code goes through the same lint, test and security checks as human code |

These lower the risk but aren't a guarantee. That's why the most important rule is in [9.2](#92-secrets-management): **production secrets never live on laptops**, so there's nothing for an AI to accidentally read.

---

## 7. Code Quality

Every check runs automatically, so quality doesn't depend on anyone remembering. Like a car's safety inspection: the car doesn't leave the lot until it passes.

| Tool | What it does | When it runs | Config |
| --- | --- | --- | --- |
| [gofmt](https://pkg.go.dev/cmd/gofmt) + [goimports](https://pkg.go.dev/golang.org/x/tools/cmd/goimports) | Formats code the same way for everyone; sorts imports | On save, pre-commit, CI (fails if unformatted) | [`.vscode/settings.json`](starter-kit/.vscode/settings.json), [`.golangci.yml`](starter-kit/.golangci.yml) |
| [golangci-lint](https://golangci-lint.run) v2 | Runs many linters at once (bugs, security, style) | Pre-commit, CI | [`.golangci.yml`](starter-kit/.golangci.yml) |
| [gosec](https://github.com/securego/gosec) (inside golangci-lint) | Finds insecure code patterns | Pre-commit, CI | [`.golangci.yml`](starter-kit/.golangci.yml) |
| [depguard](https://github.com/OpenPeeDeeP/depguard) (inside golangci-lint) | Blocks any library not on the approved list | Pre-commit, CI | [`.golangci.yml`](starter-kit/.golangci.yml) |
| [govulncheck](https://go.dev/doc/tutorial/govulncheck) | Scans our code and libraries for known vulnerabilities | CI on every PR + weekly | [`go-ci.yml`](.github/workflows/go-ci.yml) |
| [gitleaks](https://github.com/gitleaks/gitleaks) | Finds committed secrets | Pre-commit, CI | [`lefthook.yml`](starter-kit/lefthook.yml), [`go-ci.yml`](.github/workflows/go-ci.yml) |
| [Dependabot](https://docs.github.com/en/code-security/dependabot) | Opens PRs to update outdated or vulnerable libraries | Weekly | [`dependabot.yml`](starter-kit/.github/dependabot.yml) |
| [Lefthook](https://github.com/evilmartians/lefthook) | Runs the checks above as git hooks (the Go-friendly Husky) | Every commit | [`lefthook.yml`](starter-kit/lefthook.yml) |
| [GitHub Actions](https://docs.github.com/en/actions) | Runs all checks + tests + build in the cloud | Every PR and merge to `main` | [`go-ci.yml`](.github/workflows/go-ci.yml) |
| [`log/slog`](https://pkg.go.dev/log/slog) | Go's built-in structured (JSON) logging | Always; logs go to AWS CloudWatch | via `platform/log` |

**Run everything locally** with one command: `make check` (see the [`Makefile`](starter-kit/Makefile)).

**One shared CI workflow.** [`go-ci.yml`](.github/workflows/go-ci.yml) is a [reusable workflow](https://docs.github.com/en/actions/sharing-automations/reusing-workflows). Each app repo calls it with a few lines ([`starter-kit/.github/workflows/ci.yml`](starter-kit/.github/workflows/ci.yml)), so every repo runs the same checks and we update them in one place. (If this playbook repo is private, allow it in **Settings → Actions → General → Access → "Accessible from repositories owned by the user"** so other repos can call it.)

**Turn on GitHub's free security features** in each repo: Settings → **Advanced Security** (called *Code security* on some accounts) → enable **Dependabot alerts** and **Dependabot security updates**.

---

## 8. Testing Frameworks

**Tests prove every change is on purpose.** Our goal is **80% code coverage**. The shared CI workflow reports coverage and warns when it is below the goal; it does not currently block a merge based on coverage.

| Test type | What it checks | Tools | When it runs |Overview
Imagine you have just hired a new developer. Before their first day, you want a playbook you can hand them that explains exactly what your team uses and how to get up and running -- a getting-started guide. 

Your job is to create a tech employee playbook: something you could hand to a new team member that describes what your team is using and how to get started. Document the items discussed in class, listed below.

Key rule: Be specific. Wherever possible, have things actually set up -- a real link to your Kanban board, a real link to download the IDE, real instructions for setting up a control -- rather than describing tools in the abstract.

Note: Although this assignment seems big, use AI to make it manageable. That is what you will need to do in a real startup. This is a realistic time span to put this together. You instruct it, it creates, and you review/edit. 

What to Include
Your playbook should document all nine of the following areas:

1. Language Selection
Which programming language(s) your team uses, and why.

2. IDE Selection and Setup
Which IDE your team uses, a link to download it, and setup steps.

3. Development Strategy
Your team's process (e.g., Scrum, Kanban) and how it runs day to day.

4. Git Strategy and Controls
Branching strategy, commit conventions, and review/approval controls.

5. Issue Tracking
The tool you use to track work, plus a link to your live board.

6. AI Agents and Controls
Which AI tools are approved for use, and the guardrails around them.

7. Code Quality
Linting, formatting, and vulnerability scanning tools and configs.

8. Testing Frameworks
What you use to test code, and how testing fits your workflow.

9. Other Controls, Tools, and Procedures
Anything else a new hire would need -- deployment steps, secrets handling, on-call, etc.

Format and Submission
Your playbook must be accessible -- for example, a webpage (an AI tool can build this for you!), a shared doc, or a wiki page. It should be easy for a new hire to open and navigate, not a wall of unformatted text.

Tip: Write it like you would hand it to a real new hire on day one. If a specific link, command, or setup step would help them get started faster, include it.

Watch out: Generic answers cost points. "We use good coding practices" will not score as well as "We use ESLint with the Airbnb config, enforced through a pre-commit hook."

Submit a link to your published playbook in this assignment's submission box.

Grading
Points	What This Looks Like
5 / 5	You have a playbook covering all nine items above. You are specific, and where possible, things are actually set up (e.g., a link to your Kanban board, a link to download the IDE, instructions for setting up a control). The playbook itself is in an accessible format (e.g., a webpage).
| --- | --- | --- | --- |
| **Unit** | One function returns the right output for an input | `go test` + [testify](https://github.com/stretchr/testify) | Every PR |
| **Component** | One part of the app still works and hasn't disappeared (e.g. the sign-in handler) | `go test` + [httptest](https://pkg.go.dev/net/http/httptest) | Every PR |
| **Integration** | Our code works with real services like the database | [Testcontainers for Go](https://golang.testcontainers.org) | Run with `make test-int`; add to an app's CI as needed, not currently in the shared workflow |
| **API** | Each endpoint returns the right responses | [Postman](https://www.postman.com) collections with [Newman](https://github.com/postmanlabs/newman) | Add to an app's CI when API collections are available; not currently in the shared workflow |
| **End-to-end** | A full user journey works: sign up → search restaurants → save a favorite | [Playwright](https://playwright.dev) | Add to an app's CI when the app has a runnable user journey; not currently in the shared workflow |

### Commands

```bash
make test        # unit + component tests with the race detector and coverage
make test-int    # integration tests (needs Docker running)
make cover       # open the coverage report in your browser
```

Tests live next to the code they test (`search.go` → `search_test.go`). Integration tests use the build tag `//go:build integration`.

The shared workflow currently runs unit and component Go tests on every PR. Integration, API and end-to-end tests need to be configured for each app; they are not currently run by the shared workflow.

### How we grow coverage

1. **Start with what matters most.** List the most critical functions and write example inputs and expected outputs for each.
2. **Let AI fill in the rest.** An AI agent runs nightly and writes a test for the next critical function on the list. A human reviews it like any PR.
3. **Every bug gets a regression test.** Write a test that fails because of the bug, then fix it so it passes. That bug can't sneak back.
4. **Record end-to-end flows.** Record yourself doing a user journey with [Playwright codegen](https://playwright.dev/docs/codegen), then refine it into a test (AI can help).

---

## 9. Other Controls, Tools, and Procedures

### 9.1 Infrastructure and deployment

**We host on AWS.** Web apps and APIs run on **[ECS on Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html)**; small background jobs run on **[Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)**. Everything is written as code with **[OpenTofu](https://opentofu.org)** (the open-source fork of Terraform). Decision record: [ADR-0004](docs/adr/0004-aws-ecs-fargate.md).

| Piece | Analogy | What it does |
| --- | --- | --- |
| **ECS on Fargate** | A furnished apartment | We hand AWS a container (our Go app in a box); Fargate runs it with no servers to manage, restarts crashed copies, and adds more when traffic grows |
| **Lambda** | A taxi | Runs a small function only when triggered (schedule, upload, queue message) and charges per run — nightly jobs, webhooks, emails |
| **OpenTofu** | A recipe | Anyone can rebuild the whole kitchen from the recipe instead of from memory |

**Request path:** user → **Application Load Balancer** (front door) → one of our **Fargate tasks** in private subnets → reads secrets from **Secrets Manager** and data from the database → logs to **CloudWatch**.

**Rules**

- **No click-ops.** Nothing is created by hand in the AWS console. If it exists, it's in `infra/`.
- **Infra changes go through PRs** and show the `tofu plan` output so reviewers see exactly what will change.
- **Separate environments:** `dev` and `prod` in separate AWS accounts, built from the same modules.
- **Shared, locked state:** OpenTofu state lives in S3 with locking, never on a laptop.
- **Least access:** sign in through AWS IAM Identity Center with only the permissions you need. Only FTEs get prod access.
- **Cost alerts:** AWS Budgets emails the founder if monthly spend passes the budget.

**How a deploy works**

1. You merge a PR to `main`.
2. GitHub Actions builds the container and pushes it to **Amazon ECR** (our container storage).
3. ECS rolls it out to **`dev`**, replacing old copies one at a time (no downtime).
4. To release to prod, an FTE creates a tag: `git tag v1.4.0 && git push origin v1.4.0`.
5. The deploy pauses at the `production` environment until an FTE approves it, then rolls out to **`prod`**.
6. **Rollback:** re-run the deploy for the previous tag.

### 9.2 Secrets management

**Secrets (API keys, database passwords, tokens) live in [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)** — never in code, chat or AI prompts. Think of it as a bank vault: each app gets a key card (an IAM role) that opens only its own box.

**Why not Vault?** We're already on AWS, so Secrets Manager plugs straight into ECS and Lambda with nothing extra to run.

| Option | What it is | Notes |
| --- | --- | --- |
| **AWS Secrets Manager** ✅ | AWS's built-in vault | Our pick. Auto-rotation for databases; pay per secret |
| AWS SSM Parameter Store | Simple AWS settings store | For non-secret config like feature flags |
| [HashiCorp Vault](https://www.vaultproject.io) | Powerful self-run vault | Lots to operate; license is no longer open source |
| [OpenBao](https://openbao.org) | Open-source fork of Vault | Same power, same upkeep |
| [Doppler](https://www.doppler.com) / [Infisical](https://infisical.com) | Hosted secrets platforms | Nice developer experience; another vendor to trust |

**Rules**

1. **Prod secrets never touch laptops.** Only the running app reads them, through its IAM role.
2. **Local dev uses dev-only values** in `.env.local`, which is in [`.gitignore`](starter-kit/.gitignore).
3. **Rotate** at least every 90 days and whenever someone leaves.
4. **Shared logins** (AWS root, domain registrar, GitHub owner) live in the company password manager.

**Layers of defense** — like locks on the door, the window and the gate:

| Layer | What it catches | Cost |
| --- | --- | --- |
| [`.gitignore`](starter-kit/.gitignore) | `.env*`, key files and credentials never get staged | Free |
| gitleaks pre-commit hook | Secrets in a commit, before it leaves your laptop | Free |
| gitleaks in CI | Anything that slipped past the hook | Free |
| Claude Code deny rules | AI reading secret files | Free |
| [GitHub secret scanning + push protection](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning) | Blocks pushes containing known key formats | Free on public repos; paid **Secret Protection** add-on for private repos (we'll add it as we grow) |

**If a secret leaks**

1. **Rotate it right away** in AWS. Deleting the commit isn't enough — assume it's already copied.
2. Tell the founder immediately.
3. Remove it from Git history and check logs for misuse.
4. Add a gitleaks rule or test so it can't happen the same way again.

### 9.3 Account security and MFA

**Multi-factor authentication (MFA) is required on every company account. No exceptions.** A password is a key that can be copied; MFA adds a second lock that only opens with something you physically have.

**MFA rules**

1. **Strong MFA only.** Best: a passkey or hardware security key (e.g. [YubiKey](https://www.yubico.com)). Good: an authenticator app. **Not allowed: SMS codes** (phone numbers can be stolen by SIM swap).
2. **Admins use hardware keys.** Anyone with owner, admin or prod access registers **2** keys — one daily, one backup in a safe place.
3. **Save recovery codes** in the company password manager — never in a note or screenshot.
4. **Lost a phone or key?** Tell the founder right away so the old device can be removed.

**Where it's enforced**

| Account | How |
| --- | --- |
| GitHub | Org settings → [Require two-factor authentication](https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-two-factor-authentication-for-your-organization/requiring-two-factor-authentication-in-your-organization) for everyone |
| AWS | Sign in only through IAM Identity Center with MFA required. **Root account** locked with a hardware key and never used day to day |
| Google Workspace | Admin console → enforce 2-Step Verification |
| Password manager | MFA required on the team vault |
| Domain registrar / DNS | MFA + registrar lock (whoever controls the domain controls our email and website) |
| AI tools, Slack, others | MFA on, or "Sign in with Google" so Google's MFA protects them |

**Other account rules:** work email for every work tool · one person, one account (no shared logins) · just 2 owners on GitHub, AWS and Google · a unique password for everything via the password manager · **access review every 3 months** · remove all access on someone's last day.

### 9.4 Templates, shared packages and approved libraries

We don't start from a blank page, and we don't each reinvent sign-in or logging. Like LEGO: everyone builds with the same approved bricks, so the pieces always fit.

**Starter web app template.** Every new web app starts from the `webapp-template` repo (click **Use this template** on GitHub — don't copy files by hand). It includes everything in [`starter-kit/`](starter-kit/) plus:

- Go service layout, config loading and graceful shutdown
- Our shared `platform` packages already wired in
- A sample unit, component and end-to-end test
- Dockerfile and an OpenTofu module for deploying to ECS Fargate
- A README with "run it locally in 5 minutes"

**Shared internal packages** live in our own Go module, `platform`, so app code stays short:

| Package | What it gives engineers |
| --- | --- |
| `platform/auth` | Sign-in, sessions and permission checks as one line of middleware |
| `platform/log` | Pre-configured `slog` JSON logging with request IDs |
| `platform/ui` | Shared UI components and styles so every app looks the same |
| `platform/httpx` | Standard server setup, middleware, error responses, health checks |
| `platform/config` | Loads settings and secrets from AWS the same way in every app |
| `platform/db` | Database connection, migrations and test helpers |

Shared packages are versioned with tags, have their own tests and changelog, and breaking changes need an ADR.

**Approved packages only.** The approved list is the `depguard` **allow** list in [`.golangci.yml`](starter-kit/.golangci.yml). Anything else fails lint locally and in CI. Fewer, well-known libraries means fewer security holes and less to learn.

| Custom CI check | What it blocks | Tool |
| --- | --- | --- |
| Approved dependencies | Libraries not on the allow list | depguard |
| Licenses | Licenses we can't use (e.g. GPL in closed code) | [go-licenses](https://github.com/google/go-licenses) |
| Known vulnerabilities | Libraries with published security holes | govulncheck |
| Secrets | Committed keys and passwords | gitleaks |

### 9.5 Architecture Decision Records (ADRs)

**Big technical decisions and every new package get a short written record** in [`docs/adr/`](docs/adr/). This is our shared ADR page: anyone can read why we chose what we chose.

**To propose one**

1. Copy [`0000-template.md`](docs/adr/0000-template.md) to the next number, e.g. `docs/adr/0005-use-chi-router.md`.
2. Fill it in and open a PR titled `ADR-0005: Use chi router`.
3. Leave it open for comments for at least **2 working days**.
4. The founder (later, a tech lead) approves → status becomes **Accepted** and it merges. For a new package, add it to the `depguard` allow list in the same PR.

**Current ADRs**

| # | Decision | Status |
| --- | --- | --- |
| [0001](docs/adr/0001-use-go.md) | Use Go for backend services | Accepted |
| [0002](docs/adr/0002-trunk-based-development.md) | Use trunk-based development | Accepted |
| [0003](docs/adr/0003-github-projects.md) | Track work in GitHub Projects | Accepted |
| [0004](docs/adr/0004-aws-ecs-fargate.md) | Host on AWS ECS Fargate + Lambda with OpenTofu | Accepted |

---

## Learning resources

Do the **Must do** items in your first two weeks.

| Topic | Resource | Priority |
| --- | --- | --- |
| Go basics | [A Tour of Go](https://go.dev/tour/) | Must do |
| Go by example | [Go by Example](https://gobyexample.com) | Must do |
| Writing good Go | [Effective Go](https://go.dev/doc/effective_go) | Must do |
| Go review habits | [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments) | Must do |
| Trunk-based development | [trunkbaseddevelopment.com](https://trunkbaseddevelopment.com) | Must do |
| Trunk-based development | [Atlassian: Trunk-based development](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development) | Must do |
| Feature flags | [Martin Fowler: Feature Toggles](https://martinfowler.com/articles/feature-toggles.html) | Must do |
| Commit messages | [Conventional Commits](https://www.conventionalcommits.org) | Must do |
| Kanban | [Atlassian: What is Kanban](https://www.atlassian.com/agile/kanban) | Must do |
| GitHub Projects | [Quickstart for Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/quickstart-for-projects) | Must do |
| Testing in Go | [Go: Add a test](https://go.dev/doc/tutorial/add-a-test) | Must do |
| Vulnerability scanning | [govulncheck tutorial](https://go.dev/doc/tutorial/govulncheck) | Must do |
| Git basics | [Pro Git book (free), ch. 1–3](https://git-scm.com/book/en/v2) | If new to Git |
| Git practice | [Learn Git Branching](https://learngitbranching.js.org) | If new to Git |
| Scrum | [The Scrum Guide](https://scrumguides.org/scrum-guide.html) | Nice to have |
| GitHub Actions | [Actions quickstart](https://docs.github.com/en/actions/writing-workflows/quickstart) | Nice to have |
| OpenTofu | [OpenTofu intro](https://opentofu.org/docs/intro/) | Before touching infra |
| AWS | [AWS Skill Builder — Cloud Practitioner Essentials](https://skillbuilder.aws) | Before touching infra |
| ADRs | [adr.github.io](https://adr.github.io) | Before your first ADR |

Found a better resource? Open a PR and add it.
