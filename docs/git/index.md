# 📚 Git Configuration & Workflows

Configuração avançada do Git, aliases úteis e workflows modernos para desenvolvimento colaborativo.

## ⚙️ **Core Configuration**

### Initial Git Setup
```bash
# Install Git and GitHub CLI
brew install git gh

# Basic configuration
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
```

## 🔧 **Advanced Configuration**

### Enhanced Configuration
```bash
# Editor and diff tools
git config --global core.editor "code --wait"
git config --global diff.tool vscode
git config --global merge.tool vscode

# Line endings (important for macOS/Linux)
git config --global core.autocrlf input
git config --global pull.rebase false

# Performance optimizations
git config --global core.preloadindex true
git config --global core.fscache true
```

### Performance & Security
```bash
# GPG signing para commits
git config --global commit.gpgsign true
git config --global user.signingkey YOUR_GPG_KEY

# Credential helper
git config --global credential.helper osxkeychain

# Performance para repositórios grandes
git config --global core.preloadindex true
git config --global core.fscache true
```

## ⚡ **Power User Aliases**

### Essential Shortcuts
```bash
# Status e logs melhorados
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit

# Logs visuais
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global alias.lga "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all"
```

### Advanced Workflows
```bash
# Undo commits (mantém mudanças)
git config --global alias.undo "reset HEAD~1 --mixed"

# Clean branches
git config --global alias.cleanup "!git branch --merged | grep -v '^*\\|main\\|master' | xargs -n 1 git branch -d"

# Push force com segurança
git config --global alias.pushf "push --force-with-lease"
```

## 🌊 **GitHub Workflow**

### Repository Setup
```bash
# Create and connect to GitHub
mkdir my-project && cd my-project
git init
touch README.md
echo "# My Project" > README.md

# First commit
git add .
git commit -m "Initial commit"

# Connect to GitHub (GitHub CLI)
gh repo create my-project --public --source=. --remote=origin --push
```

### Daily Workflow
```bash
# Create feature branch
git checkout -b feature/new-feature

# Make changes and commit
git add .
git commit -m "feat: implement new feature"

# Push and create PR
git push -u origin feature/new-feature
gh pr create --title "Add new feature" --body "Description"
```

## 🔍 **Essential Git Commands**

### Daily Operations
```bash
# Status and inspection
git status
git diff
git log --oneline --graph

# Branch management
git checkout -b new-branch    # Create and switch
git branch -a                 # List all branches
git branch -d branch-name     # Delete branch

# Stashing changes
git stash                     # Stash changes
git stash pop                 # Apply and remove stash
git stash list                # List stashes
```

### Advanced Operations
```bash
# Interactive rebase
git rebase -i HEAD~3

# Amend last commit
git commit --amend

# Cherry-pick commits
git cherry-pick <commit-hash>

# Reset operations
git reset --soft HEAD~1       # Keep changes staged
git reset --hard HEAD~1       # Discard changes
```

## 🔀 **Branch Management**

### Naming Conventions
```bash
# Features
feature/user-authentication
feature/payment-integration

# Bug fixes
bugfix/login-error
hotfix/critical-security-issue

# Releases
release/v1.2.0
```

### Branch Protection Rules
- **Require PR reviews** antes de merge
- **Status checks** obrigatórios
- **Up-to-date branches** antes de merge
- **Linear history** enforcement

## 🔒 **SSH & Security Setup**

### SSH Key Configuration
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519

# Copy public key
pbcopy < ~/.ssh/id_ed25519.pub
# Add to GitHub: Settings → SSH and GPG keys
```

## 📝 **Git Best Practices**

### Commit Guidelines
```bash
# Conventional commit format
feat(auth): add OAuth2 integration
fix(api): resolve memory leak
docs(readme): update setup instructions
refactor(utils): simplify date formatting
test(auth): add login integration tests
```

### Branch Strategy
- **main** → Production-ready code
- **feature/feature-name** → New features
- **bugfix/issue-description** → Bug fixes
- **hotfix/critical-fix** → Emergency fixes

### Clean History
```bash
# Interactive rebase before merge
git rebase -i HEAD~3

# Squash commits in PR
gh pr merge 123 --squash

# Force push safely
git push --force-with-lease
```

## 🤖 **GitHub Actions Integration**

### Basic CI/CD Setup
```yaml
# .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-node@v4
      with:
        node-version: '18'
    - run: npm ci
    - run: npm test
```

### Auto-deployment
```yaml
# Deploy on push to main
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Deploy
      run: echo "Deploy to production"
```

---

## 🚑 **Emergency Git Operations**

### Fixing Mistakes
```bash
# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Revert commit (safe for shared repos)
git revert <commit-hash>

# Find lost commits
git reflog
git cherry-pick <lost-commit-hash>
```

### Conflict Resolution
```bash
# During merge conflict
git status                    # See conflicted files
git diff                     # See conflict markers

# After resolving
git add <resolved-file>
git commit

# Abort merge
git merge --abort
```

---

> 🚀 **Pro Tip:** Use GitHub CLI (`gh`) para acelerar workflows. Combine `git rebase -i` com conventional commits para um histórico limpo!