# 📚 Git Configuration & Workflows

Configuração avançada do Git, aliases úteis e workflows modernos para desenvolvimento colaborativo.

## ⚙️ **Core Configuration**

- **[Git Complete Setup](git.md)** - Configuração completa com aliases e best practices

## 🔧 **Advanced Configuration**

### Essential Git Settings
```bash
# Configuração global básica
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Editor e diff tool
git config --global core.editor "code --wait"
git config --global diff.tool vscode
git config --global merge.tool vscode

# Line endings (importante para macOS/Linux)
git config --global core.autocrlf input
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

## 🌊 **Modern Git Workflows**

### GitHub Flow
1. **Create branch** from main
2. **Add commits** com mensagens descritivas
3. **Open PR** para review
4. **Deploy** para testing
5. **Merge** após approval

### GitFlow (projetos complexos)
- **main** - Production ready
- **develop** - Integration branch
- **feature/** - New features
- **release/** - Release preparation
- **hotfix/** - Emergency fixes

## 🛠️ **Git Tools & Integrations**

### Command Line Tools
- **GitHub CLI** (`gh`) - GitHub integration
- **Git Delta** - Better diff viewer
- **Lazygit** - Terminal UI for Git
- **GitKraken** - Visual Git client

### VSCode Extensions
- **GitLens** - Git supercharged
- **Git Graph** - Repository visualization
- **Git History** - File history viewer

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

## 🏷️ **Commit Best Practices**

### Conventional Commits
```bash
# Formato: type(scope): description
feat(auth): add OAuth2 integration
fix(api): resolve memory leak in user service  
docs(readme): update installation instructions
refactor(utils): simplify date formatting
```

### Commit Types
- **feat** - Nova funcionalidade
- **fix** - Bug fix
- **docs** - Documentação
- **style** - Formatting (sem mudança de código)
- **refactor** - Code refactoring
- **test** - Adicionar/modificar tests
- **chore** - Maintenance tasks

---

> 🚀 **Pro Tip:** Usa `git rebase -i` para limpar o histórico antes de fazer merge para main!