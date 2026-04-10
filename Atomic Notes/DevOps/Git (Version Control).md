---
status: seedling
tags: [devops, vcs, git, programming]
created: 2026-04-10
updated: 2026-04-10
---

# Git (Version Control)

## Summary
A distributed version control system for tracking changes in source code across multiple developers.

## Details
Git is a **distributed** version control system (DVCS), meaning every developer has a full copy of the codebase and its entire history locally.

### Core DevOps Principles (CAMS)
- **Culture**: Shared responsibility across development and operations.
- **Automation**: Reducing manual tasks like testing and deployment.
- **Measurement**: Using metrics to improve performance.
- **Sharing**: Collaborative documentation and knowledge.

### DevOps Logic
- **Collaboration**: Removes silos by promoting shared responsibility for the product lifecycle.
- **Automation**: Replaces manual tasks like testing and deployment, reducing errors and increasing delivery speed.

### Git Workflows & Conflicts
1. **Branching**: `git checkout -b feature/new-module` for isolated development.
2. **Fetch & Pull**: `git fetch origin` followed by `git pull origin main` to synchronize local code with remote changes.
3. **Merge Conflict Resolution**: Occurs when two developers change the same line.
   - Open conflicted files and find markers (`<<<<<<<`, `=======`, `>>>>>>>`).
   - Edit the file to the desired state and save.
   - Stage the resolution: `git add <filename>`.
4. **Finalizing**: `git commit -m "Resolved conflicts"` and `git push origin feature/new-module`.

### Git Comparison & Comparison Commands
- **Branch Comparison**: `git diff branchA..branchB` to see differences in code.
- **Merge**: `git merge feature` combines the history of branches.
- **Rebase**: `git rebase main` rewrites the feature branch's base to the latest `main` commit, creating a clean, linear history.

### Advanced Operations
- **`git stash` / `git stash pop`**: Temporarily shelving uncommitted changes.
- **`git reset --soft HEAD~1`**: Undoes the last commit but keeps the code changes staged.
- **`git reset --hard HEAD~1`**: Undoes the last commit and discards all local changes.

### Comparison of Methodologies
- **GitFlow**: A robust, release-driven workflow using multiple long-lived branches (`master`, `develop`, `release`).
- **Feature Branch Workflow**: A simplified, trunk-based model ideal for fast, continuous delivery (CD).

## Associative Trails
Git is the foundation of modern software development and CI/CD pipelines. This note exists to document common operations and workflows, refining the transition from manual file management to professional version control systems.

## Connections
- [[DevOps Engineering (MOC)]]
- [[Jenkins (CI and CD)]]
- [[Docker and Containerization]]

## Sources
- DevOps Engineering Exam Preparation.md
