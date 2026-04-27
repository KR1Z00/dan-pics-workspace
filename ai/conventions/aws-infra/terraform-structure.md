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

```hcl
# variables.tf
variable "environment" {
  description = "Deployment environment: prod or dev"
  type        = string
  default     = "prod"
}

variable "aws_region" {
  description = "AWS region to deploy into"
  type        = string
  default     = "ap-southeast-2"
}

variable "db_password" {
  description = "Master password for the RDS Postgres instance"
  type        = string
  sensitive   = true  # never logged or shown in plan output
}

variable "stripe_secret_key" {
  description = "Stripe secret API key injected from CI or Secrets Manager"
  type        = string
  sensitive   = true
}
```

```hcl
# dev.tfvars — safe to commit (no secrets)
environment = "dev"
aws_region  = "ap-southeast-2"
app_base_url = "https://app-dev.peakreps.com"
# db_password and stripe_secret_key supplied via CI env vars, not in this file
```

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

```hcl
# main.tf
locals {
  # single source of truth for resource naming
  prefix = var.environment == "prod" ? "peakreps" : "peakreps-dev"

  # reused tag block — merge into resource tags blocks
  common_tags = {
    ManagedBy   = "Terraform"
    Environment = var.environment
  }
}

# usage in a resource
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true

  tags = merge(local.common_tags, {
    Name = "${local.prefix}-vpc"
  })
}
```

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

```hcl
# outputs.tf
output "alb_dns_name" {
  description = "DNS name of the application load balancer"
  value       = aws_alb.main.dns_name
}

output "ecr_repository_url" {
  description = "ECR URL for pushing and pulling the backend container image"
  value       = aws_ecr_repository.backend.repository_url
}

output "ecs_cluster_name" {
  description = "ECS cluster name for deployment scripts"
  value       = aws_ecs_cluster.main.name
}

# ❌ avoid — leaks credentials in plan and state output
output "db_password" {
  value = var.db_password
}
```