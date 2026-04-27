# AWS Infrastructure Conventions

These conventions define how PeakReps AWS infrastructure should be described and changed using Terraform in `peakreps-infra`.

They are intentionally practical: they assume the current single-root Terraform setup, but they describe the cleaner target behavior the team should follow as infrastructure grows.

## Scope

This stack covers:

- Terraform root configuration and environment tfvars
- AWS networking, compute, storage, database, IAM, DNS, certificates, and secrets
- remote state, plans, applies, and change management

## Golden Rules

1. Terraform is the source of truth for managed infrastructure.
2. Environment naming and tagging must be consistent and derivable.
3. Security boundaries should be explicit in code, not implied by convention.
4. Secrets and sensitive values must flow through secure inputs and secret stores, not source control.
5. Plans should be reviewed before apply.
6. Prefer small, reviewable infrastructure changes over broad mixed refactors.
7. Outputs should help downstream systems consume infrastructure safely.
8. As the stack grows, extract modules when reuse or review clarity justifies it, not by default.

## Document Map

- `terraform-structure.md` covers file layout, variables, naming, tagging, and outputs.
- `security-and-networking.md` covers VPC shape, security groups, IAM, storage, secrets, and network access.
- `delivery-and-change-management.md` covers workflow, state management, review expectations, and operational discipline.