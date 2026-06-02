---
name: initialize-git-repo
description: "Creates a new private GitHub repository using the current workspace directory name and initializes it with an initial commit."
version: 1.0.0
---

## **Skill: Initialize Git Repository**

### **Description**
This skill initializes a new local Git repository (if not already initialized), creates a corresponding private repository on GitHub using the workspace directory name, and pushes an initial commit.

## Prerequisites Check

1. **Verify `gh` CLI is installed and authenticated**:
   ```bash
   gh --version
   gh auth status
   ```
   If `gh` is not authenticated, run `gh auth login` or ask the user to authenticate.

2. **Verify `git` is installed**:
   ```bash
   git --version
   ```

3. **Determine the repository name**:
   The repository name will be derived from the current workspace directory name (the folder name containing the project).

   ```bash
   $repoName = (Get-Item .).Name
   Write-Output "Repository name will be: $repoName"
   ```

## Performing the Workflow

1. **Initialize Git locally (if not already initialized)**:
   ```powershell
   git init
   ```

2. **Stage all files and create an initial commit**:
   ```powershell
   git add .
   git commit -m "Initial commit"
   ```
   > **Note**: If you already have uncommitted work that should not be in the initial commit, stage only the desired files with `git add <path>` instead of `git add .`.

3. **Create the private GitHub repository**:
   Use `gh` to create a private repo with the workspace directory name.
   ```powershell
   $repoName = (Get-Item .).Name
   gh repo create $repoName --private --source=. --remote=origin --push
   ```
   **Flags explained**:
   - `--private`: Creates a private repository
   - `--source=.`: Uses the current directory as the source
   - `--remote=origin`: Names the remote "origin"
   - `--push`: Pushes the local commits to the remote

4. **Verify the remote is configured**:
   ```powershell
   git remote -v
   ```
   This should show `origin` pointing to `https://github.com/<your-username>/<repo-name>.git` or `git@github.com:<your-username>/<repo-name>.git`.

5. **Verify the repository is set to track the main branch**:
   ```powershell
   git branch --show-current
   ```
   If the branch is `master` instead of `main`, rename it:
   ```powershell
   git branch -m master main
   git push origin main
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
  git add .gitkeep
  git commit -m "Initial commit"
  ```
- **Push rejected**: If the remote has existing commits, use `git pull origin main --allow-unrelated-histories` and resolve any conflicts before pushing.
