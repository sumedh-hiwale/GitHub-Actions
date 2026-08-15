# Docker CI Setup Using GitHub Self-Hosted Runner

## Objective

Configure a GitHub Self-Hosted Runner on an AWS EC2 Ubuntu instance and create a Docker Continuous Integration (CI) pipeline using GitHub Actions and Docker Hub.

The workflow should:

* Trigger automatically on code push.
* Run on a Self-Hosted Runner.
* Build a Docker image.
* Authenticate with Docker Hub.
* Push the image to Docker Hub.

---

## Prerequisites

* GitHub Account
* Docker Hub Account
* AWS Account
* EC2 Ubuntu Instance
* Git installed
* Docker installed
* GitHub Repository

---

# Step 1: Create GitHub Repository

Create a new GitHub repository.

Example:

```text
docker-ci-selfhosted-runner-demo
```

Clone the repository:

```bash
git clone https://github.com/<github-username>/docker-ci-selfhosted-runner-demo.git

cd docker-ci-selfhosted-runner-demo
```

---

# Step 2: Create Dockerfile

Create a file named:

```text
Dockerfile
```

Add the following content:

```dockerfile
FROM nginx:latest

COPY . /usr/share/nginx/html

EXPOSE 80
```

Commit and push:

```bash
git add .

git commit -m "Add Dockerfile"

git push origin main
```

---

# Step 3: Launch AWS EC2 Instance

Launch an Ubuntu EC2 instance.

Recommended:

* AMI: Ubuntu Server
* Instance Type: t2.micro
* Storage: 8 GB
* Security Group:

  * SSH (22)

Connect to the instance.

---

# Step 4: Install Docker

Update packages:

```bash
sudo apt update
```

Install Docker:

```bash
sudo apt install docker.io -y
```

Start Docker:

```bash
sudo systemctl enable docker

sudo systemctl start docker
```

Verify Docker:

```bash
docker --version
```

Add Ubuntu user to Docker group:

```bash
sudo usermod -aG docker ubuntu
```

Logout and login again.

Verify:

```bash
docker ps
```

---

# Step 5: Configure GitHub Self-Hosted Runner

Navigate to:

```text
Repository
→ Settings
→ Actions
→ Runners
→ New Self-Hosted Runner
```

Select:

```text
Linux
x64
```

Run the provided commands:

```bash
mkdir actions-runner && cd actions-runner
```

Download runner package:

```bash
curl -o actions-runner-linux-x64-2.334.0.tar.gz -L <runner-download-url>
```

Extract package:

```bash
tar xzf ./actions-runner-linux-x64-2.334.0.tar.gz
```

Configure runner:

```bash
./config.sh --url https://github.com/<username>/<repository-name> --token <runner-token>
```

Accept default options.

Start runner:

```bash
./run.sh
```

Expected output:

```text
Listening for Jobs
```

---

# Step 6: Create Docker Hub Access Token

Login to Docker Hub.

Navigate to:

```text
Account Settings
→ Personal Access Tokens
```

Create a token.

Copy:

* Docker Hub Username
* Docker Hub Access Token

---

# Step 7: Configure GitHub Secrets

Navigate to:

```text
Repository
→ Settings
→ Secrets and Variables
→ Actions
```

Create the following repository secrets:

### Secret 1

```text
DOCKERHUB_USERNAME
```

Value:

```text
your-dockerhub-username
```

### Secret 2

```text
DOCKERHUB_TOKEN
```

Value:

```text
your-dockerhub-access-token
```

---

# Step 8: Create GitHub Actions Workflow

Create directory:

```text
.github/workflows
```

Create file:

```text
main.yml
```

Add the following workflow:

```yaml
name: Docker_CI_setup_using_self-hosted_runner

on:
  push:

jobs:
  docker:
    runs-on: self-hosted

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and Push Docker Image
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: your-dockerhub-username/docker-ci-selfhosted-runner-demo:latest
```

Commit and push:

```bash
git add .

git commit -m "Add GitHub Actions workflow"

git push origin main
```

---

# Step 9: Trigger Workflow

Push any change to GitHub.

GitHub Actions automatically starts the workflow.

Navigate to:

```text
Repository
→ Actions
```

Expected workflow stages:

```text
Set up job
Set up QEMU
Set up Docker Buildx
Login to Docker Hub
Build and Push Docker Image
```

All steps should complete successfully.

---

# Verification

## Verify Runner Status

Navigate to:

```text
Repository
→ Settings
→ Actions
→ Runners
```

Expected:

```text
Runner Status: Idle
```

---

## Verify Workflow

Navigate to:

```text
Repository
→ Actions
```

Expected:

```text
Workflow Status: Success
```

---

## Verify Docker Hub Repository

Navigate to Docker Hub.

Expected image:

```text
your-dockerhub-username/docker-ci-selfhosted-runner-demo
```

Tag:

```text
latest
```

---

# Conclusion

Successfully configured a GitHub Self-Hosted Runner on an AWS EC2 Ubuntu instance and implemented a Docker CI pipeline using GitHub Actions and Docker Hub. The workflow automatically builds and pushes Docker images whenever code is pushed to the repository.
