# Module-by-Module Workflow Guide

This guide explains **how to work through each module**, step-by-step, using Git, GitHub, and the `gh` CLI.

---

## Overview of the Workflow

For **each module**, you will:

1. **Create an issue** to track the module (e.g., "Module 01: Git Basics")
2. **Create a feature branch** for the module's exercises
3. **Complete exercises** by committing your work
4. **Open a Pull Request (PR)** for code review
5. **Merge the PR** when ready
6. **Close the issue** and update progress

This workflow trains you to use GitHub professionally and helps you practice the `gh` CLI.

---

## Step-by-Step Workflow

### Phase 1: Preparation

#### 1a. Clone the learning repo (first time only)

```bash
git clone https://github.com/rmansoura/github-learning-path.git
cd github-learning-path
```

#### 1b. Verify your `gh` CLI is authenticated

```bash
gh auth status
```

You should see:
```
github.com
  ✓ Logged in to github.com account rmansoura (keyring)
```

If not logged in:
```bash
gh auth login
```

---

### Phase 2: Start a Module

Let's walk through **Module 01** as an example.

#### 2a. Create a GitHub Issue to track the module

This issue will serve as your **task tracker** for the module.

```bash
gh issue create \
  --title "Module 01: Git Basics — Complete Exercises" \
  --body "Complete all hands-on exercises in MODULE-01-GIT-BASICS.md

## Checklist
- [ ] Exercise 1: Initialize and commit
- [ ] Exercise 2: Explore history
- [ ] Exercise 3: Branch and merge
- [ ] Exercise 4: Resolve a conflict
- [ ] Exercise 5: Recover from mistakes

See MODULE-01-GIT-BASICS.md for details.

**Assignee:** rmansoura" \
  --label "module" \
  --label "git-basics"
```

**Output:**
```
Creating issue in rmansoura/github-learning-path
✓ Created issue #1
```

Keep note of the issue number (e.g., `#1`).

#### 2b. Create a feature branch for this module

Branch naming convention: `module/{MODULE-NUMBER}/{description}`

```bash
git switch -c module/01/git-basics
```

Verify you're on the new branch:
```bash
git branch -v
```

Output:
```
* module/01/git-basics    b13e56c Initial commit
  main                    b13e56c Initial commit
```

---

### Phase 3: Complete Exercises

#### 3a. Read the module

Open and read `MODULE-01-GIT-BASICS.md`:

```bash
cat MODULE-01-GIT-BASICS.md
```

or open it in your editor:
```bash
code MODULE-01-GIT-BASICS.md
```

#### 3b. Perform Exercise 1: Initialize and commit

From the module:
> Create a new directory `exercise-01` and run `git init`.
> Create a file `hello.md`, add content, commit with clear message.

**Steps:**

```bash
# Create exercise directory
mkdir -p exercises/module-01/exercise-01
cd exercises/module-01/exercise-01

# Initialize a git repo
git init

# Create a file with content
cat > hello.md << 'EOF'
# Hello, Git!

This is my first exercise in the GitHub Learning Path.

## What I learned
- How to initialize a repository
- How to stage and commit changes
- How to write clear commit messages
EOF

# Stage the file
git add hello.md

# Commit with a clear message
git commit -m "feat: create hello.md with initial content"

# View your commit
git log --oneline
```

Output:
```
abc1234 feat: create hello.md with initial content
```

#### 3c. Perform remaining exercises

Repeat **3b** for Exercises 2–5, creating commits for each one. Examples:

**Exercise 2: Explore history**
```bash
# Add another file
echo "More content" >> hello.md
git add hello.md
git commit -m "docs: add more content to hello.md"

# View history
git log --oneline --graph --decorate
```

**Exercise 3: Branch and merge**
```bash
# Create a feature branch
git switch -c feature/improvement

# Make a change
echo "Improved version" >> hello.md
git add hello.md
git commit -m "feat: improve hello.md on feature branch"

# Switch back to main
git switch main

# Merge the feature
git merge feature/improvement

# View the merge in log
git log --oneline --graph --decorate --all
```

#### 3d. Commit your exercise work to the learning path repo

After completing exercises in the local `exercises/` directory, commit your work **in the learning path repo itself**.

```bash
# Go back to the learning path root
cd /path/to/github-learning-path

# Stage your exercise files
git add exercises/module-01/

# Commit with a clear message
git commit -m "exercise: complete module 01 exercises 1-5

- Ex 1: Initialize repo and commit
- Ex 2: Explore git log with branching
- Ex 3: Create, merge branches
- Ex 4: Resolve merge conflicts
- Ex 5: Practice git reset and revert"
```

---

### Phase 4: Push Your Branch and Open a PR

#### 4a. Push your branch to GitHub

```bash
git push --set-upstream origin module/01/git-basics
```

Output:
```
Enumerating objects: 25, done.
Counting objects: 100% (25/25), done.
...
* [new branch]      module/01/git-basics -> module/01/git-basics
Branch 'module/01/git-basics' set up to track 'origin/module/01/git-basics'.
```

#### 4b. Open a Pull Request using `gh pr create`

```bash
gh pr create \
  --title "Complete Module 01: Git Basics" \
  --body "## Summary
Completed all 5 hands-on exercises for Module 01.

### What I did
- Initialized a repository and made commits
- Explored git log and history
- Created branches and merged them
- Resolved merge conflicts
- Practiced undoing commits with git reset/revert

### Exercises completed
✅ Exercise 1: Initialize and commit
✅ Exercise 2: Explore history
✅ Exercise 3: Branch and merge
✅ Exercise 4: Resolve conflicts
✅ Exercise 5: Recover from mistakes

### Issue
Closes #1 (or your issue number)

### Testing
All exercises tested locally. Git workflows verified." \
  --reviewer rmansoura \
  --label "module" \
  --label "review" \
  --draft
```

**Output:**
```
Creating pull request for rmansoura:module/01/git-basics into master

✓ Created pull request #2
  https://github.com/rmansoura/github-learning-path/pull/2
```

#### 4c. Review your own PR (or have others review it)

Go to GitHub and review your PR:
```bash
gh pr view 2 --web
```

This opens the PR in your browser. Review:
- Commits are clean and well-written
- Exercises are completed
- No merge conflicts

#### 4d. Request reviews from peers (optional)

```bash
gh pr review 2 --comment --body "Please review my Module 01 exercises."
```

Or add more reviewers:
```bash
gh pr edit 2 --add-reviewer other-github-user
```

---

### Phase 5: Merge the PR and Close the Issue

#### 5a. Merge the PR

Once approved (or self-approved), merge:

```bash
# Using gh CLI (recommended)
gh pr merge 2 --squash --delete-branch
```

Or with more control:
```bash
gh pr merge 2 \
  --squash \
  --subject "Complete Module 01: Git Basics" \
  --delete-branch
```

**Output:**
```
✓ Squashed and merged pull request #2 (module/01/git-basics)
✓ Deleted branch module/01/git-basics
```

#### 5b. Close the issue

```bash
gh issue close 1 --comment "✅ Module 01 completed! All exercises done."
```

**Output:**
```
✓ Closed issue #1
```

#### 5c. Update progress tracking

Update `progress.md` to reflect completion:

```bash
# Edit progress.md
code progress.md
```

Change:
```markdown
- [ ] Module 01 — Git Basics
```

To:
```markdown
- [x] Module 01 — Git Basics ✅ (Completed exercises, opened PR #2, merged)
```

Commit and push:
```bash
git add progress.md
git commit -m "docs: mark module 01 as complete"
git push origin main
```

---

## Quick Reference: Commands for Each Phase

### Setup
```bash
git clone https://github.com/rmansoura/github-learning-path.git
cd github-learning-path
gh auth status
```

### Start Module (use for each module)
```bash
# 1. Create issue
gh issue create --title "Module 01: Git Basics — Complete Exercises" \
  --body "See MODULE-01-GIT-BASICS.md for exercises." --label "module"

# 2. Create branch
git switch -c module/01/git-basics

# 3. Work on exercises
# ... (complete exercises, make commits)
git add exercises/module-01/
git commit -m "exercise: complete module 01 exercises"
```

### Submit Work
```bash
# 1. Push branch
git push --set-upstream origin module/01/git-basics

# 2. Create PR
gh pr create --title "Complete Module 01" --body "..." --reviewer rmansoura --draft

# 3. Review and merge
gh pr merge 2 --squash --delete-branch

# 4. Close issue
gh issue close 1 --comment "✅ Complete"

# 5. Update progress
code progress.md  # mark module as done
git add progress.md && git commit -m "docs: mark module 01 done" && git push
```

---

## Tips & Best Practices

### Commit Messages
Write clear, descriptive commit messages following **Conventional Commits**:
- `feat: add new feature`
- `fix: fix bug in X`
- `docs: update README`
- `exercise: complete module 01 exercises`

### Branch Naming
Use consistent naming:
- `module/{NUMBER}/{description}` for module work
- `exercise/{MODULE}/{EXERCISE}` for individual exercises
- `fix/{issue-number}/{description}` for fixes

### PR Reviews
Even if working alone, use the PR workflow to:
- Practice code review and self-review
- Document your thought process
- Build muscle memory for professional workflows

### Tracking Progress
- Use GitHub Issues for high-level tracking
- Use `progress.md` for a human-readable checklist
- Use GitHub Projects (v2) for a visual board

---

## Example: Full Module 01 Workflow (Copy-Paste)

Here's a complete example you can copy and paste:

```bash
#!/bin/bash

# 1. Clone repo (first time only)
# git clone https://github.com/rmansoura/github-learning-path.git
# cd github-learning-path

# 2. Create issue
ISSUE=$(gh issue create --title "Module 01: Git Basics" --body "Complete Module 01 exercises" --label "module" --json number --query '.number')
echo "Created issue #$ISSUE"

# 3. Create branch
git switch -c module/01/git-basics

# 4. Do exercises
mkdir -p exercises/module-01
cd exercises/module-01

# Exercise 1
mkdir exercise-01 && cd exercise-01
git init
cat > hello.md << 'EOF'
# Hello, Git!
Learning git basics.
EOF
git add hello.md
git commit -m "feat: initialize repo with hello.md"
cd ..

# 5. Commit to learning path
cd /path/to/github-learning-path
git add exercises/module-01/
git commit -m "exercise: complete module 01 exercises"

# 6. Push and create PR
git push --set-upstream origin module/01/git-basics
gh pr create --title "Complete Module 01" --body "Module 01 exercises done" --reviewer rmansoura --draft

# 7. Merge and close
sleep 5  # give time to review
PR=$(gh pr list --state open --search "Complete Module 01" --json number --query '.[0].number')
gh pr merge $PR --squash --delete-branch
gh issue close $ISSUE --comment "✅ Module 01 complete"

echo "✅ Module 01 workflow complete!"
```

Save this as `workflow-module-01.sh` and run:
```bash
chmod +x workflow-module-01.sh
./workflow-module-01.sh
```

---

## Troubleshooting

**Q: How do I switch back to main and start a new module?**
```bash
git switch main
git pull origin main
git switch -c module/02/github-cli
```

**Q: I made a mistake in my branch. How do I undo?**
```bash
# Reset to a previous commit (keep changes)
git reset --soft HEAD~1

# Or reset and discard changes
git reset --hard HEAD~1
```

**Q: How do I update my branch if main changed?**
```bash
git fetch origin
git rebase origin/main
git push --force-with-lease origin module/01/git-basics
```

**Q: Can I delete a branch after merging?**
Yes, `gh pr merge --delete-branch` does this automatically. Or manually:
```bash
git branch -d module/01/git-basics
```

---

## Advanced: Automate Issue & PR Creation

Want to automate the start of each module? Create a script:

```bash
#!/bin/bash

MODULE=$1
TITLE=$2
DESCRIPTION=$3

if [ -z "$MODULE" ]; then
  echo "Usage: ./start-module.sh <number> <title> <description>"
  exit 1
fi

# Create issue
ISSUE=$(gh issue create --title "Module $MODULE: $TITLE" --body "$DESCRIPTION" --label "module" --json number --query '.number')
echo "✅ Created issue #$ISSUE"

# Create branch
git switch -c module/$MODULE/$(echo $TITLE | tr ' ' '-' | tr '[:upper:]' '[:lower:]')
echo "✅ Created branch"

echo "Ready to work on Module $MODULE!"
```

Save as `start-module.sh` and use:
```bash
chmod +x start-module.sh
./start-module.sh 01 "Git Basics" "Complete git basics exercises"
```

---

**Ready to start? Begin with Module 01 and work at your own pace. Ask questions anytime!** 🚀
