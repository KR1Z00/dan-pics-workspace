# Terraform Structure Conventions

## Root Layout

The current root-level file split is acceptable and should remain the baseline until the stack becomes too large for a single root.

Preferred files:

- `backend.tf` for remote state and backend configuration
- `main.tf` for resource definitions and shared locals
- `variables.tf` for typed input variables
- `outputs.tf` for exported values
- `dev.tfvars` and `prod.tfvars` for environment-specific values

If complexity grows significantly, extract modules by domain such as network, compute, data, and identity, but keep the root composition readable.

## Naming

Resource naming should be deterministic.

Preferred pattern:

- derive a shared prefix from environment
- use `peakreps` for production and `peakreps-dev` or similarly explicit names for non-production
- construct resource names from `prefix + resource-purpose`

Examples:

- VPC: `peakreps-vpc`
- Dev ALB SG: `peakreps-dev-alb-sg`
- Secrets path: `/${prefix}/service_name`

Conventions:

- keep names human-readable
- avoid embedding transient ticket or branch names in long-lived infrastructure resources
- use the same environment discriminator across all AWS services

## Variables

Variables should be typed, described, and minimal.

Conventions:

- every input variable should have a description unless its meaning is completely obvious
- mark sensitive values as `sensitive = true`
- keep defaults only for safe, non-secret values that are actually defaults
- prefer explicit environment tfvars over editing defaults for deployment changes
- do not commit secrets to tfvars files that live in source control

Good variable categories:

- environment and region
- networking and CIDR inputs
- app domains and URLs
- third-party integration secrets or identifiers
- certificate and email settings

## Locals

Use `locals` for derived values that improve consistency.

Appropriate uses:

- environment-derived prefixes
- repeated tags
- name templates
- small repeated configuration fragments

Avoid using locals to hide important infrastructure intent or create a second abstraction language inside Terraform.

## Tagging

The current repo mostly uses `Name` tags. That is workable, but the target convention should be slightly stronger.

Minimum tagging standard:

- `Name`
- `Environment`
- `ManagedBy = Terraform`

Optional but useful when the team grows:

- `Service`
- `Owner`
- `CostCenter`

If the stack remains intentionally small, keep tags lean but consistent.

## Outputs

Outputs should make infrastructure consumable without leaking secrets.

Good outputs:

- ALB DNS names
- ECS cluster or service identifiers
- ECR repository URLs
- subnet and security group IDs needed by related systems
- certificate and SES verification records for DNS setup

Avoid outputting:

- plaintext secrets
- complete credentials
- values that encourage copy-paste of insecure connection strings

If a connection string is output, keep it templated or clearly safe for operational use.