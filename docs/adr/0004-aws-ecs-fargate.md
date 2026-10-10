# ADR-0004: Host on AWS ECS Fargate + Lambda, managed with OpenTofu

- **Status:** Accepted
- **Date:** 2026-10-09
- **Author:** Founder

## Context
We need hosting that runs containers without managing servers, and infrastructure that anyone can rebuild from code.

## Decision
- Web apps and APIs: Amazon ECS on Fargate behind an Application Load Balancer.
- Small background jobs: AWS Lambda.
- All infrastructure: OpenTofu, state in S3 with locking.
- Secrets: AWS Secrets Manager.

## Options considered
| Option | Pros | Cons |
| --- | --- | --- |
| **ECS on Fargate** | No servers; always-on web apps; well supported in OpenTofu | More setup than a managed platform |
| ECS Express Mode | Simplest on-ramp to Fargate | New; OpenTofu support unconfirmed |
| Lambda only | Pay per request | Cold starts; awkward for always-on web apps |
| EC2 | Full control | We'd manage servers and patching |
| EKS (Kubernetes) | Very flexible | Overkill until many services and a platform team |
| App Runner | Simple | Closed to new customers |

## Consequences
Engineers learn basic ECS and OpenTofu before touching infra. Only FTEs get prod AWS access.
