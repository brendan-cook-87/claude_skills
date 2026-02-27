---
name: infrastructure-naming
description: >
  Explains AWS resource naming conventions using product names from CLAUDE.md.
  Use when creating or discussing AWS infrastructure to ensure consistent naming patterns.
---

# Infrastructure Naming

Ensure consistent naming for all AWS resources by following organizational naming conventions that include the product name.

## Naming Patterns

All AWS resources must follow kebab-case naming conventions using the product name from the project's `CLAUDE.md` file.

### Global Resources

For AWS resources that require globally unique names:

```
athena-{account-id}-{product}-{suffix}
```

**Global resources include:**
- S3 buckets
- CloudFront distributions
- IAM roles and policies
- Route53 hosted zones and records
- Any resource with globally unique naming requirements

**Examples:**
- S3 bucket: `athena-123456789012-user-service-uploads`
- IAM role: `athena-123456789012-user-service-lambda-execution`
- CloudFront distribution: `athena-123456789012-user-service-cdn`

### Regional Resources

For AWS resources scoped to a specific region:

```
{product}-{suffix}
```

**Regional resources include:**
- Lambda functions
- RDS databases
- VPCs and subnets
- CloudWatch log groups
- API Gateway APIs
- Application Load Balancers
- ECS services and tasks

**Examples:**
- Lambda function: `user-service-auth-handler`
- RDS database: `user-service-primary-db`
- VPC: `user-service-vpc`
- Log group: `user-service-api-logs`

## Product Name Source

### From CLAUDE.md

The `{product}` value **must** be sourced from the project's `CLAUDE.md` file under the `## Project Ownership` section:

```markdown
## Project Ownership

- **Product:** user-service
- **Owner:** platform-team
```

### Missing Product Name

If no product name exists in `CLAUDE.md`:

1. **Stop and inform** the user that infrastructure naming requires a product name
2. **Ask the user** to provide the product name for this project
3. **Update CLAUDE.md** with the provided product name in the Project Ownership section
4. **Continue** with resource naming using the confirmed product name

## Account ID Parameterization

The `{account-id}` should be **parameterized** and not hardcoded, since builds must work across different AWS accounts.

### Recommended Approaches

**Environment Variables:**
```bash
BUCKET_NAME="athena-${AWS_ACCOUNT_ID}-${PRODUCT}-uploads"
```

**CloudFormation Parameters:**
```yaml
Parameters:
  AccountId:
    Type: String
    Default: !Ref 'AWS::AccountId'
  Product:
    Type: String
    Default: user-service

Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'athena-${AccountId}-${Product}-uploads'
```

**Terraform Variables:**
```hcl
variable "product" {
  description = "Product name from CLAUDE.md"
  type        = string
  default     = "user-service"
}

data "aws_caller_identity" "current" {}

resource "aws_s3_bucket" "uploads" {
  bucket = "athena-${data.aws_caller_identity.current.account_id}-${var.product}-uploads"
}
```

**CI/CD Pipeline Integration:**
```yaml
# Bitbucket Pipelines example
- export PRODUCT="user-service"  # from CLAUDE.md
- export AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
- aws s3 mb s3://athena-${AWS_ACCOUNT_ID}-${PRODUCT}-deployment-artifacts
```

## Suffix Guidelines

The `{suffix}` should clearly describe the resource's purpose or function:

- **Be descriptive:** `auth-handler` not `handler1`
- **Use kebab-case:** `user-data-processor` not `userDataProcessor`
- **Be concise:** `api-logs` not `application-programming-interface-logs`
- **Avoid redundancy:** `uploads` not `uploads-bucket` (the resource type is already S3)

## Validation

When reviewing infrastructure code, verify:

1. **Product name** matches exactly what's in `CLAUDE.md`
2. **Naming pattern** follows global vs regional conventions
3. **Account ID** is parameterized, not hardcoded
4. **Kebab-case** formatting is consistent
5. **Suffixes** are descriptive and appropriate