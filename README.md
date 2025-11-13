# GitHub Mastery — Complete Learning Path 🚀# GitHub Mastery — A Practical Learning Path



A comprehensive, hands-on learning path to master Git, GitHub, and professional development workflows. Work module-by-module with practical exercises, real-world examples, and the `gh` CLI.This is a modular, hands-on learning path to master Git and GitHub, with an emphasis on professional usage of the `gh` CLI and automation via GitHub Actions. Work module-by-module, complete exercises, open issues to track progress, and practice by pushing work to this repo.



## 🎯 What You'll LearnHow to use this repository



- **Git fundamentals**: Local workflows, branching, merging, history1. Clone this repo to your machine and work branch-by-branch per module.

- **GitHub CLI (`gh`)**: Professional repository and PR management2. Complete exercises in the `exercises/` folder. Create a branch for each exercise and open a PR to merge when complete.

- **Branching strategies**: GitHub Flow, Git Flow, and best practices3. Track progress using GitHub Issues or the `progress.md` file.

- **CI/CD**: GitHub Actions for build, test, and deployment

- **Security & Releases**: Signed commits, releases, and dependabotRepository layout



## 📚 Learning Path Structure- `MODULE-01-GIT-BASICS.md` — Core Git concepts and hands-on exercises

- `MODULE-02-GITHUB-CLI.md` — Professional `gh` CLI workflows and labs

| Module | Topic | Time | Difficulty |- `MODULE-03-BRANCHING-PRS.md` — Branching strategies and PR best practices

|--------|-------|------|------------|- `MODULE-04-CI-CD.md` — GitHub Actions practical examples and exercises

| [Module 01](./modules/MODULE-01-GIT-BASICS.md) | Git Fundamentals | 2-3 hrs | Beginner |- `MODULE-05-RELEASES-Security.md` — Releases, signing, and security practices

| [Module 02](./modules/MODULE-02-GITHUB-CLI.md) | GitHub CLI Professional Usage | 2-3 hrs | Beginner |- `exercises/` — Exercise prompts and solution branches

| [Module 03](./modules/MODULE-03-BRANCHING-PRS.md) | Branching & Pull Requests | 3-4 hrs | Intermediate |- `progress.md` — Track your completion status and milestones

| [Module 04](./modules/MODULE-04-CI-CD.md) | GitHub Actions & CI/CD | 3-4 hrs | Intermediate |- `LICENSE` — MIT

| [Module 05](./modules/MODULE-05-RELEASES-SECURITY.md) | Releases, Signing & Security | 2-3 hrs | Advanced |

External free resources

## 🚀 Getting Started

- Official Git docs: https://git-scm.com/doc

### Option 1: Learn Locally (Recommended)- Pro Git (book, free): https://git-scm.com/book/en/v2

Clone this repo and work through each module:- GitHub Docs: https://docs.github.com

- GitHub Learning Lab (interactive): https://lab.github.com

```bash- GitHub CLI docs: https://cli.github.com/manual

git clone https://github.com/rmansoura/github-learning-path.git- GitHub Actions docs: https://docs.github.com/actions

cd github-learning-path

```---



### Option 2: Fork and Create Your Learning BranchWork iteratively: finish Module 1, create an issue titled `module-1/done` and close it when complete. Use `gh` to automate issue creation and PRs as part of the exercises.
```bash
# After forking on GitHub
git clone https://github.com/YOUR-USERNAME/github-learning-path.git
cd github-learning-path
git checkout -b learning/your-name
```

## 📖 How to Use This Repository

1. **Read the module** — Start with Module 01 and read the concepts
2. **Do the exercises** — Try hands-on exercises before looking at solutions
3. **Review solutions** — Check the `solutions/` folder for reference implementations
4. **Create an issue** — Track your progress with GitHub Issues
5. **Open a PR** — Practice PR workflow by committing your exercise work and opening a PR

### Example Workflow for Each Module
```bash
# 1. Create a branch for the module
git checkout -b exercise/module-01/fundamentals

# 2. Complete exercises in exercises/module-01/
# 3. Commit your work
git add .
git commit -m "Complete Module 01 exercises: git basics"

# 4. Push and create a PR
git push -u origin exercise/module-01/fundamentals
gh pr create --fill

# 5. Track in progress.md
```

## 📁 Repository Structure

```
github-learning-path/
├── README.md                          # You are here
├── progress.md                        # Track your completion
├── LICENSE                            # MIT License
├── modules/
│   ├── MODULE-01-GIT-BASICS.md
│   ├── MODULE-02-GITHUB-CLI.md
│   ├── MODULE-03-BRANCHING-PRS.md
│   ├── MODULE-04-CI-CD.md
│   └── MODULE-05-RELEASES-SECURITY.md
├── exercises/
│   ├── module-01/
│   │   ├── 01-init-and-commit/
│   │   ├── 02-explore-history/
│   │   ├── 03-branch-and-merge/
│   │   ├── 04-resolve-conflicts/
│   │   └── 05-recover-from-mistakes/
│   ├── module-02/
│   ├── module-03/
│   ├── module-04/
│   └── module-05/
├── solutions/
│   ├── module-01/
│   ├── module-02/
│   ├── module-03/
│   ├── module-04/
│   └── module-05/
├── scripts/
│   ├── auto-issue.sh                 # Auto-create issues
│   ├── setup-gh-aliases.sh            # Setup useful gh aliases
│   └── scaffold-exercise.sh            # Quick exercise setup
├── .github/
│   ├── workflows/
│   │   ├── ci.yml                     # CI/CD pipeline
│   │   └── module-tracking.yml        # Progress automation
│   ├── ISSUE_TEMPLATE/
│   │   ├── module-completion.md
│   │   └── bug.md
│   └── pull_request_template.md
└── resources/
    └── EXTERNAL-RESOURCES.md         # Curated free resources
```

## 🎓 Prerequisites

- **Git** installed locally: `git --version`
- **GitHub account** (free tier is fine)
- **GitHub CLI (`gh`)** installed: `gh --version`
- **Text editor** (VS Code, vim, nano, etc.)
- **Terminal/shell** experience (zsh, bash)

### Quick Setup Check
```bash
git --version
gh --version
gh auth status
```

## ⚡ Quick Reference Commands

### Git
```bash
git init                              # Start a repo
git add .                             # Stage changes
git commit -m "message"               # Commit
git branch -a                         # List branches
git checkout -b feature/xyz           # Create branch
git merge feature/xyz                 # Merge branch
git log --oneline --graph --all       # View history
```

### GitHub CLI (gh)
```bash
gh repo create myrepo --public        # Create repo
gh pr create --fill                   # Create PR
gh issue create -t "Title" -b "Body"  # Create issue
gh pr list                            # List PRs
gh release create v1.0.0              # Create release
```

## 📊 Progress Tracking

Track your completion in [progress.md](./progress.md):
- [ ] Module 01 — Git Basics
- [ ] Module 02 — GitHub CLI
- [ ] Module 03 — Branching & PRs
- [ ] Module 04 — CI/CD
- [ ] Module 05 — Releases & Security

### Milestones
- **Fundamentals**: Complete Modules 1-2
- **Intermediate**: Complete Modules 1-3
- **Advanced**: Complete all 5 modules

## 🔗 Free External Resources

See [EXTERNAL-RESOURCES.md](./resources/EXTERNAL-RESOURCES.md) for:
- Official documentation links
- Interactive tutorials
- YouTube playlists
- Books and articles
- GitHub Learning Lab courses

## 💡 Tips for Success

1. **Hands-on is key** — Type every command yourself; don't copy-paste everything
2. **Experiment** — Break things and fix them; that's how you learn
3. **Use `gh`** — Prefer the CLI over web UI to build professional skills
4. **Read error messages** — They're usually helpful!
5. **Ask questions** — Open issues if you're stuck or want clarification
6. **Teach others** — The best way to solidify learning is to explain it

## 🤝 Contributing & Feedback

Found a typo, unclear explanation, or want to add exercises?
- Open an issue: `gh issue create -t "Feedback: Module X" -b "Description"`
- Fork, fix, and PR: `gh pr create --fill`

## 📝 License

MIT License — feel free to use, modify, and share this learning path.

---

**Ready to level up your GitHub skills?** Start with [Module 01](./modules/MODULE-01-GIT-BASICS.md) now! 🚀
