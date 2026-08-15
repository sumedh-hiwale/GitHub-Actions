# 🚀 GitHub Actions Self-Hosted Runner Setup on Ubuntu VM

## 🎯 Objective

Configure a Self-Hosted Runner on an Ubuntu Virtual Machine and connect it with a GitHub repository so that GitHub Actions workflows can run on the local Ubuntu VM instead of GitHub-hosted runners.

---

## 📋 Prerequisites

- GitHub Account
- GitHub Repository
- Ubuntu VM (VirtualBox)
- Internet Connection

---

## 🏗️ Step 1: Create GitHub Repository

Create a new GitHub repository.

Example:

- Repository Name: `github-actions-self-hosted-runner`
- Visibility: `Public`
- Initialize with `README.md`

---

## ⚙️ Step 2: Navigate to Runner Settings

```text
Repository
    ↓
Settings
    ↓
Actions
    ↓
Runners
```

---

## ➕ Step 3: Add New Self-Hosted Runner

Click:

```text
New self-hosted runner
```

Select:

```text
Operating System : Linux
Architecture     : x64
```

---

## 🖥️ Step 4: Open Ubuntu VM

Start Ubuntu VM and open Terminal.

---

## 📁 Step 5: Create Runner Directory

```bash
mkdir actions-runner && cd actions-runner
```

### 🔍 Purpose

Creates a directory for the GitHub Actions runner and moves into it.

---

## ⬇️ Step 6: Download Runner Package

```bash
curl -o actions-runner-linux-x64-2.334.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.334.0/actions-runner-linux-x64-2.334.0.tar.gz
```

### 🔍 Purpose

Downloads the GitHub Actions runner package.

---

## ✅ Step 7: Verify Package Integrity

```bash
echo "HASH_VALUE actions-runner-linux-x64-2.334.0.tar.gz" | shasum -a 256 -c
```

### 🔍 Purpose

Verifies the downloaded package using SHA256 checksum.

### Expected Output

```text
actions-runner-linux-x64-2.334.0.tar.gz: OK
```

---

## 📦 Step 8: Extract Runner Package

```bash
tar xzf ./actions-runner-linux-x64-2.334.0.tar.gz
```

### 🔍 Purpose

Extracts the downloaded runner package.

---

## 🔗 Step 9: Configure Self-Hosted Runner

```bash
./config.sh --url https://github.com/<USERNAME>/<REPOSITORY-NAME> --token <TOKEN>
```

### 🔍 Purpose

Registers the Ubuntu VM as a Self-Hosted Runner with GitHub.

---

## ⚡ Step 10: Accept Default Configuration

Press **Enter** for all prompts.

```text
Runner Group     → Enter
Runner Name      → Enter
Additional Labels→ Enter
Work Folder      → Enter
```

### Expected Output

```text
Runner successfully added
Runner connection is good
Settings Saved
```

---

## 🔎 Step 11: Verify Runner Registration

Navigate to:

```text
Repository
    ↓
Settings
    ↓
Actions
    ↓
Runners
```

### Expected Result

```text
Runner Name : node01
Status      : Idle
Labels      : self-hosted, Linux, X64
```

---

## ▶️ Step 12: Start Runner

```bash
./run.sh
```

### 🔍 Purpose

Starts the Self-Hosted Runner and begins listening for GitHub Actions jobs.

### Expected Output

```text
Current runner version: '2.334.0'

Listening for Jobs
```

---

## 🔄 Architecture Flow

```text
Developer Pushes Code
          │
          ▼
GitHub Actions Workflow
          │
          ▼
Self-Hosted Runner (Ubuntu VM)
          │
          ▼
Job Execution
```

---

## 🎉 What We Achieved

✅ Configured a Self-Hosted Runner

✅ Connected Ubuntu VM with GitHub Repository

✅ Enabled GitHub Actions execution on a local machine

✅ Verified runner registration and connectivity

---

## 🏁 Conclusion

Successfully configured a GitHub Actions Self-Hosted Runner on an Ubuntu Virtual Machine and connected it to a GitHub repository. The runner is now ready to execute GitHub Actions workflows locally on the Ubuntu VM.

⭐ This setup allows GitHub Actions jobs to run on your own infrastructure instead of GitHub-hosted runners.
