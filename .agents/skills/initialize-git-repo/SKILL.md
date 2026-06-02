---
name: initialize-git-repo
description: "Creates a new private GitHub repository using the current workspace directory name and initializes it with an initial commit."
version: 1.1.0
---

## **Skill: Initialize Git Repository**

### **Description**
This skill initializes a new local Git repository (if not already initialized), creates a corresponding private repository on GitHub using the workspace directory name, and pushes an initial commit.

## Prerequisites Check

1. **Verify `gh` CLI is installed and authenticated**:
   ```powershell
   gh --version
   gh auth status
   ```
   If `gh` is not authenticated, run `gh auth login` or ask the user to authenticate.

2. **Verify `git` is installed**:
   ```powershell
   git --version
   ```

3. **Determine the repository name**:
   The repository name will be derived from the current workspace directory name (the folder name containing the project).

   ```powershell
   $repoName = (Get-Item .).Name
   Write-Output "Repository name will be: $repoName"
   ```

## ⚠️ Important: PowerShell Command Execution

**Do not concatenate multiple commands with `;` or `&&` in a single terminal invocation.** PowerShell may misinterpret flags (e.g., `-m`) as belonging to the first command instead of the second. Always execute each command as a **separate terminal call** — one command per invocation. This avoids parsing errors and ensures reliable execution.

## Performing the Workflow

1. **Create a `.gitignore` file** (if one does not already exist):
   A `.gitignore` prevents unwanted files (dependencies, build artifacts, IDE config, OS files) from being committed.

   Check if a `.gitignore` already exists:
   ```powershell
   if (-not (Test-Path -Path .gitignore -PathType Leaf)) { Write-Output "MISSING" }
   ```
   If it outputs `MISSING`, create one with sensible defaults for a Node.js/Python project. Adjust the patterns to match your tech stack:
   ```powershell
   @"
   # Dependencies
   node_modules/
   .pnp
   .pnp.js

   # Build output
   dist/
   build/
   out/

   # Environment
   .env
   .env.local
   .env.*.local

   # IDE
   .vscode/settings.json
   .idea/
   *.swp
   *.swo

   # OS
   .DS_Store
   Thumbs.db

   # Logs
   logs/
   *.log
   npm-debug.log*

   # Runtime
   pids/
   *.pid
   *.seed
   *.pid.lock

   # Coverage
   coverage/
   *.lcov
   "@ | Set-Content -Path .gitignore
   ```
   > **Note**: Customize the `.gitignore` patterns to match your project's tech stack. For example, add `__pycache__/` and `*.pyc` for Python projects, or `vendor/` for PHP projects.

2. **Initialize Git locally (if not already initialized)**:
   ```powershell
   git init
   ```

3. **Stage all files and create an initial commit**:
   ```powershell
   git add .
   ```
   Then, as a separate command:
   ```powershell
   git commit -m "Initial commit"
   ```
   > **Note**: If you already have uncommitted work that should not be in the initial commit, stage only the desired files with `git add <path>` instead of `git add .`.

4. **Create the private GitHub repository**:
   Use `gh` to create a private repo with the workspace directory name.

   First, capture the directory name:
   ```powershell
   $repoName = (Get-Item .).Name
   ```
   Then, as a separate command, create the repo:
   ```powershell
   gh repo create $repoName --private --source=. --remote=origin --push
   ```
   **Flags explained**:
   - `--private`: Creates a private repository
   - `--source=.`: Uses the current directory as the source
   - `--remote=origin`: Names the remote "origin"
   - `--push`: Pushes the local commits to the remote

5. **Verify the remote is configured**:
   ```powershell
   git remote -v
   ```
   This should show `origin` pointing to `https://github.com/<your-username>/<repo-name>.git` or `git@github.com:<your-username>/<repo-name>.git`.

6. **Verify the repository is set to track the main branch**:
   ```powershell
   git branch --show-current
   ```
   If the branch is `master` instead of `main`, rename it.

   First, rename the local branch:
   ```powershell
   git branch -m master main
   ```
   Then push the renamed branch:
   ```powershell
   git push origin main
   ```
   Finally, update the remote default branch and delete the old `master`:
   ```powershell
   gh api repos/<your-username>/<repo-name> -f default_branch=main
   git push origin --delete master
   ```

## Final Confirmation

Output a confirmation message:
> "Git repository initialized successfully. Private repo '`<repo-name>`' has been created on GitHub and the initial commit has been pushed."

## Error Handling

- **`gh` not authenticated**: If `gh auth status` fails, guide the user to run `gh auth login` or set up a GitHub token.
- **Repository already exists**: If `gh repo create` fails with "already exists", ask the user if they want to use the existing repository or choose a different name.
- **Git already initialized**: If `git init` warns that a repository already exists, proceed normally — this is not an error.
- **No files to commit**: If `git commit` fails because there are no files staged, add a `.gitkeep` or similar placeholder file:
  ```powershell
  New-Item -ItemType File -Name ".gitkeep"
  ```
  Then:
  ```powershell
  git add .gitkeep
  ```
  Then:
  ```powershell
  git commit -m "Initial commit"
  ```
- **Push rejected**: If the remote has existing commits, use `git pull origin main --allow-unrelated-histories` and resolve any conflicts before pushing.
