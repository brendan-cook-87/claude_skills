---
name: aws-iam-role-export
description: >
  Analyzes AWS infrastructure and exports required IAM execution roles to .aws-core/<product>.yml.
  Use when defining lambdas, step functions, or other AWS resources that require IAM roles.
---

# AWS IAM Role Export

Generate IAM execution roles for AWS resources following organizational security standards and export them to the aws-core repository structure.

## Overview

When defining AWS resources that require IAM roles (Lambda functions, Step Functions, etc.), this skill:

1. **Analyzes infrastructure** to identify resources needing execution roles
2. **Generates IAM roles** following least privilege principles
3. **Exports roles** to `.aws-core/<product>.yml` for deployment via aws-core repo

## Prerequisites

Ensure the project has a product name defined in `CLAUDE.md`. If missing, inform the user and request it.

## Role Generation Process

### 1. Resource Analysis

Scan infrastructure code (CloudFormation, Terraform, CDK, etc.) to identify resources requiring IAM roles:

**Lambda Functions:**
- Execution role for basic runtime permissions
- Additional permissions based on resources accessed (S3, DynamoDB, SQS, etc.)

**Step Functions:**
- State machine execution role
- Permissions to invoke Lambda functions, access resources

**Other Services:**
- ECS tasks, Batch jobs, API Gateway, etc.

### 2. Role Template Structure

All roles follow this CloudFormation template:

```yaml
AWSTemplateFormatVersion: 2010-09-09

Resources:
  <RoleName>:
    Type: AWS::IAM::Role
    Properties:
      RoleName: <product>-<suffix>
      Path: /athena/core/iam/
      Tags:
        - Key: "ath:owner"
          Value: "<owner-from-claude-md>"
        - Key: "ath:product"
          Value: "<product-from-claude-md>"
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal:
              Service: <service>.amazonaws.com
            Action: sts:AssumeRole
      ManagedPolicyArns:
        - <required-managed-policies>
      Policies:
        - PolicyName: <product>-<suffix>-policy
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              - <least-privilege-statements>
```

### 3. Common Role Types

**Lambda Execution Role:**
```yaml
  <Product>Lambda:
    Type: AWS::IAM::Role
    Properties:
      RoleName: <product>-lambda
      Path: /athena/core/iam/
      Tags:
        - Key: "ath:owner"
          Value: "<owner>"
        - Key: "ath:product"
          Value: "<product>"
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal:
              Service: lambda.amazonaws.com
            Action: sts:AssumeRole
      ManagedPolicyArns:
        - Fn::Sub: arn:aws:iam::${AWS::AccountId}:policy/athena/core/iam/ServicePutLog
        - Fn::Sub: arn:aws:iam::${AWS::AccountId}:policy/athena/core/iam/LambdaInVpc
      Policies:
        - PolicyName: <product>-lambda-policy
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              # Add specific permissions based on resources accessed
```

**Step Functions Execution Role:**
```yaml
  <Product>StepFunction:
    Type: AWS::IAM::Role
    Properties:
      RoleName: <product>-sfn
      Path: /athena/core/iam/
      Tags:
        - Key: "ath:owner"
          Value: "<owner>"
        - Key: "ath:product"
          Value: "<product>"
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal:
              Service: states.amazonaws.com
            Action: sts:AssumeRole
      ManagedPolicyArns:
        - Fn::Sub: arn:aws:iam::${AWS::AccountId}:policy/athena/core/iam/ServiceStateMachineLogToCloudWatch
      Policies:
        - PolicyName: <product>-sfn-policy
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              - Effect: Allow
                Action:
                  - lambda:InvokeFunction
                Resource:
                  # List specific Lambda function ARNs
```

## Security Principles

### Least Privilege

**Always apply minimal necessary permissions:**

```yaml
# BAD - Too broad
- Effect: Allow
  Action: s3:*
  Resource: "*"

# GOOD - Specific actions and resources
- Effect: Allow
  Action:
    - s3:GetObject
    - s3:PutObject
  Resource:
    - Fn::Sub: arn:aws:s3:::athena-${AWS::AccountId}-${Product}-data/*
```

### Tag-Based Restrictions

**Use product tags to restrict access where supported:**

```yaml
# S3 bucket access restricted by product tag
- Effect: Allow
  Action:
    - s3:GetObject
    - s3:PutObject
  Resource: "arn:aws:s3:::*/*"
  Condition:
    StringEquals:
      "s3:ExistingObjectTag/ath:product": "<product>"

# DynamoDB table access restricted by product tag
- Effect: Allow
  Action:
    - dynamodb:GetItem
    - dynamodb:PutItem
    - dynamodb:UpdateItem
  Resource: "arn:aws:dynamodb:*:*:table/*"
  Condition:
    StringEquals:
      "dynamodb:Attributes":
        - "ath:product"
      "aws:RequestedRegion": "ap-southeast-2"
    ForAllValues:StringEquals:
      "dynamodb:Select":
        - "SpecificAttributes"
```

### Explicit Resource Listing

**Always list specific resources - avoid wildcards:**

```yaml
# List all Lambda functions that Step Function can invoke
- Effect: Allow
  Action:
    - lambda:InvokeFunction
  Resource:
    - Fn::Sub: arn:aws:lambda:ap-southeast-2:${AWS::AccountId}:function:${Product}-handler
    - Fn::Sub: arn:aws:lambda:ap-southeast-2:${AWS::AccountId}:function:${Product}-processor
    - Fn::Sub: arn:aws:lambda:ap-southeast-2:${AWS::AccountId}:function:${Product}-cleanup
```

## Common Managed Policies

Reference existing athena core managed policies:

```yaml
# Standard policies available
ManagedPolicyArns:
  - Fn::Sub: arn:aws:iam::${AWS::AccountId}:policy/athena/core/iam/ServicePutLog
  - Fn::Sub: arn:aws:iam::${AWS::AccountId}:policy/athena/core/iam/LambdaInVpc
  - Fn::Sub: arn:aws:iam::${AWS::AccountId}:policy/athena/core/iam/AccessDWHSecret
  - Fn::Sub: arn:aws:iam::${AWS::AccountId}:policy/athena/core/iam/ServiceStateMachineLogToCloudWatch
  - arn:aws:iam::aws:policy/service-role/AWSLambdaVPCAccessExecutionRole
```

## Export Process

### 1. File Creation

Create `.aws-core/<product>.yml` in the project root with the generated roles.

### 2. File Structure

```yaml
AWSTemplateFormatVersion: 2010-09-09

Resources:
  # Generated roles based on infrastructure analysis
  <ProductName>Lambda:
    Type: AWS::IAM::Role
    # ... role definition

  <ProductName>StepFunction:
    Type: AWS::IAM::Role
    # ... role definition
```

### 3. Validation

Before exporting, verify:
- Product and owner tags match `CLAUDE.md`
- Role names follow `<product>-<suffix>` pattern
- Path is `/athena/core/iam/`
- Permissions follow least privilege
- Resources are explicitly listed
- Tag-based conditions are used where applicable

## Usage Workflow

1. **Analyze infrastructure** code to identify resources needing IAM roles
2. **Generate appropriate roles** following security principles
3. **Export to `.aws-core/<product>.yml`**
4. **Inform user** about the generated file and next steps for aws-core repo deployment

## Example Output

For a project with Lambda functions and Step Functions:

```yaml
AWSTemplateFormatVersion: 2010-09-09

Resources:
  UserServiceLambda:
    Type: AWS::IAM::Role
    Properties:
      RoleName: user-service-lambda
      Path: /athena/core/iam/
      Tags:
        - Key: "ath:owner"
          Value: "platform-team"
        - Key: "ath:product"
          Value: "user-service"
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal:
              Service: lambda.amazonaws.com
            Action: sts:AssumeRole
      ManagedPolicyArns:
        - Fn::Sub: arn:aws:iam::${AWS::AccountId}:policy/athena/core/iam/ServicePutLog
        - Fn::Sub: arn:aws:iam::${AWS::AccountId}:policy/athena/core/iam/LambdaInVpc
      Policies:
        - PolicyName: user-service-lambda-policy
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              - Effect: Allow
                Action:
                  - dynamodb:GetItem
                  - dynamodb:PutItem
                Resource:
                  - Fn::Sub: arn:aws:dynamodb:ap-southeast-2:${AWS::AccountId}:table/user-service-users
```