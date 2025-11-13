# Module 04 — GitHub Actions & CI/CD 🚀

Automate testing, building, and deployment using GitHub Actions workflows.

## 🎯 Learning Objectives

After this module, you'll be able to:
- Understand the anatomy of a GitHub Actions workflow
- Write and trigger workflows based on events
- Use matrix builds for testing across environments
- Cache dependencies for faster builds
- Manage secrets and environment variables
- Create reusable workflows and composite actions
- Integrate third-party actions

## 📖 Concepts

### Workflow Structure

A workflow is a YAML file in `.github/workflows/` that defines:
- **Triggers** — When to run (push, PR, schedule, manual)
- **Jobs** — What to do (build, test, deploy)
- **Steps** — Individual commands/actions
- **Runners** — Where to run (ubuntu-latest, windows, macos)

### Key Components

```yaml
name: CI                              # Workflow name
on: [push, pull_request]             # Triggers
jobs:
  build:                             # Job name
    runs-on: ubuntu-latest           # Runner
    steps:
      - uses: actions/checkout@v3    # Action
        with:
          ref: main
      - run: npm test                # Shell command
        env:
          NODE_ENV: test
```

### Common Runners

- `ubuntu-latest` — Default, Linux
- `windows-latest` — Windows
- `macos-latest` — macOS
- Self-hosted runners — Your own servers

## 🏃 Quick Commands

| Command | Purpose |
|---------|---------|
| `gh workflow list` | List workflows |
| `gh workflow run <id>` | Manually trigger workflow |
| `gh run list` | List recent runs |
| `gh run view <id>` | View run details |
| `gh run log <id>` | View run logs |

## 💪 Hands-On Exercises

### Exercise 01: Your First Workflow

**Objective**: Create a simple CI workflow that runs on every push.

**Create `.github/workflows/ci.yml`**:

```yaml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Check files
        run: |
          echo "Files in repo:"
          ls -la
      - name: Check git history
        run: git log --oneline | head -5
```

**Steps**:
1. Create the workflow file:
   ```bash
   mkdir -p .github/workflows
   cat > .github/workflows/ci.yml << 'EOF'
   name: CI
   on: [push, pull_request]
   jobs:
     test:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - name: Check files
           run: ls -la
         - name: Check git history
           run: git log --oneline | head -5
   EOF
   ```

2. Commit and push:
   ```bash
   git add .github/workflows/ci.yml
   git commit -m "ci: add basic workflow"
   git push origin main
   ```

3. Watch it run:
   ```bash
   gh run list
   gh run view <run-id>
   ```

---

### Exercise 02: Matrix Builds (Test Multiple Versions)

**Objective**: Test against multiple Node.js versions simultaneously.

**Create `.github/workflows/matrix.yml`**:

```yaml
name: Matrix Build

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [14.x, 16.x, 18.x]
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - run: node --version
      - run: npm install
      - run: npm test
```

**Steps**:
1. Create the matrix workflow:
   ```bash
   cat > .github/workflows/matrix.yml << 'EOF'
   name: Matrix Build
   on: [push]
   jobs:
     test:
       runs-on: ubuntu-latest
       strategy:
         matrix:
           node-version: [14.x, 16.x, 18.x]
       steps:
         - uses: actions/checkout@v3
         - name: Setup Node ${{ matrix.node-version }}
           uses: actions/setup-node@v3
           with:
             node-version: ${{ matrix.node-version }}
         - run: node --version
   EOF
   ```

2. Commit and push:
   ```bash
   git add .github/workflows/matrix.yml
   git commit -m "ci: add matrix build workflow"
   git push origin main
   ```

3. Observe: 3 jobs run in parallel, one per Node version!

---

### Exercise 03: Caching for Performance

**Objective**: Speed up builds by caching dependencies.

**Create `.github/workflows/cached-build.yml`**:

```yaml
name: Build with Cache

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - run: npm test
```

**Steps**:
1. Create the workflow:
   ```bash
   cat > .github/workflows/cached-build.yml << 'EOF'
   name: Build with Cache
   on: [push, pull_request]
   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - name: Setup Node
           uses: actions/setup-node@v3
           with:
             node-version: '18'
             cache: 'npm'
         - run: npm ci
         - run: npm run build
   EOF
   ```

2. Commit and push:
   ```bash
   git add .github/workflows/cached-build.yml
   git commit -m "ci: add caching to build workflow"
   git push origin main
   ```

3. First run: caches npm dependencies
4. Second run: uses cache, much faster!

---

### Exercise 04: Using Secrets

**Objective**: Store and use sensitive data securely.

**Steps**:

1. Add a secret via `gh`:
   ```bash
   gh secret set MY_SECRET --body "super-secret-value"
   ```

2. Or via web UI:
   - Go to Settings → Secrets and variables → Actions → New repository secret

3. Create a workflow that uses the secret:
   ```yaml
   name: Secret Usage
   on: [push]
   jobs:
     use-secret:
       runs-on: ubuntu-latest
       steps:
         - name: Use secret (safe, won't print)
           run: |
             echo "Secret length: ${#MY_SECRET}"
           env:
             MY_SECRET: ${{ secrets.MY_SECRET }}
   ```

4. Create the workflow file:
   ```bash
   cat > .github/workflows/secrets.yml << 'EOF'
   name: Secret Usage
   on: [push]
   jobs:
     use-secret:
       runs-on: ubuntu-latest
       steps:
         - run: echo "Secret is set" > /dev/null
           env:
             MY_SECRET: ${{ secrets.MY_SECRET }}
   EOF
   ```

5. Commit and push:
   ```bash
   git add .github/workflows/secrets.yml
   git commit -m "ci: add workflow using secrets"
   git push origin main
   ```

6. Verify: The secret value won't appear in logs!

---

### Exercise 05: PR Checks (Block Merge Until Tests Pass)

**Objective**: Require tests to pass before merging PRs.

**Create `.github/workflows/pr-checks.yml`**:

```yaml
name: PR Checks

on: [pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Lint code
        run: |
          echo "Running linter..."
          # Example: shellcheck *.sh || true

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run tests
        run: |
          echo "Running tests..."
          # Example: npm test
```

**Steps**:
1. Create the workflow:
   ```bash
   cat > .github/workflows/pr-checks.yml << 'EOF'
   name: PR Checks
   on: [pull_request]
   jobs:
     lint:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - run: echo "Running linter..."
     test:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - run: echo "Running tests..."
   EOF
   ```

2. Commit and push:
   ```bash
   git add .github/workflows/pr-checks.yml
   git commit -m "ci: add PR checks workflow"
   git push origin main
   ```

3. Create a test PR:
   ```bash
   git checkout -b test/pr-checks
   echo "test" >> test.txt
   git add test.txt
   git commit -m "test: add test file"
   git push -u origin test/pr-checks
   gh pr create --fill
   ```

4. Notice: PR shows check status, can require checks to pass!

---

### Exercise 06: Release Workflow (Auto-Release on Tag)

**Objective**: Create a workflow that publishes a release when you push a tag.

**Create `.github/workflows/release.yml`**:

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Extract version
        id: version
        run: echo "VERSION=${GITHUB_REF#refs/tags/}" >> $GITHUB_OUTPUT
      - name: Create release
        run: |
          gh release create ${{ steps.version.outputs.VERSION }} \
            --title "Release ${{ steps.version.outputs.VERSION }}" \
            --notes "Auto-released"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

**Steps**:
1. Create the workflow:
   ```bash
   cat > .github/workflows/release.yml << 'EOF'
   name: Release
   on:
     push:
       tags:
         - 'v*'
   jobs:
     release:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - name: Extract version
           id: version
           run: echo "VERSION=${GITHUB_REF#refs/tags/}" >> $GITHUB_OUTPUT
         - name: Create release
           run: gh release create ${{ steps.version.outputs.VERSION }} --title "Release ${{ steps.version.outputs.VERSION }}"
           env:
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
   EOF
   ```

2. Commit and push:
   ```bash
   git add .github/workflows/release.yml
   git commit -m "ci: add release workflow"
   git push origin main
   ```

3. Test by pushing a tag:
   ```bash
   git tag -a v0.2.0 -m "Release v0.2.0"
   git push origin v0.2.0
   ```

4. Watch the workflow run and create a release!

---

## 🔑 Pro Tips

1. **Use `actions/checkout@v3`** — Always check out code first
2. **Use `run: set -e`** — Stop on first error
3. **Cache aggressively** — Faster builds = faster feedback
4. **Parallelize jobs** — Multiple jobs run simultaneously
5. **Use conditional steps** — `if: always()`, `if: failure()`
6. **Set environment variables** — Reuse values across steps
7. **Limit runners** — Test only what changed

## 📚 Key Takeaways

- **Workflows automate repetitive tasks** — No manual testing needed
- **Matrix builds catch edge cases** — Test multiple versions
- **Caching is critical** — Save time and CI resources
- **Secrets stay secure** — Never logged or exposed
- **PR checks prevent bugs** — Failing tests block merges
- **Releases can be automatic** — Tag-triggered workflows

## 🔗 Resources

- GitHub Actions docs: https://docs.github.com/actions
- Starter workflows: https://github.com/actions/starter-workflows
- Actions marketplace: https://github.com/marketplace?type=actions
- YAML syntax: https://yaml.org/

## ✅ Completion Checklist

- [ ] Completed Exercise 01 (first workflow)
- [ ] Completed Exercise 02 (matrix builds)
- [ ] Completed Exercise 03 (caching)
- [ ] Completed Exercise 04 (using secrets)
- [ ] Completed Exercise 05 (PR checks)
- [ ] Completed Exercise 06 (release workflow)

**Next**: Move to [Module 05 — Releases & Security](./MODULE-05-RELEASES-SECURITY.md) when ready!
