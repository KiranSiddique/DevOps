# 🧪 Lab 03: Git History, Stashing, and Reverting Commits

![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)

---

## 👤 Student Information

| Field | Details |
|---|---|
| **Name** | Kiran Siddique |
| **Registration No.** | 2023-BCS-052 |
| **Course** | CS-649 (Cloud DevOps) |
| **Section** | A |
| **Instructor** | Sir Waqas Saleem |
| **GitHub Username** | KiranSiddique |

---

## 🎯 Objective

This lab covers advanced Git operations for managing commit history, recovering old versions, and undoing mistakes safely, both locally and on a remote repository.

---

## 🛠️ Tools Used

- **Git** for version control
- **GitHub** for remote repositories and GitHub Pages
- **Ubuntu Server** (running in VMware) accessed through SSH from Windows
- **Docker Compose** for running Gitea in GitHub Codespaces

---

## 📋 Tasks Overview

| # | Task | Key Commands | Outcome |
|:-:|---|---|---|
| 1 | Pull vs Pull `--rebase` | `git pull --no-rebase`, `git pull --rebase` | Merged and rebased local and remote commits |
| 2 | Manual merge conflict | `git rebase --continue` | Resolved a conflict in `README.md` |
| 3 | `.gitignore` and tracked files | `git rm -r --cached` | Untracked a directory and kept the local files |
| 4 | Temporary changes | `git stash`, `git stash pop` | Saved and restored uncommitted work |
| 5 | Checkout a commit | `git log`, `git checkout <hash>` | Viewed an old commit in detached HEAD |
| 6 | Soft vs hard reset | `git reset --soft`, `git reset --hard` | Compared how each reset affects files and history |
| 7 | Amend the last commit | `git commit --amend` | Added a file to the previous commit |
| 8 | Safe revert | `git revert` | Undid a commit by adding a new commit |
| 9 | Force push | `git push --force` | Rewrote a test branch's history (test branch only) |
| 10 | Gitea in Codespaces | `docker compose up -d` | Ran Gitea and created a repository |
| 11 | GitHub Pages | GitHub Pages settings | Published a portfolio website |

---

## 🔍 Key Learnings

- **`pull --rebase`** keeps the history linear, while **`pull --no-rebase`** creates a merge commit.
- **`.gitignore`** does not affect files that are already tracked. Use `git rm --cached` to untrack them.
- **`git stash`** lets you switch branches safely when you have uncommitted changes.
- **Soft reset** keeps changes staged, and **hard reset** discards them.
- **`git revert`** is the safe way to undo a commit on a shared branch, because it does not rewrite history.
- **`git push --force`** rewrites remote history. Use it only on your own branches.

---

## 📁 Repository Structure

```
Labs/
└── Lab03/
    ├── README.md
    ├── temp.txt
    ├── compose.yaml
    ├── Lab3_Solution.pdf
    └── screenshots/
```

The full step-by-step procedure, all commands, and every screenshot with its caption are in **`Lab3_Solution.pdf`**.

---

## ✅ Result

All 11 tasks were completed. Each task was verified with a screenshot saved in the `screenshots/` folder.
