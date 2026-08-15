# Docker CI Setup using GitHub Actions

## Overview

This project demonstrates how to implement a Docker Continuous Integration (CI) pipeline using GitHub Actions. The workflow automatically builds a Docker image and pushes it to Docker Hub whenever code is pushed to the GitHub repository.

---

## Objective

* Create a Docker image using a Dockerfile.
* Configure GitHub Actions workflow.
* Authenticate with Docker Hub using GitHub Secrets.
* Automatically build and push Docker images to Docker Hub.
* Verify successful image deployment.

---

## Technologies Used

* GitHub
* GitHub Actions
* Docker
* Docker Hub

---

## Repository Structure

```text
docker-ci-demo
│
├── Dockerfile
│
└── .github
    └── workflows
        └── main.yaml
```

---

## Step 1: Create GitHub Repository

Created a GitHub repository:

```text
docker-ci-demo
```

---

## Step 2: Create Dockerfile

Created a Dockerfile in the repository root directory.

### Dockerfile

```dockerfile
FROM alpine:latest

CMD ["echo", "Hello from Docker!"]
```

This Dockerfile creates a lightweight Alpine Linux image and prints a message when the container starts.

---

## Step 3: Generate Docker Hub Access Token

Generated a Personal Access Token from Docker Hub.

### Navigation

```text
Docker Hub
→ Account Settings
→ Personal Access Tokens
→ Generate New Token
```

### Token Permissions

```text
Read
Write
Delete
```

---

## Step 4: Configure GitHub Secrets

Added the following repository secrets:

### Secret 1

```text
DOCKERHUB_USERNAME
```

Value:

```text
Docker Hub Username
```

### Secret 2

```text
DOCKERHUB_TOKEN
```

Value:

```text
Docker Hub Personal Access Token
```

### Navigation

```text
GitHub Repository
→ Settings
→ Secrets and Variables
→ Actions
→ New Repository Secret
```

---

## Step 5: Create GitHub Actions Workflow

Created workflow file:

```text
.github/workflows/main.yaml
```

### Workflow Configuration

```yaml
name: Docker_CI_setup

on:
  push:

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      -
        name: Set up QEMU test
        uses: docker/setup-qemu-action@v3

      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      -
        name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      -
        name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/qod:docker-ci-setup
```

---

## Workflow Explanation

### Trigger

```yaml
on:
  push:
```

The workflow is triggered automatically whenever code is pushed to the repository.

### Set Up QEMU

```yaml
uses: docker/setup-qemu-action@v3
```

Enables multi-platform Docker image builds.

### Set Up Docker Buildx

```yaml
uses: docker/setup-buildx-action@v3
```

Provides advanced Docker build capabilities.

### Login to Docker Hub

```yaml
uses: docker/login-action@v3
```

Authenticates with Docker Hub using GitHub Secrets.

### Build and Push Docker Image

```yaml
uses: docker/build-push-action@v6
```

Builds the Docker image from the Dockerfile and pushes it to Docker Hub.

---

## Step 6: Commit and Push Changes

```bash
git add .

git commit -m "Add Docker CI setup workflow"

git push origin main
```

---

## Step 7: Verify Workflow Execution

Navigate to:

```text
GitHub Repository
→ Actions
```

Verify that the workflow runs successfully.

Expected Result:

```text
Set up QEMU           ✅
Set up Docker Buildx  ✅
Login to Docker Hub   ✅
Build and Push        ✅
Complete Job          ✅
```

---

## Issue Encountered

### Error

```text
401 Unauthorized
access token has insufficient scopes
```

### Root Cause

The Docker Hub Personal Access Token was initially created with:

```text
Public Repo Read-only
```

permissions.

The workflow required push access to Docker Hub.

### Resolution

Updated the token permissions to:

```text
Read
Write
Delete
```

Re-ran the GitHub Actions workflow successfully.

---

## Step 8: Verify Docker Image in Docker Hub

Navigate to:

```text
Docker Hub
→ Repositories
→ qod
```

Verification:

```text
Repository Created Successfully
Image Pushed Successfully
Latest Push Timestamp Updated
```

---

## Result

Successfully implemented a Docker Continuous Integration (CI) pipeline using GitHub Actions.

The workflow automatically:

1. Builds a Docker image.
2. Authenticates with Docker Hub.
3. Pushes the Docker image to Docker Hub.
4. Runs automatically on every code push.

---

## Outcome

This project demonstrates a complete Docker CI pipeline using GitHub Actions and Docker Hub for automated container image build and deployment.
