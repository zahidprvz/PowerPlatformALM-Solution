# Power Platform Solution ALM with GitHub Actions

This repository contains **two GitHub Actions workflows** designed purely for **learning and practice**.  
They demonstrate how to automate exporting, storing, and deploying Power Platform solutions using GitHub Actions and the Power Platform CLI.

---

## 📌 Workflows in This Repo

### 1. `export-solution.yml`
This workflow is responsible for **exporting a solution** from your development environment and saving it in the repo.

What it does:
- Authenticates to the **Dev Environment** using a Service Principal.
- Exports the selected solution in **two formats**:
  - **Unmanaged** (`out/unmanaged/`) → used for unpacking, source control, and future changes.
  - **Managed** (`out/managed/`) → used for deployment into Test/Prod environments.
- Unpacks the unmanaged solution into the `solutions/` folder so the solution can be versioned and tracked in Git.
- Commits and pushes changes back into the repo.

👉 **Usage**:  
- Trigger manually from the **Actions** tab.
- Provide:
  - `solution_name` → The logical name of the solution in your Dev environment.
  - `source_branch` → The branch to save the exported solution into (usually `main`).

---

### 2. `deploy-solution-to-test.yml`
This workflow is responsible for **importing a managed solution** into your Test environment.

What it does:
- Checks out the repo and verifies that a **managed solution zip file** exists in `out/managed/`.
- Installs the Power Platform CLI.
- Authenticates to the **Test Environment** using a Service Principal.
- Imports the managed solution with retry logic (in case of transient errors).
- Publishes the solution after a successful import.
- Clears authentication after completion.

👉 **Usage**:  
- Trigger manually from the **Actions** tab.
- Provide:
  - `solution_name` → The solution you want to deploy (must already have been exported with workflow 1).
  - `source_branch` → The branch where the managed zip is stored (usually `main`).

---

## 🛠️ Requirements

To run these workflows successfully, you need:
- **Service Principal authentication** configured in both Dev and Test environments.
- GitHub repo **secrets** set up:
  - `ENVIRONMENT_URL` → Dev environment URL
  - `TARGET_ENVIRONMENT_URL` → Test environment URL
  - `CLIENT_ID` → Azure AD app registration ID
  - `TENANT_ID` → Azure AD tenant ID
  - `CLIENT_SECRET` → Secret from the app registration
  - `GITTOKEN` → GitHub Personal Access Token (with `repo` permissions) to allow commits back into the repo.

---

## ⚠️ Notes

- These workflows are **simplified for practice and learning**.  
  In real-world ALM pipelines:
  - You would integrate automated testing, quality checks, and approvals.
  - Managed solutions would typically flow from Dev → Test → UAT → Prod in sequence.
  - Branching strategies (feature branches, pull requests) would manage changes to the `solutions/` folder.

- The **solution files and folders** in this repo are for **educational purposes only** and may be deleted or reset to repeat the exercise.

---

## ✅ Summary

- **Workflow 1 (`export-solution.yml`)** → Pull solutions **out of Dev**, commit unmanaged + managed versions to GitHub.  
- **Workflow 2 (`deploy-solution-to-test.yml`)** → Push the **managed solution into Test**, simulating a deployment pipeline.  

This repo is intended as a **practice playground** to understand how GitHub Actions can be used for **Power Platform ALM automation**.
