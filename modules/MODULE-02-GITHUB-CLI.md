# Module 02 — GitHub CLI Professional Usage 🎯

Master the `gh` CLI tool for professional repository management, issues, pull requests, and automation.

## 🎯 Learning Objectives

After this module, you'll be able to:
- Authenticate with `gh` and manage credentials
- Create and clone repositories via CLI
- Manage issues and pull requests professionally
- Create releases and upload assets
- Use `gh api` for advanced operations
- Write shell scripts that automate GitHub workflows

## 📖 Concepts

### Why Use `gh` CLI?

1. **Speed** — No browser context switching
2. **Automation** — Scriptable workflows in CI/CD
3. **Power** — Access to advanced features not in web UI
4. **Professional** — Industry-standard tool for DevOps/engineers

### Authentication

`gh` supports multiple auth methods:
- **OAuth (recommended)** — Secure, browser-based
- **Token** — Personal access tokens (PAT) for scripts
- **SSH** — If you have SSH keys

## 🏃 Quick Commands

| Command | Purpose |
|---------|---------|
| `gh auth login` | Authenticate with GitHub |
| `gh repo create <name>` | Create a new repository |
| `gh repo clone <owner/repo>` | Clone a repository |
| `gh issue list` | List issues in current repo |
| `gh issue create -t "Title"` | Create an issue |
| `gh pr list` | List PRs |
| `gh pr create --fill` | Create a PR (interactive) |
| `gh release create v1.0.0` | Create a release |
| `gh api repos/{owner}/{repo}` | Make API calls |

## 💪 Hands-On Exercises

### Exercise 01: Authenticate and Verify

**Objective**: Set up `gh` and confirm authentication.

**Steps**:
1. Check if `gh` is installed:
   ```bash
   gh --version
   ```

2. Check authentication status:
   ```bash
   gh auth status
   ```

3. If not authenticated, log in:
   ```bash
   gh auth login
   # Follow the prompts:
   # - What account do you want to log into? → GitHub.com
   # - What is your preferred protocol? → HTTPS (or SSH)
   # - Authenticate? → Paste/authorize
   ```

4. Verify your account:
   ```bash
   gh auth status
   gh api user --jq '.login'  # Should show your username
   ```

---

### Exercise 02: Create a Repository via CLI

**Objective**: Use `gh repo create` instead of web UI.

**Steps**:
1. Create a new empty repo:
   ```bash
   gh repo create my-cli-test --public --source=. --remote=origin
   ```

2. Or initialize locally first, then create:
   ```bash
   mkdir my-cli-repo
   cd my-cli-repo
   git init
   echo "# Test Repo" > README.md
   git add .
   git commit -m "docs: initial README"
   gh repo create my-cli-repo --public --source=. --remote=origin --push
   ```

3. Verify the repo was created:
   ```bash
   gh repo view rmansoura/my-cli-repo
   ```

4. View it in your browser:
   ```bash
   gh repo view rmansoura/my-cli-repo --web
   ```

---

### Exercise 03: Create Issues via CLI

**Objective**: Manage issues without leaving the terminal.

**Steps**:
1. Clone or cd into a repo you own:
   ```bash
   cd my-cli-repo
   ```

2. Create a simple issue:
   ```bash
   gh issue create -t "Fix: bug in README" -b "The README has a typo."
   ```

3. Create an issue with labels and assignee:
   ```bash
   gh issue create \
     -t "feat: add API documentation" \
     -b "We need comprehensive API docs" \
     -l "documentation,enhancement" \
     --assignee rmansoura
   ```

4. List issues:
   ```bash
   gh issue list
   ```

5. View a specific issue:
   ```bash
   gh issue view 1  # Issue #1
   ```

6. Close an issue:
   ```bash
   gh issue close 1
   ```

---

### Exercise 04: PR Workflow via CLI

**Objective**: Master the complete PR workflow from the CLI.

**Steps**:
1. Create a feature branch:
   ```bash
   git checkout -b feature/add-docs
   ```

2. Make changes and commit:
   ```bash
   echo "## API Documentation" >> README.md
   git add README.md
   git commit -m "docs: add API section"
   ```

3. Push the branch:
   ```bash
   git push -u origin feature/add-docs
   ```

4. Create a PR with `gh`:
   ```bash
   gh pr create --title "Add API documentation" \
     --body "Adds comprehensive API docs to README" \
     --reviewer rmansoura \
     --draft
   ```

5. Or use interactive mode:
   ```bash
   gh pr create --fill
   # Opens editor for interactive PR creation
   ```

6. List PRs:
   ```bash
   gh pr list
   ```

7. View a PR:
   ```bash
   gh pr view 1
   ```

8. Checkout a PR locally (pull remote branch):
   ```bash
   gh pr checkout 1
   ```

9. Merge the PR:
   ```bash
   gh pr merge 1 --squash  # or --rebase, --merge
   ```

---

### Exercise 05: Create a Release via CLI

**Objective**: Publish releases and upload assets.

**Steps**:
1. Ensure you have commits:
   ```bash
   git log --oneline | head -5
   ```

2. Create a tag:
   ```bash
   git tag -a v0.1.0 -m "Initial release"
   git push origin v0.1.0
   ```

3. Create a draft release:
   ```bash
   gh release create v0.1.0 --draft --title "v0.1.0" --notes "Initial release"
   ```

4. Upload an asset to the release:
   ```bash
   # Create a dummy asset
   echo "Sample artifact" > artifact.txt
   
   # Upload it
   gh release upload v0.1.0 artifact.txt
   ```

5. Publish the release:
   ```bash
   gh release edit v0.1.0 --draft=false
   ```

6. List releases:
   ```bash
   gh release list
   ```

7. View a release:
   ```bash
   gh release view v0.1.0
   ```

---

### Exercise 06: Advanced — Using `gh api`

**Objective**: Use `gh api` for operations not yet in the CLI.

**Steps**:
1. Get your user info:
   ```bash
   gh api user
   ```

2. Get a specific repository:
   ```bash
   gh api repos/rmansoura/my-cli-repo
   ```

3. List repository contributors:
   ```bash
   gh api repos/rmansoura/my-cli-repo/contributors
   ```

4. Create a repository via API (alternative to `gh repo create`):
   ```bash
   gh api repos \
     -f name=api-created-repo \
     -f description="Created via gh api" \
     -f private=false
   ```

5. Update repository settings:
   ```bash
   gh api repos/rmansoura/my-cli-repo \
     --method PATCH \
     -f description="Updated description via gh api"
   ```

---

### Exercise 07: Scripting with `gh`

**Objective**: Write a shell script that automates GitHub tasks.

**Create a file `scripts/bulk-issue-creator.sh`**:

```bash
#!/bin/bash

# Script: Create multiple issues from a list

REPO="${1:-.}"  # Current repo if not specified
TITLE_PREFIX="${2:-task}"

ISSUES=(
  "Setup CI pipeline"
  "Add API documentation"
  "Implement tests for module A"
  "Review security practices"
  "Optimize database queries"
)

echo "📋 Creating issues in $REPO..."

for issue in "${ISSUES[@]}"; do
  echo "Creating: $issue"
  gh issue create \
    -R "$REPO" \
    -t "$TITLE_PREFIX: $issue" \
    -b "Issue created via automation script"
done

echo "✅ Done!"
```

**Usage**:
```bash
chmod +x scripts/bulk-issue-creator.sh
./scripts/bulk-issue-creator.sh rmansoura/my-cli-repo "task"
```

---

### Exercise 08: Create Useful `gh` Aliases

**Objective**: Shorthand commands for frequent operations.

**Create a file `scripts/setup-gh-aliases.sh`**:

```bash
#!/bin/bash

# Setup useful gh aliases

echo "Setting up gh aliases..."

# Quick PR operations
gh alias set prl "pr list -L 10"          # List recent PRs
gh alias set prc "pr create --fill"       # Create PR interactively
gh alias set prm "pr merge -s"            # Merge PR with squash

# Quick issue operations
gh alias set il "issue list -L 10"        # List recent issues
gh alias set ic "issue create --fill"     # Create issue interactively

# View repo info
gh alias set repo-info "repo view --json url,description,owner,visibility"

# List collaborators
gh alias set collabs "repo view --json collaborators"

echo "✅ Aliases created! Try: gh prl, gh prc, gh il, etc."
```

**Usage**:
```bash
chmod +x scripts/setup-gh-aliases.sh
./scripts/setup-gh-aliases.sh

# Now use aliases:
gh prl        # Same as: gh pr list -L 10
gh prc        # Same as: gh pr create --fill
gh il         # Same as: gh issue list -L 10
```

---

## 🔑 Pro Tips

1. **Use `--help`** — All `gh` commands support `--help`:
   ```bash
   gh pr create --help
   gh issue create --help
   ```

2. **JSON output** — Parse output with jq:
   ```bash
   gh pr list --json title,number | jq '.[] | .number'
   ```

3. **Multiple accounts** — Use `--repo` or `-R` to switch repos:
   ```bash
   gh issue list -R owner/repo
   ```

4. **Batch operations** — Loop through issues/PRs:
   ```bash
   for pr in $(gh pr list --json number -q '.[] | .number'); do
     gh pr merge $pr --squash
   done
   ```

5. **Automation** — Use in GitHub Actions and cronjobs:
   ```bash
   # In CI/CD, set: GITHUB_TOKEN secret
   gh issue create -t "Nightly build report" -b "Automated report"
   ```

## 📚 Key Takeaways

- **`gh` is faster than the web UI** for repetitive tasks
- **Automate with scripts** — Build your workflow into shell scripts
- **Use `gh api`** when you need low-level control
- **Set up aliases** — Speed up your most common operations
- **Combine with jq** — Parse and filter JSON output
- **Use in CI/CD** — `GITHUB_TOKEN` is automatically available

## 🔗 Resources

- Official gh documentation: https://cli.github.com/manual
- GitHub CLI repository: https://github.com/cli/cli
- `gh` examples: https://github.com/cli/cli/tree/trunk/examples

## ✅ Completion Checklist

- [ ] Completed Exercise 01 (auth and verify)
- [ ] Completed Exercise 02 (create repo via CLI)
- [ ] Completed Exercise 03 (create issues)
- [ ] Completed Exercise 04 (PR workflow)
- [ ] Completed Exercise 05 (create release)
- [ ] Completed Exercise 06 (gh api calls)
- [ ] Completed Exercise 07 (scripting)
- [ ] Completed Exercise 08 (gh aliases)

**Next**: Move to [Module 03 — Branching & PRs](./MODULE-03-BRANCHING-PRS.md) when ready!
