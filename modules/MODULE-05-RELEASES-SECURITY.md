# Module 05 — Releases, Signing & Security 🔐

Publish releases professionally, sign commits for authenticity, and implement security best practices.

## 🎯 Learning Objectives

After this module, you'll be able to:
- Create and manage releases with semantic versioning
- Sign commits and tags with GPG for verification
- Protect branches with enforce rules
- Use Dependabot for automated security updates
- Scan code and dependencies for vulnerabilities
- Implement security policies in GitHub

## 📖 Concepts

### Semantic Versioning (SemVer)

Version format: `MAJOR.MINOR.PATCH` (e.g., `1.2.3`)

- **MAJOR** — Incompatible API changes
- **MINOR** — Backward-compatible new features
- **PATCH** — Backward-compatible bug fixes

**Examples**:
- `1.0.0` → First release
- `1.1.0` → Added feature (backward-compatible)
- `1.1.1` → Bug fix
- `2.0.0` → Breaking change (new major version)

### Signing Commits

GPG signing proves you made a commit:
- Creates a digital signature with your private key
- Others verify with your public key
- Prevents impersonation

### Release Best Practices

1. **Tag before releasing** — `git tag v1.0.0`
2. **Write release notes** — Explain changes, breaking changes, migration steps
3. **Include assets** — Binaries, archives, documentation
4. **Follow SemVer** — Clear versioning scheme
5. **Announce releases** — Email, Slack, Twitter

## 🏃 Quick Commands

| Command | Purpose |
|---------|---------|
| `git tag -a v1.0.0 -m "Release"` | Create an annotated tag |
| `git show v1.0.0` | Show tag details |
| `gh release create v1.0.0` | Create a release |
| `gh release upload v1.0.0 file.zip` | Upload asset |
| `git config commit.gpgsign true` | Auto-sign commits |
| `gpg --list-keys` | List GPG keys |

## 💪 Hands-On Exercises

### Exercise 01: Create Your First Release

**Objective**: Publish a release with release notes and assets.

**Steps**:

1. Ensure you have a clean main branch:
   ```bash
   git checkout main
   git pull origin main
   ```

2. Create an annotated tag:
   ```bash
   git tag -a v1.0.0 -m "Initial release with core features"
   ```

3. Push the tag:
   ```bash
   git push origin v1.0.0
   ```

4. Create a release via `gh`:
   ```bash
   gh release create v1.0.0 \
     --title "v1.0.0 - Learning Path Scaffold" \
     --notes "Initial release with 5 comprehensive modules and exercises"
   ```

5. Or with draft first:
   ```bash
   gh release create v1.0.0 \
     --draft \
     --title "v1.0.0" \
     --notes "- Module 01: Git Basics\n- Module 02: GitHub CLI\n- Module 03: Branching\n- Module 04: CI/CD\n- Module 05: Security"
   ```

6. View the release:
   ```bash
   gh release view v1.0.0
   gh release view v1.0.0 --web  # Open in browser
   ```

---

### Exercise 02: Upload Assets to Release

**Objective**: Create and upload release artifacts (binaries, archives).

**Steps**:

1. Create sample artifacts:
   ```bash
   # Create a sample artifact
   tar -czf learning-path-v1.0.0.tar.gz modules/ exercises/ README.md
   echo "v1.0.0 Release Notes" > RELEASE-v1.0.0.txt
   ```

2. Upload to release:
   ```bash
   gh release upload v1.0.0 learning-path-v1.0.0.tar.gz
   gh release upload v1.0.0 RELEASE-v1.0.0.txt
   ```

3. Verify assets:
   ```bash
   gh release view v1.0.0
   ```

4. Or upload on creation:
   ```bash
   gh release create v1.1.0 \
     --title "v1.1.0" \
     --notes "Add Exercise solutions" \
     learning-path-v1.1.0.tar.gz
   ```

---

### Exercise 03: Setup GPG Signing (Optional but Recommended)

**Objective**: Configure GPG to sign commits and tags.

**Steps**:

1. Check if you have GPG:
   ```bash
   gpg --version
   ```

2. If not installed (Ubuntu/Debian):
   ```bash
   sudo apt-get install gnupg
   ```

3. Generate a key (skip if you have one):
   ```bash
   gpg --full-generate-key
   # Follow prompts: RSA (default), 4096 bits, expires never, name/email
   ```

4. List your keys:
   ```bash
   gpg --list-secret-keys --keyid-format=long
   # Output: sec   rsa4096/ABC123DEF456 2025-01-15
   ```

5. Configure Git to use your key:
   ```bash
   git config --global user.signingkey ABC123DEF456
   git config --global commit.gpgsign true
   ```

6. Now all commits are automatically signed:
   ```bash
   echo "Signed commit" > test.txt
   git add test.txt
   git commit -m "test: verify signed commit"
   git log --oneline -1
   # Notice: commit hash (you can verify it's signed)
   ```

7. Show a signed commit:
   ```bash
   git show --show-signature HEAD
   ```

8. Sign a tag:
   ```bash
   git tag -s v1.2.0 -m "Signed release"
   git show v1.2.0
   ```

---

### Exercise 04: Branch Protection Rules

**Objective**: Enforce code quality with branch protection.

**Steps**:

1. Create a basic protection rule (web UI):
   - Go to Settings → Branches → Add rule
   - Pattern: `main`
   - ✓ Require a pull request before merging
   - ✓ Dismiss stale PR approvals
   - ✓ Require status checks to pass
   - ✓ Require branches to be up to date before merging

2. Or via `gh api`:
   ```bash
   gh api repos/rmansoura/github-learning-path/branches/main/protection \
     --method PUT \
     -f required_pull_request_reviews='{"required_approving_review_count": 1}' \
     -f require_code_owner_reviews=true \
     -f dismiss_stale_reviews=true \
     -f required_status_checks='{"strict": true, "contexts": ["CI"]}' \
     -f restrict_who_can_push='[{"type": "App", "id": 1}]'
   ```

3. Test the protection:
   - Try to push directly to main:
     ```bash
     echo "test" > test.txt
     git add test.txt
     git commit -m "test: direct push (should fail)"
     git push origin main  # Should be rejected!
     ```
   - Must create a PR instead

---

### Exercise 05: Enable Dependabot

**Objective**: Auto-update dependencies with security fixes.

**Steps**:

1. Create `.github/dependabot.yml`:
   ```yaml
   version: 2
   updates:
     # npm dependencies
     - package-ecosystem: "npm"
       directory: "/"
       schedule:
         interval: "weekly"
       open-pull-requests-limit: 5

     # Python dependencies
     - package-ecosystem: "pip"
       directory: "/"
       schedule:
         interval: "weekly"

     # GitHub Actions
     - package-ecosystem: "github-actions"
       directory: "/"
       schedule:
         interval: "weekly"
   ```

2. Commit and push:
   ```bash
   mkdir -p .github
   cat > .github/dependabot.yml << 'EOF'
   version: 2
   updates:
     - package-ecosystem: "github-actions"
       directory: "/"
       schedule:
         interval: "weekly"
   EOF
   git add .github/dependabot.yml
   git commit -m "chore: enable dependabot"
   git push origin main
   ```

3. Monitor Dependabot PRs:
   ```bash
   gh pr list --search "dependabot"
   ```

4. Dependabot will automatically create PRs for:
   - Security updates (high priority)
   - Version updates (weekly)
   - GitHub Actions updates

---

### Exercise 06: Code Scanning (CodeQL)

**Objective**: Automatically scan code for vulnerabilities.

**Create `.github/workflows/codeql.yml`**:

```yaml
name: CodeQL

on: [push, pull_request]

jobs:
  analyze:
    runs-on: ubuntu-latest
    permissions:
      security-events: write
    steps:
      - uses: actions/checkout@v3
      - name: Initialize CodeQL
        uses: github/codeql-action/init@v2
        with:
          languages: 'javascript'
      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v2
```

**Steps**:
1. Create the workflow:
   ```bash
   cat > .github/workflows/codeql.yml << 'EOF'
   name: CodeQL
   on: [push]
   jobs:
     analyze:
       runs-on: ubuntu-latest
       permissions:
         security-events: write
       steps:
         - uses: actions/checkout@v3
         - name: Initialize CodeQL
           uses: github/codeql-action/init@v2
           with:
             languages: 'javascript'
         - name: Perform CodeQL Analysis
           uses: github/codeql-action/analyze@v2
   EOF
   ```

2. Commit and push:
   ```bash
   git add .github/workflows/codeql.yml
   git commit -m "security: add CodeQL scanning"
   git push origin main
   ```

3. Monitor results:
   - Go to Security → Code scanning alerts
   - Fix any vulnerabilities

---

### Exercise 07: Release Planning with Milestones

**Objective**: Plan releases using GitHub Milestones.

**Steps**:

1. Create a milestone:
   ```bash
   gh milestone create --title "v2.0.0" --description "Add advanced modules"
   ```

2. Link issues to the milestone:
   ```bash
   gh issue create -t "Add advanced Git internals module" -m "v2.0.0"
   gh issue create -t "Add monorepo support module" -m "v2.0.0"
   ```

3. View milestone progress:
   ```bash
   gh milestone list
   gh milestone view "v2.0.0"
   ```

4. When releasing v2.0.0, close the milestone:
   ```bash
   gh milestone close "v2.0.0"
   ```

---

## 🔑 Pro Tips

1. **Use semantic versioning** — Makes it clear what changed
2. **Write detailed release notes** — Include breaking changes, migration steps
3. **Sign important releases** — Proves authenticity
4. **Use Dependabot** — Keeps dependencies secure and up-to-date
5. **Automate releases** — Tag → workflow → release
6. **Test releases** — Don't release without testing
7. **Archive old releases** — Keep them available for users on old versions

## 📚 Key Takeaways

- **Releases are milestones** — Mark stable points in your project
- **Semantic versioning is standard** — MAJOR.MINOR.PATCH
- **GPG signing adds trust** — Prove commits are authentic
- **Branch protection prevents accidents** — Enforce quality
- **Dependabot keeps you secure** — Auto-update dependencies
- **Scanning catches vulnerabilities** — CodeQL, SAST, DAST

## 🔗 Resources

- Semantic versioning: https://semver.org/
- GitHub Releases: https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases
- GPG signing: https://docs.github.com/en/authentication/managing-commit-signature-verification
- Dependabot docs: https://docs.github.com/en/code-security/dependabot
- CodeQL: https://codeql.github.com/

## ✅ Completion Checklist

- [ ] Completed Exercise 01 (create release)
- [ ] Completed Exercise 02 (upload assets)
- [ ] Completed Exercise 03 (GPG signing - optional)
- [ ] Completed Exercise 04 (branch protection)
- [ ] Completed Exercise 05 (Dependabot)
- [ ] Completed Exercise 06 (CodeQL scanning)
- [ ] Completed Exercise 07 (release planning)

**Congratulations!** You've completed all 5 modules! 🎉

---

## 🚀 What's Next?

- **Keep practicing** — Work on real projects using these skills
- **Advanced topics** — Explore Git internals, monorepos, large-scale workflows
- **Community** — Contribute to open-source projects
- **Mentoring** — Teach these concepts to teammates
- **Automation** — Build custom workflows and actions
