# Module 03 — Branching Strategies & Pull Requests 🌳

Master professional branching workflows and pull request best practices for collaborative development.

## 🎯 Learning Objectives

After this module, you'll be able to:
- Choose and implement branching strategies (GitHub Flow, Git Flow, Trunk-Based)
- Write effective PR descriptions and commit messages
- Review code and provide constructive feedback
- Configure branch protection and code owners
- Handle merge conflicts and complex integrations
- Use `CODEOWNERS` for automated reviewer assignment

## 📖 Concepts

### Branching Strategies

#### GitHub Flow (Recommended for most teams)
- Single long-lived `main` branch (always deployment-ready)
- Feature branches off `main`: `feature/xyz`
- PR → review → merge → deploy
- Simple, fast, good for continuous deployment

**When to use**: SaaS apps, continuous deployment, agile teams

#### Git Flow
- Two long-lived branches: `main` (production) and `develop` (integration)
- Feature branches off `develop`
- Release branches for release prep
- Hotfix branches off `main`
- More complex but great for planned releases

**When to use**: Scheduled releases, multiple versions in production

#### Trunk-Based Development
- All work on `main` (or `trunk`)
- Short-lived feature branches (< 1 day)
- Multiple commits/day to main
- Continuous integration required

**When to use**: High-velocity teams, CI/CD-heavy projects

### Commit Message Conventions

Use **Conventional Commits**:
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

**Examples**:
```
feat(auth): add login endpoint

fix(api): handle null pointer in getUserName

docs(readme): update setup instructions

test(users): add unit tests for user service
```

### Pull Request Guidelines

A good PR:
- **Has a clear title** — Describes what changes
- **Has a detailed description** — Why, what, how
- **Is small** — Review-friendly (< 400 lines)
- **Has linked issues** — Closes #123
- **Passes tests** — Green CI checks
- **Has 1-2 reviewers** — Diverse perspectives

## 🏃 Quick Commands

| Command | Purpose |
|---------|---------|
| `git flow init` | Initialize Git Flow structure |
| `git flow feature start xyz` | Start a feature branch |
| `git flow feature finish xyz` | Finish a feature branch |
| `git push -u origin feature/xyz` | Push feature branch |
| `gh pr create --fill` | Create a PR interactively |
| `gh pr merge --squash` | Squash-merge a PR |
| `gh pr review --approve` | Approve a PR |

## 💪 Hands-On Exercises

### Exercise 01: GitHub Flow in Practice

**Objective**: Use GitHub Flow to add a feature.

**Steps**:

1. Ensure you're on `main`:
   ```bash
   git checkout main
   git pull origin main
   ```

2. Create a feature branch:
   ```bash
   git checkout -b feature/add-contributing-guide
   ```

3. Add a `CONTRIBUTING.md` file:
   ```markdown
   # Contributing to This Project

   Thank you for contributing! Here's how:

   1. Fork the repo
   2. Create a feature branch: `git checkout -b feature/xyz`
   3. Make changes and commit: `git commit -m "feat: xyz"`
   4. Push: `git push origin feature/xyz`
   5. Open a PR on GitHub
   6. Get reviewed and merge!

   ## Code of Conduct
   Be respectful and inclusive.
   ```

4. Commit the change:
   ```bash
   git add CONTRIBUTING.md
   git commit -m "docs: add contributing guide"
   ```

5. Push the branch:
   ```bash
   git push -u origin feature/add-contributing-guide
   ```

6. Create a PR:
   ```bash
   gh pr create --fill
   # Fill in title, body, and submit
   ```

7. Review your own PR (check for typos, formatting):
   ```bash
   gh pr view 1
   ```

8. Merge the PR:
   ```bash
   gh pr merge 1 --squash --delete-branch
   ```

9. Pull the updated `main`:
   ```bash
   git checkout main
   git pull origin main
   ```

---

### Exercise 02: Conventional Commits

**Objective**: Practice writing semantic commit messages.

**Steps**:

1. Create a new branch:
   ```bash
   git checkout -b feature/structured-commits
   ```

2. Add a feature with a `feat:` commit:
   ```bash
   echo "## Roadmap" >> README.md
   echo "- Complete all 5 modules" >> README.md
   git add README.md
   git commit -m "feat(docs): add roadmap section to README"
   ```

3. Fix a bug with a `fix:` commit:
   ```bash
   sed -i 's/Roadmap/Project Roadmap/' README.md
   git add README.md
   git commit -m "fix(docs): correct typo in roadmap title"
   ```

4. Update documentation with a `docs:` commit:
   ```bash
   echo "" >> README.md
   echo "## Authors" >> README.md
   echo "- Your Name" >> README.md
   git add README.md
   git commit -m "docs: add authors section"
   ```

5. View the clean history:
   ```bash
   git log --oneline
   ```

6. Create a PR and squash commits:
   ```bash
   git push -u origin feature/structured-commits
   gh pr create --title "Add structured commits practice"
   gh pr merge 1 --squash
   ```

---

### Exercise 03: Linking PRs to Issues

**Objective**: Connect your PR to an issue using GitHub syntax.

**Steps**:

1. Create an issue first:
   ```bash
   gh issue create -t "Add license file" -b "We need a LICENSE file"
   ```

2. Note the issue number (e.g., #5)

3. Create a branch and fix it:
   ```bash
   git checkout -b feature/add-license
   cp /path/to/LICENSE .  # Or create a LICENSE file
   git add LICENSE
   git commit -m "docs: add MIT license"
   ```

4. Create a PR that closes the issue:
   ```bash
   git push -u origin feature/add-license
   gh pr create --title "Add MIT license" \
     --body "Closes #5"  # or Fixes #5, Resolves #5
   ```

5. When merged, the issue closes automatically!

6. Verify:
   ```bash
   gh issue view 5  # Should show as closed
   ```

---

### Exercise 04: Code Owners and Reviewers

**Objective**: Set up `CODEOWNERS` for automatic reviewer assignment.

**Steps**:

1. Create a `.github/CODEOWNERS` file:
   ```bash
   mkdir -p .github
   cat > .github/CODEOWNERS << 'EOF'
   # Default reviewers for all files
   * @rmansoura

   # Module reviewers (if different)
   /modules/ @rmansoura
   /exercises/ @rmansoura
   /scripts/ @rmansoura

   # Documentation is critical
   /*.md @rmansoura
   EOF
   ```

2. Commit and push:
   ```bash
   git add .github/CODEOWNERS
   git commit -m "chore: add CODEOWNERS file"
   git push origin main
   ```

3. Now when you create a PR, reviewers are auto-assigned:
   ```bash
   git checkout -b feature/test-codeowners
   echo "Test" >> test.txt
   git add test.txt
   git commit -m "test: verify codeowners"
   git push -u origin feature/test-codeowners
   gh pr create --fill
   # Notice: rmansoura is automatically assigned as reviewer!
   ```

---

### Exercise 05: Merge Strategies

**Objective**: Understand and use different merge strategies.

**Steps**:

1. Create three separate branches:
   ```bash
   # Branch 1: Squash merge
   git checkout -b feature/squash-example
   echo "A" >> file1.txt
   git add file1.txt
   git commit -m "add file1 line1"
   echo "B" >> file1.txt
   git add file1.txt
   git commit -m "add file1 line2"
   git push -u origin feature/squash-example
   gh pr create --fill

   # After creating PR, merge with squash
   gh pr merge 1 --squash --delete-branch
   ```

2. Check the main history (1 squashed commit):
   ```bash
   git checkout main
   git pull origin main
   git log --oneline | head -5
   # Notice: 2 commits squashed into 1
   ```

3. Try a rebase merge:
   ```bash
   git checkout -b feature/rebase-example
   echo "C" >> file2.txt
   git add file2.txt
   git commit -m "add file2"
   git push -u origin feature/rebase-example
   gh pr create --fill
   gh pr merge 2 --rebase --delete-branch
   ```

4. Try a regular merge (creates merge commit):
   ```bash
   git checkout -b feature/merge-example
   echo "D" >> file3.txt
   git add file3.txt
   git commit -m "add file3"
   git push -u origin feature/merge-example
   gh pr create --fill
   gh pr merge 3 --merge --delete-branch
   ```

5. View the different commit patterns:
   ```bash
   git log --oneline --graph --all | head -20
   # Notice: squash=1 commit, rebase=1 commit (but rebased), merge=merge commit
   ```

**Choosing merge strategy**:
- **Squash**: Clean history, lose commit details (good for feature branches)
- **Rebase**: Linear history, preserves commits (good for major changes)
- **Merge**: Preserves full history, creates merge commit (good for releases)

---

### Exercise 06: Branch Protection Rules

**Objective**: Protect main branch with required checks.

**Steps**:

1. Go to your repository on GitHub (or use `gh api`):
   ```bash
   # View current branch protection (if any)
   gh api repos/rmansoura/github-learning-path/branches/main/protection
   ```

2. Set up protection via web UI:
   - Go to Settings → Branches → Add rule
   - Branch name pattern: `main`
   - Enable:
     - ✓ Require a pull request before merging
     - ✓ Require status checks to pass
     - ✓ Require branches to be up to date

3. Or set via `gh api`:
   ```bash
   gh api repos/rmansoura/github-learning-path/branches/main/protection \
     --method PUT \
     -f required_pull_request_reviews='{"required_approving_review_count": 1}' \
     -f required_status_checks='null' \
     -f dismiss_stale_reviews=true
   ```

4. Test the protection:
   - Try to push directly to main (should be blocked)
   - Must create a PR instead

---

## 🔑 Pro Tips

1. **Keep PRs small** — Easier to review, faster to merge
2. **Write descriptive PR titles** — Not "Update stuff", but "Add authentication module"
3. **Link issues in PR** — Use "Closes #123" to auto-close issues
4. **Request specific reviewers** — Not everyone needs to review everything
5. **Use draft PRs** — Mark as draft while still working, convert to ready when done
6. **Rebase before merge** — Keep history clean: `git rebase main` before pushing

## 📚 Key Takeaways

- **GitHub Flow** is simpler and recommended for most teams
- **Conventional Commits** make history readable and enable automation
- **Small PRs** review faster and merge safer
- **CODEOWNERS** automates reviewer assignment
- **Branch protection** prevents accidents
- **Merge strategies** matter for history readability

## 🔗 Resources

- GitHub Flow guide: https://guides.github.com/introduction/flow/
- Conventional Commits: https://www.conventionalcommits.org/
- Git Flow: https://nvie.com/posts/a-successful-git-branching-model/
- Branch protection: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches

## ✅ Completion Checklist

- [ ] Completed Exercise 01 (GitHub Flow)
- [ ] Completed Exercise 02 (Conventional Commits)
- [ ] Completed Exercise 03 (Linking PRs to Issues)
- [ ] Completed Exercise 04 (CODEOWNERS)
- [ ] Completed Exercise 05 (Merge Strategies)
- [ ] Completed Exercise 06 (Branch Protection)

**Next**: Move to [Module 04 — CI/CD](./MODULE-04-CI-CD.md) when ready!
