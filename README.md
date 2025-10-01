# CI/CD Pipeline with Jenkins, AWS ECR, ArgoCD & EKS  

This repository contains a **Jenkins Declarative Pipeline** (`Jenkinsfile`) to automate the build, push, and deployment process of a full-stack application (Frontend + Backend) on **AWS EKS** using **ArgoCD**.  

---

## Pipeline Overview  

The pipeline performs the following stages:  

1. **Clone Repo (Frontend + Backend)**  
   - Clones the application source code from GitHub.  

2. **Backend Build & Push**  
   - Builds a Docker image for the backend service.  
   - Pushes it to **AWS Elastic Container Registry (ECR)**.  

3. **Frontend Build & Push**  
   - Builds a Docker image for the frontend service.  
   - Pushes it to **AWS ECR**.  

4. **Update Kubernetes Manifests**  
   - Clones the deployment manifests repo (`appdeploy`).  
   - Updates image tags in `backend.yml` & `frontend.yml`.  
   - Commits and pushes changes to GitHub.  

5. **Deploy with ArgoCD**  
   - Logs in to ArgoCD.  
   - Syncs the application with updated manifests.  
   - Deploys to AWS EKS cluster in the specified namespace.  

---

## ⚙️ Environment Variables  

| Variable       | Description |
|----------------|-------------|
| `CUSTOM_DIR`   | Directory for cloning application code |
| `CUSTOM_DIR2`  | Directory for cloning Kubernetes manifests repo |
| `GIT_BRANCH`   | Git branch to build from (default: `main`) |
| `GIT_URL`      | Application repository URL |
| `IMAGE_NAME`   | AWS ECR repository name |
| `IMAGE_TAG1`   | Backend Docker image tag (with build number) |
| `IMAGE_TAG2`   | Frontend Docker image tag (with build number) |
| `AWS_REGION`   | AWS region |
| `ECR_URL`      | AWS ECR registry URL |
| `GIT_YAML_URL` | Git repo for Kubernetes manifests |
| `ARGOCD_SERVER`| ArgoCD server endpoint |
| `NAMESPACE`    | Kubernetes namespace for deployment |
| `K8S_SERVER`   | EKS cluster API endpoint |

---

## Credentials Required  

- **AWS Credentials (`aws-cred`)** – for ECR & EKS access.  
- **SSH Key (`ssh-key`)** – for Git push to the manifests repo.  
- **ArgoCD Token (`ARGOCD_TOKEN`)** – for ArgoCD authentication.  

---

## Docker Images  

- Backend image: 
- Frontend image:

  
These images are automatically built, tagged, and pushed to **AWS ECR**.  

---

## Deployment  

- Kubernetes manifests are stored in a **separate repo** (`kubernetes-Deployment-Project`).  
- Jenkins updates the `backend.yml` and `frontend.yml` with new image tags.  
- ArgoCD syncs the manifests and deploys the app to **EKS**.  

---

## 📜 Prerequisites  

- Jenkins server with:  
- Docker  
- AWS CLI  
- ArgoCD CLI (`argocd`)  
- kubectl  
- AWS EKS cluster running.  
- AWS ECR repository created.  
- ArgoCD configured with access to `appdeploy` repo.  

---

##  Usage  

1. Update the environment variables in the `Jenkinsfile`.  
2. Ensure credentials are set in Jenkins (`aws-cred`, `ssh-key`, `ARGOCD_TOKEN`).  
3. Trigger the Jenkins pipeline.  
4. Watch your application get built, pushed, and deployed automatically!  

---

## Project Flow  

```mermaid
graph TD
  A[GitHub Repo - App-code] -->|Clone| B[Jenkins Pipeline]
  B --> C[Backend Build & Push to ECR]
  B --> D[Frontend Build & Push to ECR]
  B --> E[Update K8s Manifests Repo]
  E --> F[Git Push Manifests Repo]
  F --> G[ArgoCD Sync]
  G --> H[Deploy to AWS EKS]


