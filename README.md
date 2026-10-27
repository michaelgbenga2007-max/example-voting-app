# Voting App — DevOps Capstone Project

A containerized distributed voting application enhanced with DevOps practices, including automated builds, security scanning, Kubernetes deployment, Helm, and Infrastructure as Code.

## 1. Project Overview

This project demonstrates how to package, validate, scan, and deploy a distributed application using modern DevOps tools.

The application allows users to vote between two options and view the results in real time.

### Objectives

- Run the application using Docker Compose.
- Containerize application components.
- Validate application configuration.
- Automate CI tasks using GitHub Actions.
- Scan the project for security vulnerabilities and misconfigurations using Trivy.
- Deploy application components to Kubernetes.
- Package Kubernetes resources using Helm.
- Define cloud infrastructure using Terraform.
- Document deployment, security, and rollback procedures.

## 2. Technology Stack

| Technology | Purpose |
|---|---|
| Docker | Containerization |
| Docker Compose | Local multi-container orchestration |
| Kubernetes | Container orchestration |
| Helm | Kubernetes application packaging and deployment |
| Terraform | Infrastructure as Code |
| GitHub Actions | Continuous Integration automation |
| Trivy | Security and misconfiguration scanning |
| Python | Voting frontend |
| Node.js | Results frontend |
| .NET | Background worker |
| Redis | Temporary vote messaging |
| PostgreSQL | Vote storage |

## 3. Application Architecture

The application consists of the following components:

1. **Vote:** A Python web application that accepts votes.
2. **Redis:** Receives and temporarily holds votes.
3. **Worker:** A .NET service that transfers votes from Redis to PostgreSQL.
4. **PostgreSQL:** Stores the votes.
5. **Result:** A Node.js web application that displays voting results.

### Architecture Flow

```text
                 Users
                   |
          +--------+--------+
          |                 |
     Vote Frontend     Result Frontend
          |                 |
          v                 |
        Redis               |
          |                 |
          v                 |
        Worker               |
          |                 |
          v                 |
      PostgreSQL ------------+
```

## 4. Run Locally with Docker Compose

### Prerequisites

- Docker Desktop
- Git
- PowerShell on Windows, or a compatible terminal

### Start the application

Clone your repository if you have not already done so:

```bash
git clone https://github.com/michaelgbenga2007-max/example-voting-app.git
cd example-voting-app
```

Build and start the application:

```bash
docker compose up --build -d
```

Check the running containers:

```bash
docker compose ps
```

Open the application:

- Voting interface: http://localhost:8080
- Results interface: http://localhost:8081

View application logs:

```bash
docker compose logs
```

Stop the application:

```bash
docker compose down
```

## 5. Kubernetes Deployment

The repository includes Kubernetes specifications in `k8s-specifications/` and a Helm chart in `voting-app/`.

Ensure that your Kubernetes cluster is running and that `kubectl` is configured to access it.

### Deploy using Kubernetes manifests

```bash
kubectl apply -f k8s-specifications/
```

Check deployments and pods:

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

The example manifests expose the voting interface on NodePort `31000` and the results interface on NodePort `31001`, subject to the current cluster and service configuration.

### Deploy using Helm

Install or upgrade the application using the project's Helm chart:

```bash
helm upgrade --install voting-app ./voting-app
```

Check the release:

```bash
helm list
helm status voting-app
```

View the rendered Kubernetes resources:

```bash
helm template voting-app ./voting-app
```

Validate the chart:

```bash
helm lint ./voting-app
```

### Rollback

Inspect the release history:

```bash
helm history voting-app
```

If a previous release exists, roll back to a known-good revision:

```bash
helm rollback voting-app 1
```

Replace `1` with the actual revision you intend to restore. Verify the result using `helm status voting-app` and `kubectl get pods`.

## 6. Continuous Integration

The GitHub Actions workflow is located at:

`.github/workflows/ci.yml`

The workflow is configured to run on pushes to `main` and pull requests targeting `main`.

Its intended checks include:

1. Checking out the repository.
2. Scanning the project with Trivy.
3. Validating the Docker Compose configuration.
4. Building the Docker Compose application images.

Workflow execution can be inspected in the repository's **Actions** tab.

A successful workflow run should be confirmed from GitHub Actions. Security findings or failed steps must be investigated before the pipeline can be considered fully successful.

## 7. Security Practices

The project uses Trivy to identify potential security vulnerabilities, exposed secrets, and configuration issues.

Recommended security practices include:

- Use trusted and appropriately pinned container image versions.
- Avoid committing passwords, access keys, or other secrets.
- Run application containers as non-root users where supported.
- Apply least-privilege permissions.
- Configure suitable Kubernetes security contexts.
- Avoid using development credentials in production.
- Review and resolve high- and critical-severity findings.

Trivy findings must be reviewed individually. A scan that reports unresolved findings should not be described as clean.

## 8. Infrastructure as Code

Terraform files are maintained in the `terraform/` directory.

The standard Terraform workflow is:

```bash
terraform -chdir=terraform init
terraform -chdir=terraform validate
terraform -chdir=terraform plan
```

Review the proposed changes before applying infrastructure.

The presence of Terraform files does not by itself mean that cloud infrastructure has been provisioned. Cloud deployment requires a configured provider, valid authentication, and the required infrastructure resources.

## 9. Deployment Status and Limitations

The project has been developed with local container execution, Kubernetes resources, Helm, Terraform scaffolding, and a GitHub Actions workflow.

The following items must be verified against the actual project state before final submission:

- Latest GitHub Actions result.
- Trivy scan findings and remediation status.
- Successful Helm deployment and rollback evidence.
- Completeness of the Terraform infrastructure configuration.
- Cloud infrastructure provisioning and deployment, if required by the assessment.

AWS deployment is pending until AWS authentication and infrastructure provisioning are completed.

This project should not be considered fully deployed to the cloud until the relevant infrastructure and application deployment have been verified.

## 10. Repository Structure

```text
example-voting-app/
├── .github/
│   └── workflows/
│       └── ci.yml
├── vote/
├── result/
├── worker/
├── seed-data/
├── k8s-specifications/
├── voting-app/
├── terraform/
├── compose.yaml
└── README.md
```

The exact repository structure may vary as the project evolves.

## 11. Author and Project Context

**Project:** DevOps Engineering Capstone — Voting App  
**Project objective:** Demonstrate containerization, automated validation, security scanning, Kubernetes deployment, Helm packaging, and Infrastructure as Code.

**Repository:** https://github.com/michaelgbenga2007-max/example-voting-app