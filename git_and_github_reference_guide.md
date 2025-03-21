By Joshua Hall, Edited by Claude.ai

Additional Recommended Resources:
[Introduction to Git and Github by Launch School](https://launchschool.com/books)
[Coursera: Version Control with Git](https://www.coursera.org/learn/version-control-with-git)
[Oh My GIT!](https://ohmygit.org/)
## Overview

Git is a distributed version control system (DVCS) that allows developers to track changes, collaborate on code, and maintain a history of their work. GitHub is a popular platform that hosts Git repositories, making it easier to collaborate and share code with others.

## Setup and Configuration

### Installation

```bash
# Check Git version
git --version

# Install Git on Ubuntu
sudo apt-get update
sudo apt-get install git
```

### Configuration

```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default branch name
git config --global init.defaultBranch main
```

## Basic Operations

### Creating Repositories

```bash
# Initialize a new repository
git init

# Clone an existing repository
git clone <repository-url> [local-directory-name]
```

### Tracking Files

```bash
# Check repository status
git status

# Add files to staging area
git add <file>           # Add specific file
git add .                # Add all changes

# Remove files from staging
git rm --cached <file>
```

### Committing Changes

```bash
# Create a commit
git commit -m "Your commit message"

# Amend the last commit
git commit --amend -m "Updated commit message"
```

### Viewing History

```bash
# View commit history
git log                   # Full history
git log --oneline         # Compact view
git log --oneline --graph # Visual representation
git log --oneline --graph --all # All branches

# View specific commit
git show <commit-hash>
git show HEAD             # Latest commit
git show HEAD~            # Parent of latest commit
git show HEAD~2           # Grandparent of latest commit
```

## Working with Branches

### Branch Management

```bash
# List branches
git branch                # Local branches
git branch --all          # Include remote branches

# Create branch
git branch <branch-name>

# Switch branches
git checkout <branch-name>

# Create and switch (single command)
git checkout -b <branch-name>

# Delete branches
git branch -d <branch-name>   # Safe delete
git branch -D <branch-name>   # Force delete
```

### Merging

```bash
# Merge a branch into current branch
git merge <branch-name>

# Create a merge commit (no fast-forward)
git merge --no-ff <branch-name>

# Squash merge (combines all commits into one)
git merge --squash <branch-name>
git commit -m "Squashed commit message"
```

### Resolving Merge Conflicts

```bash
# When a merge conflict occurs:
# 1. Edit files to fix conflicts
# 2. Add resolved files to staging
git add <resolved-file>
# 3. Complete the merge
git commit

# Abort a merge with conflicts
git merge --abort
```

### Rebasing

```bash
# Rebase current branch onto another
git rebase <branch-name>

# Interactive rebase
git rebase -i <commit-hash>
# This opens editor where you can:
# - pick: keep the commit
# - squash: combine with previous commit
# - drop: remove the commit
# - edit: stop to modify the commit

# Continue after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort
```

## Remote Repositories

### Managing Remotes

```bash
# View remote repositories
git remote -v

# Add a remote
git remote add <name> <url>
# Example:
git remote add origin https://github.com/username/repo.git
```

### Synchronizing with Remotes

```bash
# Fetch changes from remote
git fetch
git fetch <remote-name>

# Pull changes (fetch + merge)
git pull
git pull --ff-only  # Fast-forward only
git pull <remote> <branch>

# Push changes to remote
git push <remote> <branch>
git push -u origin main  # Push and set tracking
```

### Working with Tracking Branches

```bash
# Set up tracking
git branch --set-upstream-to=<remote>/<branch>

# Check tracking branches
git branch -vv
```

## Tags

```bash
# List tags
git tag

# Create a lightweight tag
git tag <tag-name>

# Create an annotated tag
git tag -a -m "Tag message" <tag-name>

# Push tags to remote
git push origin <tag-name>
git push --tags  # Push all tags
```

## Advanced Operations

### Stashing Changes

```bash
# Save changes temporarily
git stash save "Description"

# List stashes
git stash list

# Apply most recent stash
git stash apply

# Apply specific stash
git stash apply stash@{n}

# Remove stash after applying
git stash pop

# Drop a stash
git stash drop stash@{n}
```

### Cleaning the Working Directory

```bash
# Discard changes in working directory
git checkout -- <file>

# Discard all changes
git reset --hard

# Remove untracked files
git clean -f
```

## GitHub-Specific Operations

### Pull Requests

1. **Creating a Pull Request:**
    
    - Fork the repository (if not your own)
    - Create a feature branch
    - Make changes and push to your fork
    - On GitHub, navigate to the original repository
    - Click "New Pull Request"
    - Select branches to compare
    - Add description and create
2. **Reviewing a Pull Request:**
    
    - On GitHub, navigate to "Pull Requests"
    - Select the PR to review
    - Add comments, approve, or request changes
3. **Merging a Pull Request:**
    
    - After approval, click "Merge"
    - Choose merge strategy (merge commit, squash, rebase)
    - Confirm the merge

### Fork and Sync

```bash
# Clone your fork
git clone https://github.com/your-username/repo.git

# Add original repo as upstream
git remote add upstream https://github.com/original-owner/repo.git

# Fetch upstream changes
git fetch upstream

# Merge upstream changes to local branch
git checkout main
git merge upstream/main
```

## Gitflow Workflow

Gitflow is a branching model with two main branches:

- `main`: Production-ready code
- `develop`: Integration branch for features

Feature branches:

```bash
# Create feature branch from develop
git checkout develop
git checkout -b feature/new-feature

# Finish feature
git checkout develop
git merge --no-ff feature/new-feature
git branch -d feature/new-feature
```

Release branches:

```bash
# Create release branch
git checkout develop
git checkout -b release/1.0.0

# Finish release
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Version 1.0.0"
git checkout develop
git merge --no-ff release/1.0.0
git branch -d release/1.0.0
```

Hotfix branches:

```bash
# Create hotfix branch
git checkout main
git checkout -b hotfix/1.0.1

# Finish hotfix
git checkout main
git merge --no-ff hotfix/1.0.1
git tag -a v1.0.1 -m "Version 1.0.1"
git checkout develop
git merge --no-ff hotfix/1.0.1
git branch -d hotfix/1.0.1
```

## Best Practices

1. **Commit messages:**
    
    - Use present tense ("Add feature" not "Added feature")
    - Be descriptive but concise
    - Start with a verb
2. **Branching:**
    
    - Use descriptive branch names (feature/login, bugfix/header)
    - Delete branches after merging
3. **Pull frequently:**
    
    - Regularly pull changes from shared branches
    - Resolve conflicts early
4. **Review before pushing:**
    
    - Use `git diff` to review changes
    - Check status before committing
5. **Use .gitignore:**
    
    - Exclude build artifacts, dependencies, and sensitive files
    - Create a .gitignore file in your repository root
6. **Security:**
    
    - Never commit sensitive data (passwords, keys)
    - If accidentally committed, change credentials and consider repository history cleanup

## Common Issues

### Undoing Changes

```bash
# Unstage a file
git reset HEAD <file>

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Undo public commits
git revert <commit-hash>
```

### Recovering Lost Work

```bash
# View reference log
git reflog

# Recover deleted branch
git checkout -b <branch-name> <commit-hash>
```

### Handling Large Files

Use Git LFS (Large File Storage):

```bash
git lfs install
git lfs track "*.psd"
git add .gitattributes
```

### Working in Detached HEAD State

```bash
# Create branch to save work
git checkout -b new-branch-name
```

This reference guide covers the essential Git and GitHub operations you'll need for effective version control and collaboration. Remember that Git is a powerful tool with many more features and options than listed here, but these commands will handle most common scenarios.