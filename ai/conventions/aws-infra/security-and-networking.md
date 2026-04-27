# AWS Security And Networking Conventions

## Network Topology

Network design should remain explicit and boring.

Conventions:

- define a single clear VPC per environment unless there is a strong isolation reason to split it further
- spread critical resources across at least two availability zones where the service type benefits from it
- use public subnets only for internet-facing resources such as load balancers or NAT infrastructure
- keep application tasks and databases in private subnets when architecture allows it

If the current stack keeps some data resources in a less isolated configuration for simplicity, treat that as transitional rather than ideal.

## Security Groups

Security groups should describe allowed traffic narrowly.

Conventions:

- model each boundary explicitly, such as ALB, ECS tasks, and RDS
- prefer security-group-to-security-group ingress over broad CIDR rules when both resources are in AWS
- keep admin IP ingress narrowly scoped and documented when temporary direct access is needed
- use dedicated `aws_security_group_rule` resources when they improve clarity or lifecycle safety

```hcl
# ECS tasks only accept traffic from the ALB — not from the internet directly
resource "aws_security_group" "ecs" {
  name        = "${local.prefix}-ecs-sg"
  description = "Allow inbound from ALB only"
  vpc_id      = aws_vpc.main.id

  ingress {
    from_port       = 3000
    to_port         = 3000
    protocol        = "tcp"
    security_groups = [aws_security_group.alb.id]  # SG reference, not CIDR
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# RDS uses separate rule resources for fine-grained lifecycle control
resource "aws_security_group_rule" "rds_ingress_ecs" {
  type                     = "ingress"
  from_port                = 5432
  to_port                  = 5432
  protocol                 = "tcp"
  security_group_id        = aws_security_group.rds.id
  source_security_group_id = aws_security_group.ecs.id
  description              = "Allow ECS tasks to connect to Postgres"
}

# Admin access is temporary and narrowly scoped by IP
resource "aws_security_group_rule" "rds_ingress_admin" {
  type              = "ingress"
  from_port         = 5432
  to_port           = 5432
  protocol          = "tcp"
  security_group_id = aws_security_group.rds.id
  cidr_blocks       = [var.my_ip_cidr]  # /32 single IP, never 0.0.0.0/0
  description       = "Temporary admin access from office IP"
}
```

Default intent:

- internet reaches ALB
- ALB reaches app container port
- app reaches database and required external services
- database does not accept arbitrary public traffic

## IAM

IAM should follow least privilege even in a small stack.

Conventions:

- separate task execution permissions from application runtime permissions
- grant access per secret or service capability, not through broad wildcard policies
- keep assume-role policies minimal and service-specific
- name IAM roles and policies consistently with the environment prefix

When a task needs S3, SES, Secrets Manager, or other AWS services, grant only the actions it actually uses.

```hcl
# Task execution role only needs ECR pull and CloudWatch logs
resource "aws_iam_role" "ecs_task_execution_role" {
  name = "${local.prefix}-ecs-execution-role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect    = "Allow"
      Action    = "sts:AssumeRole"
      Principal = { Service = "ecs-tasks.amazonaws.com" }
    }]
  })
}

resource "aws_iam_role_policy_attachment" "ecs_execution_managed" {
  role       = aws_iam_role.ecs_task_execution_role.name
  policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
}

# App task role — only the S3 actions the backend actually needs
resource "aws_iam_policy" "app_s3_access" {
  name = "${local.prefix}-app-s3-access"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect   = "Allow"
      Action   = ["s3:GetObject", "s3:PutObject", "s3:DeleteObject"]
      Resource = "${aws_s3_bucket.storage.arn}/*"
      # ❌ do NOT use "s3:*" or "*" just to make it work quickly
    }]
  })
}
```

## Secrets

Secrets should never be embedded in Terraform source or application container definitions directly.

Conventions:

- pass secrets in as sensitive variables only when needed to provision resources
- prefer storing application runtime secrets in AWS Secrets Manager or another managed secret store
- organize secret names under a predictable hierarchy such as `/${prefix}/<domain>/<name>` or `/${prefix}/<name>`
- reference secret ARNs in IAM policies and task definitions, not plaintext values

## Storage

S3 buckets should default to private.

Conventions:

- grant bucket access through IAM roles, not public ACLs
- expose media through controlled backend behavior or signed URLs where needed
- avoid force-destroy in long-lived production buckets unless the data-loss implications are explicitly accepted

## Certificates, DNS, And Email

Infrastructure that requires external DNS actions should still be Terraform-driven as far as possible.

Conventions:

- create ACM and SES resources in Terraform
- output verification records cleanly for DNS providers when automated DNS management is not yet in place
- keep domain names environment-aware when non-production needs independent endpoints

## Database Access

Database access should be treated as a privileged boundary.

Conventions:

- allow application access through security groups
- allow human access only when necessary, from tightly scoped addresses or approved access paths
- avoid normalizing direct database access as a standard workflow for application debugging