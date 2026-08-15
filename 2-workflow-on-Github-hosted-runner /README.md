# GitHub Actions Demo

## Objective

The objective of this demo is to understand how GitHub Actions works and how to create a simple CI (Continuous Integration) workflow using a GitHub-hosted runner.

---

## Prerequisites

* GitHub Account
* GitHub Repository
* Basic understanding of GitHub repositories and commits

---

## Workflow Overview

```text
Create Repository
        ↓
Create Workflow File
        ↓
Commit Changes
        ↓
Workflow Triggered
        ↓
GitHub-hosted Runner Started
        ↓
Commands Executed
        ↓
Workflow Completed Successfully
```

---

## Steps Performed

### Step 1: Create Repository

Created a GitHub repository named:

```text
github-actions-demo
```

Repository settings:

* Public Repository
* No README
* No .gitignore
* No License

---

### Step 2: Create Workflow

Navigated to:

```text
Actions
    ↓
Set up a workflow yourself
```

Created workflow file:

```text
.github/workflows/main.yml
```

---

### Step 3: Configure Workflow

Added a workflow named:

```yaml
name: CI
```

Configured triggers:

* Push on main branch
* Pull Request on main branch
* Manual workflow dispatch

Configured job:

```yaml
build:
  runs-on: ubuntu-latest
```

---

### Step 4: Add Workflow Steps

The workflow performs the following actions:

1. Checkout repository code
2. Execute one-line script
3. Execute multi-line script

Commands executed:

```bash
echo Hello, world!

echo Add other actions to build
echo test, and deploy your project.
```

---

### Step 5: Commit Workflow

Committed the workflow file to the main branch.

Commit Message:

```text
Create CI workflow for main branch
```

---

### Step 6: Verify Workflow Execution

Opened the Actions tab and verified:

* Workflow triggered successfully
* Build job executed successfully
* GitHub-hosted Ubuntu runner started successfully
* All workflow steps completed successfully

Workflow Status:

```text
Success
```

---

## Output

```text
Hello, world!

Add other actions to build
test, and deploy your project.
```

---

## Result

Successfully created and executed a GitHub Actions workflow using a GitHub-hosted Ubuntu runner.

---

## Conclusion

This demo demonstrated the basic implementation of GitHub Actions for Continuous Integration (CI). The workflow was automatically triggered after committing changes, executed on a GitHub-hosted Ubuntu runner, and completed successfully.
