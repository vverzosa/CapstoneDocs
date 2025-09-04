# Welcome

This documentation provides a detailed overview of our **CI/CD Case Study** project.  
The project demonstrates how to design, implement, and operate a CI/CD pipeline using **TeamCity**, **Jenkins**, and **Spinnaker**, with **GitHub** as the version control system and **Kubernetes** as the deployment platform.

By following this documentation, you will gain an understanding of:
- How CI/CD pipelines are built with modern tools.
- How automated deployments work in Kubernetes.
- How to handle failures with structured runbooks.
- How to maintain reliability with disaster recovery planning.

---

## Web App Deployment

### Requirements
- Displays a plain colored background when accessed.
- Must be accessible over the internet.
- Source code (e.g., Flask or Java) should be buildable and compilable in **TeamCity**.

---

## Kubernetes Deployment

### Requirements
- Canary deployment strategy must be used.
- Each deployment must maintain **five (5) healthy replicas**.

---

## Jenkins – Image Build

- Jenkins will be deployed to build Docker images.
- Build is managed with a **Jenkinsfile**.
- Docker image must be pushed to [Docker Hub](https://registry.hub.docker.com).

---

## TeamCity – Continuous Integration (CI)

- Deploy TeamCity to handle CI processes.
- Integrate GitHub repository containing the app source code.
- Automated unit tests must run with each build.

---

## Spinnaker – Continuous Deployment (CD)

- Spinnaker is responsible for deploying to Kubernetes.
- Deployment triggers:
  - After Jenkins job succeeds, or
  - When a Docker image is successfully pushed.

---

## Version Control

- Git repository must have **two branches**:
  - `main` → Production environment.
  - `dev` → Development environment.
- Branch protection requirements:
  - PR approval required before merging.
  - All status checks must pass (TeamCity build success).
- GitHub repo must contain:
  - Automation codebase
  - Infrastructure configurations
  - Documentation
- Commit history must show only **merged PRs**.

---

## Runbooks

This section provides step-by-step procedures to handle failures.

### CI/CD Failures
- Breaking code changes (syntax or logical errors).
- Breaking TeamCity or Spinnaker configs.
- TeamCity build agent unavailability.

### Version Control Failures
- Merge conflicts between `main` and `dev`.
- Steps to resolve and redeploy.

---

## Disaster Recovery Plan

- Backup and restore strategies for Kubernetes resources.
- Rollback strategies for failed deployments.
- Recovery steps for CI/CD platforms (Jenkins, TeamCity, Spinnaker).

---

## Operations Simulation

Failures introduced for testing resilience:

- **CI/CD Failures**
  - Broken code
  - Broken configs
  - Missing build agents
- **Version Control Failures**
  - Merge conflicts between branches

Each scenario is tested and resolved through defined **runbooks**.

---
