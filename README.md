# Jenkins CI/CD Pipeline for Full-Stack App

This Jenkins pipeline automates the build and deployment process for a full-stack Java/Angular application.

## 🔧 What It Does

- Pulls frontend and backend code from GitHub
- Builds the Angular frontend
- Copies the frontend build to the backend webapp directory
- Builds the backend with Maven and generates a WAR file
- Renames the WAR file with a timestamp
- Deploys the WAR file to a Nexus repository

## ⚙️ Jenkins Configuration

### Tools Required
- NodeJS (label: `NODEJS`)
- Maven
- Git

### Credentials Needed
- **GitHub Token** stored in Jenkins as: `gestinova_secret`
- **Nexus Credentials** stored in Jenkins as: `nexus_credentials`

### Environment Variables (Set in Jenkinsfile)
- `FRONTEND_REPO_NAME`
- `FRONTEND_GITHUB_BRANCH`
- `BACKEND_REPO_NAME`
- `BACKEND_GITHUB_BRANCH`
- `FRONTEND_PROJECT_PATH`
- `BACKEND_PROJECT_PATH`
- `WEBAPP_PATH`
- `RESOURCES_PATH`
- `NEXUS_URL`
- `NEXUS_CREDENTIALS_ID`

## 📦 Deployment Info

- GroupId: `com.algebra`
- ArtifactId: `algebraSystem`
- Version: `0.0.1-SNAPSHOT`
- Packaging: `war`
- Nexus Repository ID: as defined in the pipeline

## 🖥️ Notes

- This pipeline is intended to run on **Windows**
- Uses `bat` commands for build and copy steps
- WAR file is deployed to Nexus with a timestamped filename
