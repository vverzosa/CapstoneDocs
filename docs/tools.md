# Tools

## GitHub
- Manages version control with **`main` (production)** and **`dev` (development)** branches.  
- Pull Requests enforce collaboration, approval, and status checks.  
- Prevents direct commits to **`main`**.  

---

## [TeamCity](tools/teamcity.md)
- Acts as our **Continuous Integration** system.  
- Builds the **Java app**, runs automated unit tests, and reports results.  
- Provides status checks that must pass before merging **PRs**.  

---

## Jenkins
- Specializes in building **container images**.  
- Uses **Jenkinsfile** to automate Docker builds and push images to **DockerHub**.  
- Ensures every build has a versioned tag and a moving latest tag.

---

## DockerHub
- Central registry for all container images.  
- Acts as the trigger source for Spinnaker pipelines.  

---

## Spinnaker (CD)
- Orchestrates **continuous delivery**.  
- Deploys canary first, then promotes to stable if successful.  
- Integrates with DockerHub to auto-trigger on new images.  

---

## Prometheus
- Monitors application health during deployments.
- Tracks latency, error rates, and pod readiness.
- Provides feedback for rollback or promotion decisions.

---

## Istio
- Service mesh that controls **traffic routing**.  
- Splits traffic between **stable** and canary deployments.  
- Provides observability into rollout performance.  

---

## Kubernetes
- Hosts the application workloads.  
- Ensures **five healthy replicas** are always running.  
- Provides scaling, self-healing, and deployment orchestration.

---

## CI/CD Workflow

The figure below illustrates the end-to-end workflow of our CI/CD pipeline:

## Step-by-Step CI/CD Pipeline Explanation  

### 1. GitHub Pull Request  
- Developers open a Pull Request (PR) with new changes.
- Ensures collaboration, review, and branch protection.

### 2. TeamCity Tests  
- The PR triggers **unit tests** in TeamCity.
- Only passing PRs can move forward.

### 3. Changes in Repo (Merge)  
- Once approved, PRs are merged into `main` or `dev`.
- `main` → Production, `dev` → Development/testing.

### 4. Jenkins Build 
- Merges trigger Jenkins to:
    - Build a Docker image.
    - Tag the image with:
        - `:buildNumber-hash` (e.g., 42-a1b2c3d) → unique, traceable version.
        - `:latest` → always points to the newest successful build.
    - Push both tags to Docker Hub.

### 5. Spinnaker Deployment
- Spinnaker detects the new image.
- Deploys it to Kubernetes with a **canary rollout**:
    - Canary pods → test stability.
    - If successful → promote to all pods (stable).

### 6. Prometheus Monitoring
- Prometheus collects metrics (pod health, latency, error rates).
- Canary rollout decisions are supported by Prometheus data.

---

## Jenkinsfile

Our Jenkins pipeline automates image building and pushing to Docker Hub. Below is the pipeline definition and its explanation.

```groovy title=""
pipeline {
agent { label 'i9a-builder' }
options { timestamps() }
```

```groovy title=""
environment {
IMAGE = 'afk24cmd/miniapp'
TAG = "${env.BUILD_NUMBER}-${env.GIT_COMMIT.take(7)}"
SPINNAKER_WEBHOOK = 'https://spinnaker-api.example.com/webhooks/webhook/my-pipeline'
}
```

```groovy title=""
stages {
stage('Checkout') {
steps { checkout scm }
}
}
```

```groovy title=""
stage('Docker Login') {
steps {
withCredentials([usernamePassword(
credentialsId: 'dockerhub-creds',
usernameVariable: 'DHU',
passwordVariable: 'DHP'
)]) {
sh 'echo "$DHP" | docker login -u "$DHU" --password-stdin'
}
}
}
```

```groovy title=""
stage('Build Image') {
steps {
sh 'docker build -t $IMAGE:$TAG -t $IMAGE:latest .'
}
}
```

```groovy title=""
stage('Build Image') {
steps {
sh 'docker build -t $IMAGE:$TAG -t $IMAGE:latest .'
}
}
```

```groovy title=""
stage('Trigger Spinnaker') {
steps {
sh 'curl -X POST -H "Content-Type: application/json" \\
-d "{\\\"artifacts\\\":[{\\\"type\\\":\\\"docker/image\\\",\\\"reference\\\":\\\"$IMAGE:$TAG\\\"}]}" $SPINNAKER_WEBHOOK'
}
}
}
```

```groovy title=""
post {
always {
sh 'docker logout || true'
}
success {
echo "✅ Pushed $IMAGE:$TAG and $IMAGE:latest and notified Spinnaker"
}
failure {
echo "❌ Build/push failed — check logs."
}
}
```

---

## Explaination of Stages

- **Checkout** → Pulls latest source code from GitHub.
- **Docker Login** → Uses Jenkins credentials for secure DockerHub authentication.
- **Build Image** → Builds Docker image with dual tagging (`:buildNumber-hash` + `:latest`).
- **Push Image** → Pushes both tags to DockerHub registry.
- **Post Actions** → Ensures logout from DockerHub and logs build result.