# ADR-0002: Use trunk-based development

- **Status:** Accepted
- **Date:** 2026-10-09
- **Author:** Founder

## Context
We need a branching model that keeps merge conflicts small and works well with AI agents making frequent changes.

## Decision
`main` is the only long-lived branch. Short-lived `feat/` and `fix/` branches merge within 1-2 days. Releases are `v*` tags. Unfinished work hides behind feature flags.

## Options considered
| Option | Pros | Cons |
| --- | --- | --- |
| **Trunk-based** | Tiny merges, always-releasable `main`, fits CI/CD and AI | Needs feature flags and good tests |
| GitHub Flow | Simple | Branches can live too long |
| Git Flow | Clear release stages | Heavy for a small team; big merge conflicts |

## Consequences
Strong CI and tests are required. Engineers new to this read the trunk-based resources in the playbook.
