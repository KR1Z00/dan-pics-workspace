# AWS Delivery And Change Management Conventions

## Terraform Workflow

The standard workflow should remain:

```bash
terraform init
terraform plan -var-file=<env>.tfvars
terraform apply -var-file=<env>.tfvars
```

Conventions:

- always plan before apply
- keep the plan scoped to the intended environment
- review destructive actions explicitly before approving apply
- avoid mixing unrelated infrastructure changes into one plan when possible

```bash
# dev workflow
terraform init
terraform plan  -var-file=dev.tfvars  -out=dev.tfplan
terraform apply dev.tfplan

# production workflow — same pattern, different tfvars
terraform plan  -var-file=prod.tfvars -out=prod.tfplan
terraform apply prod.tfplan

# pass sensitive values that are not in tfvars via environment vars
TF_VAR_db_password=$DB_PASSWORD \
TF_VAR_stripe_secret_key=$STRIPE_KEY \
terraform apply prod.tfplan
```

Saving the plan to a file with `-out` ensures that the `apply` step executes exactly the change that was reviewed.

## Remote State

Remote state with locking is mandatory for team safety.

Conventions:

- keep state in an S3 backend
- keep locking in DynamoDB or the chosen equivalent
- do not use local state for shared environments
- treat backend bootstrap separately and document it clearly

```hcl
# backend.tf
terraform {
  backend "s3" {
    bucket         = "peakreps-terraform-state"
    key            = "peakreps/terraform.tfstate"
    region         = "ap-southeast-2"
    dynamodb_table = "terraform-locks"  # prevents concurrent apply
    encrypt        = true
  }
}
```

> The S3 bucket and DynamoDB table must exist before running `terraform init`. Bootstrap them manually or with a one-time Terraform root the first time.

## Environment Management

Environment differences should be data-driven.

Conventions:

- express differences through tfvars or clearly bounded conditional logic
- keep prod and dev configuration drift intentional and reviewable
- do not fork whole Terraform roots just to change a handful of values unless environments have genuinely different architectures

## Review Expectations

An infrastructure change is not ready if it lacks:

- clear variable changes
- predictable naming impact
- security group or IAM review where permissions change
- output review where downstream consumers depend on new resources
- rollback or mitigation thinking for risky changes

## Operational Discipline

Prefer scripted, documented workflows over tribal knowledge.

Conventions:

- document required CLI tools and versions
- keep environment prerequisites explicit
- record manual post-apply steps when they still exist
- reduce manual DNS, certificate, or secret wiring over time where automation is justified

## Refactoring Guidance

Extract Terraform modules when one of these becomes true:

- multiple environments need the same infrastructure shape with only parameter changes
- a domain area such as ECS services or networking has become hard to review in a single file
- a reusable pattern is being copied rather than composed

Do not extract modules just to appear more enterprise. Extract when it improves reviewability, safety, or reuse.