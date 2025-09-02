# 🐍 Python Development

Setup moderno e workflows para desenvolvimento Python usando as melhores práticas e ferramentas atuais.

## ⚡ **UV Package Manager - Ultra-Fast Python Development**

<details>
<summary><strong>🚀 Quick Start & Installation</strong></summary>

### Installation
```bash
# macOS/Linux - Método recomendado
curl -LsSf https://astral.sh/uv/install.sh | sh

# Homebrew (alternativa)
brew install uv

# Verificar instalação
uv --version
```

### New User Setup (Cloning Projects)
```bash
# 1. Clone existing UV project
git clone <repository-url>
cd <project-name>

# 2. UV auto-setup
uv sync                         # Install from pyproject.toml + uv.lock
uv run python main.py          # Run without activating environment

# 🔄 Traditional fallback (if needed)
python -m venv .venv
source .venv/bin/activate       
pip install -r requirements.txt
```

> ⚡ **Pro Tip:** `uv sync` installs **exact versions** from `uv.lock` for total consistency!

</details>

<details>
<summary><strong>🏗️ Project Creation & Setup</strong></summary>

### New Project Workflow
```bash
# Create new project
uv init my-analytics-project --python 3.12
cd my-analytics-project

# Auto-created structure:
# my-analytics-project/
# ├── pyproject.toml     # Config + dependencies
# ├── README.md
# ├── .python-version    # Python version requirement  
# └── src/               # Source code directory
```

### Existing Project Setup
```bash
# Convert existing project
cd existing-project
uv init                        # Create pyproject.toml
uv add -r requirements.txt     # Migrate dependencies
```

</details>

<details>
<summary><strong>📦 Dependency Management</strong></summary>

### Core Dependencies
```bash
# Data Science essentials
uv add pandas numpy matplotlib seaborn plotly
uv add jupyter sqlalchemy psycopg2-binary

# Advanced analytics
uv add scikit-learn scipy statsmodels
uv add great-expectations pandas-profiling
```

### Development Dependencies
```bash
# Quality tools (dev only)
uv add --dev pytest pytest-cov black ruff mypy
uv add --dev pre-commit jupyter-lab ipykernel

# Group dependencies by purpose
uv add --group test pytest pytest-mock
uv add --group lint ruff black mypy
uv add --group docs mkdocs mkdocs-material
```

### Smart Version Control
```bash
# Specific versions with constraints
uv add pandas>=2.0.0 "numpy<2.0"
uv add "sqlalchemy[postgresql,mysql]"

# Remove & update
uv remove package-name
uv sync --upgrade
```

</details>

<details>
<summary><strong>🐍 Python Version Management</strong></summary>

```bash
# Install specific versions
uv python install 3.11 3.12 3.13

# List available versions
uv python list

# Project with specific version
uv init --python 3.12 my-project

# Change version in existing project  
uv python pin 3.11
uv sync  # Reinstall with new version
```

### Version Strategy
```toml
# pyproject.toml - Flexible constraint
[project]
requires-python = ">=3.11"

# .python-version - Exact version for team
3.12.0
```

</details>

<details>
<summary><strong>⚡ High-Performance Execution</strong></summary>

### Direct Execution (No venv activation needed)
```bash
uv run python analysis.py
uv run jupyter lab
uv run pytest tests/
uv run mypy src/

# Temporary packages (not added to project)
uv run --with requests --with beautifulsoup4 python scraper.py

# Inline dependencies for scripts
uv run --with pandas --with matplotlib python data_viz.py
```

### Performance Benefits
| Operation | pip | conda | **UV** | Speedup |
|----------|-----|-------|--------|---------|
| Install 50 packages | 45s | 60s | **2s** | **22-30x** |
| Resolve conflicts | 30s | 40s | **1s** | **30-40x** |
| Create venv | 8s | 15s | **0.3s** | **25-50x** |

</details>

<details>
<summary><strong>🔒 Reproducible Builds</strong></summary>

### Lock File Management
```bash
uv lock                        # Create/update uv.lock
uv sync --frozen               # Fail if not synced with lock
uv sync --locked               # Use exact versions from lock
uv sync --upgrade              # Upgrade within constraints
```

### Export Compatibility
```bash
# Export to other formats
uv export --format requirements-txt --output-file requirements.txt
uv export --no-dev --output-file requirements.txt

# Import from other formats
uv add -r requirements.txt
```

</details>

<details>
<summary><strong>🛠️ Complete Project Workflow</strong></summary>

### 1. New Project Creation
```bash
# Create directory and navigate
mkdir my-analytics-project
cd my-analytics-project

# Git setup
git init
gset-dcs  # Your git config alias
gh dcs    # Your GitHub CLI alias
```

### 2. Python Environment with UV
```bash
# Install specific Python version
uv python install 3.12

# Initialize UV project
uv init --python 3.12

# Auto-created structure:
# my-analytics-project/
# ├── pyproject.toml
# ├── README.md  
# ├── .python-version
# └── src/
```

### 3. Dependencies & Project Structure
```bash
# Core data science stack
uv add pandas numpy matplotlib seaborn plotly
uv add jupyter sqlalchemy psycopg2-binary

# Development tools
uv add --dev pytest black ruff mypy pre-commit

# Create full project structure
mkdir -p {data/{raw,processed,interim},notebooks,scripts,tests,docs}
touch {main.py,.env.example,.gitignore}
```

### 4. Configuration Files
```bash
# Enhanced .gitignore
echo -e ".venv/\n__pycache__/\n.ipynb_checkpoints/\n*.pyc\n.DS_Store\ndata/raw/\n.env" > .gitignore

# Lock dependencies
uv lock

# Generate pip compatibility (optional)
uv export --format requirements-txt --output-file requirements.txt
```

### 5. GitHub Integration
```bash
# Initial commit
git add .
git commit -m "Initial commit: Project structure and dependencies"

# Create and push to GitHub (requires gh auth login)
gh repo create $(basename "$PWD") --private --source=. --remote=origin --push

# OR: Manual repository link
# git remote add origin git@github.com:username/project.git
# git branch -M main
# git push -u origin main
```

</details>

<details>
<summary><strong>👥 Team Collaboration Setup</strong></summary>

### For New Team Members (Cloning)
```bash
# UV Method (recommended)
git clone <repository-url>
cd <project-name>
uv sync                         # Install from pyproject.toml + uv.lock
uv run python main.py          # Run without activation

# Traditional Method (fallback)
git clone <repository-url>
cd <project-name>
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Daily Development Workflow
```bash
# Pull latest changes
git pull

# Work with UV commands
uv run python main.py
uv run jupyter lab
uv run pytest

# Add new dependencies
uv add new-package
uv lock  # Lock new versions

# Commit & push
git add .
git commit -m "Describe changes"
git push
```

</details>

<details>
<summary><strong>🔧 Advanced Configuration</strong></summary>

### Enhanced pyproject.toml
```toml
[project]
name = "my-analytics-project"
version = "0.1.0"
description = "Data analytics project with modern Python stack"
requires-python = ">=3.11"
dependencies = [
    "pandas>=2.0.0",
    "numpy",
    "matplotlib",
    "seaborn",
]

[project.optional-dependencies]
dev = ["pytest", "black", "ruff", "mypy", "pre-commit"]
ml = ["scikit-learn", "xgboost", "lightgbm"] 
api = ["fastapi", "uvicorn", "pydantic"]
dashboard = ["streamlit", "plotly", "dash"]

[project.scripts]
start = "python main.py"
test = "pytest tests/"
lint = "ruff check src/"
```

### Pre-commit Setup (Optional)
```bash
# Install pre-commit hooks
uv add --dev pre-commit
uv run pre-commit install

# Create .pre-commit-config.yaml
echo "repos:
  - repo: https://github.com/psf/black
    rev: 23.0.0
    hooks:
      - id: black
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.1.0
    hooks:
      - id: ruff" > .pre-commit-config.yaml
```

</details>

- **[Python Setup](python.md)** - Configuração completa do ambiente Python

## 🏗️ **Project Structure & Best Practices**

### Modern Python Stack
- **UV** para gestão de dependências e ambientes virtuais
- **Ruff** para linting e formatting ultra-rápido
- **pytest** para testing robusto
- **pre-commit hooks** para qualidade de código
- **GitHub Actions** para CI/CD

### Development Workflow
```bash
# Inicializar projeto moderno
uv init --python 3.12
uv add fastapi uvicorn pytest ruff

# Desenvolvimento
uv run python main.py
uv run pytest
uv run ruff check .
```

## 🚀 **Performance & Optimization**

- **Type hints** para melhor maintainability
- **Async/await** patterns para concorrência
- **Pydantic** para validation de dados
- **FastAPI** para APIs modernas e rápidas

## 📦 **Package Management**

### UV Advantages
- **10-100x mais rápido** que pip/conda
- **Lock files** automáticos para builds reproduzíveis
- **Python version management** integrado
- **Virtual environments** automáticos

### Migration from pip/conda
```bash
# Migrar requirements.txt
uv add -r requirements.txt

# Migrar environment.yml
uv python install 3.12
uv venv --python 3.12
```

## 🧪 **Testing & Quality**

- **pytest** com fixtures e parametrização
- **Coverage reports** integrados
- **Ruff** para linting moderno
- **pre-commit** para validações automáticas

---

> 🔥 **Pro Tip:** UV revoluciona o desenvolvimento Python - experimenta e nunca mais voltarás ao pip tradicional!