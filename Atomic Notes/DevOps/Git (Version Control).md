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
Distributed version control system for tracking changes in source code.

### Workflow: Modifying a File
1. **Create a Feature Branch**: `git checkout -b feature/new-module`.
2. **Fetch and Pull Latest Changes**: `git fetch origin` followed by `git pull origin main`.
3. **Resolve Merge Conflicts**: Open the conflicted file, manually check markers (`<<<<<<<`, `=======`, `>>>>>>>`), edit, and save.
4. **Stage Changes**: `git add <filename>`.
5. **Push Merged Code**: `git commit -m "Resolved merge conflicts"` followed by `git push origin feature/new-module`.

### Comparison of Operations
- **`git stash` / `git stash pop`**: Temporarily shelving and reapplying changes.
- **`git reset --soft HEAD~1`**: Undoes the last commit but keeps the changes staged.
- **`git reset --hard HEAD~1`**: Undoes the last commit and discards all changes.
- **`git rebase main`**: Rewrites commit history by applying changes onto the latest `main` branch.
- **`git merge feature`**: Combines the history of two branches.

### Comparison of Workflows
- **GitFlow**: A release-driven workflow with multiple long-lived branches (e.g., `develop`, `release`, `master`).
- **Feature Branch Workflow**: A simplified workflow ideal for continuous delivery.

## Associative Trails
Git is the foundation of modern software development and CI/CD pipelines. This note exists to document common operations and workflows, refining the transition from manual file management to professional version control systems.

## Connections
- [[DevOps Engineering (MOC)]]
- [[Jenkins (CI and CD)]]
- [[Docker and Containerization]]

## Sources
- DevOps Engineering Exam Preparation.md
