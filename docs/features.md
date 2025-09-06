# Features

## 📦 Web App Deployment
- Java-based web app containerized with Docker.  
- Runs in Kubernetes with **five replicas** for reliability.  
- Canary deployment strategy → traffic gradually shifted using Istio.  

---

## ⚙️ CI with TeamCity
- GitHub repo integrated with TeamCity.  
- Code compilation + unit testing automated on every Pull Request (PR).  
- Status checks ensure only passing builds can be merged.  

---

## 🏗️ Image Build with Jenkins
- Jenkins builds Docker images from source.  
- Dual tagging: `BUILD_NUMBER-hash` (versioned) + `latest`.  
- Pushes securely to DockerHub.  

---

## 🚀 CD with Spinnaker
- Spinnaker watches DockerHub and/or Jenkins webhook.  
- Automatically deploys new images to **canary**, then **stable**.  
- Supports rollback if canary fails.  

---

## Automated Canary Releases (Spinnaker + Istio + Prometheus)
- **Spinnaker** → Orchestrates canary-first deployments.
- **Istio** → Splits traffic (e.g., 90% stable, 10% canary).
- **Prometheus** → Provides health metrics for Automated Canary Analysis (ACA).
- If canary succeeds → promoted to full rollout.
- If canary fails → automatic rollback to stable version.


---

## 🌿 Git Workflow & Branch Protection
- `main` → production branch.  
- `dev` → development/testing branch.  
- Pull Request workflow enforced with reviews + approvals.  

---

## 🛡️ Resilience & Recovery
- Runbooks prepared for CI/CD failures, merge conflicts, and rollout issues.  
- Ability to quickly rollback to last stable release.  