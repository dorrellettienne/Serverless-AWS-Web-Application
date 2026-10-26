# Serverless AWS Web Application

**AWS Cloud Resume Challenge · Python Lambda · DynamoDB · Terraform**

[![Validate](https://github.com/dorrellettienne/Serverless-AWS-Web-Application/actions/workflows/validate.yml/badge.svg)](https://github.com/dorrellettienne/Serverless-AWS-Web-Application/actions/workflows/validate.yml)

A resume website extended into a small serverless application. CloudFront and a private S3 bucket deliver the frontend; a Python Lambda function and DynamoDB provide a visitor counter. Terraform defines the infrastructure, and GitHub Actions validates the code and configuration.

> **Deployment status:** The live AWS infrastructure is currently offline. The code remains available, the frontend runs locally, and Terraform describes how to rebuild the cloud environment.

![AWS serverless architecture](docs/cloud-resume-architecture-v5.svg)

## What this project demonstrates

- **Cloud integration:** A browser frontend calling a Lambda Function URL, backed by DynamoDB.
- **Infrastructure as code:** S3, CloudFront, Lambda, DynamoDB and IAM described in Terraform.
- **Testing and delivery:** Pytest, Terraform validation and a GitHub Actions workflow.

## Request flow

1. CloudFront serves the website from a private S3 bucket.
2. The browser requests the current visitor count through a Lambda Function URL.
3. Python Lambda updates the count stored in DynamoDB.
4. The response is displayed on the page.

## Technology

| Area | Tools |
| --- | --- |
| Frontend | HTML, CSS, JavaScript |
| Hosting | Amazon S3, CloudFront |
| Backend | Python, AWS Lambda, DynamoDB |
| Infrastructure | Terraform, IAM |
| Quality checks | Pytest, Terraform validation, GitHub Actions |

## Run locally

The static website does not need a build step. From the repository root:

```powershell
python -m http.server 8000
```

Open `http://localhost:8000/Resume/`.

The local visitor counter shows `Available when deployed` because the live AWS API endpoint is not published in the repository.

## Run the tests

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -r requirements-dev.txt
python -m pytest -q
```

The [validation workflow](.github/workflows/validate.yml) also runs Python tests and Terraform checks on pull requests and pushes to `main`.

## Rebuild the infrastructure

Terraform is under `infra/`. Rebuilding requires an AWS account, credentials and Terraform. **Applying the plan may create billable AWS resources.**

```powershell
cd infra
terraform init
terraform plan
terraform apply
```

The infrastructure includes a private S3 origin behind CloudFront, a Lambda Function URL, DynamoDB and the required IAM configuration. Sensitive credentials and state files should remain outside Git.

## Repository structure

```text
.github/workflows/   Validation and deployment workflows
Resume/              Website files
infra/               Terraform infrastructure
infra/lambda/        Python visitor-counter function
tests/               Lambda tests
docs/                Architecture documentation
```

The original AWS Cloud Resume Challenge is the starting point; the broader purpose of this repository is to demonstrate a reproducible, tested serverless application rather than a permanently hosted website.
