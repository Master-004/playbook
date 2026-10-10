# ADR-0001: Use Go for backend services

- **Status:** Accepted
- **Date:** 2026-10-09
- **Author:** Founder

## Context
We need one main language so code is consistent, reviews are fast, and new hires ramp up quickly.

## Decision
All backend services are written in Go. Python or TypeScript only where Go is a poor fit, with a note in the PR.

## Options considered
| Option | Pros | Cons |
| --- | --- | --- |
| **Go** | Simple, fast, one-file deploys, great concurrency, strong AI support, founder knows it | Smaller web-UI ecosystem |
| Python | Huge ecosystem, founder knows it | Slower, packaging/deploys are messier |
| TypeScript | Same language front and back end | Heavier tooling; founder less familiar |

## Consequences
Hiring targets Go (or engineers willing to learn it). Shared packages are built once, in Go.
