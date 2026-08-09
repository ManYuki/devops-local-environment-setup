
# DevOps Workstation Setup Documentation

## Overview
This document logs the setup, package management choices, and fixes applied while configuring an Ubuntu Linux machine for DevOps workflows.

---
Setup Evidence & Verification Screenshots

All setup verification evidence is organized inside the dedicated `screenshots/` directory:

* **VS Code Installation:** `screenshots/vscode_installation.png`
* **GitHub Account Dashboard:** `screenshots/github_dashboard.png`
* **AWS Console Dashboard:** `screenshots/aws_dashboard_1.png`
* **Azure Portal Dashboard:** `screenshots/azure_dashboard_1.png`
* **Minikube Cluster Status:** `screenshots/Local_cluster_proof.png`
* **Tools Verification Status:** `screenshots/tooling_verification_report_1.png`
* **Tools Verification Status2:** `screenshots/tooling_verification_report_2.png`
* **Git Verification Status2:** `screenshots/evidence_of_git_installation.png`


## 1. Package Management 
Tools were installed using a few different methods depending on how they are best maintained:

* **APT (Advanced Package Tool):** Used for standard system packages, core utilities, and base runtimes (`git`, `curl`, `jq`, `nodejs`, `npm`, `ansible`).
* **NPM (Node Package Manager):** Used for Node-based tools like `@google/gemini-cli`. A custom prefix (`~/.npm-global`) was set up so global packages run without needing `sudo`.
* **Direct Binaries & Official Repos:** Used for cloud and container tools (`kubectl`, `minikube`, `helm`, `terraform`, `aws-cli`, `az-cli`). Binaries were placed directly in `/usr/local/bin` to keep vendor versions up to date.

---

## 2. Environment Configuration
* **Shell Setup:** **Bash** is the primary shell for this environment.
* **Profile Settings:** Custom `PATH` exports (including `~/.npm-global/bin`) and aliases were configured in `~/.bashrc`.
* **Backup File:** A non-hidden copy (`environment_bashrc_backup`) was created in the project folder to meet assignment submission requirements.

---

## 3. Local Cluster Verification
* **Kubernetes Environment:** **Minikube** was configured to manage a single-node local cluster using the **Docker container driver**.
* **Status Check:** Node health was confirmed with `kubectl get nodes`, showing the single node in a **`Ready`** status. The output is shown in `Local_cluster_proof.png`.


---

## 4. Troubleshooting Steps

* **NPM Permission Errors (`EACCES`):**
  * *Issue:* Global npm installs failed because default system folders required root permissions.
  * *Fix:* The npm directory was changed to `~/.npm-global`, and `export PATH="$HOME/.npm-global/bin:$PATH"` was added to `~/.bashrc`.

* **Docker Running Without Sudo:**
  * *Issue:* Running `docker` commands without `sudo` threw permission errors on the unix socket.
  * *Fix:* User account `yuki` was added to the `docker` group (`sudo usermod -aG docker yuki`), and session access was updated using `newgrp docker`.

* **Ansible Version Output Format:**
  * *Issue:* Running `ansible --version` printed several lines of system paths and Python details instead of a single version line.
  * *Fix:* Confirmed that multi-line output is normal behavior for Ansible Core on Linux, and logged the complete output in `tooling_verification_report_2.png`.
