# Module 01 — Git Fundamentals 📚

Master the core Git workflow: repositories, commits, branching, merging, and fixing mistakes.

## 🎯 Learning Objectives

After completing this module, you'll be able to:
- Initialize a Git repository and make meaningful commits
- Understand the staging area (index) and working tree
- View commit history and understand the DAG (directed acyclic graph)
- Create branches and merge code
- Resolve merge conflicts
- Undo and recover from common mistakes

## 📖 Concepts

### The Three States of Git

1. **Working Tree** — Your files on disk (modified but not staged)
2. **Staging Area (Index)** — Prepared files ready to commit
3. **Repository (.git/)** — Committed history

```
Working Tree  →  (git add)  →  Staging Area  →  (git commit)  →  Repository
```

### Commits

A commit is a snapshot with:
- Content hash (SHA-1)
- Author metadata (name, email, timestamp)
- Parent commit reference (linked list)
- Commit message

### Branching

A branch is a pointer to a commit. Merging combines two branches.

## 🏃 Quick Commands

| Command | Purpose |
|---------|---------|
| `git init` | Initialize a repository |
| `git add <file>` | Stage changes |
| `git commit -m "msg"` | Commit staged changes |
| `git status` | Show working tree status |
| `git diff` | Show unstaged changes |
| `git log --oneline` | View commit history |
| `git branch` | List/create branches |
| `git checkout <branch>` | Switch branches |
| `git merge <branch>` | Merge another branch |
| `git reset` | Unstage or revert commits |
| `git revert <commit>` | Create new commit that undoes a commit |

## 💪 Hands-On Exercises

### Exercise 01: Initialize and Commit

**Objective**: Create a repo, make a meaningful commit.

**Steps**:
1. Create a directory `exercise-01-hello`:
   ```bash
   mkdir exercise-01-hello
   cd exercise-01-hello
   ```

2. Initialize Git:
   ```bash
   git init
   ```

3. Create a `README.md` file:
   ```markdown
   # Hello Git
   
   This is my first Git repository.
   ```

4. Check status:
   ```bash
   git status
   ```

5. Stage and commit:
   ```bash
   git add README.md
   git commit -m "docs: initial README"
   ```

6. View the commit:
   ```bash
   git log --oneline
   ```

**Expected output**:
```
abc1234 docs: initial README
```

**Bonus**: Configure Git user (if not done):
```bash
git config user.name "Your Name"
git config user.email "you@example.com"
# Or globally: git config --global user.name "..."
```

---

### Exercise 02: Explore History and Diffs

**Objective**: View commits, diffs, and understand file changes.

**Steps**:
1. Add a few more commits to your repo:
   ```bash
   echo "- First item" >> README.md
   git add README.md
   git commit -m "docs: add first item to readme"
   
   echo "- Second item" >> README.md
   git add README.md
   git commit -m "docs: add second item to readme"
   ```

2. View one-line log:
   ```bash
   git log --oneline
   ```

3. View detailed log with graph:
   ```bash
   git log --oneline --graph --decorate --all
   ```

4. Show a specific commit:
   ```bash
   git show <commit-hash>
   ```

5. View changes between commits:
   ```bash
   git diff HEAD~2 HEAD
   ```

**Expected output**: Three commits in history with file changes shown.

---

### Exercise 03: Create, Switch, and Merge Branches

**Objective**: Practice the GitHub Flow (main branch + feature branches).

**Steps**:
1. Ensure you're on main:
   ```bash
   git branch -a
   git status
   ```

2. Create a new branch for a feature:
   ```bash
   git checkout -b feature/add-features
   # Or newer syntax: git switch -c feature/add-features
   ```

3. Make changes on the branch:
   ```bash
   echo "" >> README.md
   echo "## Features" >> README.md
   echo "- Feature A" >> README.md
   git add README.md
   git commit -m "feat: add features section"
   ```

4. Switch back to main:
   ```bash
   git checkout main
   ```

5. View the file (notice it doesn't have the features section):
   ```bash
   cat README.md
   ```

6. Merge the feature branch:
   ```bash
   git merge feature/add-features
   ```

7. View the file again (now it has the features section):
   ```bash
   cat README.md
   git log --oneline --graph --all
   ```

---

### Exercise 04: Resolve a Merge Conflict

**Objective**: Simulate and resolve a real merge conflict.

**Steps**:
1. Create two branches from main:
   ```bash
   git checkout -b feature/conflict-a
   ```

2. Edit README.md on this branch:
   ```bash
   sed -i 's/## Features/## Features from Branch A/' README.md
   git add README.md
   git commit -m "feat(a): update features title"
   ```

3. Switch and create another branch from main:
   ```bash
   git checkout main
   git checkout -b feature/conflict-b
   ```

4. Edit the same line differently:
   ```bash
   sed -i 's/## Features/## Features from Branch B/' README.md
   git add README.md
   git commit -m "feat(b): update features title"
   ```

5. Try to merge branch A:
   ```bash
   git merge feature/conflict-a
   ```

6. Observe the conflict:
   ```bash
   cat README.md
   git status
   ```

7. Manually resolve the conflict (edit README.md and keep the version you want):
   ```bash
   # Edit README.md and choose which version to keep (or combine them)
   # Remove the <<<<<<, ======, and >>>>>> markers
   ```

8. Stage and complete the merge:
   ```bash
   git add README.md
   git commit -m "merge: resolve conflict in features section"
   ```

9. View the resolved history:
   ```bash
   git log --oneline --graph --all
   ```

---

### Exercise 05: Undo and Recover from Mistakes

**Objective**: Learn `git reset`, `git revert`, and `git reflog`.

**Steps**:

#### Part A: Unstage with `git reset`
```bash
# Make a change
echo "Oops, this is a mistake" >> README.md
git add README.md

# Unstage it (keep the file modified)
git reset HEAD README.md
git status  # Should show it as modified but not staged

# Discard the change
git restore README.md
```

#### Part B: Revert a commit (safe, creates new commit)
```bash
# Make a bad commit
echo "Bad code" >> README.md
git add README.md
git commit -m "bad: this is wrong"

# Revert it (creates a new commit that undoes the bad one)
git revert HEAD
git log --oneline
# Notice: you now have TWO commits, the original and its revert
```

#### Part C: Hard reset (dangerous, use with caution)
```bash
# Make multiple commits
echo "Line 1" >> README.md
git add README.md
git commit -m "add line 1"

echo "Line 2" >> README.md
git add README.md
git commit -m "add line 2"

# Check log
git log --oneline

# Reset to 2 commits ago, discarding all changes
git reset --hard HEAD~2
git log --oneline
# Lines 1 and 2 are gone!
```

#### Part D: Recover deleted commits with `git reflog`
```bash
# After a hard reset, you can still recover if you remember the commit
git reflog  # Shows ALL operations, even undone ones
git reset --hard <commit-hash>  # Go back to the commit
```

---

## 📚 Key Takeaways

- **Commit early and often** — Small, meaningful commits are better than huge ones
- **Write good commit messages** — Use imperative mood ("add feature" not "added feature")
- **Stage strategically** — Use `git add -p` to stage parts of a file
- **Never force-push to shared branches** — `git push --force` can break teammates' work
- **Use `git revert` for shared branches** — Create an undo commit instead of rewriting history
- **Use `git reset` for local branches** — Safe to rewrite history on branches only you use

## 🔗 Resources

- Pro Git book (Chapter 2): https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository
- Git docs: https://git-scm.com/docs
- Learn Git Branching: https://learngitbranching.js.org/
- GitHub's Git Handbook: https://guides.github.com/introduction/git-handbook/

## ✅ Completion Checklist

- [ ] Completed Exercise 01 (init and commit)
- [ ] Completed Exercise 02 (explore history)
- [ ] Completed Exercise 03 (branch and merge)
- [ ] Completed Exercise 04 (resolve conflicts)
- [ ] Completed Exercise 05 (undo and recover)
- [ ] Opened a PR with your exercise code

**Next**: Move to [Module 02 — GitHub CLI](./MODULE-02-GITHUB-CLI.md) when ready!
