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