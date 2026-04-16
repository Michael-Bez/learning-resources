# Git Guide

A complete reference for version control with Git.

---

## Table of Contents

1. [What is Git?](#what-is-git)
2. [Setup](#setup)
3. [Core Concepts](#core-concepts)
4. [Starting a Repository](#starting-a-repository)
5. [Staging and Committing](#staging-and-committing)
6. [Viewing History and Status](#viewing-history-and-status)
7. [Undoing Changes](#undoing-changes)
8. [Branches](#branches)
9. [Merging](#merging)
10. [Rebasing](#rebasing)
11. [Remote Repositories](#remote-repositories)
12. [Stashing](#stashing)
13. [Tags](#tags)
14. [.gitignore](#gitignore)
15. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## What is Git?

Git is a **distributed version control system**. It tracks changes to files over time so you can recall any version, collaborate with others, and maintain a full history of your project.

**Key ideas:**
- Every developer has a complete copy of the entire repository (including its history)
- Changes are saved as **commits** — snapshots of the project at a point in time
- **Branches** allow parallel lines of work that can later be merged together

---

## Setup

Run these once after installing Git to identify yourself in commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Set your preferred editor for commit messages:

```bash
git config --global core.editor "code --wait"   # VS Code
git config --global core.editor "nano"           # Nano
```

View your current configuration:

```bash
git config --list
```

---

## Core Concepts

| Term | Meaning |
|------|---------|
| **Repository (repo)** | A directory tracked by Git, containing all files and their history |
| **Working directory** | The files on disk you are currently editing |
| **Staging area (index)** | A preparation zone — files you've marked to include in the next commit |
| **Commit** | A saved snapshot of staged changes, permanently stored in history |
| **Branch** | A named pointer to a commit — lets you work on separate features in parallel |
| **HEAD** | A pointer to the currently checked-out commit or branch |
| **Remote** | A version of the repository stored on another server (e.g. GitHub) |
| **Clone** | A full local copy of a remote repository |
| **Push** | Send your local commits to a remote |
| **Pull** | Fetch and merge changes from a remote into your local branch |

### The Three States

A file in a Git project exists in one of three states:

```
Modified → Staged → Committed
```

1. **Modified** — you've changed the file but haven't staged it
2. **Staged** — you've marked the change to go into the next commit
3. **Committed** — the change is safely stored in Git's history

---

## Starting a Repository

### Create a New Repo

```bash
git init
```

Turns the current directory into a Git repository. Creates a hidden `.git` folder.

```bash
git init my-project
```

Creates a new directory called `my-project` and initialises it as a repo.

### Clone an Existing Repo

```bash
git clone https://github.com/user/repo.git
```

Downloads the full repository (including history) into a new folder named after the repo.

```bash
git clone https://github.com/user/repo.git my-folder
```

Clones into `my-folder` instead.

---

## Staging and Committing

### Check What Has Changed

```bash
git status
```

Shows which files are modified, staged, or untracked.

### Stage Files

```bash
git add filename.txt        # Stage a specific file
git add src/                # Stage all files in a directory
git add .                   # Stage all changes in the current directory
git add -p                  # Interactively choose which changes to stage (hunk by hunk)
```

### Unstage a File

```bash
git restore --staged filename.txt
```

### Commit Staged Changes

```bash
git commit -m "Add login form validation"
```

Write a message in the imperative mood: *"Add feature"*, not *"Added feature"*.

### Stage and Commit in One Step

```bash
git commit -am "Fix typo in header"
```

> `-a` only stages **tracked** (previously committed) files. New files must be `git add`ed first.

### Amend the Last Commit

Fix the message or add a forgotten file to the most recent commit:

```bash
git commit --amend -m "Better commit message"
```

> Only amend commits that haven't been pushed to a shared remote, or you will rewrite history others have.

---

## Viewing History and Status

### View Commit Log

```bash
git log                          # Full log
git log --oneline                # One line per commit
git log --oneline --graph        # With branch graph
git log --oneline -10            # Last 10 commits
git log --author="Alice"         # Filter by author
git log -- filename.txt          # History for one file
```

### View Changes

```bash
git diff                         # Unstaged changes vs last commit
git diff --staged                # Staged changes vs last commit
git diff main..feature-branch    # Differences between two branches
git show abc1234                 # Show changes in a specific commit
```

---

## Undoing Changes

### Discard Unstaged Changes to a File

```bash
git restore filename.txt
```

**Warning:** This permanently discards your unsaved edits.

### Undo a Commit (keep changes staged)

```bash
git reset --soft HEAD~1
```

### Undo a Commit (keep changes unstaged)

```bash
git reset HEAD~1
```

### Undo a Commit (discard changes entirely)

```bash
git reset --hard HEAD~1
```

**Warning:** `--hard` permanently deletes the changes. Cannot be undone easily.

### Revert a Commit (safe — creates a new undo commit)

```bash
git revert abc1234
```

Creates a new commit that undoes the specified commit. Safe to use on shared branches because it doesn't rewrite history.

### Restore a File to a Specific Commit

```bash
git restore --source abc1234 filename.txt
```

---

## Branches

### List Branches

```bash
git branch            # Local branches
git branch -a         # Local and remote branches
```

### Create a Branch

```bash
git branch feature/login
```

### Switch to a Branch

```bash
git switch feature/login
```

### Create and Switch in One Step

```bash
git switch -c feature/login
```

### Rename a Branch

```bash
git branch -m old-name new-name
```

### Delete a Branch

```bash
git branch -d feature/login       # Safe delete (fails if unmerged)
git branch -D feature/login       # Force delete
```

### View the Current Branch

```bash
git branch --show-current
```

---

## Merging

Merging integrates changes from one branch into another.

```bash
git switch main
git merge feature/login
```

### Fast-Forward Merge

If `main` hasn't changed since the branch was created, Git simply moves the pointer forward. No merge commit is created.

### Merge Commit

If both branches have diverged, Git creates a new merge commit combining both histories.

```bash
git merge --no-ff feature/login    # Force a merge commit even if fast-forward is possible
```

### Resolving Merge Conflicts

When two branches edit the same part of a file, Git marks the conflict:

```
<<<<<<< HEAD
This is the version on main.
=======
This is the version on feature/login.
>>>>>>> feature/login
```

To resolve:
1. Edit the file to keep what you want (remove the conflict markers)
2. `git add filename.txt`
3. `git commit`

```bash
git merge --abort    # Cancel the merge and return to pre-merge state
```

---

## Rebasing

Rebase replays your branch's commits on top of another branch, creating a linear history.

```bash
git switch feature/login
git rebase main
```

This moves all commits on `feature/login` so they start from the tip of `main`.

### Interactive Rebase

Rewrite, reorder, squash, or drop commits:

```bash
git rebase -i HEAD~3    # Interactively edit the last 3 commits
```

In the editor, change `pick` to:
- `reword` — edit the commit message
- `squash` — combine with the previous commit
- `drop` — delete the commit

> **Rule:** Never rebase commits that have been pushed to a shared remote branch. Rebase rewrites history, which will cause problems for anyone else who has those commits.

---

## Remote Repositories

### View Remotes

```bash
git remote -v
```

### Add a Remote

```bash
git remote add origin https://github.com/user/repo.git
```

`origin` is the conventional name for the primary remote.

### Push

```bash
git push origin main              # Push local 'main' to remote 'origin'
git push -u origin feature/login  # Push and set upstream (track the remote branch)
git push                          # Push to the tracked remote branch (after -u is set)
```

### Pull

```bash
git pull                          # Fetch + merge from the tracked remote branch
git pull origin main              # Pull specifically from origin/main
git pull --rebase                 # Fetch + rebase instead of merge
```

### Fetch

Download remote changes without merging them:

```bash
git fetch origin
git fetch --all
```

After fetching, inspect with `git log origin/main` or `git diff origin/main`.

### Delete a Remote Branch

```bash
git push origin --delete feature/login
```

---

## Stashing

Stashing temporarily shelves changes so you can switch context without committing.

```bash
git stash                    # Stash current working directory changes
git stash push -m "WIP: login form"   # Stash with a descriptive name
git stash list               # List all stashes
git stash pop                # Apply most recent stash and remove it from the list
git stash apply stash@{1}    # Apply a specific stash without removing it
git stash drop stash@{1}     # Delete a specific stash
git stash clear              # Delete all stashes
```

> `git stash` only stashes tracked files by default. Use `git stash -u` to also stash untracked files.

---

## Tags

Tags mark specific commits — typically used for release versions.

```bash
git tag v1.0.0                          # Lightweight tag
git tag -a v1.0.0 -m "Version 1.0.0"  # Annotated tag (recommended)
git tag                                 # List all tags
git push origin v1.0.0                 # Push a tag to remote
git push origin --tags                 # Push all tags
git tag -d v1.0.0                      # Delete a local tag
git push origin --delete v1.0.0        # Delete a remote tag
```

---

## .gitignore

A `.gitignore` file tells Git which files and directories to ignore (never track).

Create `.gitignore` in the root of your repository:

```
# Dependencies
node_modules/

# Build output
dist/
build/

# Environment variables
.env
.env.local

# OS files
.DS_Store
Thumbs.db

# Editor files
.vscode/
.idea/

# Logs
*.log
```

### Patterns

| Pattern | Matches |
|---------|---------|
| `*.log` | All `.log` files anywhere |
| `build/` | Directory named `build` |
| `doc/*.txt` | `.txt` files in `doc/` only |
| `**/temp` | `temp` anywhere in the tree |
| `!important.log` | Exception — do NOT ignore this file |

### Ignoring Already-Tracked Files

If a file was committed before being added to `.gitignore`, you must untrack it:

```bash
git rm --cached filename.txt
git rm --cached -r node_modules/
```

Then commit the change. The file stays on disk but is no longer tracked.

---

## Quick Reference Cheat Sheet

| Task | Command |
|------|---------|
| Initialise a repo | `git init` |
| Clone a repo | `git clone <url>` |
| Check status | `git status` |
| Stage a file | `git add <file>` |
| Stage all changes | `git add .` |
| Commit | `git commit -m "message"` |
| View log (short) | `git log --oneline` |
| View unstaged diff | `git diff` |
| View staged diff | `git diff --staged` |
| Create branch | `git switch -c <branch>` |
| Switch branch | `git switch <branch>` |
| List branches | `git branch` |
| Merge branch | `git merge <branch>` |
| Rebase branch | `git rebase <branch>` |
| Delete branch | `git branch -d <branch>` |
| Push | `git push` |
| Pull | `git pull` |
| Fetch | `git fetch` |
| Add remote | `git remote add origin <url>` |
| Stash changes | `git stash` |
| Apply stash | `git stash pop` |
| Discard file changes | `git restore <file>` |
| Unstage file | `git restore --staged <file>` |
| Undo last commit | `git reset HEAD~1` |
| Revert a commit | `git revert <hash>` |
| Tag a commit | `git tag -a v1.0 -m "msg"` |

---

*Part of the learning-resources collection.*
