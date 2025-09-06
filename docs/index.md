This document represents our team’s effort to design and implement a modern **Continuous Integration (CI)** and **Continuous Deployment (CD)** using industry-standard DevOps tools.

Our goal is to create a solution that does more than just build and deploy code — it must follow best practices for **automation, reliability, collaboration, and recovery**.  

The technologies chosen are **TeamCity, Jenkins, Spinnaker, and Istio**, which reflect the tools used in real-world production environments across many organizations.

---

## 🎯 Purpose of this Document

- To serve as a technical reference for panelists, advisers, and engineers reviewing our case study.  
- Provide clarity on how code changes move from integration to development all the way to production.  
- To act as a runbook and operations manual, showing how we plan to respond when failures happen.  

---

## 📌 Scope

This case study covers:

- **Source code version control** using GitHub with strict branching discipline.  
- **Build automation** using TeamCity for testing and Jenkins for Docker image creation.  
- **Deployment automation** using Spinnaker with canary deployment enabled by Istio.  
- **Operational readiness** through simulated incidents, troubleshooting, and recovery.  

---

## 📖 Outcome

By following this documentation, you’ll gain both a **technical roadmap** and an **operational playbook** for managing CI/CD.
